<div align="center">

# 🧩 Claude Skills

**An evolving, open-sourced toolkit of [Claude Code](https://claude.com/claude-code) building blocks — skills, hooks, and commands.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-d97757.svg)](https://claude.com/claude-code)
[![PowerShell](https://img.shields.io/badge/PowerShell-5391FE.svg?logo=powershell&logoColor=white)](sync-skills.ps1)
[![Skills: 24](https://img.shields.io/badge/skills-24-6f42c1.svg)](INDEX.md)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#-contributing)

</div>

---

Every skill is stack-neutral and works on Windows, macOS and Linux unless its
**Requirements** column says otherwise. Skills that touch the outside world (posting PR
comments, filing tickets, attaching files) show you what they will send and wait for a yes.

Not just skills — a toolkit, because the right Claude Code primitive depends on the job:

| Primitive | Use it for | Triggered by |
|-----------|-----------|--------------|
| **Skill** | knowledge / a workflow the model *applies with judgment* ("how to do X") | the model, on a matching task |
| **Hook** | deterministic **must-run-on-event** enforcement ("always do X when Y") | the harness, automatically |
| **Command** | a saved prompt the **user** fires | `/name` |

Rule of thumb: "*always do X when Y happens*" is a **hook**, not a skill — the model
forgets, the harness doesn't. "*how should I approach X*" is a **skill**.

## ✨ What's inside

### Skills (`plugins/parvez/skills/`)

| Skill | What it does | Scope |
|-------|--------------|-------|
| **vps-deploy** | Vendor-neutral push-to-deploy CI/CD to a VPS (any runtime: Node/Python/Go/.NET), Caddy auto-TLS, OAuth/OIDC redirect setup, DNS (proxied-by-default), and baseline hardening (fail2ban, SSH-port rotation, firewall, CDN origin lockdown). | generic |
| **honest-health-check** | Design health endpoints that assert real readiness (DB/deps reachable) instead of a hardcoded 200, and wire them into deploy gates + container/orchestrator probes. Pairs with `vps-deploy`. | generic |
| **generate-ux-improvements** | Parallel UX-gap audit that ends in tracked work. Partitions the app into disjoint territories, dispatches N auditors (default 3) for M issues each (default 3), triages, writes a markdown file, and files to GitHub, GitLab, Jira or Azure DevOps only after you confirm. Audits from source when the app cannot be run. | generic |
| **clean-architecture** | Rulebook + bootstrap + config-driven guard for Clean / Hexagonal architecture in any language (.NET/TS/Java/Go/Python). The Dependency Rule, ports & adapters, per-stack layout, and a layering check to enforce boundaries. | generic |
| **refactoring-guru** | Orchestrator for refactoring + design-pattern work. Understands intent, routes to the spokes below, and only recommends a pattern when a code site meets scored preconditions with cited evidence. | generic |
| **code-smell-detector** | Detect the 22 refactoring.guru smells (Bloaters, OO-abusers, Change-preventers, Dispensables, Couplers) in a file/dir, each with `file:line` evidence mapped to a candidate fix. Spoke of refactoring-guru. | generic |
| **refactoring-methods** | Martin Fowler's refactoring catalog: preconditions + ordered, safe mechanics to fix a known smell (extract method, replace conditional with polymorphism, introduce parameter object...). Spoke. | generic |
| **creational-patterns** | Score whether a creational GoF pattern (Factory, Abstract Factory, Builder, Prototype, Singleton) fits a site; weighted preconditions + hard vetoes, emits APPLY/CONSIDER/REJECT with evidence. Spoke. | generic |
| **structural-patterns** | Same scoring for structural patterns (Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy). Spoke. | generic |
| **behavioral-patterns** | Same scoring for behavioral patterns (Strategy, State, Command, Observer, Chain of Responsibility, Template Method, Visitor, Mediator, Iterator, Memento). Spoke. | generic |
| **platform-audit** | Find what to make shared across a milestone/backlog with DRY/KISS/YAGNI judgment: candidate commons, a verdict each (build-now / narrow / resist), a cross-phase ownership check, and a build order. Resists over-abstraction. | generic |
| **grill-feature** | Promise-level, code-grounded interrogation of shipped features (does the real promise hold in the code, not "does it compile") returning BULLETPROOF / CRACKED / FAKE with `file:line` evidence, plus an optional unit-economics pass. Inline or workflow sweep. | generic |
| **show-usage** | WakaTime-style local active time + token/cost/tool stats from Claude Code session transcripts. Read-only, zero-dep. | generic |
| **qa-ux** | Pressure-test a running web app: a QA release report and a UX product-experience audit, two cross-linked HTML reports with screenshot evidence. | generic |
| **video-maker** | Animated promo / explainer / social videos built entirely from code: HTML scenes, local TTS voice-over, generated music, captions and thumbnail, rendered to MP4. A claims check gates every on-screen statement. | generic |
| **demo-maker** | Pixel-exact clickable HTML demos of a planned workflow, captured from the real running app (DOM + CSS snapshot) with an optional Windows desktop (WinForms) adapter, so the product owner sees exactly how it will look. Stays close to today's app. | generic |
| **user-guide-maker** | User-facing feature guide for a ticket or PR as self-contained HTML + PDF. Learns from the code, then a fresh agent checks every claim. Optional attach to Azure DevOps, Jira or GitHub. | generic |
| **branch-reviewer** | Multi-seat review of a branch / PR (backend, frontend, architecture, tests, QA, text, plus a licence seat for new dependencies), a refutation pass, a ranked punch list and an HTML report. Advanced mode adds an adversarial round; comment mode re-verifies each claim before posting to GitHub, Azure DevOps or GitLab. Reads your project rules file. | generic |
| **branch-test-report** | Runs a branch against the live app (not a reading of the diff): seeds data, predicts results with queries, drives the browser, and writes an HTML report with screenshots for a product owner (base vs branch, open items only). | generic |
| **bug-fixer** | One bug end-to-end: reproduce, root cause before fix, a regression test that fails without the fix, then verify. Hard-won debugging traps included. | generic |
| **story-implementer** | One user story end-to-end: analyze, break down, pick tasks, implement, test, verify, with a pre-PR gate (criteria trace, backward compatibility, every client that calls a changed endpoint). | generic |
| **plain-writer** | House style for prose that lands in git, PRs and docs: ASCII punctuation, Simplified Technical English, no jargon, short commit messages, why-only code comments, plus a PR-description sub-skill that fills any repo's template. | generic |
| **seo-aeo** | Audit and improve a site's SEO and AEO (AI answer engines): meta, JSON-LD, robots, sitemap, llms.txt, prerendering for SPAs, then prove it with local test scripts. | generic |
| **timesheet-from-transcripts** | Reconstruct what you worked on for past dates (tickets, descriptions, time blocks) from local Claude Code transcripts, ready for a timesheet. Configurable ticket patterns. | generic |

### Requirements

Most skills are pure reasoning and need nothing beyond Claude Code. The ones that run
bundled tools declare it in their `compatibility` frontmatter:

| Skill | Needs |
|-------|-------|
| video-maker | Node 18+, `npm ci` in `engine/` (bundles Playwright Chromium and ffmpeg) |
| demo-maker | Python 3, a browser tool (e.g. Playwright MCP); optional .NET 8 for the Windows desktop adapter |
| user-guide-maker | Python 3.8+ with PyMuPDF, Chrome / Chromium / Edge; tracker CLI only if attaching |
| qa-ux, branch-test-report | the app running locally, a browser tool (e.g. Playwright MCP) |
| seo-aeo, show-usage | Node 18+ |
| clean-architecture | PowerShell 7 (`pwsh`, cross-platform) or Windows PowerShell 5.1 for the layering guard |
| vps-deploy | GitHub Actions, an Ubuntu VPS with Docker, `gh`; `pwsh` or `bash` for secret migration |
| branch-reviewer | git; `gh` / `glab` / `az` only for comment mode |

### Hooks (`hooks/`)

| Hook | What it does | Install |
|------|--------------|---------|
| **secret-hygiene** | Block a commit that introduces a secret (gitleaks when present, conservative regex fallback). | git pre-commit hook, or a Claude Code PreToolUse hook — see `hooks/secret-hygiene/README.md` |
| **safe-commit** | commit-msg validator: no stray `@`, conventional subject, length; optional em-dash + AI-coauthor rejection. | git commit-msg hook — see `hooks/safe-commit/README.md` |

### Commands (`commands/`)

User-fired prompt shortcuts. None yet — coming as patterns stabilize.

> See [`INDEX.md`](INDEX.md) for the full catalog.

## 📦 Installation

This repo is a **Claude Code plugin marketplace**. The skills ship as one plugin,
`parvez`, so on any machine:

```
/plugin marketplace add parvez-ahammed/claude-skills
/plugin install parvez@parvez-tools
```

Every skill goes live, namespaced `parvez:<skill>` (e.g. `parvez:vps-deploy`,
`parvez:clean-architecture`, `parvez:honest-health-check`), triggering on its
`description`, or fire one directly with `/parvez:<skill>`. Update later with
`/plugin marketplace update parvez-tools`.

**Just one skill?** Copy its folder into `~/.claude/skills/` (personal) or a project's
`.claude/skills/`. Each skill is self-contained; the pattern spokes also read
`refactoring-guru/references/verdict-format.md` when it is installed next to them and
fall back to inline rules when it is not.

**Local authoring loop** (test before pushing): `/plugin marketplace add .` from a
clone, then install — relative paths resolve against the repo.

**Hooks** are **git** hooks (commit-msg / pre-commit), not Claude lifecycle hooks, so
install per-hook (a git hook, or a `settings.json` snippet). See each hook's `README.md`.

### Local sync (authoring)

`sync-skills.sh` / `sync-skills.ps1` copy every skill into `~/.claude/skills` on the
current machine (skipping `node_modules` and archives). Handy while editing a skill; the
plugin marketplace is the way to install elsewhere.

```bash
bash sync-skills.sh --dry-run           # macOS / Linux / Git Bash: preview
bash sync-skills.sh                     # sync all
```

```powershell
pwsh -File sync-skills.ps1 -WhatIf      # Windows: preview
pwsh -File sync-skills.ps1              # sync all
```

## 🗂️ Layout

```
claude-skills/
  README.md                  - this file
  LICENSE                    - MIT
  INDEX.md                   - catalog
  .claude-plugin/
    marketplace.json         - marketplace manifest (name: parvez-tools)
  plugins/
    parvez/                  - the installable plugin (namespace prefix)
      .claude-plugin/plugin.json
      skills/                - skills (one folder each)
  _template/                 - starting point for a new skill
  hooks/                     - git hooks (script + install notes per hook)
  commands/                  - user-fired commands
  sync-skills.sh / .ps1      - copy skills into ~/.claude/skills (authoring loop)
```

## ✍️ Writing a good skill

- **Pushy, specific `description`** — the only trigger signal; name the contexts and
  phrases it should fire on.
- Keep `SKILL.md` under ~500 lines; push detail into `references/` (loaded on demand).
- `description` <= 1,024 chars (portable); `description` + `when_to_use` <= 1,536 (Claude
  Code listing cap). Add `license`, and `compatibility` when the skill needs tools installed.
- Reference bundled files as `${CLAUDE_SKILL_DIR}/...` and run scripts through their
  interpreter (`node`, `python`, `bash`, `pwsh`).
- Bundle deterministic steps as `scripts/`.
- Explain the **why**; avoid rigid ALWAYS/NEVER walls.
- If it's really "enforce X every time," make it a **hook**, not a skill.

## 🤝 Contributing

Issues and PRs welcome. Start from [`_template/SKILL.md`](_template/SKILL.md). Each piece
should be self-contained, stack-neutral, documented, and (where it has a verifiable
output) shipped with a tested script. Before opening a PR:

```bash
claude plugin validate . && claude plugin validate plugins/parvez
```

## 📄 License

MIT © 2026 Parvez Ahammed — see [`LICENSE`](LICENSE).
