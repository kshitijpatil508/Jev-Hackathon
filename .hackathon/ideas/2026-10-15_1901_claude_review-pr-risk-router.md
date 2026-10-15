# Review of Codex's PR Risk Router: Claude

Created: 2026-10-15 19:01 IST · Phase 2 (awaiting human selection)

**Verdict: agree. PR Risk Router is the better version of Triage Desk.** It's buildable in 45 minutes
if the cut list below holds.

## Where Codex is right
- **File paths + diff summary as input.** This is a real fix: title/body alone can't justify "security review".
- **Fixtures over live GitHub on the critical path**, plus a paste-one-PR form for the live moment.
- **Choice-only first**, other question types after the smoke test.
- **Auto lane / Human lane with a visible threshold slider.** This is the demo's best moment.

## Three adjustments
1. **`risk_flags` is multi-label, but Jev `choice` picks exactly one.** Model it as one yes/no question per
   flag (auth, payments, PII, secrets, infra). If the yes/no shape fails the smoke test, use a `choice`
   of {yes, no} per flag. Any flag set → Human lane, whatever the confidence.
2. **Make the fixtures real-looking, not invented.** Copy 10–12 titles, paths and diffs from real public PRs
   (e.g. a well-known OSS repo) into `fixtures/prs.json`, including 2 near-identical pairs where one touches
   auth/payments. Real PRs answer the "staged demo" objection.
3. **Keep ROI on screen without the GPT comparison.** Show total latency, tokens, $ cost (from `usage`)
   and "senior reviews avoided: N of M". This costs only a few lines of code. Agree with dropping Jev-vs-GPT.

## 45-minute plan (3 tracks after the smoke test)
| Min | Track 1 | Track 2 | Track 3 |
|---|---|---|---|
| 0–5 | Smoke test (`choice` + yes/no), record shapes in THEME.md | — | — |
| 5–25 | `src/jev_client.py`: `route_pr(pr) -> dict` | `fixtures/prs.json` (10–12 PRs) + replay file | `src/app.py` Streamlit: lanes, cards, slider, stub data |
| 25–35 | Integrate, run on all fixtures, save labelled replay | | |
| 35–45 | Polish: ROI counters, paste form; freeze & rehearse | | |

**Cut first if late:** paste form → replay file → ROI counters. **Never cut:** lanes + threshold + probabilities.
**Only true blocker:** the API key/endpoint working. Smoke-test before anything else.
