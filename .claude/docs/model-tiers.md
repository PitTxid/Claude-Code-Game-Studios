# Model & Effort Tier Assignment (Antigravity Edition)

> **Zero `inherit` Policy:** Every agent (`.claude/agents/*.md`) and every skill (`.claude/skills/*/SKILL.md`) declares an explicit pinned **`model:`** (`pro`, `flash`, or `flash_lite`) and **`effort:`** (`high`, `medium`, `low`, or `minimal`) in its YAML frontmatter. When invoking any subagent via `invoke_subagent`, always pass the pinned `Model` and prepend the matching **Reasoning Effort Directive** from `.agents/model-effort-routing.md`.

## Model & Effort Tiers Overview

| Tier | `model:` | `effort:` | When to use |
|------|----------|-----------|-------------|
| **Tier A — Strategic & High-Risk Architecture** | `pro` | `high` | Multi-document synthesis, phase-gate verdicts, core engine/networking/security architecture, deep code review |
| **Tier B — Core Gameplay/AI Coding & System Design** | `pro` | `medium` | Main game logic implementation (`/dev-story` gameplay & AI), system GDD authoring, primary engine architecture |
| **Tier C — Feature Implementation, UI & Engine Idioms** | `flash` | `medium` | UI code, feature/team orchestration, sprint/epic decomposition, 12 engine sub-specialists |
| **Tier D — Direct Execution, Content & Prototyping** | `flash` | `low` | Content/ops department leads, throwaway prototypes, build/editor tools, release checklists |
| **Tier E — Fast Checklists, QA & Lookups** | `flash_lite` | `minimal` | Read-only status checks, formatting, QA test-case scaffolding, localization & accessibility checklists |

---

## 1. Main Coding Workflow (`/dev-story` & `/team-*` Phase 3)

- **`pro` + `high`:** `engine-programmer` (Foundation), `network-programmer` (Networking), `security-engineer`, `lead-programmer` (`/code-review`)
- **`pro` + `medium`:** `gameplay-programmer` (Core/Feature Gameplay — ~70% of game code), `ai-programmer` (NPC/Boss AI & pathfinding) *(escalate to `high` for complex state machines, physics, or custom math)*
- **`flash` + `medium`:** `ui-programmer` (UI), `tools-programmer`, `prototyper`
- **`flash` + `low`:** 12 Engine Sub-Specialists (`godot-*`, `unity-*`, `ue-*`) for post-code engine API verification
- **`flash_lite` + `minimal`:** `qa-tester` and Config/Data JSON updates

---

## 2. Planning Pipeline Tiers

1. **Concept & Brainstorming (`/brainstorm`, `/quick-design`):** `flash` + `medium` (Director gates at `full` rigor run `creative-director` on `pro` + `high`)
2. **Systems & Architecture Blueprint (`/map-systems`, `/create-architecture`, `/architecture-decision`, `/create-control-manifest`):** `pro` + `high` / `medium`
3. **Production & Sprint Planning (`/create-epics`, `/create-stories`, `/sprint-plan`, `/estimate`):** Decomposition on `flash` + `medium`/`low`; Scope & readiness gate by `producer` on `pro` + `high`
4. **Pre-Coding Architecture Sketch (Phase 2 of `/team-*` & `/dev-story`):** `pro` + `medium` (`lead-programmer` / `<engine>-specialist`)

---

## 3. All 49 Agents by `[model + effort]`

- **`pro` + `high` (8):** `creative-director`, `technical-director`, `producer`, `lead-programmer`, `systems-designer`, `economy-designer`, `network-programmer`, `security-engineer`
- **`pro` + `medium` (7):** `gameplay-programmer`, `ai-programmer`, `engine-programmer`, `game-designer`, `godot-specialist`, `unity-specialist`, `unreal-specialist`
- **`flash` + `medium` (17):** `ui-programmer`, `performance-analyst`, `level-designer`, `ux-designer`, `live-ops-designer`, `godot-gdscript-specialist`, `godot-csharp-specialist`, `godot-shader-specialist`, `godot-gdextension-specialist`, `unity-dots-specialist`, `unity-shader-specialist`, `unity-addressables-specialist`, `unity-ui-specialist`, `ue-gas-specialist`, `ue-blueprint-specialist`, `ue-replication-specialist`, `ue-umg-specialist`
- **`flash` + `low` (12):** `art-director`, `audio-director`, `narrative-director`, `qa-lead`, `release-manager`, `prototyper`, `tools-programmer`, `devops-engineer`, `technical-artist`, `sound-designer`, `writer`, `world-builder`
- **`flash_lite` + `minimal` (5):** `qa-tester`, `accessibility-specialist`, `localization-lead`, `analytics-engineer`, `community-manager`

---

## 4. All 74 Skills by `[model + effort]`

- **`pro` + `high` (5):** `/architecture-review`, `/gate-check`, `/review-all-gdds`, `/create-architecture`, `/security-audit`
- **`pro` + `medium` (8):** `/map-systems`, `/design-system`, `/architecture-decision`, `/create-control-manifest`, `/balance-check`, `/design-review`, `/code-review`, `/dev-story`
- **`flash` + `medium` (31):** `/brainstorm`, `/quick-design`, `/create-epics`, `/create-stories`, `/sprint-plan`, `/prototype`, `/vertical-slice`, `/ux-design`, `/ux-review`, `/art-bible`, `/asset-spec`, `/propagate-design-change`, `/reverse-document`, `/perf-profile`, `/tech-debt`, `/soak-test`, `/regression-suite`, `/test-flakiness`, `/skill-improve`, `/hotfix`, `/day-one-patch`, `/localize`, `/team-combat`, `/team-narrative`, `/team-ui`, `/team-release`, `/team-polish`, `/team-audio`, `/team-level`, `/team-live-ops`, `/team-qa`
- **`flash` + `low` (20):** `/start`, `/settings`, `/setup-engine`, `/adopt`, `/story-done`, `/story-readiness`, `/estimate`, `/bug-report`, `/bug-triage`, `/changelog`, `/patch-notes`, `/release-checklist`, `/launch-checklist`, `/retrospective`, `/milestone-review`, `/playtest-report`, `/content-audit`, `/qa-plan`, `/test-setup`, `/test-evidence-review`
- **`flash_lite` + `minimal` (10):** `/help`, `/onboard`, `/project-stage-detect`, `/scope-check`, `/sprint-status`, `/smoke-check`, `/asset-audit`, `/consistency-check`, `/test-helpers`, `/skill-test`
