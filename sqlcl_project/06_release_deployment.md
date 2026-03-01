# 06 - Release Deployment

The deploy step extracts the artifact generated in the previous step and applies it to a target database. It executes `@dist/install.sql` and initiates the Liquibase installer against the connected database.

This command is run while connected to the **target database** (e.g. UAT, production) — not the development database used during export and stage.

---

## Deploy

```
SQL> project deploy -file <path-to-artifact>
```

Example:

```
SQL> project deploy -file /Users/user1/Desktop/project1/artifact/R1-1.zip -verbose
```

### Useful flags

| Flag | Description |
|------|-------------|
| `-file` | **(Required)** Path to the artifact file (`.zip` or `.tgz`) to deploy |
| `-log-path` | Directory where log files will be written |
| `-verbose` | Displays additional details during the process |
| `-debug` | Displays debug information in the output |

---

## Full End-to-End Workflow

| Step | Command |
|------|---------|
| 1. Initialize project | `project init` |
| 2. Export database objects | `project export` |
| 3. Stage changes | `project stage -branch-name main` |
| 4. Verify | `project verify verify-stage -comparedBranchName main` |
| 5. Release | `project release -version <version>` |
| 6. Generate artifact | `project gen-artifact -name <name> -version <version> -format zip` |
| 7. Deploy | `project deploy -file <path-to-artifact>` |
