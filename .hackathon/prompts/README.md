# Next-Agent Prompts (context switching)

One file per handoff, holding the copy-paste prompt that starts the next session.
Name: `YYYY-MM-DD_HHMM_<agent>_next.md`, with the **same timestamp** as the handoff it belongs to
(IST, UTC+05:30, 24-hour, converted from the current clock). Use the prompt explicitly linked in
`../CURRENT_STATE.md`; filename order alone does not establish integration order. For a same-minute
name collision, use a distinct agent/session label in both prompt and handoff; never overwrite a file.
Keep prompt text here only; the handoff links to it. Prompts describe current state and proposed next
steps, but do not grant approval or start another agent. The human's current assignment takes priority.

## Template
```markdown
# Next-Agent Prompt

Created: <YYYY-MM-DD HH:MM IST>
By: <agent>
Handoff: .hackathon/handoffs/<matching handoff file>
Phase when written: <phase>
Look at first: <files / diffs / task ids the next agent should focus on>

## Prompt (copy everything below)
> <copy-paste-ready instruction that references the shared files and works for any LLM>
```

The timestamped format is adopted in AGENTS.md and the templates. Legacy handoffs with embedded
prompts remain valid historical records and do not need to be renamed or rewritten.
