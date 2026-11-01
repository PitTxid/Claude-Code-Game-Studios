# Antigravity Model & Effort Routing Matrix (CCGS)

This document defines the **pinned `[Model + Effort]` assignment** for all **49 Agents**, **74 Skills**, **Planning Phases**, and **Main Coding Workflows** in Claude Code Game Studios when running on **Google Antigravity**.

> **Core Rule — No `inherit`:** Never pass `Model: "inherit"` when invoking a subagent via `invoke_subagent`. Always pass the explicit pinned `Model` (`"pro"`, `"flash"`, or `"flash_lite"`) and prepend the corresponding **Reasoning Effort Directive** (`HIGH`, `MEDIUM`, `LOW`, or `MINIMAL`) to the subagent's prompt.

---

## 1. Reasoning Effort Directives (Injected into Subagent Prompts)

Because `invoke_subagent` selects the model family via `Model: "pro" | "flash" | "flash_lite"`, the reasoning depth (Thinking & Output effort) is enforced by prepending one of the following directives to the subagent's `SystemPrompt` / `Prompt`:

- **`[EFFORT: HIGH — Deep Multi-Angle Reasoning]`**
  > *"Evaluate trade-offs across all project pillars, architecture constraints, performance budgets, and edge cases. Present 2–3 concrete options with pros/cons before recommending a verdict. Do not skip edge-case or cross-system impact analysis."*
- **`[EFFORT: MEDIUM — Focused Architectural & Logic Reasoning]`**
  > *"Follow the approved GDD, ADR, and Story specifications directly. Focus your reasoning strictly on algorithm correctness, state transitions, engine idioms, and listed edge cases. Keep explanations concise and avoid theoretical essays."*
- **`[EFFORT: LOW — Direct Template Execution]`**
  > *"Execute the task directly against the provided template or specification. Do not redesign the architecture or propose unrequested scope. Keep thinking brief and output compact."*
- **`[EFFORT: MINIMAL — Zero-Fluff Checklist & Lookup]`**
  > *"Perform a fast, literal check or extraction. Output bullet points or table rows only (`terse`). No narrative preamble."*

---

## 2. Main Coding Workflow (`/dev-story` & `/team-*` Phase 3)

| Coding Task / Layer | Assigned Agent(s) | `invoke_subagent` `Model` | Effort Directive | Notes |
| :--- | :--- | :---: | :---: | :--- |
| **1. Core Engine, Foundation & Networking**<br>*(Save/Load, Event Bus, Pooling, Multiplayer Sync, Security)* | `engine-programmer`<br>`network-programmer`<br>`security-engineer` | `"pro"` | **`HIGH`** | Highest risk of memory leaks, race conditions, or desyncs. |
| **2. Core Gameplay & AI Logic (~70% of Game Code)**<br>*(Combat, Movement, Physics, Inventory, NPC/Boss AI, Pathfinding)* | `gameplay-programmer`<br>`ai-programmer` | `"pro"` | **`MEDIUM`**<br>*(Escalate to **`HIGH`** for complex state machines / physics / custom math)* | Pinned to `"pro"` for top-tier code quality; `MEDIUM` effort prevents over-thinking when Story & ADR specs are already clear. |
| **3. Code Review & Architectural Sign-off**<br>*(Running `/code-review` before `/story-done`)* | `lead-programmer` | `"pro"` | **`HIGH`** | Deep static & architectural review. |
| **4. UI, Editor Tools & Throwaway Prototypes**<br>*(HUD, Menus, Inspector plugins, `prototypes/`)* | `ui-programmer`<br>`tools-programmer`<br>`prototyper` | `"flash"` | **`MEDIUM`** | Event-driven UI binding and rapid prototyping run 3x faster on `"flash"`. |
| **5. Post-Code Engine API Check & Unit Test Scaffolding**<br>*(Phase 5/6 of `/dev-story`)* | 12 Engine Sub-Specialists (`godot-*`, `unity-*`, `ue-*`) & `qa-tester` | `"flash"` | **`LOW`** | Cross-references written code against `docs/engine-reference/<engine>/` and runs parse checks. |
| **6. Config / JSON Data Edits**<br>*(Tuning values in `assets/data/`)* | Direct or `qa-tester` | `"flash_lite"` | **`MINIMAL`** | Simple data mutations + JSON validation. |

---

## 3. Planning Pipeline Routing (4 Levels of Planning)

| Planning Level | Skills | Execution Strategy (`[Model + Effort]`) | Output Artifacts |
| :--- | :--- | :--- | :--- |
| **Level 1: Concept & Brainstorming** | `/brainstorm`<br>`/quick-design` | • **Interactive drafting:** `Model: "flash"` + `Effort: MEDIUM`<br>• **Director Pillar Gate (`full` mode):** `creative-director` on `Model: "pro"` + `Effort: HIGH` | `design/game-brief.md` or `design/gdd/game-concept.md` |
| **Level 2: Systems & Technical Architecture** | `/map-systems`<br>`/create-architecture`<br>`/architecture-decision`<br>`/create-control-manifest` | • **Decomposition & Blueprinting:** `Model: "pro"` + `Effort: HIGH` (`technical-director`, `<engine>-specialist`, `game-designer`) | `design/gdd/systems-index.md`, `docs/architecture/architecture.md`, `docs/architecture/adr-NNNN.md` |
| **Level 3: Production, Epics, Stories & Sprint Plan** | `/create-epics`<br>`/create-stories`<br>`/sprint-plan`<br>`/estimate` | • **Backlog Decomposition (Drafting Epics/Stories/Sprint tables):** `Model: "flash"` + `Effort: LOW` (at `minimal`) or `MEDIUM` (at `standard`/`full`)<br>• **Scope & Readiness Gate:** `producer` on `Model: "pro"` + `Effort: HIGH` | `production/epics/<slug>/EPIC.md`, `story-NNN.md`, `production/sprints/sprint-NNN.md` |
| **Level 4: Pre-Coding Architecture Sketch** | Phase 2 of `/team-*` & complex `/dev-story` | • **Interface & File List Sketch:** `lead-programmer` / `<engine>-specialist` on `Model: "pro"` + `Effort: MEDIUM` | `docs/architecture/[feature]-sketch.md` |

---

## 4. Complete 49-Agent Routing Table (Zero `inherit`)

### Tier A: `"pro"` + `HIGH` Effort (8 Agents — Strategic Directors & High-Risk Architects)
1. `creative-director`
2. `technical-director`
3. `producer`
4. `lead-programmer`
5. `systems-designer`
6. `economy-designer`
7. `network-programmer`
8. `security-engineer`

### Tier B: `"pro"` + `MEDIUM` Effort (7 Agents — Core Gameplay/Engine Code & Primary Engine Architects)
9. `gameplay-programmer` *(escalate to `HIGH` for complex math/physics/state-machines)*
10. `ai-programmer` *(escalate to `HIGH` for complex utility AI / GOAP / navmesh)*
11. `engine-programmer` *(escalate to `HIGH` for Foundation multithreading / memory architecture)*
12. `game-designer`
13. `godot-specialist`
14. `unity-specialist`
15. `unreal-specialist`

### Tier C: `"flash"` + `MEDIUM` Effort (17 Agents — Feature Specialists, UI Code & Engine Sub-Specialists)
16. `ui-programmer`
17. `performance-analyst`
18. `level-designer`
19. `ux-designer`
20. `live-ops-designer`
21. `godot-gdscript-specialist`
22. `godot-csharp-specialist`
23. `godot-shader-specialist`
24. `godot-gdextension-specialist`
25. `unity-dots-specialist`
26. `unity-shader-specialist`
27. `unity-addressables-specialist`
28. `unity-ui-specialist`
29. `ue-gas-specialist`
30. `ue-blueprint-specialist`
31. `ue-replication-specialist`
32. `ue-umg-specialist`

### Tier D: `"flash"` + `LOW` Effort (12 Agents — Content/Ops Leads, Prototyping, Tools & Asset Specs)
33. `art-director`
34. `audio-director`
35. `narrative-director`
36. `qa-lead`
37. `release-manager`
38. `prototyper`
39. `tools-programmer`
40. `devops-engineer`
41. `technical-artist`
42. `sound-designer`
43. `writer`
44. `world-builder`

### Tier E: `"flash_lite"` + `MINIMAL` Effort (5 Agents — Checklists, QA Test Cases, Localization & Comms)
45. `qa-tester`
46. `accessibility-specialist`
47. `localization-lead`
48. `analytics-engineer`
49. `community-manager`

---

## 5. Complete 74-Skill Routing Table

When a skill performs heavy reading, drafting, or analysis, offload its worker phase to a subagent using the tier below so the main session's context window and quota stay lean:

- **`"pro"` + `HIGH` (5 Skills):**
  `/architecture-review`, `/gate-check`, `/review-all-gdds`, `/create-architecture`, `/security-audit`
- **`"pro"` + `MEDIUM` (7 Skills):**
  `/map-systems`, `/design-system`, `/architecture-decision`, `/create-control-manifest`, `/balance-check`, `/design-review`, `/code-review`, `/dev-story` *(when implementing Foundation/Gameplay/AI stories)*
- **`"flash"` + `MEDIUM` (32 Skills):**
  `/brainstorm`, `/quick-design`, `/create-epics`, `/create-stories`, `/sprint-plan`, `/prototype`, `/vertical-slice`, `/ux-design`, `/ux-review`, `/art-bible`, `/asset-spec`, `/propagate-design-change`, `/reverse-document`, `/perf-profile`, `/tech-debt`, `/soak-test`, `/regression-suite`, `/test-flakiness`, `/skill-improve`, `/hotfix`, `/day-one-patch`, `/localize`, `/team-combat`, `/team-narrative`, `/team-ui`, `/team-release`, `/team-polish`, `/team-audio`, `/team-level`, `/team-live-ops`, `/team-qa`
- **`"flash"` + `LOW` (19 Skills):**
  `/start`, `/settings`, `/setup-engine`, `/adopt`, `/story-done`, `/story-readiness`, `/estimate`, `/bug-report`, `/bug-triage`, `/changelog`, `/patch-notes`, `/release-checklist`, `/launch-checklist`, `/retrospective`, `/milestone-review`, `/playtest-report`, `/content-audit`, `/qa-plan`, `/test-setup`, `/test-evidence-review`
- **`"flash_lite"` + `MINIMAL` (10 Skills):**
  `/help`, `/onboard`, `/project-stage-detect`, `/scope-check`, `/sprint-status`, `/smoke-check`, `/asset-audit`, `/consistency-check`, `/test-helpers`, `/skill-test`
