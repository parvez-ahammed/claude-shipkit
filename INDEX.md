# Skill index

Skills currently in this library (`plugins/shipkit/skills/`). Install via the plugin:
`/plugin marketplace add parvez-ahammed/shipkit` then `/plugin install shipkit@shipkit`.

| Skill | What it does | Reuse |
|-------|--------------|-------|
| **deploy-to-vps** | Vendor-neutral push-to-deploy CI/CD to a VPS (any runtime), Caddy auto-TLS, OAuth/OIDC redirect setup, DNS (proxied-by-default), and baseline hardening (fail2ban, SSH-port rotation, firewall, CDN origin lockdown). Encodes the gotchas: env_file vs `$`-substitution, dishonest liveness `/health`, no-curl slim images, host-allowlist 400, ACME staging fallback, proxied-vs-grey-cloud, exact `redirect_uri` matching. | generic |
| **add-health-check** | Health endpoints that assert real readiness (DB/deps reachable) not a hardcoded 200; liveness vs readiness; wiring into deploy gates + container/orchestrator probes. Node/Python/Go/.NET snippets. | generic |
| **enforce-clean-architecture** | Rulebook + bootstrap + config-driven layering guard for Clean / Hexagonal architecture (any language). Dependency Rule, ports & adapters, per-stack scaffolding, tested guard script. | generic |
| **refactor** | Orchestrator for refactoring + design-pattern work: understands intent, routes to the spokes, merges findings. Patterns recommended only when a site meets scored preconditions with cited evidence. | generic |
| **find-code-smells** | Detects the 22 refactoring.guru smells (five groups) in a file/function/dir, each with `file:line` evidence and the candidate refactoring/pattern it maps to. Spoke of refactor; standalone too. | generic |
| **apply-refactoring** | Fowler's refactoring catalog (composing methods, moving features, organizing data, simplifying conditionals/calls) with preconditions + ordered mechanics + a safety rule. Recommends mechanics; never auto-edits. Spoke. | generic |
| **pick-creational-pattern** | Scores whether a creational pattern (Factory Method, Abstract Factory, Builder, Prototype, Singleton) fits a site: opportunity signatures, weighted preconditions, threshold, hard-veto anti-indicators -> APPLY/CONSIDER/REJECT. Spoke. | generic |
| **pick-structural-pattern** | Same scoring for structural patterns (Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy). Spoke. | generic |
| **pick-behavioral-pattern** | Same scoring for behavioral patterns (Strategy, State, Command, Observer, Chain of Responsibility, Template Method, Visitor, Mediator, Iterator, Memento). Spoke. | generic |
| **find-shared-code** | Across a milestone/backlog of issues, finds the shared engines/primitives/UI worth extracting with DRY/KISS/YAGNI judgment: cross-phase ownership table, per-candidate verdict (build-now/narrow/resist), a mandatory "what to resist" list, and a dependency-ordered build order. | generic |
| **grill-feature** | Stands up a "grill room" of independent, code-grounded interrogators that test features at the PROMISE level and return BULLETPROOF/CRACKED/FAKE with `file:line` evidence, plus an optional CEO unit-economics pass. One feature inline, or a whole release as a background workflow sweep. | generic |
| **show-usage** | WakaTime-style active time + token/cost/tool tables computed locally from Claude Code session transcripts. Read-only, zero-dep `node` script; per-project or all-projects. | generic |
| **audit-app** | Pressure-test a running web app and emit TWO cross-linked HTML reports by default: a Staff-QA release report (14 sections, 🟢/🟡/🔴 recommendation, Playwright + API/data/file evidence) and a UX product-experience audit (6 personas x 6 journeys, Product Experience Score /100, ranked redesigns). Drives the browser for screenshot proof incl. mobile; never invents data (UNKNOWN when unverified). Scope to one report only if asked. | generic |
| **make-video** | Animated promo / explainer / social videos built entirely from code: HTML scenes, local TTS voice-over, generated music, captions and thumbnail, rendered to MP4. A claims check gates every on-screen statement. | generic |
| **make-demo** | Pixel-exact clickable HTML demos of a planned workflow, captured from the real running app (DOM + CSS snapshot) with an optional Windows desktop (WinForms) adapter, so the product owner sees exactly how it will look. Stays close to today's app. | generic |
| **write-user-guide** | User-facing feature guide for a ticket or PR as self-contained HTML + PDF. Learns from the code, then a fresh agent checks every claim. Optional attach to Azure DevOps, Jira or GitHub. | generic |
| **review-branch** | Multi-seat review of a branch / PR (backend, frontend, architecture, tests, QA, text, plus a licence seat for new dependencies), a refutation pass, a ranked punch list and an HTML report. Advanced mode adds an adversarial round; comment mode re-verifies each claim before posting to GitHub, Azure DevOps or GitLab. Reads your project rules file. | generic |
| **test-branch** | Runs a branch against the live app (not a reading of the diff): seeds data, predicts results with queries, drives the browser, and writes an HTML report with screenshots for a product owner (base vs branch, open items only). | generic |
| **fix-bug** | One bug end-to-end: reproduce, root cause before fix, a regression test that fails without the fix, then verify. Hard-won debugging traps included. | generic |
| **build-story** | One user story end-to-end: analyze, break down, pick tasks, implement, test, verify, with a pre-PR gate (criteria trace, backward compatibility, every client that calls a changed endpoint). | generic |
| **write-plainly** | House style for prose that lands in git, PRs and docs: ASCII punctuation, Simplified Technical English, no jargon, short commit messages, why-only code comments, plus a PR-description sub-skill that fills any repo's template. | generic |
| **boost-seo** | Audit and improve a site's SEO and AEO (AI answer engines): meta, JSON-LD, robots, sitemap, llms.txt, prerendering for SPAs, then prove it with local test scripts. | generic |
| **fill-timesheet** | Reconstruct what you worked on for past dates (tickets, descriptions, time blocks) from local Claude Code transcripts, ready for a timesheet. Configurable ticket patterns. | generic |
| **groom-issue** | Scores a ticket's readiness against a rubric, finds gaps and undeclared blockers, and rewrites it into a buildable plan only after you confirm. | generic |
| **brief-milestone** | Executive brief of a milestone or set of issues: issue by issue in build order, then the whole picture with risks and open questions. | generic |
| **deliver-issue** | Delivers one issue or a whole milestone end to end: groom, plan, build, review, test, ship. Pushing, PRs and merges wait for your yes. | generic |
| **price-product** | Pricing tiers, margin-safety per tier, competitor prices with cited sources, value metric, and a check of every price surface for contradictions. | generic |
| **simplify-code** | Removes over-engineering in one behavior-preserving change set: dead code, needless layers, reinvented helpers. Verified by your own build and tests. | generic |
| **trim-comments** | Deletes comments that restate the code or narrate history and keeps the ones that explain a non-obvious why. Comment-only, gate-verified. | generic |
| **find-ux-gaps** | Fan out parallel read-only UX auditors over an app, then turn what survives into tickets. Partitions the surface into disjoint territories first (unpartitioned agents all return the same loudest two findings), enforces a fixed issue contract with `file:line` evidence, then dedups, drops the unevidenced, writes a markdown file, and files to GitHub, GitLab, Jira or Azure DevOps only after explicit confirmation. Configurable agents (default 3) and issues each (default 3). Works when the app cannot be run. Complements `audit-app`, which needs a running app and emits HTML reports instead of tickets. | generic |

## Hooks (`hooks/`)

| Hook | What it does |
|------|--------------|
| **secret-hygiene** | Pre-commit block on secrets (gitleaks + regex fallback). git pre-commit hook or Claude Code PreToolUse hook. |
| **safe-commit** | commit-msg validator: no stray `@`, conventional subject, length; optional em-dash + AI-coauthor rejection. Skips git-generated merge / revert / autosquash subjects. |

Both hooks install with `bash install.sh <repo>` or `pwsh -File install.ps1 -Repo <repo>`.
