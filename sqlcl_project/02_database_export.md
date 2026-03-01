# 03 - Stage Changes

The stage step generates **Liquibase changelogs and changesets** from the `src/` files (exported database objects) and any custom SQL files, then produces deployment-ready artifacts under `dist/releases/next/`.

> **Important:** `project stage` is blocked on protected branches (e.g. `main`). It must always be run from a feature branch. SQLcl enforces this and will return an error if attempted on a protected branch.

The baseline established by `project export` on `main` (the `src/` files) is the reference point. Changelogs are always generated as diffs between a feature branch and `main`.

---

## Stage

On a feature branch, `project stage` diffs the current branch against the reference branch and generates changelogs only for what changed:

```
SQL> project stage
```

The reference branch defaults to the value of `git.defaultBranch` in the project config (fallback: `develop`). To specify it explicitly:

```
SQL> project stage -branch-name <branch>
```

### Useful flags

| Flag | Description |
|------|-------------|
| `-branch-name` | Branch to use as the comparison base. Defaults to the value of `git.defaultBranch` in the config (fallback: `develop`) |
| `-verbose` | Displays additional details during the process |
| `-debug` | Displays debug information in the output |

---

## Feature Branch Workflow (main → feature1)

With only `main` and `feature1` branches, `main` is the reference. Since the default fallback is `develop` (which does not exist), you must always pass `-branch-name main` explicitly when staging from a feature branch.

### Step-by-step

**1. Switch to the feature branch**

```
SQL> !git checkout feature1
```

**2. Apply your database changes**

Make the desired changes directly in the database (new tables, modified procedures, etc.).

**3. Export the updated objects**

```
SQL> project export
```

This overwrites the relevant files in `src/` with the current state of the database.

**4. Stage the changes against main**

```
SQL> project stage -branch-name main
```

SQLcl diffs `feature1` against `main` and generates changelogs only for what changed, writing them to `dist/releases/next/feature1/`.

**5. Commit and push**

```
SQL> !git add .
SQL> !git commit -m "feat: <description>"
SQL> !git push
```

> **Note:** if you run `project stage` without `-branch-name main`, SQLcl will look for a `develop` branch which does not exist and the command will fail.

---

## Adding a Custom SQL File

Custom SQL files (e.g. data migrations, one-off scripts) can be included in the stage using the `add-custom` subcommand:

```
SQL> project stage add-custom -file-name <file-name>
```

Example:

```
SQL> project stage add-custom -file-name feature1.sql
```

The specified file is added to the staged changeset alongside the auto-generated object changelogs.
