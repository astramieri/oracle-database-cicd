# 05 - Artifact Generation

Artifact generation is the final packaging step. It consists of two commands that work in sequence:

1. **`project release`** — promotes the staged changelogs into a versioned release folder.
2. **`project gen-artifact`** — packages the release into a deployable archive (zip or tgz).

---

## 1. Release

Moves the current staged work from `dist/releases/next/` into a versioned release folder under `dist/releases/`, labelling it with the specified version.

```
SQL> project release -version <version>
```

Example:

```
SQL> project release -version 1.0
```

### Useful flags

| Flag | Description |
|------|-------------|
| `-version` | **(Required)** Version label for this release |
| `-verbose` | Displays additional details during the process |
| `-debug` | Displays debug information in the output |

---

## 2. Generate Artifact

Packages the current state of the project into a deployable archive file. The artifact is saved in the `artifact/` folder (created automatically if it does not exist).

```
SQL> project gen-artifact -name <name> -version <version> -format <zip|tgz>
```

Example:

```
SQL> project gen-artifact -name R1 -version 1 -format zip -verbose
```

### Useful flags

| Flag | Description |
|------|-------------|
| `-name` | Custom name for the artifact |
| `-version` | Version label for the artifact |
| `-format` | Compression format: `zip` or `tgz` |
| `-force` | Overwrites the artifact if one with the same name already exists |
| `-verbose` | Displays additional details during the process |
| `-debug` | Displays debug information in the output |

---

## Typical Sequence

```
SQL> project release -version 1.0
SQL> project gen-artifact -name R1 -version 1 -format zip
```

Run `project release` first to lock in the versioned changelog, then `project gen-artifact` to produce the deployable archive.
