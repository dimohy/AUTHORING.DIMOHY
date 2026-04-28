# AUTHORING.DIMOHY.r29.md — Agent Customization Authoring Guide

| Item | Value |
|---|---|
| **Author** | DIMOHY |
| **Revision** | r29 |
| **Last updated** | 2026-04-28 |
| **License** | Apache License 2.0 — see the repository [`LICENSE`](../LICENSE) file |
| **Canonical repository** | <https://github.com/dimohy/AUTHORING.DIMOHY> |
| **Localized paths** | Korean: `ko/AUTHORING.DIMOHY.r{revision}.md` · English: `en/AUTHORING.DIMOHY.r{revision}.md` |
| **Filename convention** | `AUTHORING.DIMOHY.r{revision}.md`; the metadata revision and filename revision must match. |

## Canonical repository and localization sync

- The canonical source for this guide is <https://github.com/dimohy/AUTHORING.DIMOHY>.
- The repository stores localized editions under `ko/` and `en/`. Whenever authoring rules or behavior change, update both language files in the same revision.
- The root `README.md` is the repository’s default entry point and must be **English-first**. Korean guidance lives in `ko/README.md`; the root README must link to `ko/README.md` and the Korean source directory (`ko/`).
- `README.md` and `ko/README.md` are user-facing entry points. Keep them friendly and current: language directory links, copy/install steps, and tag information.
- The root `README.md` should include the English visual SVG overview (`assets/authoring-workflow.svg`) so readers can understand the workflow at a glance. Korean `ko/README.md` should use the Korean SVG (`assets/authoring-workflow-ko.svg`) so each README’s visual text matches its language.
- After updating the guide, sync the `main` branch and the revision tag (`r{revision}`) to the canonical repository whenever credentials allow it. If pushing fails, report the reason and the exact follow-up needed.
- If Korean and English editions drift, treat it as a release-blocking defect. Fix both before declaring the revision complete.

## What this document is

`AUTHORING.DIMOHY` is a meta authoring guide for configuring AI coding and authoring agents such as GitHub Copilot, Claude Code, Google Antigravity, Cursor, and similar tools. It does not run agents by itself. Instead, it defines how a project should generate and maintain the customization assets that make agents behave consistently:

- `AGENTS.md` — workspace-level operating constitution
- `SPECS.md` — project/domain-specific rules
- Rules / Instructions
- Prompts / Commands
- Chat modes
- Skills (`SKILL.md`)
- Hooks
- Subagents / custom agents
- MCP / tool integrations

This guide is **not loaded automatically** during normal coding or writing work. Agents should read it only when creating, updating, auditing, or realigning customization assets. During ordinary work, `AGENTS.md` is the lightweight authority.

## Quick start

### Common workflows

| Scenario | Trigger phrase | Result |
|---|---|---|
| New project bootstrap | `# Read AUTHORING.DIMOHY.md and create AGENTS.md for this project` | Creates `AGENTS.md`, optionally `SPECS.md`, and selected subagents. |
| Interactive bootstrap | `Run AUTHORING` / `Execute AUTHORING.DIMOHY.md` | Runs a four-step interview: purpose → structure → detailed specs → generate files. |
| Existing project realignment | `realign`, `clean up`, `make this match AUTHORING` | Audits existing customization files, proposes changes, applies approved updates. |
| Customization authoring | `create an instruction`, `add a prompt`, `organize skills` | Writes the requested asset using the relevant section of this guide. |
| Upstream AUTHORING improvement | `this AUTHORING improvement is useful upstream`, `create an AUTHORING improvement issue` | Lets the user choose local-only use, local AUTHORING update, upstream Issue creation, Issue draft preparation, or skip/later. |

### Tool-specific notes

#### GitHub Copilot in VS Code

- Keep the selected localized guide in the workspace root when bootstrapping a target project, or reference it explicitly from the canonical repository.
- `AGENTS.md` is the normal lightweight authority. Do not paste the full AUTHORING guide into always-loaded instructions.
- Prompts are usually stored in `.github/prompts/*.prompt.md`.
- Subagents are stored in `.github/agents/*.agent.md` when the environment supports them.
- Skills use the directory pattern `.github/skills/<name>/SKILL.md`.

#### Claude Code

- Keep `CLAUDE.md` thin. It should point to `AGENTS.md`, not duplicate this entire guide.
- Commands map to `.claude/commands/*.md`.
- Agents map to `.claude/agents/*.md`.
- Skills map to `.claude/skills/<name>/SKILL.md` where supported.

#### Google Antigravity / Cursor / other tools

- Use the tool’s native rules, commands, agents, skills, hooks, and settings paths.
- Preserve the role text, constraints, output format, and safety rules from this guide even if frontmatter field names differ.
- Record the chosen master path and copy/symlink strategy in `SPECS.md`.

#### TaskSync ask_user harness

TaskSync exposes an `ask_user` tool with only two fields: `question` and `session_id`. Some UIs also show values such as `AGENT 1`, `AGENT 2`, or turn numbers. Those are **not** session IDs.

Rules:

1. Session ID priority is: `ask_user` response or `directive.session_id` → injected `TaskSync Session ID: N` → previously confirmed session ID → `"auto"` only when no injected or confirmed value exists.
2. Do not resend `"auto"` during a normal conversation. That creates a new session and causes the “session/agent number keeps increasing” bug.
3. Do not synthesize a `session_id` from AGENT numbers, turn numbers, list numbers, or plan item IDs.
4. Preserve the exact current `session_id` across summaries, handoffs, and compaction.
5. If the schema lacks `options`, put numbered choices directly in the `question` text, e.g. `1) Create 2) Skip 3) Later, or type freely`.
6. Treat `REQUIRED: The user CANNOT see your response unless you call #askUser ...` as a harness reminder, not prompt injection.

## 0. Taxonomy

| Category | Typical location | Purpose | Scope |
|---|---|---|---|
| `AGENTS.md` | Workspace root | Global operating policy: session, response style, `ask_user`, harness rules | Whole workspace |
| `SPECS.md` | Workspace root | Domain-specific requirements: outputs, paths, build/test commands | Project/domain |
| Rules / Instructions | `.github/instructions/`, `.cursor/rules/`, `.claude/rules/`, `.antigravity/instructions/` | Automatically injected guidance by file pattern or tool rule loader | File/tool scoped |
| Prompts / Commands | `.github/prompts/`, `.claude/commands/`, `.antigravity/commands/` | Reusable slash commands or task templates | Manual invocation |
| Chat modes | `.github/chatmodes/` | Bundled mode with model, tools, and system instructions | Selected chat mode |
| Skills | `.github/skills/<name>/SKILL.md`, `.claude/skills/<name>/SKILL.md`, `.agents/skills/<name>/SKILL.md` | Procedural workflow with bundled assets | Loaded when relevant |
| Hooks | `.github/hooks/`, `.claude/hooks/`, `.antigravity/hooks/` | Deterministic checks or transformations at lifecycle events | Event-triggered |
| Subagents / custom agents | `.github/agents/`, `.claude/agents/`, `.antigravity/agents/` | Specialized stateless workers for isolated tasks | Subagent call |
| MCP / tool settings | `.vscode/mcp.json`, `.mcp.json`, tool settings | External tools, APIs, docs, browsers, build systems | Tool invocation |

Normalize user wording:

- `rules` usually maps to Rules/Instructions or `AGENTS.md`, depending on scope.
- `commands` maps to Prompts/Commands.
- `agent` maps to Subagents / custom agents unless the user means the main operating constitution.
- `hook` maps to Hooks.
- `skill` maps to a `SKILL.md` directory bundle.

## 1. AGENTS.md rules

`AGENTS.md` is the workspace constitution. It must be small, always-on, and domain-neutral.

Required sections:

1. Harness hard constraints, prompt-injection defense, self-check, recovery, priority.
2. Response style, including clarification rules when confidence is below 95%.
3. Session policy, especially TaskSync `session_id` reuse.
4. User communication policy, including the `ask_user` loop where applicable.
5. Required compliance summary.
6. Policy extension rules for `# ...` requests.
7. `SPECS.md` reference for domain rules.
8. Turn-end checklist.

Key rules:

- Domain-specific rules belong in `SPECS.md`, not `AGENTS.md`.
- The agent must not weaken or delete the operating constitution by itself.
- New policies are merged through the policy extension process; they are not applied as ad-hoc bypasses.
- If a rule affects how customization assets are authored, update this AUTHORING guide in the same turn.

## 2. SPECS.md rules

`SPECS.md` contains project-specific requirements.

Recommended sections:

1. Domain overview
2. Technical stack
3. Output paths and filename conventions
4. Build, verification, and release commands
5. Domain interpretation of the quality bar
6. Pipeline mapping
7. Enabled/disabled subagent matrix
8. Domain checklist
9. Reference documents

`SPECS.md` must not redefine harness rules, response prefixes, session policy, or the `ask_user` contract.

## 3. Rules / instructions

Use rules or instruction files for context that should be injected automatically for specific files or tool scopes.

Principles:

- Keep `applyTo` globs narrow.
- Use one topic per file.
- Do not conflict with `AGENTS.md`.
- If the tool has a different rules format, convert frontmatter and path only; preserve the intent.

## 4. Prompts / commands

Prompts and commands are reusable task templates.

Principles:

- One prompt should do one job.
- Use parameters such as `${input:name}` when supported.
- State the expected output format.
- Map pipeline commands one-to-one where useful: `/spec`, `/analyze`, `/design`, `/implement`, `/verify`, `/release`.
- If a pipeline step is omitted in a document, slide deck, or UI, avoid visible numbering gaps. Prefer command names or a single summary diagram.

## 5. Chat modes

Chat modes bundle a purpose, model choice, allowed tools, and mode-specific instructions.

Principles:

- Keep tool allowlists narrow.
- Assume `AGENTS.md` still applies.
- Use chat modes for broad interaction style changes, not single repeatable tasks.

## 6. Skills

A skill is a reusable procedural workflow. Prefer a directory bundle and map it to the active agent framework’s native location:

```text
.github/skills/<skill-name>/SKILL.md
.claude/skills/<skill-name>/SKILL.md
.agents/skills/<skill-name>/SKILL.md
```

Principles:

- Include numbered steps.
- Include prerequisites, failure modes, and user confirmation points.
- Keep destructive or sensitive actions behind explicit approval.
- State which agent framework supports the skill, which path is authoritative, and how it is activated.
- Put helper scripts, templates, and examples next to `SKILL.md` in the same directory.

Example categories:

- Code projects: release tags, hotfix workflow, migration run, benchmarks.
- Documentation/content: Marp authoring, link check, style lint, beginner-friendly writing.
- Research: reproducibility package, statistics verification, dataset manifest.
- Common: self-audit, bootstrap check.

## 7. Hooks and tool integrations

Hooks are deterministic lifecycle checks or transformations.

Principles:

- Keep hooks short and single-purpose.
- Record event names and framework support.
- Do not use hooks to bypass `AGENTS.md`.
- Use filename prefixes such as `00-` or `10-` if ordering matters.

MCP/tool integrations describe the capabilities the agent can call.

Principles:

- Do not store secrets in docs. Use `.env` placeholders and setup instructions.
- Map tools to pipeline steps.
- Keep actual build/test commands in `SPECS.md`.
- Verify external tool paths, command names, and API claims through primary sources when they may change.

## 8. Subagents / custom agents

Subagents are specialized, stateless workers.

Principles:

- Define one role per agent.
- Specify allowed and forbidden tools.
- State that the subagent returns one final report and does not keep state.
- Read-only agents must not write files or run commands.
- Research/audit agents that make claims about external tools must use primary sources and cite them.

Recommended catalog:

| Agent | Role | Pipeline position |
|---|---|---|
| Feasibility Investigator | Checks technical, integration, and compatibility feasibility | Analyze |
| Implementation Planner | Breaks a goal into executable steps, risks, and acceptance criteria | Design |
| User Simulator | Reviews UX, reader flow, and beginner experience | Spec / Verify |
| Code/Asset Explorer | Read-only exploration and summarization | Any |
| Auditor | PASS/FAIL/UNVERIFIABLE review against acceptance criteria | Verify / Release |

## 9. Creation workflow

When asked to create or update customization assets:

1. Read this guide first.
2. Classify the request by taxonomy.
3. Apply the relevant format and principles.
4. Check for conflicts with `AGENTS.md`.
5. Update related references and checklists.
6. Report results to the user.

### 9.1 Upstream AUTHORING improvement option

When an agent using this guide in another workspace discovers an improvement that appears reusable for AUTHORING.DIMOHY itself, it must **ask the user to choose** before modifying `AUTHORING.DIMOHY*.md` or preparing any GitHub request.

Most users will consume this guide by fetching or copying the latest localized file from the web into their own project repository. That is enough for local use. The upstream path for ordinary external users is an **Issue**. Agents should either create an approved upstream Issue when authenticated tooling is available or prepare an Issue draft for manual submission.

Offer at least these choices, using numbered text if the `ask_user` schema has no structured options:

1. **Use only / no upstream** — keep the improvement only in the current project’s `AGENTS.md`, `SPECS.md`, prompts, skills, or other local customization files.
2. **Update local AUTHORING copy** — modify the workspace’s copied `AUTHORING.DIMOHY*.md`, but do not prepare an upstream request.
3. **Create upstream Issue** — create an Issue in <https://github.com/dimohy/AUTHORING.DIMOHY> when authenticated GitHub tooling or browser automation is available.
4. **Prepare Issue draft** — generate an Issue title/body for manual submission when authentication or browser automation is unavailable.
5. **Skip / later** — record nothing unless the user asks again.

Rules:

- Apply only the scope selected by the user. Do not silently convert a local project improvement into an upstream AUTHORING change.
- For Issue creation, include usage context, problem/gap, proposed improvement, why it is generally useful, affected area, and optional suggested text or patch.
- Do not create an Issue automatically without explicit user approval. If authentication is unavailable, prepare the Issue draft for manual submission.
- Do not create, comment on, label, or close an Issue without explicit user approval and available credentials.
- In this canonical repository, maintainers may use `.github/prompts/triage-authoring-issues.prompt.md` to read registered Issues, judge whether they are generally useful, and apply accepted changes to AUTHORING.DIMOHY.

## 10. Initial loading rules

- `AGENTS.md` should only link to this guide and state when to read it.
- Do not auto-load the full AUTHORING guide in ordinary coding or writing turns.
- If a newer revision exists, the newest revision is authoritative.

## 11. Bootstrap

Single-shot bootstrap:

1. Copy the desired localized guide to the target workspace root, or reference it from the canonical repository.
2. Ask the agent to read the guide and create `AGENTS.md`.
3. If the project has domain rules, create a `SPECS.md` stub.
4. Offer optional subagents, prompts, skills, chat modes, hooks, and tool settings based on the project type.

Interactive bootstrap:

1. Ask the project purpose and audience.
2. Propose the structure: pipeline, agents, skills, prompts, output paths, quality interpretation.
3. Interview for `SPECS.md` details.
4. On confirmation, generate all approved files in one turn.

Do not overwrite existing files without explicit approval.

## 12. Realignment

For existing projects, audit these assets:

- `AGENTS.md`
- `SPECS.md`
- Rules / instructions
- Prompts / commands
- Chat modes
- Skills
- Hooks
- Subagents
- MCP / tool settings

Classify each item as:

- `keep` — already valid
- `update` — valid but needs format/reference fixes
- `remove` — obsolete or duplicate; delete only after approval
- `missing` — recommended but absent

Then present a change proposal, apply approved updates, re-audit, and summarize results.

## 13. Pipeline mapping

Default six-stage pipeline:

| # | Stage | Command | Main agent | Main output |
|---|---|---|---|---|
| 1 | Spec / requirements | `/spec` | Main agent, optional User Simulator | `.planning/PRD.md` |
| 2 | Analyze / feasibility | `/analyze` or `/feasibility` | Feasibility Investigator | `.planning/FEASIBILITY_REPORT.md` |
| 3 | Design / plan | `/design` or `/plan` | Implementation Planner | `.planning/IMPLEMENTATION_PLAN.md` |
| 4 | Implement | `/implement` | Main agent | Code, docs, tests, content |
| 5 | Verify | `/verify` | Auditor, optional User Simulator | `.planning/AUDIT_REPORT.md` |
| 6 | Release / publish | `/release` | Auditor + main agent | Release notes, tag, artifacts |

`SPECS.md` specializes this table for each project.

## 14. Quality bar

A stage or release is complete only when all applicable gates pass:

1. Checklist is 100% complete.
2. Unit tests exist and pass where testable.
3. Manual verification is recorded for non-automatable items.
4. Project build/test commands are green.
5. Documentation references, paths, and names are consistent.
6. Visual/page-boundary verification is done for UI, PDF, slides, ebooks, or print outputs.
7. Traceability exists through commit, tag, and change summary.

If any gate fails, the stage is not complete.

## 15. Trigger phrases

| User phrase | Flow |
|---|---|
| `Read AUTHORING.DIMOHY.md and create AGENTS.md` | New bootstrap |
| `Run AUTHORING` | Interactive bootstrap |
| `realign`, `clean up`, `audit customization files` | Existing project realignment |
| `# ...` | Policy extension and routing to `AGENTS.md` or `SPECS.md` |
| `propose this AUTHORING improvement upstream`, `create an AUTHORING improvement issue` | Upstream AUTHORING improvement option |
| `AUTHORING audit`, `is this file enough?` | Self-audit |
| Revision bump | Rename metadata/files, update both languages, README, tags |

## 16. Generalization and specialization

The framework is domain-neutral. Code, documentation, research, education, slide decks, and mixed projects should use the same operating model.

General rules stay in this guide and `AGENTS.md`. Domain rules go to `SPECS.md`.

Examples of domain specialization:

| Axis | Code | Documentation/content | Research |
|---|---|---|---|
| Outputs | `src/`, `tests/` | `docs/`, `slides/` | `papers/`, `datasets/`, `notebooks/` |
| Verification | build/test | lint, link check, readability | reproducibility, statistics, data pipeline |
| Manual review | UI screenshots, performance | reader flow, references | reviewer perspective, figures |

## 17. Self-audit protocol

Self-audit checks this guide itself.

Criteria:

1. The `AGENTS.md` template is self-contained.
2. Supported tools have installation notes and warnings.
3. Taxonomy covers Rules/Instructions, Prompts/Commands, Chat Modes, Skills, Hooks, Agents, and MCP/Tools.
4. The six-stage pipeline is mapped.
5. The Auditor is integrated into verify/release.
6. The seven quality gates are preserved.
7. Generalization beyond code is explicit.
8. Section references are valid.
9. Trigger phrases match the actual flows.
10. Typos, broken links, and external factual claims are checked.

External facts about tools, API signatures, paths, command names, and versions should be verified from primary sources and recorded with URL and access date.

## 18. Revisions and filenames

- Use simple monotonically increasing revisions: `r1`, `r2`, `r3`, ... . The `r` prefix is part of the identifier.
- Store localized files under `ko/` and `en/` with matching revision numbers.
- Keep the current revision number out of README and body references whenever possible. The current revision should normally appear only in the localized AUTHORING file title, top metadata table, revisioned filename, and revision history entries. Elsewhere, use language directory links (`en/`, `ko/`) or generic selectors such as `AUTHORING.DIMOHY.r*.md`.
- On every revision bump:
  1. Update both localized document titles and top metadata tables.
  2. Rename both localized files.
  3. Update README or internal references only when their structure or wording changes; do not change them just to replace the current revision number.
  4. Replace exact revision references only when they intentionally point to the latest file instead of a historical revision.
  5. Update the bilingual revision history documents with matching structure and user-visible changes.
  6. Commit and tag `r{revision}`.
  7. Push `main` and the tag to <https://github.com/dimohy/AUTHORING.DIMOHY> when credentials allow it.
- Keep only the latest revision in each language directory unless the user explicitly asks to archive old revisions.
- If symlinks are useful in a target project, `AUTHORING.DIMOHY.md` may point to the selected language/revision file.

## License

Copyright 2026 DIMOHY

Licensed under the Apache License, Version 2.0. See the repository [`LICENSE`](../LICENSE) file for the full license text.
