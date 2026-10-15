# Lead Acquisition option: Claude

Created: 2026-10-15 19:06 IST · Requested by Praveen: something a non-engineer understands instantly.

## What the organizer slide shows (theme3.png, "ICP Matching")
Left: an **Ideal customer** card (B2B software · 50–250 employees · CTO/VP Engineering · active AI project).
Right: leads ranked with a score bar (Northstar Labs 94, Cedar Cloud 87, Harbor Retail 36), steps
Define ICP → Compare → Rank → Shortlist, and the note "**Keep the match reasons with the lead.**"
Slide ideas: (01) score LinkedIn-sourced leads against an ICP; (02) enrich/prioritize, then draft outreach.

## Recommendation: "Inbox to Pipeline", an inbound lead sorter
Every company's "Contact us / Book a demo" inbox is full of mixed messages: real buyers, students,
job seekers, vendors pitching, support requests, spam. Sales reps read them all by hand.

For each inbound message plus sender info (name, title, company, size, message), **one Jev call** answers:
- `intent` (choice): buyer · support · job-seeker · vendor-pitch · partner · spam
- ICP fit, one question per ICP criterion, matching the slide's card: company size fits? decision-maker role?
  target industry? active buying signal? (yes/no each, with probability)
- `urgency` (choice): now · this quarter · just exploring
- `route` (choice): account executive · SDR follow-up · nurture email · support desk · discard

The app turns the ICP answers into a **0–100 fit score** (a weighted sum of the probabilities) and shows the
**matched criteria as ticks next to each lead**. These are the "match reasons", and Jev provides them
without writing any prose.

**Screen:** ICP card on the left (editable); on the right, **Hot leads** (ranked, like the slide) ·
**Needs a human** (Jev unsure) · **Not a lead** (routed to support/discard). A confidence slider moves cards live.

**One-line pitch:** "Your sales team reads 200 inbound messages a week. Jev sorts them in 2 seconds for
under a cent, and puts the 5 real buyers on top with the reasons."

## Why this over the slide's own example
- Same ICP-matching core (category fit), plus a real filter step, which is exactly where reps waste time.
- Inbound messages are easy to make realistic and need no LinkedIn scraping.
- Pure classification/scoring, so it's fully Jev-centric. Outreach drafting stays an optional stretch (OpenAI key).
- Demo trick: two senders from the same company. A CTO writing "we're evaluating vendors this month" goes
  to Hot; an intern asking for a summer internship goes to Not a lead.

## Buildable in 45 minutes: same 3-track split as PR Risk Router
Jev client (`src/jev_client.py`) · 12–15 sample inbound messages (`fixtures/leads.json`) · Streamlit UI (`src/app.py`).
Smoke-test Jev first.
