# Changelog

## 1.0.0 - 2026-05-31

### Added
- Initial public plugin release for `logic-patterns` and `development-patterns`.
- Logic plugin skills: `1-3-1`, `adversary-loop`, `bifurcate`, `first-principles-rebuild`, `gaslight-loop`, `greyhat`, `make-a-plan`, `multidimensional-planning`.
- Development plugin skills: `architecture-decision-records`, `architecture-spec`, `centralized-fix-selection`, `ephemeral-testing`, `implement-plan`, `plan-checklists`, `spec-driven-development`.
- Plugin manifests with proper `.codex-plugin/plugin.json` metadata and logos.
- Root README with install guidance and plugin/skill catalog.

### Changed
- Reorganized repository into two plugins: `logic-patterns` and `development-patterns`.
- Standardized skill documentation and references to local plugin paths.
- Updated distribution model to support direct Codex marketplace installation.

### Removed
- Removed outdated or duplicate/deprecated skills from this plugin package (`go-coding-practices`, `ts-coding-practices`, `skill-routing-orchestrator`, and other non-local references).
- Removed hardcoded/incompatible external references and user-specific paths from distribution content.

### Security/Operations
- Added explicit installation path that does not require skill-by-skill configuration.
- Simplified install workflow to two concise CLI options for plugin users.
