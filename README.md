# Database CI/CD

**Automated & Isolated Database Testing:** Before deploying to production, test database changes in an isolated clone of the production environment.

**Version Control for Database Code:** Store database schema, scripts, and configurations in a version control system (e.g., Git).

**Automated Database Migrations:** Use SQLcl with Liquibase to apply changes incrementally and consistently and implement idempotent scripts to avoid duplication issues.

**Automated Validation & Rollforward Strategy:** Design idempotent migration scripts that can be reapplied without negative impact and prefer forward fixes—applying corrective scripts for errors rather than reverting changes.

## SQLcl + Liquibase

SQLcl uses Liquibase internally to capture, version, and apply database changes to Oracle Database.

**SQLcl overview:**
- Capture, version, and deploy database changes
- Embedded in SQLcl: free command line interface tool for Oracle Database
- Execute SQL & PL/SQL; supports all SQL*Plus commands

**Liquibase in SQLcl — Supercharged:**
- Contains all the great features of open-source Liquibase with much more extended functionality
- Exclusively for the Oracle Database:
  - Capturing the entire Oracle Database schema
  - Individual objects
  - APEX objects
  - Oracle REST Data Service (ORDS) objects

## SQLcl Projects

Introducing SQLcl Projects for Database Developers:

- Defines a prescribed process for database developers
- Defines a recommended file structure
- Defines tools to automate developer daily tasks
- Generates releasable artifacts based on atomic developer changes
- Flexible to work with your existing projects

## SQLcl Projects Workflow Tools

0. **Connect to the Database** — Launch SQLcl and connect to your Oracle Database

   ```
   # Thin JDBC (no Oracle Client required)
   sql user/password@//host:1521/service

   # Using a Cloud Wallet (ATP/ADW)
   sql /nolog

   SQL> set cloudconfig /path/to/wallet.zip
   SQL> connect user/password@service_tp
   ```

1. **Project Creation** — Set up the project structure and configuration (for new and existing projects)

   ```
   # Initialize a new project
   SQL> PROJECT init -name <name> -makeroot -schemas <schema>

   # Initialize git repository
   SQL> !git init --initial-branch=main
   SQL> !git add .
   SQL> !git commit -m "chore: initializing <name> git repository"
   ```
2. **Database Export** — Export source from database files (control with filters in config)

   ```
   SQL> PROJECT export
   ```
3. **Stage Changes** — Create the actual changes to be applied; control with filters in config

   ```
   SQL> PROJECT stage -verbose
   ```

   Output:
   - Starting execution of stage command using the current branch
   - Created dir: `dist/releases/next/changes`
   - Created dir: `dist/releases/next/code`
4. **Artifact Generation** — Generate a releasable set of objects and an artifact for this set of installable objects

   ```
   SQL> PROJECT release --version 1.0
   SQL> PROJECT gen-artifact
   ```
5. **Release Deployment** — Deploy the artifact from the artifact store to target environments

   ```
   SQL> PROJECT deploy <filename.zip>
   ```

   - **Dev** — Create objects from the artifact
   - **Prod** — Install artifact into the production database
