---
mode: "agent"
description: "Use when: auditing AUTHORING.DIMOHY consistency after policy changes, before release, or when checking contradictions. Keywords: AUTHORING audit, consistency, 정합성, 모순 검사, 릴리스 검수."
---

# Audit AUTHORING.DIMOHY Consistency

Audit this repository for internal consistency across AUTHORING guides, README files, AGENTS.md, prompts, and Issue templates.

## Scope

Check at least:

- `en/AUTHORING.DIMOHY.r*.md`
- `ko/AUTHORING.DIMOHY.r*.md`
- `README.md`
- `ko/README.md`
- `AGENTS.md`
- `.github/prompts/*.prompt.md`
- `.github/ISSUE_TEMPLATE/*`

## Required checks

1. **Revision references**
   - Exact current revision numbers should appear only in localized AUTHORING titles, top metadata tables, revisioned filenames, and revision history entries.
   - README/body references should use language directories or generic selectors such as `AUTHORING.DIMOHY.r*.md`.

2. **Issue-only upstream contribution flow**
   - External users who copy AUTHORING into their own projects should be guided to Issue creation or Issue draft preparation as the only upstream path.
   - No alternate upstream submission path should be presented for ordinary external suggestions.
   - No text should imply that another repository can directly update `dimohy/AUTHORING.DIMOHY`.

3. **Repository-local Issue triage loop**
   - `AGENTS.md` should describe repository-local Issue triage behavior only for this project.
   - `.github/prompts/triage-authoring-issues.prompt.md` should exist and classify Issues as `accept`, `needs-info`, `local-only`, `duplicate`, or `reject`.
   - The Issue template should collect usage context, problem/gap, proposed improvement, general value, affected area, and optional suggested text/patch.

4. **English/Korean synchronization**
   - English and Korean AUTHORING documents should describe the same behavior.
   - Root README and Korean README should preserve matching structure for user-facing workflows.

5. **Safety and approval**
   - Issue creation and Issue mutation must require explicit user approval and available authentication.
   - Agent behavior must not weaken harness/session/user-approval rules.

6. **Markdown/customization validity**
   - Prompt frontmatter must be valid YAML with a useful `description`.
   - Issue template YAML must be valid.
   - Links and file references should point to existing files or stable directories.

## Output

Return:

1. PASS/FAIL summary.
2. Findings table with file, section/line, severity, issue, and fix.
3. If fixes are needed, propose a minimal patch plan and ask for approval before editing unless the user explicitly requested auto-fix.
4. After fixing, rerun the relevant checks and report validation results.
