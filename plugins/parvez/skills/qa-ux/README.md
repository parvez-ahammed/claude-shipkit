# qa-ux

Pressure-test a **running** web app and, by default, produce **two cross-linked HTML reports**:

- **`qa-report.html`** — Staff-QA release report (14 sections) with a 🟢/🟡/🔴 release recommendation,
  backed by Playwright screenshots + API / data / file-probe evidence.
- **`ux-report.html`** — Product Experience audit from six personas across six journeys (A–F), with a
  Product Experience Score /100, ranked problems, and prioritized redesigns.

Both passes run against the same app and share one `screenshots/` folder. Scope to a single report
only if the user explicitly asks ("just QA" / "only UX").

## Use
Invoke `qa-ux`, or ask for a "QA / release report" or a "UX / product experience review" of a
running app. The skill drives the browser for screenshot proof (including mobile via viewport resize),
verifies correctness out-of-band (API, data, file probes), and never invents data (UNKNOWN when not verified).

## Layout
```
qa-ux/
  SKILL.md                         entry point, mode router, universal rules
  VERSION                          skill version
  README.md                        this file
  references/qa-mode.md            14-section QA release report spec
  references/ux-mode.md            six-persona / six-journey UX audit spec + output
  templates/qa-report-template.html
  templates/ux-report-template.html
```

## Output
Two self-contained HTML reports, `qa-report.html` and `ux-report.html` (or one, when scoped), in
`.qa-reports/` by default. Dark, Linear/Notion/Stripe style: sticky nav, severity colors, collapsible
sections, badges, callouts, execution timeline, CSS charts. They link to each other and share one
`screenshots/` folder next to them. Both open in the browser when done.

## Requirements
The app must be running. A browser-automation tool that can take screenshots and resize the viewport
(Playwright MCP, Claude in Chrome or similar). `ffprobe` is optional, for media checks. Test against a
dev or staging environment; the skill asks before actions with real-world effects.

## Install
Ships in this plugin. To use standalone, copy this folder to `~/.claude/skills/qa-ux/` (personal) or
`<repo>/.claude/skills/qa-ux/` (project).
