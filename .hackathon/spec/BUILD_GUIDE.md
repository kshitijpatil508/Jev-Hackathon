# Hot Lead Finder: Phase-by-Phase Build Guide

Status: REVIEWED by Codex, 2026-10-15 19:19 IST. Read `REQUIREMENTS.md` first. Its §7 and §8
are the contract every track builds against. Times are minutes from the human "go".

## File map and ownership
| File | Track | Purpose |
|---|---|---|
| `requirements.txt` | A | `streamlit`, `requests`, `python-dotenv`, `pytest` |
| `scripts/smoke_test.py` | A | One live Jev call; prints response shape, latency, usage (never the key) |
| `src/jev_client.py` | A | Build state + questions, call Jev, parse to `Analysis` dicts; `analyze_all()` in parallel |
| `scripts/record_replay.py` | A | Runs all fixtures live and writes `fixtures/replay.json` with a timestamp |
| `fixtures/messages.json` | B | The 8 sample messages (§ Fixtures) |
| `src/sorter.py` | B | Pure functions: `group_for`, `fit_score`, `checks`, `summarize` |
| `tests/test_sorter.py`, `tests/test_jev_client.py` | B | Unit tests (no network) |
| `src/app.py` | C | Streamlit UI; uses a stub until Track A lands |
| `src/__init__.py`, `tests/__init__.py` | A | Empty, created first |

**Shared docs** (TASK_BOARD, CURRENT_STATE): Track A's agent is the single writer during the parallel
phase. Tracks B and C put their notes in their own handoff only.
Parallel tracks are optional. If humans assign them concurrently, Codex prepares separate worktrees
from the same base; otherwise the tracks run sequentially in the shared checkout. Never run concurrent
writers in one checkout. **No Git commands except by Codex on human instruction.**

## Data contracts (copy exactly)
```python
Message = {"id": "m1", "name": str, "title": str, "company": str, "company_size": int | None,
           "industry": str, "subject": str, "body": str}
ICP     = {"company_type": str, "company_size": str, "buyer_role": str}
Answer  = {"type": "choice", "choice": str, "probabilities": {option: float}, "confidence": float}
Analysis = {
  "id": "m1",
  "model": str,
  "answers": {"intent": Answer, "company_match": Answer, "right_person": Answer, "timing": Answer},
  "latency_ms": float, "input_tokens": int, "output_tokens": int, "source": "live" | "replay",
  "error": None | str          # if set, answers may be empty and the card shows the error
}
Replay = {
  "recorded_at": str,       # ISO 8601 UTC timestamp
  "model": str,
  "icp": ICP,
  "batch_wall_ms": float,
  "analyses": list[Analysis]
}
```
Public functions:
- `jev_client.build_state(msg: Message, icp: ICP) -> str`
- `jev_client.build_questions() -> dict` (REQUIREMENTS §7, keys fixed)
- `jev_client.parse_response(msg_id: str, resp_json: dict, latency_ms: float) -> Analysis`
- `jev_client.analyze(msg, icp) -> Analysis` and `jev_client.analyze_all(msgs, icp, max_workers=4) -> list[Analysis]`
- `sorter.group_for(a: Analysis, t: float) -> "call_first"|"follow_up"|"needs_person"|"not_sales"` (errors → `needs_person`)
- `sorter.checks(a) -> {"company": bool, "person": bool, "ready": bool}`
- `sorter.fit_score(a) -> int`
- `sorter.summarize(analyses, t, batch_wall_ms) -> {"checked", "call_first", "auto_sorted", "jev_seconds", "est_cost_usd"}`

`build_state` renders missing metadata as the literal word `unknown`; it never serializes Python
`None`. The app measures `batch_wall_ms` with `time.perf_counter()` around `analyze_all`.
For an error Analysis, `group_for` returns `needs_person`, `fit_score` returns 0 and `checks`
returns three false values.

---

## Phase 0: Smoke test (0–5 min) · one agent (Track A) · GATE
1. `pip install -r requirements.txt`.
2. `scripts/smoke_test.py`: load `.env`, send one message with the `intent` question only, then one with all
   4 questions. Print status code, `answers` keys, one answer's fields, `usage`, latency.
3. Record the real response shape in `.hackathon/THEME.md` (section "Smoke test result").
- **Gate passed:** HTTP 200 and `answers.intent.choice` present → start Phase 1.
- **Gate failed:** stop and tell the humans (key/endpoint/early access issue). Tracks B and C may still
  proceed against stubs, but the demo then depends on fixing access.

## Phase 1: Parallel build (5–25 min)
### Track A: Jev client
- Implement the functions above. Headers: `Authorization: Bearer <key>`, `Content-Type: application/json`.
- `timeout=15`, one retry on timeout/5xx; on failure return Analysis with `error` set (never raise to UI).
- `analyze_all` uses `concurrent.futures.ThreadPoolExecutor(max_workers=4)`; preserve input order.
- Validate each choice answer's `type`, `choice`, `confidence` and `probabilities`. A malformed
  response returns an Analysis with `error` set rather than a fabricated fallback answer.
- Done when: `python -c "from src.jev_client import *; ..."` analyzes fixture m1 live and prints a valid Analysis.

### Track B: Fixtures + sorter + tests
- Write `fixtures/messages.json` (§ Fixtures) and `src/sorter.py` exactly per REQUIREMENTS §8.
- Write `tests/test_sorter.py` (see TESTING_GUIDE §2) and `tests/test_jev_client.py` (parse + state/questions, no network).
- Done when: `pytest -q` passes.

### Track C: Streamlit UI
- `src/app.py` with `st.set_page_config(page_title="Hot Lead Finder", layout="wide")`.
- Header: **Hot Lead Finder — Powered by Jev System One** + the one-sentence product line.
- Sidebar: Ideal customer card (3 `text_input`s), slider "How sure must Jev be?", radio Live / Replay.
- Before sorting: "Inbox" list (name · title · company · subject). Button **Find buyers**.
- On click: show **Jev is analysing...** and the visible flow **Inbox → Jev System One → Decisions**.
- After: summary bar (`st.metric` x5), then 4 columns: Call first · Follow up later · Needs a person · Not sales.
  Card = name/title/company, subject, fit score `st.progress`, three ✓/✗ checks, `st.expander("Jev details")`
  with the raw answers. Replay banner via `st.warning`.
- Store analyses in `st.session_state["analyses"]` so the slider regroups without new calls.
- Until Track A lands: `stub_analyses()` returns plausible fake Analysis dicts **labelled "STUB"** (delete before demo).
- Done when: the full layout renders with stub data and the slider regroups.

## Phase 2: Integrate (25–35 min) · one agent
1. Replace the stub with `jev_client.analyze_all` (Live) and a `fixtures/replay.json` loader (Replay).
   Validate the Replay contract, fixture IDs, `recorded_at` and `batch_wall_ms`; reject mismatches
   with a friendly error.
   On load, set each Analysis `source` to `replay` while retaining the recorded model and usage.
2. Run live on all 8 fixtures; check AC1–AC4 (TESTING_GUIDE §4).
3. If a fixture lands in an implausible group, **adjust the fixture wording or option descriptions**, not the
   grouping code. Re-run.
4. `python scripts/record_replay.py` → `fixtures/replay.json`; confirm Replay mode works.
5. Ensure the replay includes at least one relevant confidence between 0.60 and 0.90 so the slider
   demonstration is deterministic.
6. Ask the humans to have Codex commit ("integrated MVP").

## Phase 3: Polish + freeze (35–45 min)
In order, stop when time's up: FR12 own-message form → FR13 ICP re-score → visual polish (group colours,
emoji headers 🔥 / 🕓 / 👀 / 🚫). At **45 min: feature freeze**. Run the full TESTING_GUIDE §5 checklist,
then have Codex commit and push ("demo freeze").

## Phase 4: Rehearse (45–55 min): 8-minute demo script
| Time | Beat |
|---|---|
| 0:00 | Problem: "Sales teams read every Contact-us message by hand. Most aren't buyers." |
| 0:45 | Show the inbox: 8 familiar messages. Show the ideal customer card. |
| 1:30 | Click **Find buyers**. Point to **Jev is analysing...** and Inbox → Jev → Decisions; groups appear. |
| 2:30 | CTO vs intern from the same company: Call first vs Not sales. Open "Jev details" once. |
| 3:30 | Move the slider: cards move to "Needs a person". "Jev tells us when it isn't sure." |
| 4:15 | (If FR13) change company size to "1–20 employees", re-run, see the ranking change. |
| 5:00 | (If FR12) type a new message live from the audience. |
| 5:45 | ROI: messages auto-sorted × triage time; cost per message. Scalability: one cheap call per message. |
| 6:45 | How Jev is used: 4 typed questions → probabilities → plain code rules. No text generation. |
| 7:30 | Close with the one-sentence product line. |
Backup: if live fails on stage, switch to Replay and say so.

## Fixtures (`fixtures/messages.json`): intended outcomes, not guarantees
| id | Sender | Gist | Expected |
|---|---|---|---|
| m1 | Priya Shah, CTO, Northwind Analytics (120, B2B software) | "Evaluating AI vendors this month, can we book a demo next week?" | Call first |
| m2 | Leo Park, Intern, Northwind Analytics (120, B2B software) | "Are you hiring summer interns?" | Not sales |
| m3 | Dana Cole, Ops Manager, Brightpath Logistics (300, logistics) | "Our account is locked, can't log in" | Not sales |
| m4 | Raj Mehta, Founder, RankRocket SEO (6, marketing agency) | "We can get you to #1 on Google" | Not sales |
| m5 | Marcus Lee, VP of Engineering, Cedar Cloud (85, B2B software) | "Researching options for next year's budget" | Follow up later |
| m6 | Emma Ruiz, Marketing Coordinator, Northstar Labs (150, B2B software) | "My boss asked me to get pricing" | Follow up later |
| m7 | Tom Baker, Owner, Harbor Bakery (8, retail) | "Want a demo ASAP" | Follow up later (wrong company) |
| m8 | "Sam", no title, gmail.com (`company_size: null`, unknown industry) | "hi interested pls call" | Needs a person (likely) |
