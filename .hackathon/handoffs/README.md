# Handoffs

One file per substantial agent session: `YYYY-MM-DD_HHMM_<agent>_<topic>.md`.
Read the current clock, convert to IST (UTC+05:30), and use 24-hour time. Capture one timestamp for
the handoff and its `../prompts/YYYY-MM-DD_HHMM_<agent>_next.md`. If a name already exists, use a unique
agent/session label in both files (e.g. `codex-session2`); never overwrite another session.
Keep legacy numbered handoffs as history. Never edit another agent's handoff.
After both files exist, update `../CURRENT_STATE.md` → "Latest handoff" and "Latest next-agent prompt".
A handoff does **not** start another agent — the humans do that.
Keep sections to a few bullets and link to existing details. For approved parallel sessions, humans
assign distinct agent/session labels and designate one writer to update CURRENT_STATE.md. Its explicit
pointers select the current pair; filename order alone does not establish integration order.

## Template (all sections required; write "None" if empty)
```markdown
# Agent Handoff

Agent:
Session: <YYYY-MM-DD HH:MM, timezone>
Phase:
Task: <TASK_BOARD id + title>
Workspace / branch / base commit: <path and Git refs, or not initialized>

## Original Objective
What I was asked to do.

## Work Completed
What I actually accomplished.

## Files Changed
Files created or modified and why. State uncommitted work left behind and claims retained/released.

## Technical Decisions
Important implementation choices and reasoning.

## Review Findings
Bugs, concerns, or improvements identified.

## Validation
Exact commands/checks and their results. Clearly state anything not tested.

## Current Project Status
What is working and what remains incomplete.

## Pending Tasks
Specific unfinished work.

## Recommendations
What I believe should happen next, and why.

## Human Decisions Required
Anything that requires approval.

## Suggested Prompt for the Next Agent
Link to the matching file in ../prompts/ (same timestamp and agent/session label).
Keep the actual copy-paste prompt there only; use ../prompts/README.md as its template.
```
