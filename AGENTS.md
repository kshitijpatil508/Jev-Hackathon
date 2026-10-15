# AGENTS.md — Shared Operating Instructions (all LLMs)

This file is the single source of operating rules for **every** AI agent on this project
(Codex, Claude, and any other assistant). Model-specific files (e.g. `CLAUDE.md`) only point here.
Mirror approved workflow changes in the affected sections of `introPrompt.txt` and record them in
its revision log. If the files disagree, use the latest explicit human-approved revision and reconcile
the mismatch within your assigned scope; do not silently treat a proposal as an approved decision.

## 0. Who is in charge
- Two **human participants** orchestrate. They decide which agent works, on what, and when it stops.
- This is **not** an autonomous multi-agent system. You cannot contact or start other agents.
- No agent has a permanent role. Any agent may plan, code, test, review, debug or document.
- The shared files and the Git repository are the **source of truth** — not your chat history.

## 1. Mandatory start-of-session procedure (do this every time, before acting)
1. Read `AGENTS.md` (this file).
2. Read `.hackathon/CURRENT_STATE.md` — it names the latest handoff and what needs attention.
3. Read the handoff named in `CURRENT_STATE.md`, plus handoffs relevant to your assigned task.
   If the pointer is missing or stale, inspect dated handoffs and their task/status context; legacy
   numbered handoffs remain valid history. Do not recursively read
   every older handoff unless resolving a specific question.
4. Read `.hackathon/THEME.md` and `.hackathon/DECISIONS.md` (approved direction — respect it).
5. Read `.hackathon/TASK_BOARD.md` — check which tasks/files are claimed by someone else.
6. Check Git even for documentation work: run `git status --short`; when history exists, inspect
   `git log --oneline -10` and relevant diffs. If no repository or commits exist, record that fact
   and continue the authorized task; do not treat an expected Git error as a blocker.
7. Briefly tell the human what you understood and what you intend to do, then do it.

## 2. Phase gates (do not skip)
| Phase | Allowed | Not allowed |
|---|---|---|
| 1. Workspace prep | Set up/repair these docs | Inventing a theme or writing app code |
| 2. Theme announced | Fill `THEME.md`, brainstorm in `IDEAS.md`, review others' ideas | App code |
| 3. Idea approved by a human | Record in `DECISIONS.md`, plan in `PROJECT_PLAN.md`, tasks in `TASK_BOARD.md` | Reopening the decision without a real blocker |
| 4. Development | Assigned task only, within approved scope | Major architecture/scope/tech changes without approval |
| 5. Review | Verify, report, fix within authorized scope | Rewrites driven by preference |

**Never start the main implementation until a human explicitly selects an idea and authorizes development.**

## 3. Working on shared files safely
- **Claim before you edit.** In `TASK_BOARD.md`, set yourself as Owner and list the files/dirs you will touch.
  Do not edit files claimed by an in-progress task owned by someone else unless a human tells you to.
  Claims are active for `IN PROGRESS`, `BLOCKED`, and `IN REVIEW` tasks; `DONE` releases them.
  A blocked or interrupted task retains its claim until the human reassigns it or its owner explicitly
  releases the files in the task notes. Record the blocker and exact next action there.
- **Append, don't overwrite**, in `IDEAS.md`, `reviews/`, and `handoffs/`. Add your own dated, signed section.
  Do not delete or rewrite another agent's text; disagree in your own section and say why.
- **`DECISIONS.md` records human decisions only.** Agents may write an entry only when a human has approved it,
  and must say who approved it. Proposals go in `IDEAS.md` or `PROJECT_PLAN.md`.
- `CURRENT_STATE.md` is a living summary — the active writer may update it; during parallel work,
  only the designated shared-document writer updates it. Keep it short and accurate.
- Never assume code written by another LLM is correct. Never overwrite it blindly.
- Parallel work: before it starts, agree (in `PROJECT_PLAN.md`) on file ownership, interfaces and an
  integration point. Append-only text is **not** a lock: two writers can still lose each other's edits.
  Humans designate one shared-document writer for the task board, state, decisions, and idea index;
  other agents put notes in distinct, preassigned files for that writer to integrate.
  Use separate checkouts/worktrees for concurrent code work. Merely creating branches in one shared
  directory does not isolate edits; never switch that directory's branch while another agent is using it.
  Use distinct agent/session labels for parallel artifacts. No parallel work without human assignment.

## 4. Development protocol
1. Do the start-of-session procedure.
2. Review the code you are about to build on; note defects you find.
3. State your plan in 2–5 lines.
4. Implement the assigned task. Small, scoped improvements inside the task are fine.
5. Run the relevant tests/checks. If you could not run something, say so explicitly.
6. Update `TASK_BOARD.md` (status) and `CURRENT_STATE.md`.
7. Write a handoff (Section 6). Then stop — a handoff does not start the next agent.

## 5. Review protocol
1. Read the relevant handoff(s), then the **actual** code/diff — not just the description.
2. Compare against `THEME.md` requirements and `DECISIONS.md`.
3. Look for: bugs, missing requirements, wrong assumptions, needless complexity, integration problems.
4. Verify with tests or direct execution where possible; label each finding *verified* or *suspected*.
5. Report findings to the human; fix only what is in your authorized scope.
6. Write the review to `.hackathon/reviews/` and a handoff to `.hackathon/handoffs/`.
A review must add value. Keep it focused — we have ~2 hours total.

## 6. Handoffs (mandatory after any substantial session)
- File: `.hackathon/handoffs/YYYY-MM-DD_HHMM_<agent>_<topic>.md`.
  Read the current clock and convert to IST (UTC+05:30); use 24-hour time. Capture one timestamp
  for the handoff and matching `.hackathon/prompts/YYYY-MM-DD_HHMM_<agent>_next.md`.
  Never overwrite an existing artifact. If an agent creates multiple pairs in one minute, use a distinct
  agent/session label (e.g. `codex-session2`) consistently in both names. Keep legacy numbered files intact.
- Reviews use `.hackathon/reviews/YYYY-MM-DD_HHMM_<reviewer>_review-<topic>.md` with the same clock rule.
- Use the template in `.hackathon/handoffs/README.md`. Every section is required (write "None" if empty).
- The **Suggested Prompt for the Next Agent** section links to the separate prompt file; do not duplicate
  the prompt text. Use `.hackathon/prompts/README.md`. It must reference shared files and work for any LLM.
- Update `CURRENT_STATE.md` → "Latest handoff" and "Latest next-agent prompt" together after both exist.
  These explicit pointers select the current pair; timestamps alone do not prove integration order.
- Report work honestly: completed / partial / blocked / proposed. Never claim tests you didn't run.
- Keep each section brief; link to existing plans/reviews instead of copying them. Record the workspace,
  branch and base commit when available, dirty files left behind, and reproducible validation commands.
  The next-agent prompt must describe the actual current phase, not assume a theme or approval exists.

## 7. Git
- **Codex is the only Git operator.** Only Codex runs Git write commands (`init`, `add`, `commit`,
  `branch`, `pull`, `push`), and only when a human says so. Other agents may run read-only Git
  commands (`status`, `log`, `diff`) but must not change Git state.
- Non-Codex agents: when you think a commit, pull or push is due, tell the human and list it under
  "Human Decisions Required" in your handoff, naming the files to include.
- Commit or push **only when a human says so.** Never force-push, rewrite history, or discard uncommitted work.
- Before committing: check `git status`, make sure docs/handoffs are current, and scan for secrets.
- `.env` holds real credentials and is git-ignored. Never print, commit, or paste secret values.
  Read keys from the environment; document new variables (names only) in `.env.example`.
- Commit messages: `<area>: <what changed>` and mention the agent, e.g. `api: add Jev client (claude)`.

## 8. Status vocabulary (use everywhere)
`TODO` · `IN PROGRESS` · `BLOCKED` · `IN REVIEW` · `DONE` · `PROPOSED` · `REJECTED`

## 9. Map of this repository
```
AGENTS.md            ← you are here (rules for all agents)
CLAUDE.md            ← Claude entry point → points here
README.md            ← human-facing project overview
introPrompt.txt      ← master brief and revision log; mirror approved workflow changes here
.env / .env.example  ← secrets (ignored) / variable names (committed)
.hackathon/
  THEME.md           ← official theme & requirements (confirmed vs assumed)
  IDEAS.md           ← brainstorming from every agent + cross-reviews
  DECISIONS.md       ← human-approved decisions only
  PROJECT_PLAN.md    ← architecture, phases, interfaces, definition of done
  TASK_BOARD.md      ← tasks, owners, claimed files, status, dependencies
  CURRENT_STATE.md   ← short living status + pointer to latest handoff
  handoffs/          ← timestamped agent handoffs (template in README.md; legacy files preserved)
  prompts/           ← matching copy-paste prompts, linked from each handoff
  reviews/           ← code review findings (template in README.md)
src/                 ← application code
tests/               ← tests
```

## 10. Operating rules (summary)
1. Humans decide. 2. Read the shared files, not your memory. 3. No fixed roles. 4. Review critically.
5. Report accurately. 6. Always hand off. 7. Respect decisions. 8. Minimal overhead — no frameworks,
dashboards or orchestration infrastructure. 9. Don't collide on files. 10. Only Codex touches Git, and only when told.
11. Label work as done/partial/blocked/proposed. 12. You cannot talk to the other agents.
