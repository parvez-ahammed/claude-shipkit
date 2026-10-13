---
name: groom-issue
description: >-
  Scores a tracker issue (or every issue in a milestone) for build readiness and, after the user
  confirms, rewrites it into one current, self-contained implementation plan. Rates each issue
  against a readiness rubric, finds gaps and undeclared blockers, rates the security surface, checks
  stale file paths against the live repo, and reconciles review notes that contradict each other.
  Uses the project's own gold-standard issue template when the repo has one. Use when grooming a
  backlog, before an agent or developer picks an issue up, after editing an issue, or when an old
  issue has piled up conflicting comments. Triggers on "/groom-issue", "groom #42", "is this issue
  ready", "score this ticket", "re-groom the milestone", "validate this issue", "clean up this issue".
when_to_use: >-
  Not for summarizing a set of groomed issues for a stakeholder (use brief-milestone) or for taking
  an issue all the way to merged code (use deliver-issue, which calls this skill first).
argument-hint: "[issue number | URL | milestone name] [apply | dry-run]"
license: MIT
compatibility: "Reads and writes issues through the tracker's CLI or API: gh (GitHub), glab (GitLab), az boards (Azure DevOps) or a Jira CLI/API. Without one, works from pasted issue text and writes markdown."
---

# groom-issue - make an issue buildable from its body alone

An issue is a plan that someone (often an agent) will pick up cold. It is **ready** when that person
can build it end to end from the body alone. Anything they would have to ask, guess or discover
mid-build is a defect, and an **undeclared blocker** is the most expensive one.

**Core principle:** score first, write second, and write only after the user says yes. A surfaced
open question beats a fabricated answer.

## When to use / not use

- Use when: grooming one issue or a milestone; checking an issue before work starts; re-scoring after
  an edit; re-grooming an old issue buried under layered reviews.
- Not for: a stakeholder overview of many issues (`brief-milestone`); building the issue
  (`deliver-issue`, or `build-story` / `fix-bug` directly).

## Before you start

1. **Read the project's `CLAUDE.md`, `CONTRIBUTING`, and any rules or architecture doc.** Project rules
   override this file.
2. **Find the tracker.** `git remote -v` tells you the host. Map the commands:

   | Tracker | Read | Write body (only after a yes) |
   |---|---|---|
   | GitHub | `gh issue view <n> --json number,title,body,labels,milestone,state` | `gh issue edit <n> --body-file <tmp>` |
   | GitLab | `glab issue view <n>` | `glab issue update <n> --description "$(cat <tmp>)"` |
   | Azure DevOps | `az boards work-item show --id <n>` | `az boards work-item update --id <n> --description "<html>"` |
   | Jira | the Jira CLI or REST `GET /rest/api/3/issue/<key>` | REST `PUT` on the same resource |
   | none / offline | pasted text | write `groomed/issue-<n>.md` locally |

3. **Find the gold standard.** Look for a project template, in this order: a path the user names;
   `.github/ISSUE_TEMPLATE/*` (feature or implementation template), `.gitlab/issue_templates/*`,
   `docs/**/issue*template*`, `docs/**/*definition-of-ready*`; a reference issue that `CLAUDE.md` or
   `CONTRIBUTING` names as the model. If one exists, its section list and required fields are the
   target shape, and the rubric is mapped onto its sections. If none exists, use the built-in shape in
   `${CLAUDE_SKILL_DIR}/references/living-document.md`. Say which one you used in the output.
4. **Find the gate commands.** Read CI config (`.github/workflows/*`, `.gitlab-ci.yml`,
   `azure-pipelines.yml`), `package.json` scripts, `Makefile`, `pyproject.toml`, `*.csproj`, `go.mod`.
   The Verification block in a groomed issue quotes these literally, never paraphrased.

## Modes

| Mode | Trigger | Writes |
|---|---|---|
| **score** (default) | "groom #42", "is it ready" | nothing; prints the record |
| **apply** | "groom and fix #42", "apply" | shows the new body (or a diff), waits for an explicit yes, then writes |
| **dry-run** | "groom to files", "dry-run" | local markdown only, `groomed/issue-<n>.md` (ask before creating the folder) |

Labels (`needs-decision`, `ready`), comments and body edits are all outward actions: show exactly
what will be sent and wait for a yes. A yes for one issue is not a yes for the next one, unless the
user says "apply to all" for a named set.

## Process

1. **Read** the issue in full, including comments when reviews were pasted there.
2. **Classify:** feature, epic, bug, spike/test-debt, or meta/tracking. Meta issues get `N/A - meta`
   and are not edited. Rules per type are in `${CLAUDE_SKILL_DIR}/references/rubric.md`.
3. **Check freshness.** Every file path, symbol, version and "X was removed" claim is checked against
   the live repo with search and read. A path that no longer exists is a cited defect.
4. **Reconcile** layered or contradicting reviews before scoring (precedence rules in
   `${CLAUDE_SKILL_DIR}/references/living-document.md`). You score the reconciled single version,
   not the pile.
5. **Score** each applicable rubric dimension 0-2 with a one-line reason
   (`${CLAUDE_SKILL_DIR}/references/rubric.md`). Rate the security surface HIGH or LOW.
6. **Hunt undeclared blockers:** blocked-by issues that are missing, closed when the issue says open
   (or the reverse), sequencing that contradicts the milestone, a required account, key, migration or
   upstream change the issue never mentions.
7. **External paid dependency?** If the issue relies on a third-party paid API or sets a price the
   user pays, run the web check in `${CLAUDE_SKILL_DIR}/references/external-dependency-check.md`.
8. **Verdict** READY / NEEDS WORK / NOT READY with the score.
9. **Apply mode only:** draft the rewritten body in the target shape, keep everything already correct,
   move superseded lines to the Decision Log, turn every gap you cannot fill from the repo, docs or
   sibling issues into an `OPEN QUESTION` with a `Default if unanswered:` line. Re-score the draft.
   Show it. Write only after a yes, through a temp file, never a huge inline argument. Then re-read
   the issue from the tracker to confirm the write landed.

## Output

Per issue:

```
#<n> <title>
Type: feature | epic | bug | spike | meta
Shape: project template <path> | built-in living document
Score: <total>/<applicable max>  Verdict: READY | NEEDS WORK | NOT READY
Security surface: HIGH | LOW  (<one-line reason>)
Weak dims: <dim# + one-line gap, or "none">
Undeclared blockers: <list, or "none">
Freshness: verified <YYYY-MM-DD> | stale claims: <list, or "none">
Reconciled: <conflicts resolved, or "none">
External dependency: NONE | <provider> - <ready | not ready: why>
Action: scored only | proposed body shown, awaiting yes | written (sections: ...) | dry-run file <path> | N/A meta
```

For a milestone, end with a table: issue, type, score, verdict, top gap, action. Then a suggested
build order if dependencies make one obvious (or point at `brief-milestone`).

## Common mistakes

| Mistake | Fix |
|---|---|
| Writing to the tracker before the user says yes | Show the body or diff first. Every time. |
| Filling a gap with a plausible guess | `OPEN QUESTION` + `Default if unanswered:`. |
| Scoring the pile of reviews instead of the reconciled version | Reconcile first, then score. |
| Trusting the issue's file paths | Check each one against the repo. A stale anchor misleads the builder. |
| Treating a declared blocker as "not ready" | Declared blockers are fine. Undeclared ones are the defect. |
| Force-fitting the rubric to a meta or tracking issue | Mark `N/A - meta`, do not edit. |
| "Already built" taken as done | Existing code is a baseline; the issue still needs review and test criteria for it. |
| Pricing a vendor from memory | Live page, URL, access date. Prices move. |
| Silently dropping the losing side of a contradiction | Move it to the Decision Log with one line of why. |
| Inventing scope to reach READY | The score reflects the issue. Gaps become questions, not features. |
