# TASK BOARD

Claim a task by setting **Owner** and **Files claimed** before editing. Humans assign tasks.
Status: `TODO` · `IN PROGRESS` · `BLOCKED` · `IN REVIEW` · `DONE` · `PROPOSED` · `REJECTED`
Parallel-safe = can run at the same time as other open tasks without touching the same files.
Append-only changes still require one writer. `DONE` releases file claims; `BLOCKED` and `IN REVIEW`
retain them unless explicitly released in Notes. For each implementation task, put a concrete
acceptance check and any blocker/next action in Notes. All file paths are relative to the repository root.

| ID | Task | Owner | Status | Files claimed | Depends on | Parallel-safe | Notes |
|---|---|---|---|---|---|---|---|
| T-000 | Prepare collaboration workspace | Claude | DONE | AGENTS.md, CLAUDE.md, README.md, .hackathon/* | — | — | Handoff 001 |
| T-001 | Record official theme in THEME.md | Claude | DONE | Released | Theme announcement | No | Recorded from theme/ (18:48) |
| T-002 | Brainstorm ideas | Codex | IN PROGRESS | .hackathon/ideas/2026-10-15_1852_codex_theme-ideas.md, .hackathon/IDEAS.md, .hackathon/TASK_BOARD.md, .hackathon/CURRENT_STATE.md, new Codex handoff/prompt | T-001 | No while shared index/state are being updated | Claude's file is retained unchanged; Praveen assigned Codex to add an independent review and commit all theme work |
| T-003 | Humans select idea → DECISIONS.md | Humans | TODO | .hackathon/DECISIONS.md | T-002 | No | |
| T-004 | Write PROJECT_PLAN + break down tasks | — | TODO | .hackathon/PROJECT_PLAN.md, .hackathon/TASK_BOARD.md | T-003 | No | Include acceptance checks and development authorization |
| T-005 | Connect Git repo, commit workspace, and push | Codex | DONE | Released | — | No | [Handoff](handoffs/2026-10-15_1822_codex_git-publish.md); workspace published and remote SHA verified; .env excluded |
| T-006 | Review workspace and reconcile updated collaboration workflow | Codex | DONE | Released; changed files listed in handoff | T-000 | No | [Handoff](handoffs/2026-10-15_1816_codex_workspace-review.md); protocol and document-link checks passed |
| T-008 | Commit theme/ folder + Claude's theme docs | Codex (on human instruction) | TODO | theme/, .hackathon/* | T-002 | No | Requested by Claude 18:48 |
| T-007 | Record Codex-only Git operator rule | Claude | DONE | Released | T-006 | No | [Handoff](handoffs/2026-10-15_1820_claude_git-operator-rule.md) |

## Blockers
- Human must pick a theme/idea (T-003). 1-hour build clock is running.
- Jev API not yet smoke-tested.

## Pending reviews
- None.
