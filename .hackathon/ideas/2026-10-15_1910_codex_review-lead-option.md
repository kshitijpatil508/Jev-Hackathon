# Review of Claude's Lead Option: Codex

Created: 2026-10-15 19:10 IST · Phase 2 (awaiting human selection)

## Verdict

Choose **Lead Acquisition**, not Agentic SDLC. Claude's “Inbox to Pipeline” is the right basic idea,
but its name and UI labels should be simplified further.

Recommended public name: **Hot Lead Finder**

One-sentence explanation:

> Jev reads your company's contact inbox and puts the people most likely to buy at the top.

That sentence is the test. A viewer should understand the product before hearing “ICP”, “SDR”,
“routing”, “confidence threshold” or any other industry term.

This is a recommendation, not final development approval.

## Human-friendly product

The screen begins with six to ten familiar messages:

- “We need an AI vendor this month. Can we book a demo?”
- “Are you hiring interns?”
- “Our account is locked.”
- “I want to sell you SEO services.”
- “We are researching options for next year.”

Click **Find buyers**. The messages move into three obvious groups:

1. **Call first** — likely buyers ready soon.
2. **Follow up later** — possible buyers who are still exploring.
3. **Not sales** — support, jobs, vendors and spam.

Each real buyer shows three plain-language checks:

- Right kind of company
- Right person
- Ready to buy

Uncertain messages go to **Needs a person**. Jev's probability and confidence can remain visible in a
details panel, but they should not be the first thing the audience has to understand.

## Jev fit

This remains genuinely Jev-centered. For each message, Jev makes typed decisions:

- what the person wants;
- whether the company matches;
- whether the sender can make a buying decision;
- how soon they may buy;
- whether a human should check the result.

Ordinary code converts those probabilities into the three visible groups. Jev does not need to write
an email or generate an explanation.

## Why it differs from the organizer's example

The slide starts with a clean list of known leads and ranks them. Hot Lead Finder starts one step
earlier with a messy real inbox where most messages are not sales leads at all. It first finds the
buyers, then ranks them. That adds a concrete time-saving story without leaving the Lead Acquisition
theme.

## Demo and scope recommendation

- Use eight carefully written fixture messages; no LinkedIn scraping, email login or OpenAI.
- Use an editable **Ideal customer** card with three fields: company type, company size, buyer role.
- Keep the dramatic pair from Claude's proposal: a CTO and an intern from the same company.
- Show a simple result such as “8 messages checked · 2 buyers found · 6 manual reads avoided”.
- Keep a labelled replay response so the demo survives a network problem.
- Do not add outreach drafting unless the complete core demo is already frozen.

## Feasibility

This is simpler than PR Risk Router and easily fits a 45-minute implementation after the Jev smoke
test. It needs one small Jev client, one fixture file and one Streamlit screen. The main risk is using
sales jargon in the presentation, not implementation complexity.

## Final comparison

| Option | Instantly understandable | Jev is central | One-hour risk | Recommendation |
|---|---:|---:|---:|---|
| Hot Lead Finder | High | High | Low | **Choose this** |
| PR Risk Router | Low outside engineering | High | Low | Reject for this audience |
