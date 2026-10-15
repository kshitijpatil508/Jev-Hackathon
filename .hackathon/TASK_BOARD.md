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
| T-002 | Brainstorm ideas | Codex | DONE | Released | T-001 | No | Praveen rejected the technical PR framing; Claude proposed Inbox to Pipeline and Codex refined it into the human-friendly Hot Lead Finder recommendation |
| T-003 | Humans select idea → DECISIONS.md | Praveen | DONE | Released | T-002 | No | D-003 Hot Lead Finder |
| T-004 | Write requirements, build guide, testing guide | Codex | DONE | Released | T-003 | No | Claude draft reviewed/fixed against official OpenAPI; [Codex review](reviews/2026-10-15_1923_codex_review-spec.md); spec ready pending human go |
| T-010 | Phase 0: Jev smoke test (`scripts/smoke_test.py`, requirements.txt) | — | TODO | requirements.txt, scripts/smoke_test.py, src/__init__.py, tests/__init__.py, THEME.md (smoke section) | T-004 + human go | No | GATE for live demo. BUILD_GUIDE Phase 0 |
| T-011 | Track A: Jev client + record_replay | — | TODO | src/jev_client.py, scripts/record_replay.py | T-010 | Yes | Track A owner = shared-doc writer during Phase 1 |
| T-012 | Track B: fixtures + sorter + unit tests | — | TODO | fixtures/messages.json, src/sorter.py, tests/test_sorter.py, tests/test_jev_client.py | T-004 + human go | Yes | `pytest -q` passes |
| T-013 | Track C: Streamlit UI (stub first) | — | TODO | src/app.py | T-004 + human go | Yes | Stub labelled STUB |
| T-014 | Phase 2: integrate + replay | — | TODO | src/app.py, fixtures/replay.json | T-011, T-012, T-013 | No | TESTING_GUIDE §4 |
| T-015 | Phase 3: polish + freeze checklist | — | TODO | src/app.py | T-014 | No | TESTING_GUIDE §5 |
| T-005 | Connect Git repo, commit workspace, and push | Codex | DONE | Released | — | No | [Handoff](handoffs/2026-10-15_1822_codex_git-publish.md); workspace published and remote SHA verified; .env excluded |
| T-006 | Review workspace and reconcile updated collaboration workflow | Codex | DONE | Released; changed files listed in handoff | T-000 | No | [Handoff](handoffs/2026-10-15_1816_codex_workspace-review.md); protocol and document-link checks passed |
| T-008 | Commit theme/ folder + both agents' theme docs | Codex | DONE | Released | T-002 | No | Theme package committed locally as d5e36b3; closeout handoff/state committed immediately afterward |
| T-007 | Record Codex-only Git operator rule | Claude | DONE | Released | T-006 | No | [Handoff](handoffs/2026-10-15_1820_claude_git-operator-rule.md) |
| T-009 | Commit Lead Acquisition decisions, reviewed spec and handoffs | Codex (on human instruction) | TODO | Current uncommitted Claude/Codex docs under .hackathon/ plus AGENTS.md and introPrompt.txt | T-004 | No | No commit or push requested in the review turn |

## Blockers
- Hot Lead Finder is selected; human development authorization is still required.
- Jev API not yet smoke-tested.

## Pending reviews
- None.
