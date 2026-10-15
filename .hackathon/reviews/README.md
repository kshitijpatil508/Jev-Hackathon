# Reviews

One file per review: `YYYY-MM-DD_HHMM_<reviewer>_review-<what>.md`.
Read the current clock and convert to IST (UTC+05:30), using 24-hour time. Keep legacy numbered
reviews intact. If a name already exists, use a distinct reviewer/session label; never overwrite it.
Read the code, not just the handoff. Label each finding *verified* or *suspected*.

## Template
```markdown
# Review: <what was reviewed>

Reviewer:
Date:
Scope: <commits / files / handoff reviewed>
Requirements checked against: THEME.md, DECISIONS.md (<ids>)

## Summary
One paragraph verdict.

## Findings
| # | Severity (high/med/low) | File:line | Finding | Verified? | Action |
|---|---|---|---|---|---|

## Fixes made (within authorized scope)
-

## Verification results
Commands run and outcomes. State what was not tested.

## Remaining concerns
-
```
