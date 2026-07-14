---
name: commit
description: Verify repo YAML (actionlint / YAML-parse) and create a conventional commit with proper type and scope
---

# Commit Skill

Verifies the repo's YAML is well-formed (this repo has no test suite), then creates a Conventional Commit with proper type/scope.

## Usage

```
/commit
```

## Flow

1. Verify YAML if any workflow/action YAML changed:
   - If `actionlint` is available: `actionlint .github/workflows/*.yml` and YAML-parse `actions/*/action.yml`
   - Else: YAML-parse each changed `.github/workflows/*.yml` and `actions/*/action.yml`
     (`python -c "import yaml,sys; yaml.safe_load(open(sys.argv[1]))"`)
   - If neither is possible: record an explicit skip — never a silent pass
   - There is no test suite to run in this repo
2. Stop if verification fails — report the malformed file
3. Review staged/unstaged changes
4. Derive commit type and scope from changes
5. Create commit using Conventional Commits format

## Commit Format

```
<type>(<scope>): <short description>

[optional body explaining why]
```

## Types

- `feat` - new action/workflow or new capability
- `fix` - bug fix
- `refactor` - restructuring without behavior change
- `test` - test additions/changes (n/a in this repo — no test suite)
- `docs` - documentation
- `chore` - build/deps/tooling/process
- `ci` - CI/CD changes
- `perf` - performance
- `build` - build system

## Scopes

- `actions` - composite actions under `actions/*`
- `workflows` - reusable workflows under `.github/workflows/*`
- `agents` - `.claude/agents/*`
- `skills` - `.claude/skills/*`
- `docs` - README and other documentation
- `ci` - CI configuration
- Use most relevant scope

## Rules

- Subject line ≤72 characters, lowercase, no period
- Imperative mood: "add" not "added"
- Body only when "why" is non-obvious
- Stage changes before invoking
