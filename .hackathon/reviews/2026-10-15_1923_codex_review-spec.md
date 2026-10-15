# Review: Hot Lead Finder specification

Reviewer: Codex
Date: 2026-10-15 19:23 IST
Scope: Claude's 19:17 handoff; `.hackathon/spec/*.md`; related decisions, plan, task board and state
Requirements checked against: `THEME.md`, `DECISIONS.md` D-002 and D-003, official TypeSafe OpenAPI 0.2.0

## Summary

The concept and architecture are appropriate for the one-hour hackathon and keep Jev central. The
official API confirms the all-choice request/response contract. I fixed six concrete contract,
honesty and workflow issues; no blocker remains in the specification. Development still requires the
humans' explicit “go”, followed by the live Jev smoke gate.

## Findings

| # | Severity (high/med/low) | File:line | Finding | Verified? | Action |
|---|---|---|---|---|---|
| S-01 | high | BUILD_GUIDE.md:21 | Concurrent agents were told to write in one checkout, contradicting AGENTS.md and risking collisions. | verified | Require separate Codex-prepared worktrees or sequential work. |
| S-02 | high | BUILD_GUIDE.md:27 | `company_size` was typed as `int`, but fixture m8 requires an unknown value. | verified | Allow `int | None`; render missing values as `unknown`. |
| S-03 | medium | REQUIREMENTS.md:96 | “Manual reads avoided” was not defensible and excluded Call first items inconsistently. | verified | Replace it with “messages auto-sorted” = checked minus Needs a person. |
| S-04 | medium | BUILD_GUIDE.md:38 | Replay lacked a complete schema and batch wall time; the slider demo was nondeterministic. | verified | Define Replay metadata/batch time and require a recorded confidence in the demo range. |
| S-05 | medium | REQUIREMENTS.md:39 | Jev was used internally but not prominent enough for the explicit presentation requirement. | verified | Add Powered by Jev, live analysis state, Inbox → Jev → Decisions flow and AC8. |
| S-06 | medium | REQUIREMENTS.md:101 | Falling back from missing `confidence` could make a malformed API answer appear valid; official choice answers require the field. | verified | Validate the exact answer shape and return a per-card error on malformed data. |
| S-07 | low | PROJECT_PLAN.md:7 / CURRENT_STATE.md | Plan and state text still said the idea was unapproved after D-003. | verified | Reconcile plan, task board and state during closeout. |

## Fixes made (within authorized scope)

- Corrected the request/response, message, replay, usage and batch-timing contracts.
- Added deterministic replay acceptance, error-safe sorter behavior and synthetic-data privacy.
- Replaced the misleading ROI metric and made Jev visibly central to the UI/demo.
- Made the parallel workflow comply with AGENTS.md.
- Aligned unit, integration, secret and pre-demo checks with the corrected requirements.

## Verification results

- Official API schema reviewed at `https://api.typesafe.ai/openapi.json`: choice answers require
  `type`, `choice`, `confidence`, and `probabilities`; usage includes input/output tokens;
  output tokens are currently free.
- Documentation was reviewed directly; no application code exists and no Jev API call was made.
- Final whitespace, pointer and stale-text checks are recorded in the matching handoff.

## Remaining concerns

- The supplied key, account access, latency, rate limits and real answer quality are untested until T-010.
- The <5-second live batch target is a demo acceptance target, not yet measured.
- Development and any live API use remain pending the humans' explicit “go”.
