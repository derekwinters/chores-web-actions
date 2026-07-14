---
name: implementation-test
description: Test state for implemented changes — this repo has no test suite, so this is an explicit documented skip.
---

# Implementation Test Skill

This repo (`chores-web-actions`) is a shared-CI repo — composite actions and reusable workflows only, with **no test suite and no application code**. There is nothing to run here.

## Usage

```
/implementation-test
```

## Workflow

1. **Confirm no test suite exists**: this repo has never had a test framework — there is no `test`, `npm test`, or equivalent target.
2. **Record an explicit skip**: emit `test: no test suite in this repo — skip` into the run log.
3. **Never silently omit**: the skip is always reported, never dropped. The verification that the change is sound happens in the `implementation-verify` step (actionlint / YAML-parse), not here.

## Output

- ⏭️ Test state skipped (documented)
  - Reason: no test suite in this repo
  - Recorded in run log: `test: no test suite in this repo — skip`
  - Next: proceed to build-verify (`/implementation-verify`)

## Notes

- Called by orchestrator after the implement-then-verify loop
- This state is a **documented skip**, not a silent no-op — it is always recorded
- Actual soundness checking of changes is done by `implementation-verify`
