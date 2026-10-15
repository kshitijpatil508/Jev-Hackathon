# Theme Ideas: Claude

Created: 2026-10-15 18:48 IST · Phase 2 (theme announced, no idea selected) · Source: `.hackathon/THEME.md`

## The fact that shapes every idea
Jev **classifies, routes and scores; it does not write**. The ideas that score well are ones where
**many fast, cheap, calibrated decisions** are the product. The **confidence** value is a feature:
act automatically when Jev is confident, escalate to a human when it isn't. That answers ROI and
Scalability on the rubric directly. Ideas that need prose (outreach emails, summaries) push the core
work onto a second LLM and weaken the "centered on Jev" rule.

Constraints: **1 hour build, 8-minute demo**, peer-voted on ROI / Presentation / Category fit /
Scalability / Overall.

---

### Idea A: "Triage Desk", an SDLC work router  (Theme 1, slide idea 02) ⭐ RECOMMENDED
- **What it does:** paste or load a batch of tickets, issues or PRs (live from a public GitHub repo, or a
  fixture JSON). For each one, Jev answers in **one call** with several questions:
  - `complexity` (score 0-3: trivial / small / medium / large)
  - `route` (choice: `auto-agent` · `junior-dev` · `senior-dev` · `security-review`)
  - `touches_sensitive` (yes/no: auth, payments, PII, infra)
  - `type` (choice: bug / feature / chore / docs)

  The UI shows a sorted board with probabilities bars. **Low-confidence items are flagged "needs human".**
- **SDLC problem:** backlog grooming and work assignment eat senior engineers' time, and risky changes
  slip through without review.
- **MVP:** load about 20 issues, then classify and show a table/board with route, complexity and confidence.
  Add a counter for latency and cost per decision.
- **Stretch:** live GitHub repo URL input; "Jev vs GPT" side-by-side timing/cost on the same batch (needs the
  OpenAI key; strong ROI slide); tweak the routing criteria in the UI to show adapting to a team's rules.
- **Tech:** Python + Streamlit (one file UI) + `requests` to Jev; GitHub REST API (no auth for public repos,
  60 requests/hour) or a fixture JSON.
- **Complexity:** **S-M**. Fits in 1 hour with room to polish.
- **Demo value:** high and very legible. "100 issues triaged in X seconds for $0.000Y; these 7 were risky."
- **Rubric fit:** ROI ✔✔ (senior time saved, near-zero cost) · Category ✔✔ (literally slide idea 02) ·
  Scalability ✔✔ (stateless HTTP calls, batchable, cheap) · Presentation ✔ (clear before/after).
- **Risks:** unverified yes/no and score field names (mitigation: smoke test first; `choice` alone can do
  everything if needed). GitHub rate limits (mitigation: fixture file as fallback).

### Idea B: Jev-driven browser integration tester  (Theme 1, slide idea 01)
- **What it does:** Playwright opens a small sample web app. Each step, the visible clickable elements
  become a Jev `choice` ("which element advances the goal: checkout?") and `noul` checks verify assertions
  ("is the cart total $24?"). Jev picks the action and Playwright executes it.
- **SDLC problem:** integration tests are brittle and slow to write; this uses goal-based testing instead.
- **Tech:** Python + Playwright (Chromium available) + a tiny local sample shop page we'd also have to build.
- **Complexity:** **L for 1 hour.** Needs DOM-to-state serialization, a loop and a sample app.
- **Demo value:** highest wow factor when it works; a dead demo if it gets stuck.
- **Risks:** flakiness, time. Two teams might copy the slide's own example. **Only pick this if one person
  already has Playwright experience.**

### Idea C: CI failure triage bot  (Theme 1)
- **What it does:** feed failing CI logs. Jev classifies the root cause (`flaky` / `test-bug` / `code-bug` /
  `infra` / `dependency`), decides `retry?` (yes/no) and routes to an owner team.
- **Complexity:** S. **Weakness:** needs realistic logs (fixtures), and is less visual than A.
  Best used as **a second tab inside Idea A** if time allows ("Triage Desk: issues + CI failures").

### Idea D: ICP lead scorer  (Theme 2)
- **What it does:** CSV of leads plus an ICP form. Jev scores each lead on fit dimensions (size, role,
  industry, buying signal) and the app combines them into a ranked shortlist with reasons per dimension.
- **Complexity:** S. **Demo value:** medium. **Risks:** the slide shows exactly this, so it's likely
  crowded. "Draft outreach" needs a text LLM (not Jev). Lead data is fake, which undercuts the ROI story.

### Idea E: Tech-demand signal radar  (Theme 3, slide idea 02)
- **What it does:** pull recent Hacker News stories/comments (Algolia API, no key). Jev classifies each
  into need-topics (evals / retrieval / cost / agents / security…) plus "is this a buyer pain point?"
  (yes/no). The app shows counts and trends by week, with source snippets.
- **Complexity:** M. **Risks:** data fetching and volume in 1 hour; trends need a time series to look
  convincing. **Twitter/X → stock** (slide idea 01) is **not feasible** in 1 hour (no X API access,
  historical price data, causal claims).

---

## Ranking & recommendation
1. **A (Triage Desk)** has the best ratio of rubric score to risk. It fits Jev's strength exactly,
   tells an ROI story in one sentence, and can be built in about 35 minutes, leaving time to polish the demo.
2. **E**, if the team wants a non-SDLC theme with a more "insightful" feel.
3. **D** is safe but likely crowded and has a weaker ROI story.
4. **B** is high wow, high risk. Not recommended in 1 hour unless someone has done Playwright agents before.

## Suggested build plan for A (if chosen), by minute
| Min | Work | Parallel? |
|---|---|---|
| 0-5 | Smoke-test Jev with one `choice` call; confirm yes/no and score field names | blocks everything |
| 5-30 | **Track 1:** `src/jev_client.py` (`classify(item) -> dict`) · **Track 2:** `src/data.py` (GitHub fetch + `fixtures/issues.json`) · **Track 3:** `src/app.py` Streamlit UI against a stubbed `classify` | yes, 3 files, 3 owners |
| 30-40 | Integrate; run on 20-50 real issues | sequential |
| 40-50 | Stretch: cost/latency counter, confidence threshold slider, "needs human" lane | optional |
| 50-60 | Freeze; rehearse the 8-minute demo; commit | — |

Interface contract: `classify(item: {"id","title","body","labels"}) -> {"complexity": int, "route": str,
"sensitive": bool, "type": str, "confidence": float, "probabilities": dict, "latency_ms": float}`.
