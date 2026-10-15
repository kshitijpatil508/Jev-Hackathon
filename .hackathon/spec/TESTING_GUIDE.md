# Hot Lead Finder: Testing Guide

Status: REVIEWED by Codex, 2026-10-15 19:19 IST. Record results (pass/fail + command output
summary) in your handoff. Never claim a test you didn't run.

## 1. Smoke test (Phase 0, gate)
`python scripts/smoke_test.py`
- [ ] HTTP 200; `answers.intent.type == "choice"`; `choice` ∈ options; `confidence` is in [0,1];
      `probabilities` sum ≈ 1; `usage.input_tokens` and `usage.output_tokens` are present.
- [ ] The smoke script formats output without headers and internally asserts that the full API key is
      absent before printing; never put even a key prefix into a shell command or report.
- [ ] Latency recorded. Record shape in THEME.md.

## 2. Unit tests (no network): `pytest -q`
`tests/test_sorter.py` (build Analysis dicts by hand with a helper
`make(intent, company_match, right_person, timing, confidence)`):
- [ ] buyer + yes + yes + now, all conf 0.9, t=0.6 → `call_first`
- [ ] buyer + no company match → `follow_up`
- [ ] buyer + timing later → `follow_up`
- [ ] intent `support` / `job_seeker` / `vendor_pitch` / `spam` (conf 0.9) → `not_sales`
- [ ] intent conf 0.5, t=0.6 → `needs_person`; same analysis, t=0.4 → normal group (slider behaviour)
- [ ] buyer with `timing` conf 0.5, t=0.6 → `needs_person`
- [ ] analysis with `error` set → `needs_person`
- [ ] analysis with `error` set → fit score 0 and all checks false
- [ ] `fit_score`: all-yes/now probabilities 1.0 → 100; all 0 → 0; result is an int in [0,100]
- [ ] `summarize`: counts add up; auto_sorted = checked - needs_person; batch wall time is preserved;
      cost = tokens × 0.042e-6

`tests/test_jev_client.py`:
- [ ] `build_questions()` has exactly keys intent, company_match, right_person, timing, each `type == "choice"` with the options in REQUIREMENTS §7
- [ ] `build_state()` contains the ICP fields and message body; missing company size renders as `unknown`
- [ ] `parse_response()` on the documented sample JSON returns a valid Analysis including answer
      `type`; missing/malformed `confidence` or probabilities returns an error Analysis
- [ ] HTTP error (mock `requests.post` raising / returning 500) → Analysis with `error` set, no exception

## 3. Live client check (Track A)
- [ ] Analyze m1 live, print the Analysis (no key), latency < 3 s.
- [ ] `analyze_all` on 8 fixtures returns 8 results in input order in < 5 s.

## 4. Integration / acceptance (Phase 2), live mode in the UI
| Check | Pass condition |
|---|---|
| AC1 | 8 fixtures sorted, no error cards, < 5 s |
| AC2 | m1 in Call first; m2 in Not sales |
| AC3 | m3, m4 (and m2) in Not sales |
| AC4 | In Replay mode, slider from 0.60 to 0.90 moves ≥ 1 card to Needs a person; no new network calls (batch time unchanged) |
| AC5 | Summary bar shows checked / buyers / auto-sorted / batch time / est. cost |
| AC8 | Header says Powered by Jev System One; live run visibly shows Jev is analysing and Inbox → Jev → Decisions |
| AC9 | UI asks for no inbox credentials; fixture/replay files contain synthetic messages only |
| Plausibility | m5, m6, m7 not in Call first; m8 in Needs a person or Follow up later |
If a plausibility check fails, improve the fixture wording or option descriptions and re-run. Don't hard-code results.

## 5. Pre-demo freeze checklist (Phase 3)
- [ ] Fresh terminal: `pip install -r requirements.txt && streamlit run src/app.py` works
- [ ] No "STUB" data anywhere (`rg -n "STUB" src` → no matches)
- [ ] Replay mode: rename `.env` temporarily (or disconnect network) → app still sorts, banner visible
- [ ] Replay schema contains recorded_at / model / ICP / batch_wall_ms / analyses; fixture IDs match
- [ ] Wrong key → friendly error on cards, app still usable, Replay suggested
- [ ] Key not in UI, logs, terminal output or Git. Verify `git check-ignore -q .env` succeeds and
      `git ls-files --error-unmatch .env` fails; never paste any key substring into a command.
- [ ] Browser zoom ≥ 110% so the audience can read cards; group names visible without scrolling
- [ ] Demo script rehearsed once end to end within 8 minutes

## 6. Known risks and fallbacks
| Risk | Fallback |
|---|---|
| Jev access/key fails | Stop, tell humans; ask organizers; build continues on stubs; demo may need replay recorded by another team member's key |
| Native yes/no type differs | Not used: all questions are `choice` |
| Jev groups a fixture oddly | Reword fixture/option descriptions; keep honesty: never hard-code |
| Rate limiting on parallel calls | `max_workers=2`, or sequential |
| Network down on stage | Replay mode, announced as replay |
