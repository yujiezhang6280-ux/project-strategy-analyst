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

## Small-Step Query Strategy

Default to small-step execution instead of large all-in-one SQL. Use the database to answer one bounded question at a time, then assemble the analysis from the small results in chat, a notebook, or a local file. Do not write a large SQL query just because the database connection is available.

- Start with metadata, table role, time field, row grain, candidate filters, and cheap counts.
- Then run small scoped queries: explicit columns, fixed time windows, relevant tables only, and visible `LIMIT` where sampling.
- Prefer 2-5 readable SQL steps over one opaque query with many CTEs, unbounded joins, or hidden business logic.
- Do not split so much that the workflow becomes noisy or loses a consistent denominator. If multiple small queries must be combined, keep the same filters, time range, grain, and join keys visible.
- Use a larger combined SQL only when it is clearly safer or more accurate than stitching results: confirmed filters, confirmed join keys, bounded time range, checked row volume, and readable logic.
- If consistency across multiple queries matters because the source tables are changing, state the snapshot risk and either keep the query small but single-pass, or ask before using a larger query.

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
查询策略：
规模限制：
风险点：
需要用户确认：
```

No SQL may run invisibly: the user must see the SQL before execution. Low-risk SQL may run after showing the SQL and audit. High-risk, unclear, expensive, broad, sensitive, or business-semantics-dependent SQL must wait for explicit user confirmation.

After execution, report the exact SQL that ran, row count or result size, key result, and caveats. Do not return only data without the SQL. If the SQL is long, provide a local file path containing the SQL and include the key excerpt in chat.

Keep SQL readable for humans. Prefer direct joins, clear aliases, and staged small queries over deeply nested CTE stacks. Use CTEs only when they materially improve clarity; avoid many CTEs that hide business logic or make review hard. It is acceptable to do associations/joins, but join keys, grain, filters, and cardinality risk must be visible in the audit. When a task could be solved either by one large SQL or by several bounded queries, choose the bounded-query path unless there is a concrete accuracy or consistency reason not to.

## Automatic Tunnel SOP

When a local tunnel helper exists, use it before any real database work:

```bash
${CODEX_DB_TUNNEL_HELPER:-$HOME/.codex/local/db-tunnel/ensure-codex-readonly-tunnel.sh}
```

Rules:

- The helper may check `127.0.0.1:LOCAL_PORT` and restart the SSH tunnel when it is down.
- Prefer `CODEX_DB_TUNNEL_HELPER` when it is set. Otherwise look under `$HOME/.codex/local/db-tunnel/` for a project-specific helper.
- The helper must not print remote hosts, usernames, private key paths, passwords, DSNs, or secret-manager output.
- The helper must not run business SQL. If it runs any SQL health check, it must be limited to `SELECT 1` and the surrounding chat must still follow the Query Gate.
- Real connection metadata belongs only in a private local config such as `$HOME/.codex/local/db-tunnel/*.env` with `600` permissions. Do not commit it, paste it into chat, or write it into project memory.
- If the helper reports that a non-ssh process owns the local port, stop and ask instead of killing it.

## Result Presentation

User-facing result tables should be readable business tables, not raw database dumps.

- For database query deliverables with tabular results, the primary output must be an Excel `.xlsx` workbook unless the user explicitly requests another format. Chat should contain only the concise summary, caveats, executed SQL reference, and workbook path.
- Use Chinese business column names in tables shown to the user. Keep original SQL field names visible in the executed SQL, or provide a compact field mapping when the translation is new or non-obvious.
- Do not invent translations for unclear fields. If a field meaning is uncertain, label it as `中文暂定名（raw_field_name）` and call out the uncertainty.
- Order columns for reading: entity/time dimensions first, then key metrics, then rates or derived fields, then caveats or notes.
- Format values for scanning: dates as readable dates, money with currency when known, percentages with sensible decimals, counts as integers, nulls as `空` or `未记录` only when that meaning is confirmed.
- Keep chat tables compact. For anything beyond a tiny preview, create a local Excel workbook with frozen or clear headers, sensible column widths, wrapped text, number formats, filters when useful, and a short summary in chat.
- Recommended workbook sheets: `摘要`, one sheet per result table, `SQL与审计`, `字段映射`, and `口径与风险`. Do not include secrets, hosts, private key paths, passwords, or raw sensitive samples in any sheet.
- Include totals, subtotals, sorting, or highlighting only when they clarify the business answer; do not decorate tables in a way that hides caveats, filters, or denominators.

## Safe Query Ladder

1. Run the local tunnel helper if present, or otherwise confirm connection path without exposing secrets.
2. Show `SELECT 1` and audit before running; explicit approval is not required when risk is clearly low.
3. Show schema-inspection SQL and audit before running; explicit approval is not required when scoped to relevant objects.
4. Identify row grain, time field, likely partition/filter columns, and relevant join keys before writing analysis SQL.
5. Run cheap bounded counts or `MIN`/`MAX` checks where useful to estimate scale and validate filters.
6. Show sample SQL and audit before running: explicit columns, time/partition filter if possible, `LIMIT 10-100`.
7. Confirm business meanings: status values, app/product ids, event names, time fields, dedupe grain, join keys.
8. Build the answer from a small number of scoped SQL steps. After each step, inspect the result and decide the next SQL; do not jump straight to a final large query.
9. Use `EXPLAIN` or dry-run before any query that joins large tables, aggregates broad ranges, uses `DISTINCT`, or might scan a large table.
10. Show final constrained SQL and audit. Run automatically only if low-risk; otherwise wait for explicit confirmation.
11. Reconcile row counts, nulls, duplicates, denominator, and obvious anomalies across the small results.
12. Return the executed SQL list with the result summary and explain how the pieces were combined.
13. Deliver tabular results as an Excel `.xlsx` workbook with Chinese column names, readable formatting, and original-field traceability.
14. Update project memory only with confirmed reusable facts.

## Do Not Guess

Never invent business values or semantics. Do not guess that `status = 1` means success, an app id means a product, an event name is correct, a time field is the right business timestamp, or a dedupe key is the right grain. Check data dictionary, project memory, schema/sample evidence, or ask the user.

## High-Risk Stop Conditions

Stop before execution when any of these are true:

- No time range or partition filter on a likely large table.
- Multi-large-table join with unconfirmed join keys or cardinality.
- Large `DISTINCT`, `COUNT DISTINCT`, window function, recursive query, or broad aggregation.
- One SQL attempts to answer too many business questions at once, especially with many CTEs, broad joins, or hidden filters.
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
