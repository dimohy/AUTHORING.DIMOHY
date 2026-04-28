# AGENTS.md

This file defines repository-local behavior for the `AUTHORING.DIMOHY` project only. It does not define rules for projects that merely copy or reference AUTHORING.DIMOHY.

Active system, harness, and tool instructions remain higher priority. Do not use this file to bypass `ask_user`, authentication, approval, or safety requirements.

## Repository purpose

`AUTHORING.DIMOHY` is the canonical repository for the AUTHORING.DIMOHY meta authoring guide. Keep Korean and English guide editions synchronized, keep user-facing revision references generic, and preserve the repository's Apache-2.0 licensing.

## Issue-only improvement loop

This repository supports an Issue-only improvement workflow for external users.

### External user submission

When a user of AUTHORING.DIMOHY finds a reusable improvement:

1. Prefer creating an Issue in `dimohy/AUTHORING.DIMOHY` using the AUTHORING improvement issue template.
2. If the user's agent has authenticated GitHub tooling, it may create the Issue after explicit user approval.
3. If authenticated Issue creation is unavailable, the agent should prepare the Issue title/body for manual submission.
4. Use Issues as the only upstream submission path for ordinary improvement proposals.

### Maintainer/agent triage in this repository

When asked to review registered GitHub Issues for AUTHORING.DIMOHY improvements:

1. Read the requested Issue URL/number/list, including relevant comments when available.
2. If a GitHub URL is provided, fetch and inspect it before making claims about its content.
3. Classify each Issue as one of:
   - `accept` — generally useful and ready to apply.
   - `needs-info` — likely useful but missing details.
   - `local-only` — useful only for the reporter's project, not AUTHORING.DIMOHY.
   - `duplicate` — already covered by an existing rule or Issue.
   - `reject` — conflicts with project goals, safety, license, or maintainability.
4. Judge usefulness by these criteria:
   - Generalizes beyond a single project or tool configuration.
   - Improves AUTHORING workflows, safety, clarity, contribution UX, or maintenance cost.
   - Does not weaken harness/session/user-approval rules.
   - Can be documented in both English and Korean without semantic drift.
   - Has enough context to implement and verify.
5. Present a triage table and proposed changes before editing files unless the user explicitly asks to apply accepted Issues immediately.
6. For accepted Issues, update the relevant AUTHORING documents and README files as needed.
7. Keep exact current revision numbers limited to localized AUTHORING titles, top metadata, revisioned filenames, and revision history entries; use generic selectors elsewhere.
8. Run validation after changes: search for stale exact revision references, check Markdown problems, and run `git diff --check` when available.
9. Do not comment on, close, label, or otherwise mutate GitHub Issues without explicit user approval and available authentication.

## Recommended prompt

Use these repository prompts for explicit maintenance tasks:

- `.github/prompts/triage-authoring-issues.prompt.md` — review registered GitHub Issues and apply accepted improvements.
- `.github/prompts/audit-authoring-consistency.prompt.md` — audit AUTHORING/README/customization consistency before release or after policy changes.
