# chores-web-actions

Shared GitHub Actions and reusable workflows for the chores-web repos.

All consumers must reference this repo by full commit SHA (SHA pinning is
enforced account-wide):

```yaml
# Composite actions
- uses: derekwinters/chores-web-actions/actions/setup-python@<sha>
  with:
    requirements-path: requirements.txt

- uses: derekwinters/chores-web-actions/actions/setup-node@<sha>
  with:
    lockfile-path: package-lock.json

# Reusable workflows (job-level `uses`)
jobs:
  docker:
    uses: derekwinters/chores-web-actions/.github/workflows/docker-build-push.yml@<sha>
    with:
      context: .
      image-name: chores-web-backend
      push: true
      version: 2.0.1

  release:
    uses: derekwinters/chores-web-actions/.github/workflows/release-please.yml@<sha>
```

## Contents

| Path | Type | Purpose |
|---|---|---|
| `actions/setup-python` | composite action | Python + pip cache keyed to a requirements file |
| `actions/setup-node` | composite action | Node + npm cache keyed to a lockfile |
| `.github/workflows/docker-build-push.yml` | reusable workflow | GHCR login, buildx, build and optionally push `ghcr.io/<owner>/<image-name>` |
| `.github/workflows/release-please.yml` | reusable workflow | release-please with config/manifest under `.github/release-please/` |

Third-party actions used inside these workflows are SHA-pinned. Bump pins
here once; consumers pick them up by bumping their pin of this repo.
