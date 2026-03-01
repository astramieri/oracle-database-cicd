# 04 - Project Verify

The verify step validates the integrity of the staged work before promoting it to a release. According to the official SQLcl documentation, `project release` should only be run once `project verify` has successfully passed.

> **Important:** `project verify` is blocked on protected branches (e.g. `main`). It must be run from a feature branch, after `project stage`.

---

## Verify Stage

Runs all verification tests against the staged changelogs, comparing the current branch with the reference branch:

```
SQL> project verify verify-stage -comparedBranchName main
```

## Verify Snapshot

Runs all verification tests against the project snapshots:

```
SQL> project verify snapshot -comparedBranchName main
```

### Useful flags

| Flag | Description |
|------|-------------|
| `-comparedBranchName` | Branch to compare against. If omitted, uses the default branch from config |
| `-verbose` | Displays additional details during the process |
| `-debug` | Displays debug information in the output |

---

## Workflow Position

```
project export  →  project stage  →  project verify  →  project release  →  project gen-artifact
```

Only proceed to `project release` once `project verify` completes without errors.
