---
name: implementation-verify
description: Verify the repo's YAML with actionlint (or a YAML-parse fallback) and show a changes summary for user review.
---

# Implementation Verify Skill

Verifies that the repo's GitHub Actions YAML is well-formed, then shows a summary of changes for user review. This repo has no build/compile step and no test suite — the verify check is `actionlint` over the workflows plus a YAML-parse of the composite actions, with a documented-skip fallback.

## Usage

```
/implementation-verify <issue-number>
```

## Workflow

1. **Verify YAML**:
   - **If `actionlint` is available** (`command -v actionlint`): run `actionlint` over `.github/workflows/*.yml`, and YAML-parse each `actions/*/action.yml` (actionlint targets workflow files, not composite `action.yml`, so parse those separately).
   - **Else (fallback)**: YAML-parse every `.github/workflows/*.yml` and `actions/*/action.yml`, e.g. `python -c "import yaml,sys; yaml.safe_load(open(sys.argv[1]))"` per file.
   - **Else (neither possible)**: record an **explicit documented skip** in the run log — never a silent pass.
2. **Report the verify path taken**: actionlint, YAML-parse fallback, or documented skip — so downstream steps (reflect/finalize) can record it in the PR's `## Deviations and Decisions`.
3. **Prepare changes summary**:
   - List all files modified (`git diff --stat`)
   - Show line change counts
   - Summarize the change
   - Note the test state is a documented skip (no test suite)
4. **Pause workflow**: Wait for user approval or request for changes

## Parameters

- `issue_number` (optional): For reference in output

## Output

Shows:
- Verify path taken (actionlint / YAML-parse fallback / documented skip) and its result
- Files modified with line counts
- Change summary
- Test state: documented skip (no test suite)
- Ready for user to:
  - Approve for commit
  - Request more changes
  - Abort

## Notes

- Called by orchestrator after the test-state skip
- The verify check confirms no malformed YAML was introduced into workflows/actions
- The verify state **never silently passes** — if neither actionlint nor a YAML parser is available, it records an explicit skip
- Shows all changes before user reviews; user has the control point here
