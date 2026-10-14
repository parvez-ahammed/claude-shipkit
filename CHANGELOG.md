# Changelog

All notable changes to ShipKit are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this
project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [2.0.0] - 2026-10-14

### Removed

Six skills folded into the skills they served (30 -> 24). An A/B evaluation showed they added
no findings over a plain agent, and their descriptions competed with their neighbours for the
same requests.

- `find-code-smells`, `apply-refactoring`, `pick-creational-pattern`, `pick-structural-pattern`,
  `pick-behavioral-pattern` -> `refactor`. Their catalogs are now reference files in
  `refactor/references/` (`smells*.md`, `refactorings*.md`, `patterns-*.md`). `refactor` runs
  them inline instead of routing to other skills.
- `brief-milestone` -> `groom-issue` brief mode (`groom-issue/references/brief.md`).

### Upgrading

Replace `/shipkit:find-code-smells`, `/shipkit:apply-refactoring` and `/shipkit:pick-*-pattern`
with `/shipkit:refactor`, and `/shipkit:brief-milestone` with `/shipkit:groom-issue <milestone>
brief`. If you copied skills into `~/.claude/skills/`, delete the six old folders; the sync
scripts do not remove them.

### Changed

- All skill descriptions rewritten to say what the skill does and when to use it, each under
  530 characters, and each naming the neighbour skill to use instead. Seven overlapping pairs
  (for example `add-health-check` and `deploy-to-vps`, `trim-comments` and `write-plainly`) now
  route apart.
- `simplify-code`, `find-ux-gaps`, `grill-feature`: run inline on small targets; fan out
  subagents only for large ones, at most four at once, and fall back to inline work when a
  subagent cap is hit.
- `refactor` pattern scoring: one consistent anchor rule, and "downgrade one step" is
  defined. The verdict is about the class-based pattern; the language idiom goes in the
  recommendation.

### Fixed

- `make-video`: the shipped template now passes its own `check.mjs` (claims rows added,
  scene fades and entry timings shortened). `check.mjs` warns when a "template placeholder"
  claim is still present.
- `enforce-clean-architecture`: the default guard config now catches Node/TS and Python I/O
  imports and `../infra`-style paths; the skill tells you to prove the guard fails once.
- `add-health-check`: the probe gates on the HTTP status code (the reference no longer
  contradicts the skill); added the `localhost` -> `::1` and rolling-deploy traps.
- `boost-seo`: `audit-url.mjs` flags results from a localhost or stand-in server; the skill
  says how to stop the server on each OS.
- `fix-bug`: adjacent findings need the same proof as the main one; no-`CLAUDE.md` fallback;
  loop-bound trap; review step works without the Skill tool.
- `refactor` smells mode: a caller search is allowed for dead-code claims; bugs are reported
  apart from smells; a verdict rubric; Comments route to `trim-comments`.
- `groom-issue`: readiness gate rules are ordered and no longer overlap; template mapping
  and file-based input defined.
- `groom-issue` brief mode: reads an on-disk export; may check a premise the issue states
  against the repo.
- `refactor` mechanics: cross-module duplication row, multi-step plan shape, optional dry run
  in a throwaway copy.
- `trim-comments`: keep ticket refs to still-open problems and directives for tools the repo
  lacks; skip grep on small targets; prove the diff is comment-only.
- `write-plainly`: PR description length scales with the diff; the 4,000-character cap is a
  ceiling, not a target.

## [1.1.0] - 2026-10-13

### Added

Six new skills (30 in total):

- `groom-issue` - scores a ticket's readiness against a rubric, finds gaps and undeclared
  blockers, and rewrites it into a buildable plan after you confirm. GitHub, GitLab, Jira,
  Azure DevOps or plain markdown.
- `brief-milestone` - executive brief of a milestone or set of issues, in build order, with
  risks and open questions. Read-only.
- `deliver-issue` - one issue or a whole milestone end to end: groom, plan, build, review,
  test, ship. Every push, PR, comment and merge waits for a yes.
- `price-product` - pricing tiers, per-tier margin check, competitor prices with cited
  sources (never invented), value metric, and a contradiction check of every price surface.
- `simplify-code` - removes over-engineering in one behavior-preserving change set, verified
  by the repo's own lint, build and tests.
- `trim-comments` - deletes comments that restate code or narrate history, keeps the
  non-obvious why. Comment-only, any language.

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

[Unreleased]: https://github.com/parvez-ahammed/shipkit/compare/v2.0.0...HEAD
[2.0.0]: https://github.com/parvez-ahammed/shipkit/compare/v1.1.0...v2.0.0
[1.1.0]: https://github.com/parvez-ahammed/shipkit/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/parvez-ahammed/shipkit/releases/tag/v1.0.0
