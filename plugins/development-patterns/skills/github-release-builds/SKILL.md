---
name: "github-release-builds"
description: "Use when adding or repairing GitHub Actions release automation that builds multi-arch container images on semver tags, pushes them to GitHub Container Registry, updates example healthcheck manifests, creates GitHub releases, and documents the tag-only release process in RELEASE.md."
---

# GitHub Release Builds

Use this pattern when a repository should release container images from GitHub Actions only.

## Intent

A maintainer creates a semver tag. GitHub Actions then:
- validates the tag
- builds `linux/amd64` and `linux/arm64`
- pushes the image to GHCR
- updates example manifests to the released image tag
- creates or updates the GitHub release
- attaches `release-images.txt`

Prefer GHCR for image hosting. GitHub Actions artifacts are not a container registry and are not suitable for Kubernetes `image:` references.

## Required Repository Shape

Use these names unless the repository has a strong existing convention:
- release docs: `RELEASE.md`
- pull request validation workflow: `.github/workflows/validate-image-build.yaml`
- main branch image workflow: `.github/workflows/publish.yaml`
- semver tag release workflow: `.github/workflows/publish-tag.yaml`
- example manifest: root `healthcheck.yaml`
- image: `ghcr.io/<org>/<repo>:<tag>`

If an example manifest has a repo-specific name, rename it to `healthcheck.yaml` when consistency matters.

## Containerfile Requirements

For Go checks, make the build platform explicit:

```Dockerfile
FROM --platform=$BUILDPLATFORM docker.io/library/golang:1.24 AS builder

ARG TARGETOS
ARG TARGETARCH

WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o /check ./cmd

FROM gcr.io/distroless/static:nonroot
COPY --from=builder /check /check
USER nonroot:nonroot
ENTRYPOINT ["/check"]
```

Keep the existing binary path, package path, base image, and runtime shape if the repo already has working equivalents. The critical pieces are `--platform=$BUILDPLATFORM`, `ARG TARGETOS`, `ARG TARGETARCH`, and `GOOS/GOARCH`.

## Pull Request Validation

Add `.github/workflows/validate-image-build.yaml`:

```yaml
name: Validate image build

on:
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Containerfile
          platforms: linux/amd64,linux/arm64
          push: false
```

Use `file: ./Dockerfile` only when the repo already uses `Dockerfile` and the user does not want it renamed.

## Main Branch Image Workflow

Add `.github/workflows/publish.yaml` for non-release images:

```yaml
name: Publish image

on:
  push:
    branches:
      - main

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ github.token }}

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Containerfile
          platforms: linux/amd64,linux/arm64
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:latest
            ghcr.io/${{ github.repository }}:sha-${{ github.sha }}
```

If short SHA tags are required, add a step that computes `SHORT_SHA=${GITHUB_SHA::7}` and use `sha-${{ env.SHORT_SHA }}`.

## Tag Release Workflow

Add `.github/workflows/publish-tag.yaml`.

Use a specific escaped repository name in the Ruby image regex. For repo `pod-status-check`, use `pod\-status\-check`.

````yaml
name: Publish tag

on:
  push:
    tags:
      - "v*"

jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      packages: write
    steps:
      - name: Checkout tag
        uses: actions/checkout@v4

      - name: Validate release tag
        run: |
          if [[ ! "${GITHUB_REF_NAME}" =~ ^v[0-9]+[.][0-9]+[.][0-9]+(-[0-9A-Za-z.-]+)?$ ]]; then
            echo "Release tags must use semver like v1.2.3 or v1.2.3-rc.1"
            exit 1
          fi

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ github.token }}

      - name: Set image tag
        run: |
          IMAGE_REPO="ghcr.io/${GITHUB_REPOSITORY}"
          IMAGE_TAG="${IMAGE_REPO}:${GITHUB_REF_NAME}"
          PACKAGE_URL="https://github.com/orgs/${GITHUB_REPOSITORY_OWNER}/packages/container/package/${GITHUB_REPOSITORY#*/}"
          echo "IMAGE_REPO=${IMAGE_REPO}" >> "$GITHUB_ENV"
          echo "IMAGE_TAG=${IMAGE_TAG}" >> "$GITHUB_ENV"
          echo "PACKAGE_URL=${PACKAGE_URL}" >> "$GITHUB_ENV"

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Containerfile
          platforms: linux/amd64,linux/arm64
          push: true
          tags: ${{ env.IMAGE_TAG }}

      - name: Update healthcheck examples
        env:
          GH_TOKEN: ${{ github.token }}
        run: |
          git fetch origin main
          git checkout main
          git pull --ff-only origin main
          ruby -e 'image_tag = ENV.fetch("IMAGE_TAG"); files = Dir.glob("*.yaml") + Dir.glob("*.yml"); files.each do |path| text = File.read(path); updated = text.gsub(/(image:\s+)(?:\S*\/)?REPO\-NAME:[^\s]+/) { "#{$1}#{image_tag}" }; File.write(path, updated) if updated != text; end'

          if git diff --quiet; then
            echo "No healthcheck image references needed updates."
            exit 0
          fi

          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git ls-files '*.yaml' '*.yml' | xargs git add
          git commit -m "chore: update healthcheck image for ${GITHUB_REF_NAME}"
          git push origin main

      - name: Create release notes
        run: |
          cat > release-images.txt <<EOF
          Image: ${IMAGE_TAG}
          Platforms: linux/amd64, linux/arm64
          GitHub Package: ${PACKAGE_URL}
          EOF

          cat > release-notes.md <<EOF
          Container images were published for this release.

          - Image: `${IMAGE_TAG}`
          - Platforms: `linux/amd64`, `linux/arm64`
          - GitHub Package: ${PACKAGE_URL}

          The example healthcheck manifest on `main` was updated to use this image tag.

          ```sh
          docker pull ${IMAGE_TAG}
          ```
          EOF

      - name: Create GitHub release
        env:
          GH_TOKEN: ${{ github.token }}
        run: |
          if gh release view "${GITHUB_REF_NAME}" >/dev/null 2>&1; then
            gh release edit "${GITHUB_REF_NAME}" --title "${GITHUB_REF_NAME}" --notes-file release-notes.md
          else
            gh release create "${GITHUB_REF_NAME}" --title "${GITHUB_REF_NAME}" --notes-file release-notes.md --verify-tag
          fi
          gh release upload "${GITHUB_REF_NAME}" release-images.txt --clobber
````

Replace `REPO\-NAME` with the escaped repository name and `file: ./Containerfile` as needed.

Never use `git add '*.yaml' '*.yml'`; it fails when one glob has no matches.

## RELEASE.md Template

Create or update `RELEASE.md`:

````markdown
# Release Process

Releases are automated by GitHub Actions.

## Create a Release

- Choose the next semver tag, such as `v1.2.3`.
- Push the tag to the repository:

  ```sh
  git tag -a v1.2.3 -m "Release v1.2.3"
  git push origin v1.2.3
  ```

- GitHub Actions builds and pushes `ghcr.io/ORG/REPO:v1.2.3`.
- The image is built for `linux/amd64` and `linux/arm64`.
- The root `healthcheck.yaml` example is updated to the released image tag.
- A GitHub release named `v1.2.3` is created.
- The release includes `release-images.txt` with image and platform details.

## Use the Image

```sh
docker pull ghcr.io/ORG/REPO:v1.2.3
```
````

Use the actual org and repo in the final file.

## Validation

Before opening or merging PRs:
- parse workflow YAML, for example `ruby -ryaml -e 'Dir[".github/workflows/*.yaml"].each { |f| YAML.load_file(f) }'`
- run `git diff --check`
- dry-run the Ruby image updater against temp copies of root YAML files
- verify pull request `Validate image build` succeeds

After tagging:
- verify the tag workflow started from the tag
- wait for it to complete successfully
- verify the GitHub release exists
- verify `release-images.txt` exists on the release
- verify `main` has `healthcheck.yaml` pointing at `ghcr.io/<org>/<repo>:<tag>`

If a tag workflow fails because the workflow itself was wrong, fix `main` first. Moving an existing remote tag is destructive; ask for explicit approval before force-updating release tags.
