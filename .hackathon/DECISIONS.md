# DECISIONS — Human-Approved Only

Only decisions approved by a human participant go here. Agents may write an entry on a human's
instruction and must name the approver. Do not reopen a decision without a significant blocker or
new requirement; if you must, propose it in `IDEAS.md` and flag it in your handoff.

## Entry template
```markdown
### D-<NNN>: <title>  — <YYYY-MM-DD HH:MM>
- Decision:
- Approved by: <human>
- Recorded by: <agent or human>
- Reason:
- Alternatives rejected (and why):
```

---

### D-000: Collaboration workflow — 2026-10-15
- Decision: Humans orchestrate four interchangeable LLMs through shared files in this repo
  (AGENTS.md + `.hackathon/`). No autonomous agent orchestration. Git is the source of truth.
- Approved by: Human participants (per `introPrompt.txt`)
- Recorded by: Claude
- Reason: Lets any model pick up work without relying on chat history.
- Alternatives rejected: Autonomous multi-agent framework — too much overhead for 2 hours.

_Selected project idea: not yet chosen._

### D-001: Shared GitHub repository — 2026-10-15
- Decision: Use https://github.com/kshitijpatil508/Jev-Hackathon and publish the current workspace
  on its existing default branch, main, preserving remote history and excluding credentials.
- Approved by: Praveen (provided repository URL and requested pushing the workspace in chat).
- Recorded by: Codex
- Reason: Share the collaboration files and future application with the team.
- Alternatives rejected: None discussed. Existing remote commits are retained.
