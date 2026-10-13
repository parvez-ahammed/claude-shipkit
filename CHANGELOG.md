# Changelog

All notable changes to ShipKit are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2026-10-13

First public release, rebranded as **ShipKit**.

### Changed

- **Rebrand to ShipKit.** The plugin, the marketplace and the repository are all named
  `shipkit`. Skills are now invoked as `/shipkit:<skill>`.
- **All skills renamed to verb-object** names, so the name says what the skill does:

  | Old name                     | New name                     |
  | ---------------------------- | ---------------------------- |
  | `bug-fixer`                  | `fix-bug`                    |
  | `story-implementer`          | `build-story`                |
  | `branch-reviewer`            | `review-branch`              |
  | `branch-test-report`         | `test-branch`                |
  | `qa-ux`                      | `audit-app`                  |
  | `generate-ux-improvements`   | `find-ux-gaps`               |
  | `platform-audit`             | `find-shared-code`           |
  | `refactoring-guru`           | `refactor`                   |
  | `code-smell-detector`        | `find-code-smells`           |
  | `refactoring-methods`        | `apply-refactoring`          |
  | `creational-patterns`        | `pick-creational-pattern`    |
  | `structural-patterns`        | `pick-structural-pattern`    |
  | `behavioral-patterns`        | `pick-behavioral-pattern`    |
  | `clean-architecture`         | `enforce-clean-architecture` |
  | `vps-deploy`                 | `deploy-to-vps`              |
  | `honest-health-check`        | `add-health-check`           |
  | `seo-aeo`                    | `boost-seo`                  |
  | `video-maker`                | `make-video`                 |
  | `demo-maker`                 | `make-demo`                  |
  | `user-guide-maker`           | `write-user-guide`           |
  | `plain-writer`               | `write-plainly`              |
  | `timesheet-from-transcripts` | `fill-timesheet`             |
  | `grill-feature`              | `grill-feature` (unchanged)  |
  | `show-usage`                 | `show-usage` (unchanged)     |

### Added

- `CONTRIBUTING.md`, `SECURITY.md`, `CODE_OF_CONDUCT.md`, issue and PR templates.
- CI workflow that validates skill frontmatter, manifests and script syntax, and blocks
  personal email addresses.

### Upgrading

If you installed a pre-1.0 version, uninstall it, then run the two install commands in the
README. Replace old skill names with the new ones from the table above.

## [0.5.0]

Public-readiness audit of every skill.

### Changed

- All skills now declare `license: MIT`, and `compatibility` where they need installed
  tools.
- Scripts made cross-platform (Windows, macOS, Linux) and run through their interpreter.
- UX skills confirm with the user before filing issues or tickets.

### Added

- Installers for the git hooks (`install.sh` and `install.ps1` for `secret-hygiene` and
  `safe-commit`).

### Fixed

- `vps-deploy`: secrets handling.
- `clean-architecture`: layering guard.
- `seo-aeo`: crash on exit in the audit script.
- `show-usage`: `--project` option.
- `branch-reviewer`: base branch detection.

[Unreleased]: https://github.com/parvez-ahammed/shipkit/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/parvez-ahammed/shipkit/releases/tag/v1.0.0
