# PROJECT PLAN

> **Hot Lead Finder (D-003).** The full plan lives in [`spec/`](spec/):
> [REQUIREMENTS](spec/REQUIREMENTS.md) · [BUILD_GUIDE](spec/BUILD_GUIDE.md) · [TESTING_GUIDE](spec/TESTING_GUIDE.md).
> The template below is kept for reference; the spec files take precedence.

**Status:** Hot Lead Finder approved in D-003; the linked spec was reviewed by Codex. Development is
waiting for the humans' explicit “go”. The reference template below is not an active second plan.

## 1. MVP (what the demo must show)
-

## 2. Architecture
- Components and responsibilities:
- How they communicate (interfaces / data contracts):
- Tech stack & dependencies:

## 3. Interfaces & file ownership (required before parallel work)
| Component | Owner (task id) | Files / dirs | Interface it exposes |
|---|---|---|---|
| | | | |

## 4. Phases
| Phase | Goal | Time box |
|---|---|---|
| 1 | Skeleton runs end to end | |
| 2 | Core feature(s) | |
| 3 | Integration + tests | |
| 4 | Demo polish + README + submission | |

## 5. Essential vs optional
- Essential:
- Optional / stretch:

## 6. Parallelization
- Can run in parallel:
- Must be sequential:
- Integration point / branch strategy:
- Shared-document writer (human-designated; only needed for parallel sessions):
- Separate checkout/worktree per concurrent code task; distinct agent/session labels for artifacts:

## 7. Testing & integration
- First external-service check (authentication + one minimal request, once usage is authorized):
- Acceptance check for each MVP task (link to TASK_BOARD Notes):
- End-to-end demo command/steps and expected result:
- Fallback if an integration fails (label fixtures/mocks explicitly):

## 8. Definition of done
- [ ] Meets every confirmed requirement in `THEME.md`
- [ ] Runs from a clean checkout following `README.md`
- [ ] Demo path works end to end
- [ ] No secrets committed
- [ ] Handoffs, reviews and `CURRENT_STATE.md` up to date
