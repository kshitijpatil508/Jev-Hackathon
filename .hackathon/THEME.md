# THEME — Official Hackathon Brief

**Status:** ANNOUNCED. Recorded by Claude, 2026-10-15 18:48 IST, from `theme/` (slides theme1-3.png,
rules.png, grading.png and hackathon_transcript.txt, an auto-transcript, so some names are garbled).

## Event
TrueHorizon AI, Inaugural Hackathon. Teams pick **one of three themes** and build for that use case.

## Themes (pick ONE)
1. **Agentic SDLC.** "Using agents across iterative software development: sprints, code management,
   reviews, CI/CD, integration testing, and user feedback to improve software quality."
   Suggested ideas: (01) browser-based automated integration testing; (02) feature complexity ranking to
   route implementation and CI/CD work (e.g. "can an agent just do this colour change, or does it need
   security review?").
2. **Lead Acquisition.** "Identifying and qualifying prospective customers for a business's services
   against an ideal customer profile (ICP)."
   Suggested ideas: (01) score LinkedIn-sourced leads against an ICP; (02) enrich and prioritize
   qualified leads, then draft relevant outreach.
3. **Media Signals.** "Useful insights derived from social platforms, news, and other changing web
   resources that reveal trends or emerging signals."
   Suggested ideas: (01) test whether historical Twitter/X signals precede stock price increases;
   (02) identify growing technology demand among AI and software buyers.

## Confirmed rules
| # | Requirement | Source |
|---|---|---|
| R1 | Solution **MUST be centered around TypeSafe AI's Jev System One model** | rules.png |
| R2 | **One hour** to build (timer starts at kickoff; breakout rooms) | rules.png, transcript |
| R3 | At the hour mark, each team has **8 minutes to present**; order as on the teams slide | rules.png, transcript |
| R4 | Another LLM may be used inside the app, but the app must be Jev-based. OpenAI API key available on request | transcript (last line) |

## Judging: peer ranked-choice voting, 5 questions (grading.png)
1. **ROI:** biggest ROI with respect to time and money.
2. **Presentation:** demo smoothness, clarity, organization, engagement.
3. **Category fit:** how well it fits the chosen theme / nails the use case.
4. **Scalability:** cost of maintenance, adding users/data, smooth path to production.
5. **Deserving winner:** overall (can't vote for your own team).

## Mandatory technology: Jev (facts from public docs, verify with a smoke test)
- Jev is a **"System One" model**: it does **not generate text**. Given a `state` (string) and typed
  `questions`, it returns decisions with calibrated probabilities and a confidence value.
- Question types: **yes/no** (named `noul` in docs), **choice** (pick from options you define),
  **score** (ordered levels from 0).
- Endpoint: `POST https://api.typesafe.ai/v1/systemone`, `Authorization: Bearer $JEV_API_KEY`,
  body `{"state": "...", "model": "jev-latest", "questions": {"<name>": {"type": "choice",
  "instructions": "...", "criteria": {"opt": "description", ...}}}}`.
  Response: `answers.<name>.{choice, probabilities, confidence}`, plus `usage`.
- SDKs: Python `typesafe-sdk>=0.5.7`, JS/TS `TypeSafeClient.systemOne`. Context window is 32k tokens. No streaming.
- Vendor claims: around 100x faster/cheaper than LLMs on classification tasks; input about $0.042 per million tokens.
- **Unverified:** exact JSON field names for yes/no and score questions. Check https://docs.typesafe.ai/api.

## Assumptions (NOT confirmed)
- `JEV_API_KEY` in `.env` is a TypeSafe key that works against the endpoint above.
- Presenting means a live screen-share demo; no written submission was mentioned.

## Open questions for the humans
- Which theme? (Recommendation in `.hackathon/ideas/2026-10-15_1848_claude_theme-ideas.md`.)
- Do we want the OpenAI key (only needed if the app must write text, e.g. outreach drafts or summaries)?
- What time does the 1-hour clock end, and where are we in the presentation order?
