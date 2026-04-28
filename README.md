# AUTHORING.DIMOHY

AUTHORING.DIMOHY is a meta authoring guide for building consistent AI-agent customization workflows for any project. It helps agents create, update, and audit `AGENTS.md`, `SPECS.md`, rules/instructions, prompts(commands), chat modes, skills, hooks, subagents, and MCP/tool integrations.

Canonical repository: <https://github.com/dimohy/AUTHORING.DIMOHY>

> 한국어 안내와 원문은 [`ko/AUTHORING.DIMOHY.r24.md`](ko/AUTHORING.DIMOHY.r24.md)에서 볼 수 있습니다.

## Latest revision

| Language | File |
|---|---|
| English | [`en/AUTHORING.DIMOHY.r24.md`](en/AUTHORING.DIMOHY.r24.md) |
| Korean | [`ko/AUTHORING.DIMOHY.r24.md`](ko/AUTHORING.DIMOHY.r24.md) |

Current revision: **r24**  
Last updated: **2026-04-28**

## When should agents read this guide?

Do **not** keep the full AUTHORING guide loaded during ordinary coding or writing sessions. Read it only when you are doing customization work, such as:

- bootstrapping `AGENTS.md`, `SPECS.md`, or `.github/` customization files for a new project;
- realigning existing rules, prompts(commands), skills, agents, hooks, and MCP/tool settings;
- auditing customization file structure, category coverage, or workflow consistency;
- updating AUTHORING.DIMOHY itself or bumping its revision.

During normal project work, the lightweight `AGENTS.md` in that project is the primary operating constitution.

## Quick start

### 1. Install into a new project

1. Copy the latest localized guide into the target project root, or reference it explicitly from this repository.
   - English: `en/AUTHORING.DIMOHY.r24.md`
   - Korean: `ko/AUTHORING.DIMOHY.r24.md`
2. Ask your AI agent to bootstrap the project.
   - English: `Read AUTHORING.DIMOHY.md and create AGENTS.md for this project`
   - Korean: `# AUTHORING.DIMOHY.md를 읽고 이 프로젝트의 AGENTS.md를 생성해줘`
3. The agent should create `AGENTS.md`, optionally create `SPECS.md`, and offer project-appropriate agents, prompts, skills, hooks, chat modes, and tool settings.

### 2. Run interactive bootstrap

Use this when you want the agent to interview you before generating files:

- `Run AUTHORING`
- `AUTHORING 실행해줘`

Interactive bootstrap runs four steps:

1. clarify project purpose, audience, and outputs;
2. propose the recommended structure;
3. interview for `SPECS.md` details;
4. generate approved files after confirmation.

### 3. Realign an existing project

Use this when a project already has `AGENTS.md`, `.github/`, `.claude/`, `.cursor/`, or similar customization files:

- `realign this project with AUTHORING`
- `정비해줘`
- `AUTHORING 기준으로 맞춰줘`

The agent should classify each asset as `keep / update / remove / missing`, present a change proposal, apply only approved changes, and re-audit afterward.

## Managed categories

| Category | Example paths | Purpose |
|---|---|---|
| Operating constitution | `AGENTS.md` | Session, response style, `ask_user`, harness rules |
| Domain specs | `SPECS.md` | Project-specific outputs, paths, commands, acceptance criteria |
| Rules / Instructions | `.github/instructions/`, `.cursor/rules/`, `.claude/rules/` | Automatically injected context rules |
| Prompts / Commands | `.github/prompts/`, `.claude/commands/` | Reusable slash commands or task templates |
| Chat Modes | `.github/chatmodes/` | Special-purpose chat modes |
| Skills | `.github/skills/<name>/SKILL.md` | Procedural workflows with bundled assets |
| Hooks | `.github/hooks/`, `.claude/hooks/` | Lifecycle checks or transformations |
| Agents | `.github/agents/`, `.claude/agents/` | Specialized subagents |
| MCP / Tools | `.vscode/mcp.json`, `.mcp.json` | External docs, browser, APIs, build/test tools |

## TaskSync session-number fix

Revision r24 includes explicit TaskSync safeguards:

- reuse the injected or returned `session_id`;
- do not resend `session_id: "auto"` during a normal conversation;
- do not confuse UI labels such as `AGENT 1` or `AGENT 2` with `session_id`;
- if the `ask_user` schema has no `options` field, write numbered choices in the question body;
- preserve the exact `session_id` during summaries, handoffs, and compaction.

## Revision and localization policy

AUTHORING.DIMOHY uses simple revision identifiers: `r1`, `r2`, `r3`, ... . The `r` prefix is part of the identifier.

Whenever a revision changes, update all of the following together:

- `en/AUTHORING.DIMOHY.rNN.md`
- `ko/AUTHORING.DIMOHY.rNN.md`
- this `README.md`
- Git tag `rNN`
- remote `main` branch

A release is not complete if only one language is updated.

## License

Apache License 2.0. See [`LICENSE`](LICENSE) for the full text.
