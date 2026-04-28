---
mode: "agent"
description: "Use when: triaging GitHub Issues for AUTHORING.DIMOHY improvements and applying accepted Issue-only changes. Keywords: issue triage, registered issues, AUTHORING improvement, 이슈 검토, 이슈 반영."
---

# Triage AUTHORING.DIMOHY Issues

Triage GitHub Issues for the `AUTHORING.DIMOHY` repository and, with approval, apply useful improvements to the guide.

## Input

- Issue scope: `${input:issueScope}`
  - Accept an Issue URL, Issue number, label query, or a short description such as `open authoring-improvement issues`.

## Workflow

1. Confirm the Issue scope. If it is missing or ambiguous, ask which Issue URL/number/label set to review.
2. Read `AGENTS.md` and the latest localized AUTHORING files before applying changes.
3. Gather Issue content.
   - If an Issue URL is provided, fetch the page and inspect its content and relevant comments.
   - If using GitHub CLI/API/browser automation, require available authentication.
   - If Issues cannot be read automatically, ask the user to provide the Issue text or URL.
4. Classify each Issue:
   - `accept` — generally useful and ready to apply.
   - `needs-info` — likely useful but missing details.
   - `local-only` — useful only for the reporter's project.
   - `duplicate` — already covered.
   - `reject` — conflicts with goals, safety, license, or maintainability.
5. Evaluate accepted candidates against these criteria:
   - Generalizes beyond a single project.
   - Improves AUTHORING workflows, safety, clarity, contribution UX, or maintenance cost.
   - Does not weaken harness/session/user-approval rules.
   - Can be synchronized between English and Korean without semantic drift.
   - Has enough detail to implement and verify.
6. Report a triage table with Issue, classification, rationale, affected sections/files, risk, and proposed action.
7. Ask for approval before editing unless the user explicitly requested applying accepted Issues.
8. Apply approved changes.
   - Keep English and Korean guide semantics synchronized.
   - Update README files only when user-facing behavior changes.
   - Keep exact current revision numbers limited to localized AUTHORING titles, top metadata, revisioned filenames, and revision history entries.
9. Validate.
   - Search for stale exact revision references.
   - Check Markdown diagnostics.
   - Run `git diff --check` when available.
10. Report changed files, validation results, and any Issue follow-up.

## GitHub mutation policy

Do not comment on, label, close, or otherwise mutate Issues without explicit approval and available authentication. If authentication is unavailable, prepare the comment/label/close recommendation as text for manual use.
