# Jev Hackathon — SDLC Assistant (working title)

Repository: [kshitijpatil508/Jev-Hackathon](https://github.com/kshitijpatil508/Jev-Hackathon) · Branch: `main`.

> The hackathon theme has **not been announced yet**. This README will describe the product once a
> human-approved idea exists (see `.hackathon/DECISIONS.md`).

## Team
- 2 human participants (orchestrators)
- 4 AI assistants (OpenAI Codex, Claude, and two others) working as interchangeable engineers

## How this repository works
All agents coordinate through shared files, not chat history:
- **Rules for agents:** [`AGENTS.md`](AGENTS.md)
- **Where things stand:** [`.hackathon/CURRENT_STATE.md`](.hackathon/CURRENT_STATE.md)
- **Theme & requirements:** [`.hackathon/THEME.md`](.hackathon/THEME.md)
- **Decisions:** [`.hackathon/DECISIONS.md`](.hackathon/DECISIONS.md)
- **Plan & tasks:** [`.hackathon/PROJECT_PLAN.md`](.hackathon/PROJECT_PLAN.md), [`.hackathon/TASK_BOARD.md`](.hackathon/TASK_BOARD.md)
- **Collaboration record:** [`.hackathon/handoffs/`](.hackathon/handoffs/), [`.hackathon/reviews/`](.hackathon/reviews/)
- **Next-agent prompts:** [`.hackathon/prompts/`](.hackathon/prompts/) — use the one linked in CURRENT_STATE.md.

## Setup
1. If `.env` does not exist, copy `.env.example` to `.env` and fill in values. Preserve an existing
   `.env`; never commit it or include its values in prompts/handoffs.
2. _Run instructions will be added once the stack is chosen._

## Running a work session
Give the active assistant the repository folder and a concrete task. Use this prompt with any model:

> Read AGENTS.md and follow its start-of-session procedure. Your assigned task is: <task and expected
> result>. Work only within that scope and existing approvals. Verify the result, update TASK_BOARD.md
> and CURRENT_STATE.md, and leave a timestamped handoff linking to a matching prompt in
> .hackathon/prompts/. Do not start another agent.

- Switch assistants after the current writer finishes its handoff. Ask the next one to review the
  actual changed files and take one specific next task; avoid open-ended repeated reviews.
- If a session is interrupted, have its replacement inspect the working files before resuming.
  Reassign its task explicitly; a stale claim is not evidence that the work was completed.
- For simultaneous work, the two humans first assign distinct files and interfaces, a single
  shared-document writer, and separate checkouts/worktrees for code. Branch names alone do not isolate
  a shared folder. Across machines, exchange work through explicitly authorized commits/pushes and
  record the commit each assistant starts from; local files do not synchronize themselves.
- A review request should name the task/files, desired checks, and whether scoped fixes are allowed.
  One focused review after a meaningful milestone usually provides more value than model round-trips
  on every small edit.

## Suggested two-hour schedule (proposal, not an approved project plan)
| Minutes from kickoff | Focus |
|---|---|
| 0–15 | Capture requirements, compare at most 3 ideas, human selects one and authorizes development |
| 15–30 | Choose the smallest demo path; check required API access; get a skeleton running |
| 30–85 | Implement the MVP with frequent end-to-end integration |
| 85–105 | Freeze new features; focused review and fix demo-blocking defects |
| 105–120 | Rehearse demo, verify submission requirements, finish README and submit |

Adapt these time boxes to the actual deadline and judging criteria. Keep documentation updates short:
task result, evidence, blocker, next step. Test unknown integrations early once authorized, and identify
a clearly labeled fallback if they fail.
