# github-issue-triage-orchestrator

Main orchestrator for the GitHub issue triage workflow in `chores-web-actions`. Implements a minimal state machine that reviews an issue for completeness, applies the `ready-to-grill` label, and runs a lightweight pre-grilling sanity check.

This is the **minimal triage suite** for a single-purpose shared-CI repo. It references only the two ported triage skills — `github-issue-review` and `github-issue-plan`. The heavier triage skills from the canon were deliberately not ported here; milestone assignment is an epic prerequisite handled outside this flow.

## Usage

```
/triage-issue <issue-number>
```

## State Machine (3 states)

1. **review** (auto) - Run `github-issue-review`: check completeness, fill obvious gaps, apply `ready-to-grill` label. Skip if already `ready-to-grill`/`ready-for-work`.
2. **plan-check** (auto) - Run `github-issue-plan`: lightweight pre-grilling read, flag remaining ambiguities across the 4 impact areas. Adds no labels, posts no comments.
3. **complete** - Report triage result and recommend `/grill-with-docs issue <N>`.

## Automatic vs User-Involved

**Fully Automatic**: review, plan-check

**User Involved**: only if the review skill needs a judgement call on a major rewrite

## State Tracking

State persisted in a status report on the issue (or in agent context):
```
Current State: plan-check
Progress: 2/3
History: review ✓ → plan-check
```

## Integration

- Orchestrates only the ported skills: `github-issue-review`, `github-issue-plan`
- Invoked: manually (`/triage-issue <number>`) or webhook
- Returns: reviewed issue with `ready-to-grill` label and a lightweight plan sanity-check
- Next step in the chain: `/grill-with-docs issue <N>` → `ready-for-work`
