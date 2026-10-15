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

_Selected project idea: Hot Lead Finder (D-003)._

### D-001: Shared GitHub repository — 2026-10-15
- Decision: Use https://github.com/kshitijpatil508/Jev-Hackathon and publish the current workspace
  on its existing default branch, main, preserving remote history and excluding credentials.
- Approved by: Praveen (provided repository URL and requested pushing the workspace in chat).
- Recorded by: Codex
- Reason: Share the collaboration files and future application with the team.
- Alternatives rejected: None discussed. Existing remote commits are retained.

### D-002: Prefer a universally understandable concept — 2026-10-15 19:10
- Decision: Do not pursue PR Risk Router. The selected concept and live demo must be immediately
  understandable to a non-technical audience.
- Approved by: Praveen (“I want a simpler theme, anyone can understand; Pull request sort is too technical.”)
- Recorded by: Codex
- Reason: Presentation clarity is part of the judging rubric, and the intended audience should not
  need software-development context to understand the value.
- Alternatives rejected: PR Risk Router — technically sound, but too specialized for this audience.

### D-003: Build Hot Lead Finder (Lead Acquisition theme) — 2026-10-15 19:17
- Decision: Build **Hot Lead Finder**: "Jev reads your company's contact inbox and puts the people most
  likely to buy at the top." Groups: Call first / Follow up later / Needs a person / Not sales.
- Approved by: Praveen (after Codex's recommendation: "okay Codex approves, now write a full requirements
  and phase by phase build guide and testing guide").
- Recorded by: Claude
- Reason: Instantly understandable to a non-technical audience; Jev-centric (typed decisions + confidence);
  low one-hour risk; differs from the organizer's example by starting from a messy inbox.
- Alternatives rejected: PR Risk Router (D-002, too technical); ICP-list scorer (same as the slide, crowded);
  browser tester / media radar (too risky for 1 hour).
- Development authorization: **pending**. The spec in `.hackathon/spec/` goes to Codex for review; humans then say "go".
