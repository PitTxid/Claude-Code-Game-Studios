# Claude Code Game Studios — Antigravity Studio Configuration (`AGENTS.md`)

Indie game development managed through **49 coordinated subagents** and **74 workflow skills**, adapted to run natively on **Google Antigravity** with explicit **`[Model + Effort]`** quota optimization.

## Technology Stack

- **Engine**: [CHOOSE: Godot 4 / Unity / Unreal Engine 5]
- **Language**: [CHOOSE: GDScript / C# / C++ / Blueprint]
- **Version Control**: Git with trunk-based development
- **Build System**: [SPECIFY after choosing engine]
- **Asset Pipeline**: [SPECIFY after choosing engine]

> **First session?** If the project has no engine configured and no game concept, run `/start` to begin the guided onboarding flow.

## Project Structure & Engine Reference

@[directory-structure.md](.claude/docs/directory-structure.md)

<!-- ENGINE-REFERENCE-IMPORT: /setup-engine rewrites the line below to match the chosen engine -->
@[VERSION.md](docs/engine-reference/godot/VERSION.md)

## Technical Preferences & Configuration

`project.yaml` at the repo root is the primary config store — engine, specialists, naming, platform, performance, modes. Skills resolve it via `resolve_config` (see `.claude/docs/config-resolution.md`).

---

## Antigravity Runtime & Tool Mapping Protocol

All 74 skills in `.claude/skills/` (registered via `.agents/skills.json`) and all 49 agents in `.claude/agents/` execute on Antigravity using the following translation rules:

1. **Tool Name Mapping:**
   - `AskUserQuestion` $\rightarrow$ Call Antigravity's native **`ask_question`** tool (put recommended option first with `(Recommended)`).
   - `Read` / `Write` / `Edit` $\rightarrow$ Call **`read_file`** / **`write_file`** / **`edit_file`**.
   - `Glob` / `Grep` / `Bash` $\rightarrow$ Call **`run_command`** (or read files directly).
   - `WebSearch` $\rightarrow$ Call **`search_web`**.
2. **Skill Bootstrap Pre-Execution (`` !`bash ...` `` lines) & Shell Scripts on Windows:**
   - On Windows PowerShell, always run `.claude/scripts/*.sh` and `.claude/hooks/*.sh` using Git Bash (`& "C:\Program Files\Git\bin\bash.exe" .claude/...`) rather than bare `bash` (which may invoke WSL without Windows Python).
   - When a `SKILL.md` file begins with `` !`bash "${CLAUDE_SKILL_DIR}/../../hooks/yaml-helper.sh" resolve_config ...` `` (or similar `` !`...` `` line), **run that command first via `run_command`** using `& "C:\Program Files\Git\bin\bash.exe" .claude/hooks/yaml-helper.sh resolve_config ...`, and use its output as the resolved config block for the skill.
3. **Subagent Invocation (`Agent` tool $\rightarrow$ `define_subagent` + `invoke_subagent`):**
   - When a skill delegates to `subagent_type: <agent-name>`:
     1. Read `.claude/agents/<agent-name>.md`.
     2. Register the subagent once per conversation via **`define_subagent`** (`Name: "<agent-name>"`, `Description` from frontmatter, `SystemPrompt` = the **`[EFFORT: ...]` directive** + the markdown body of `.claude/agents/<agent-name>.md`, `EnableWriteTools: true` unless `disallowedTools: Bash` or read-only).
     3. Launch it via **`invoke_subagent`** with `TypeName: "<agent-name>"`, `Prompt: "<distilled brief + return contract>"`, and the **exact pinned `Model`** from the routing table below (**NEVER use `Model: "inherit"`**).

---

## Pinned `[Model + Effort]` Routing Protocol (Zero `inherit`)

**CRITICAL RULE:** Never use `Model: "inherit"` when calling `invoke_subagent`. Always select the explicit pinned `Model` (`"pro"`, `"flash"`, or `"flash_lite"`) and prepend the corresponding **Reasoning Effort Directive** (`HIGH`, `MEDIUM`, `LOW`, or `MINIMAL`) to the subagent's prompt. Full specification: `.agents/model-effort-routing.md`.

### 1. Reasoning Effort Directives (Prepend to Subagent Prompt)

- **`HIGH` (`[EFFORT: HIGH — Deep Multi-Angle Reasoning]`):** Evaluate trade-offs across all pillars, architecture constraints, performance budgets, and edge cases. Present 2–3 options with pros/cons before recommending a verdict.
- **`MEDIUM` (`[EFFORT: MEDIUM — Focused Architectural & Logic Reasoning]`):** Follow approved GDD/ADR/Story specs directly. Focus reasoning strictly on algorithm correctness, state transitions, engine idioms, and listed edge cases. No theoretical essays.
- **`LOW` (`[EFFORT: LOW — Direct Template Execution]`):** Execute directly against the provided template/spec. Do not redesign architecture or expand scope. Keep thinking brief and output compact.
- **`MINIMAL` (`[EFFORT: MINIMAL — Zero-Fluff Checklist & Lookup]`):** Fast literal check or extraction. Output bullet points or table rows only (`terse`).

### 2. Main Coding Routing (`/dev-story` & `/team-*` Phase 3)

| Coding Task / Layer | Assigned Agent(s) | `Model` | `Effort` |
| :--- | :--- | :---: | :---: |
| **Core Engine / Foundation / Networking / Security** | `engine-programmer`, `network-programmer`, `security-engineer` | `"pro"` | **`HIGH`** |
| **Core Gameplay & AI Logic (~70% of Game Code)** | `gameplay-programmer`, `ai-programmer` | `"pro"` | **`MEDIUM`** *(or **`HIGH`** for complex state machines/physics/math)* |
| **Architectural Code Review (`/code-review`)** | `lead-programmer` | `"pro"` | **`HIGH`** |
| **UI, Editor Tools & Prototypes** | `ui-programmer`, `tools-programmer`, `prototyper` | `"flash"` | **`MEDIUM`** |
| **Engine API Check & Unit Test Scaffolding** | 12 Engine Sub-Specialists (`godot-*`, `unity-*`, `ue-*`), `qa-tester` | `"flash"` | **`LOW`** |
| **Config / JSON Data Edits (`assets/data/`)** | Direct or `qa-tester` | `"flash_lite"` | **`MINIMAL`** |

### 3. Planning Pipeline Routing (4 Levels)

| Planning Level | Skills | Assigned `[Model + Effort]` |
| :--- | :--- | :--- |
| **1. Concept & Brainstorm** | `/brainstorm`, `/quick-design` | Drafting: **`"flash"` + `MEDIUM`** · Director Gate (`full` mode): `creative-director` on **`"pro"` + `HIGH`** |
| **2. Systems & Technical Architecture** | `/map-systems`, `/create-architecture`, `/architecture-decision`, `/create-control-manifest` | **`"pro"` + `HIGH`** (`technical-director`, `<engine>-specialist`, `game-designer`) |
| **3. Production, Epics, Stories & Sprint Plan** | `/create-epics`, `/create-stories`, `/sprint-plan`, `/estimate` | Backlog splitting: **`"flash"` + `LOW`/`MEDIUM`** · Scope Gate: `producer` on **`"pro"` + `HIGH`** |
| **4. Pre-Coding Architecture Sketch** | Phase 2 of `/team-*` & `/dev-story` | **`"pro"` + `MEDIUM`** (`lead-programmer`, `<engine>-specialist`) |

### 4. All 49 Agents — Pinned `[Model + Effort]` Table

- **🔴 `"pro"` + `HIGH` (8 Agents):** `creative-director`, `technical-director`, `producer`, `lead-programmer`, `systems-designer`, `economy-designer`, `network-programmer`, `security-engineer`
- **🟠 `"pro"` + `MEDIUM` (7 Agents):** `gameplay-programmer`, `ai-programmer`, `engine-programmer`, `game-designer`, `godot-specialist`, `unity-specialist`, `unreal-specialist`
- **🟡 `"flash"` + `MEDIUM` (17 Agents):** `ui-programmer`, `performance-analyst`, `level-designer`, `ux-designer`, `live-ops-designer`, `godot-gdscript-specialist`, `godot-csharp-specialist`, `godot-shader-specialist`, `godot-gdextension-specialist`, `unity-dots-specialist`, `unity-shader-specialist`, `unity-addressables-specialist`, `unity-ui-specialist`, `ue-gas-specialist`, `ue-blueprint-specialist`, `ue-replication-specialist`, `ue-umg-specialist`
- **🔵 `"flash"` + `LOW` (12 Agents):** `art-director`, `audio-director`, `narrative-director`, `qa-lead`, `release-manager`, `prototyper`, `tools-programmer`, `devops-engineer`, `technical-artist`, `sound-designer`, `writer`, `world-builder`
- **🟢 `"flash_lite"` + `MINIMAL` (5 Agents):** `qa-tester`, `accessibility-specialist`, `localization-lead`, `analytics-engineer`, `community-manager`

---

## Coordination & Collaboration Protocol

@[coordination-rules.md](.claude/docs/coordination-rules.md)

**User-driven collaboration, not autonomous execution.**
Every task follows: **Question -> Options -> Decision -> Draft -> Approval**

- Agents MUST ask `"May I write this to [filepath]?"` before using `write_file`/`edit_file` (exempt: `active.md` appends, agent memory, a subagent's new file at the path its orchestrating skill named; see `.claude/rules/agent-memory.md`).
- Agents MUST show drafts or summaries before requesting approval.
- Multi-file changes require explicit approval for the full changeset.
- No commits without user instruction.
- A skipped or dismissed question is not an answer: act on nothing, re-ask in plain text and wait.

## Coding Standards & Path-Scoped Rules

@[coding-standards.md](.claude/docs/coding-standards.md)

When reading or editing files matching the paths below, load and enforce the corresponding rule file in `.claude/rules/`:

- **Gameplay code** (`src/gameplay/**`, `Assets/**/Gameplay/**`, `Source/**/Gameplay/**`) $\rightarrow$ `.claude/rules/gameplay-code.md` (data-driven values, `delta` time, no direct UI references)
- **Core / Engine code** (`src/core/**`, `Assets/**/Core/**`, `Source/**/Core/**`) $\rightarrow$ `.claude/rules/engine-code.md` (zero hot-path allocations, thread safety)
- **AI code** (`src/ai/**`, `Assets/**/AI/**`, `Source/**/AI/**`) $\rightarrow$ `.claude/rules/ai-code.md`
- **Networking code** (`src/networking/**`, `Assets/**/Networking/**`, `Source/**/Networking/**`) $\rightarrow$ `.claude/rules/network-code.md`
- **UI code** (`src/ui/**`, `Assets/**/UI/**`, `Source/**/UI/**`) $\rightarrow$ `.claude/rules/ui-code.md` (no game state ownership, localization-ready)
- **Shaders** (`assets/shaders/**`, `**/*.gdshader`, `**/*.shader`, `**/*.hlsl`) $\rightarrow$ `.claude/rules/shader-code.md`
- **Scenes & Resources** (`**/*.tscn`, `**/*.tres`, `**/*.unity`, `**/*.prefab`) $\rightarrow$ `.claude/rules/scene-files.md`
- **Data JSON** (`assets/data/**/*.json`, `Assets/**/Data/**/*.json`, `Content/**/Data/**/*.json`) $\rightarrow$ `.claude/rules/data-files.md`
- **GDDs & Narrative** (`design/gdd/**`, `design/narrative/**`) $\rightarrow$ `.claude/rules/design-docs.md`, `.claude/rules/narrative.md`
- **Tests & Prototypes** (`tests/**`, `prototypes/**`) $\rightarrow$ `.claude/rules/test-standards.md`, `.claude/rules/prototype-code.md`

## Context Management

- **`production/session-state/active.md` is the session checkpoint.** Read it first after any compaction, crash, or new session.
- **Helpers in `.claude/scripts/` emit observations, never verdicts.**
