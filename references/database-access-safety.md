# Database Access Safety

Use this only when a task needs real database access: connecting to a database, running SQL, inspecting schema, exporting data, using a tunnel/VPN, or handling credentials. Static SQL review can use `sql-decomposition-checklist.md` first; real execution must pass this gate.

## Core Rule

Default to the safest path: read-only account, least privilege, small query, explicit plan, pre-execution audit, human confirmation for risk, and no secret disclosure. If scope, schema, row volume, business meaning, or credential handling is unclear, stop and ask.

## Token And Load Budget

- Do not load this file unless real database access is needed.
- Do not paste full schemas, large samples, or large result sets into chat.
- Inspect only relevant databases, tables, columns, and project-memory sections.
- Prefer local files for large outputs, then summarize counts, anomalies, caveats, and next steps.
- Keep the pre-execution audit short: purpose, objects, SQL type, limits, risks, confirmation needed.

## Allowed And Disallowed

Allowed by default:

- `SELECT 1`, targeted schema inspection, small samples with explicit columns and `LIMIT`.
- `EXPLAIN` or dry-run when supported and safe.
- Read-only `SELECT` with confirmed filters, time range, and row/column limits.

Disallowed unless the user explicitly approves and the account is still safe:

- Any DDL/DML: `INSERT`, `UPDATE`, `DELETE`, `DROP`, `ALTER`, `TRUNCATE`, `CREATE`.
- `SELECT *` on non-trivial tables.
- Large exports, full-table scans, unbounded joins, or unbounded `COUNT DISTINCT`.
- Printing passwords, DSNs, connection strings, environment variables, private keys, or secret-manager output.

## Query Gate

Before every SQL execution, including `SELECT 1`, schema inspection, samples, `EXPLAIN`, and final queries, show the exact SQL plus this compact audit:

```text
目的：
访问对象：
SQL 类型：
规模限制：
风险点：
需要用户确认：
```

No SQL may run silently. Execute only after the user explicitly confirms the shown SQL and audit, unless the user has already granted a scoped standing approval for that exact limited query sequence in the current turn.

After execution, report the exact SQL that ran, row count or result size, key result, and caveats. Do not return only data without the SQL. If the SQL is long, provide a local file path containing the SQL and include the key excerpt in chat.

## Safe Query Ladder

1. Confirm connection path without exposing secrets.
2. Show and get approval for `SELECT 1`.
3. Show and get approval before inspecting relevant schema.
4. Show and get approval before taking a small sample: explicit columns, time/partition filter if possible, `LIMIT 10-100`.
5. Confirm business meanings: status values, app/product ids, event names, time fields, dedupe grain, join keys.
6. Show and get approval before `EXPLAIN` or dry-run when available and safe.
7. Show and get approval before running the constrained query.
8. Reconcile row counts, nulls, duplicates, denominator, and obvious anomalies.
9. Return the executed SQL with the result summary.
10. Update project memory only with confirmed reusable facts.

## Do Not Guess

Never invent business values or semantics. Do not guess that `status = 1` means success, an app id means a product, an event name is correct, a time field is the right business timestamp, or a dedupe key is the right grain. Check data dictionary, project memory, schema/sample evidence, or ask the user.

## High-Risk Stop Conditions

Stop before execution when any of these are true:

- No time range or partition filter on a likely large table.
- Multi-large-table join with unconfirmed join keys or cardinality.
- Large `DISTINCT`, `COUNT DISTINCT`, window function, recursive query, or broad aggregation.
- Query depends on unconfirmed enum values, status meanings, or metric definitions.
- Table size or execution cost is unknown and cannot be cheaply estimated.
- Result may contain sensitive personal, payment, credential, or private communication data.

## Secret Handling

General:

- Prefer enterprise vault, OS credential store, local read-only proxy, or interactive password prompt.
- Never ask the user to paste passwords into chat.
- Never store secrets in skill files, project memory, SQL notes, data dictionaries, screenshots, or exported reports.
- Avoid command forms that place passwords in shell history or process arguments.
- Global command ban: do not run, recommend, document, or embed standalone commands that print secrets to stdout. In particular, `security find-generic-password ... -w` is forbidden for agent-run shells, examples, reusable scripts, and handoff docs.

macOS recommended paths:

- SSH tunnel plus MySQL client against `127.0.0.1:LOCAL_PORT`.
- macOS Keychain can store passwords, but raw password retrieval must not be exposed to the agent. Use a user-owned local proxy/helper that executes the query without returning the password, or fall back to an interactive prompt.
- 1Password CLI or enterprise vault if already configured.
- Interactive `mysql -p` is acceptable for one-off access.

Windows recommended paths:

- Windows Terminal or PowerShell with OpenSSH tunnel, then MySQL client against `127.0.0.1:LOCAL_PORT`.
- Prefer PowerShell SecretManagement/SecretStore or an enterprise vault for stored passwords.
- Use `Read-Host -AsSecureString` / vault UI for secret entry; do not type passwords into command arguments.
- Do not use `cmdkey /pass:...`, plaintext `.env`, or `MYSQL_PWD` unless the user explicitly accepts the risk and the secret is temporary.
- If a script retrieves a secret, keep it in process memory only, do not print it, and clear temporary variables/files after use.

Generic tunnel shape, with placeholders only:

```text
ssh -L LOCAL_PORT:REMOTE_MYSQL_HOST:3306 SSH_USER@SSH_HOST
mysql -h 127.0.0.1 -P LOCAL_PORT -u DB_USER -p DB_NAME
```

## Project Memory Updates

After safe execution, update only confirmed reusable knowledge:

- `_project_memory/02-data-dictionary.md`: table/field meaning, grain, time fields, enum values, join keys, null semantics.
- `_project_memory/03-metric-logic.md`: confirmed metric formula, denominator, filters, exclusions, reconciliation checks.
- `_project_memory/04-sql-notes.md`: reusable SQL purpose, base tables, row grain, filters, joins, dedupe logic, caveats, failure modes.
- `_project_memory/99-index.md`: add routes for major new tables, metrics, or SQL patterns.

Do not store hosts, ports, usernames, passwords, DSNs, private keys, sensitive sample values, or unconfirmed guesses in project memory.
