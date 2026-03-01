# 02 - Database Export

The export step captures the current state of the database objects into the project directory as versioned files. Unlike project creation, this command is run **repeatedly** — every time you want to sync the repository with the current state of the database.

---

## Simple Export

```
SQL> project export
```

Exports all objects from the schemas configured during `project init`. This is the baseline command and requires no additional parameters.

To target specific schemas explicitly:

```
SQL> project export -schemas <schema1>,<schema2>
```

### Useful flags

| Flag | Description |
|------|-------------|
| `-schemas` | Comma-separated list of schemas to export |
| `-objects` | Comma-separated list of specific objects to export (e.g. `HR.EMPLOYEES`) or a specific APEX app (e.g. `APEX.100`) |
| `-threads` | Number of concurrent threads for the export (default: 5) |
| `-verbose` | Displays additional details during the process |
| `-debug` | Displays debug information, including how filters are applied |
| `-list` | Lists all exportable objects without actually exporting |

### Simple Export (Dry Run)

```
SQL> project export -list
```

Lists every object that would be exported — based on the configured schemas and any active filters — without writing any files to disk. Useful to verify the export scope before running the actual export, or to confirm that filters are excluding the right objects.

---

## Filtered Export

For a more precise export, SQLcl supports **filter files**. A filter file contains SQL `WHERE`-style conditions that are applied to the data dictionary queries used during export, allowing you to exclude unwanted object types, names, or grants.

### Filter file location

```
.dbtools/filters/project.filters
```

This file is created inside your project directory and should be committed to Git along with the rest of the project configuration.

### Excluding privileges

To exclude system and object privileges from the export, add the following conditions to the filter file:

```
export_type not in ('ALL_TAB_PRIVS','USER_SYS_PRIVS')
```

### Other common filters

Exclude specific object types:

```
object_type not in ('MLE_MODULE', 'MLE_ENVIRONMENT')
```

Exclude objects matching a name pattern:

```
object_name not like 'DEV_ONLY_%'
```

Exclude grants from a specific schema:

```
grantor != 'SOME_SCHEMA'
```

### Running the filtered export

No additional flag is needed — SQLcl automatically reads the filter file from `.dbtools/filters/project.filters` when it exists:

```
SQL> project export
```

Use `-debug` to verify that your filters are being applied correctly:

```
SQL> project export -debug
```

---

## Exporting a Specific APEX Application

To export a single APEX application, use the `APEX.<app_id>` syntax with the `-objects` flag and specify the schema that owns the APEX workspace:

```
SQL> project export -objects APEX.<app_id> -schemas <schema>
```

Example:

```
SQL> project export -objects APEX.100 -schemas APEXUSER
```
