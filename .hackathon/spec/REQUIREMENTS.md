# Hot Lead Finder: Requirements

Status: REVIEWED by Codex, 2026-10-15 19:19 IST. Idea approved by Praveen (D-003).
Development starts only after the humans give the go-ahead.

## 1. One-sentence product
> **Jev reads your company's contact inbox and puts the people most likely to buy at the top.**

Every presentation line and UI label must pass this test: a non-technical viewer understands it without
hearing "ICP", "SDR", "routing" or "threshold". In the UI, use "ideal customer" and "how sure Jev must be".

## 2. Hackathon constraints (from THEME.md)
- Must be centered on TypeSafe **Jev System One** (classifies and scores; does **not** write text).
- 1 hour total (target: 45 min build + 10 min freeze/rehearse). 8-minute live presentation.
- Judged on ROI, Presentation, Category fit (Lead Acquisition), Scalability, Overall.

## 3. Users and story
A small B2B software company's sales lead opens Hot Lead Finder, sees the week's inbox messages, clicks
**Find buyers**, and immediately knows whom to call first, whom to follow up with later, what isn't
sales at all, and which few messages a person should read.

## 4. Functional requirements
| ID | Requirement | Priority |
|---|---|---|
| FR1 | Load 8 sample inbox messages from `fixtures/messages.json` and show them as an "Inbox" list before sorting | Must |
| FR2 | Sidebar **Ideal customer** card with 3 editable text fields: company type, company size, buyer role (defaults in §6) | Must |
| FR3 | **Find buyers** button sends every message + the ideal customer to Jev (one request per message, all questions in that request) | Must |
| FR4 | Jev answers 4 typed questions per message (§7): what the sender wants, company match, right person, timing | Must |
| FR5 | Code (not Jev) places each message in exactly one group: **Call first**, **Follow up later**, **Needs a person**, **Not sales** (rules in §8) | Must |
| FR6 | Each buyer card shows name, title, company, subject, a 0–100 **fit score** bar and three plain checks: ✓/✗ *Right kind of company*, *Right person*, *Ready to buy* | Must |
| FR7 | Slider **"How sure must Jev be?"** (0.30–0.95, default 0.60). Moving it re-groups instantly **without** new Jev calls | Must |
| FR8 | Summary bar: "*N messages checked · B buyers to call · A messages auto-sorted · Jev time X s · estimated cost $Y*" | Must |
| FR9 | Each card has a collapsed **"Jev details"** panel: every question, the chosen answer, probabilities, confidence, latency | Must |
| FR10 | **Replay mode** (sidebar toggle) uses recorded Jev responses from `fixtures/replay.json`, with a visible banner "REPLAY: recorded Jev responses from <time>". Live is the default | Must |
| FR11 | If a live call fails, show a friendly error on that card and suggest Replay mode; the app never crashes | Must |
| FR12 | "Try your own message" form (name, title, company, size, message) that sorts one new message live | Should |
| FR13 | Editing the ideal customer and re-clicking Find buyers re-scores live (in Replay mode, show "ideal-customer edits need Live mode") | Should |
| FR14 | Draft outreach email (needs a separate text LLM) | Won't (this hour) |
| FR15 | Keep Jev visibly central: header says **Powered by Jev System One**, a visible **Jev is analysing...** state appears during live calls, and the result view shows **Inbox → Jev → Decisions** before the groups | Must |

## 5. Non-functional requirements
- **NFR1 Secrets:** read `JEV_API_KEY` from `.env` via python-dotenv; never print, log, display or commit it.
- **NFR2 Speed:** sorting 8 messages live takes < 5 s (parallel requests, max 4 workers).
- **NFR3 Robustness:** request timeout 15 s, 1 retry; per-message failures are isolated.
- **NFR4 Simplicity:** Python 3.10+, Streamlit, requests, python-dotenv, pytest. No database or server beyond Streamlit.
- **NFR5 Honesty:** replay data is always labelled; never present replay as live.
- **NFR6 Runs from clean checkout:** `pip install -r requirements.txt` then `streamlit run src/app.py`.
- **NFR7 Demo privacy:** no Gmail/Outlook connection and no real customer inbox data. Fixtures are
  synthetic; the optional live form uses a synthetic or audience-provided example.

## 6. Default ideal customer (mirrors organizer slide theme3.png)
- Company type: **B2B software company**
- Company size: **50–250 employees**
- Buyer role: **CTO, VP of Engineering or Head of IT**

## 7. Jev usage (the core)
Endpoint `POST https://api.typesafe.ai/v1/systemone`, header `Authorization: Bearer $JEV_API_KEY`,
model `jev-latest`. **All questions use `type: "choice"`** for one uniform response shape; yes/no
questions are choices with options `yes` / `no`. TypeSafe's official OpenAPI schema confirms that a
choice answer contains `type`, `choice`, `confidence` and `probabilities`. Native `noul` and
`score` types exist but are unnecessary for this demo.

`state` (string) = ideal customer + message:
```
IDEAL CUSTOMER
Company type: {company_type}
Company size: {company_size}
Buyer role: {buyer_role}

INBOX MESSAGE
From: {name}, {title} at {company} ({company_size} employees, {industry})
Subject: {subject}
Message: {body}
```

Questions (keys are fixed; code depends on them):
| Key | Instructions | Options (criteria) |
|---|---|---|
| `intent` | What does the sender mainly want? | `buyer`: wants to buy, evaluate, get a demo or pricing for our product · `support`: existing customer needing help · `job_seeker`: wants a job or internship · `vendor_pitch`: trying to sell us something · `spam`: irrelevant or automated · `other`: none of these |
| `company_match` | Does the sender's company match the ideal customer's company type and size? | `yes`: matches both · `no`: does not match one or both |
| `right_person` | Can the sender make or strongly influence a purchase decision, matching the ideal customer's buyer role? | `yes` · `no` |
| `timing` | How soon might the sender buy? | `now`: within about a month · `later`: exploring or a future quarter · `none`: no buying intent |

## 8. Grouping rules (pure code, `src/sorter.py`); `t` = slider value
1. If `intent.confidence < t` → **Needs a person**
2. Else if `intent.choice != "buyer"` → **Not sales**
3. Else if any of `company_match`, `right_person`, `timing` has confidence `< t` → **Needs a person**
4. Else if `company_match == yes` and `right_person == yes` and `timing == now` → **Call first**
5. Else → **Follow up later**

Checks shown on cards: company = `company_match == yes`, person = `right_person == yes`, ready = `timing == now`.
`fit_score = round(100 * (P(company_match=yes) + P(right_person=yes) + P(timing=now) + 0.5*P(timing=later)) / 3)`, capped at 100.
Groups are sorted by `fit_score` descending.
For an error Analysis, `group_for` returns Needs a person, `fit_score` returns 0 and all three
checks return false so one failed card cannot crash the page.
“Messages auto-sorted” = total messages minus count(Needs a person). This is an automation metric, not
a claim that salespeople never read buyer messages. “Jev time” is measured wall-clock time around the
whole parallel batch, not the sum of per-message latencies.
Cost = total input tokens × $0.042 / 1,000,000; TypeSafe's current OpenAPI schema says output tokens
are free. Label the UI value “est.” because pricing may change.
`confidence` is required for choice answers by the official schema; malformed/missing fields become
a per-card error rather than silently fabricating a valid answer.

## 9. Out of scope
Email/LinkedIn integration, scraping, CRM sync, authentication, persistence, outreach writing, multi-user.

## 10. Acceptance criteria (demo must show all)
- [ ] AC1 The 8 fixtures sort live in < 5 s with no errors.
- [ ] AC2 The CTO "evaluating vendors this month" message lands in **Call first**; the intern from the same company lands in **Not sales**.
- [ ] AC3 The support, SEO-pitch and job messages land in **Not sales**.
- [ ] AC4 In Replay mode, moving the slider moves at least one card between groups without new Jev calls; the recorded replay must include a confidence in the demonstrated range.
- [ ] AC5 Summary bar shows checked / buyers / auto-sorted / batch wall time / estimated cost.
- [ ] AC6 Replay mode works with the network off / no key and is clearly labelled.
- [ ] AC7 `pytest` passes; key never appears in output, UI or Git.
- [ ] AC8 Jev is prominent in the header, live processing state, Inbox → Jev → Decisions flow and result details.
- [ ] AC9 The demo requests no inbox credentials and contains no real customer messages or personal data.
