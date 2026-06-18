# Stored Procedure Analysis Reference

## Dialect Cues

- T-SQL: `@parameter`, `CREATE OR ALTER PROC`, `BEGIN TRY`, `BEGIN CATCH`, `THROW`, `RAISERROR`, `@@ROWCOUNT`, `#temp`, `MERGE`, `EXEC`, `sp_executesql`, `SET XACT_ABORT`, `GO`.
- Oracle PL/SQL: `CREATE OR REPLACE PROCEDURE`, `PACKAGE BODY`, `IN OUT`, `SYS_REFCURSOR`, `%TYPE`, `%ROWTYPE`, `EXCEPTION WHEN`, `RAISE_APPLICATION_ERROR`, `BULK COLLECT`, `FORALL`.
- PostgreSQL PL/pgSQL: `LANGUAGE plpgsql`, `RETURNS`, `$$`, `RAISE EXCEPTION`, `PERFORM`, `RETURN QUERY`, `FOR ... LOOP`.
- MySQL/MariaDB: `DELIMITER`, `CREATE PROCEDURE`, `IN/OUT/INOUT`, `DECLARE ... HANDLER`, `LEAVE`, `SIGNAL`, `RESIGNAL`.
- DB2 SQL PL: `LANGUAGE SQL`, `BEGIN ATOMIC`, `DECLARE CONTINUE HANDLER`, `SIGNAL SQLSTATE`, `RESULT SETS`.

## Extraction Checklist

Capture only what the source supports:

- Header: procedure name, schema/package, dialect, purpose inferred from comments and operations.
- Inputs: parameters, defaults, direction, null handling, validation rules.
- Outputs: result sets, output parameters, return codes, status rows, logging tables, raised errors.
- Data reads: source tables/views, joins, filters, lookup tables, cursors, CTEs.
- Data writes: inserts, updates, deletes, merges, truncates, audit/log rows, queue/messages.
- Temporary state: temp tables, table variables, collections, scratch tables, staging records.
- Control flow: phases, conditionals, early exits, loops, cursors, retries.
- Transactions: explicit begin/commit/rollback, savepoints, autonomous transactions, lock hints, isolation changes.
- Error handling: catch/exception blocks, handlers, rethrows, custom errors, compensation writes.
- Dynamic SQL: constructed statements, parameters/bind variables, executed commands, injection-sensitive concatenation.
- Routine calls: nested procedures, functions, jobs, packages, external services, file operations.
- Side effects: permissions changes, sequence usage, emails/messages, scheduler jobs, config mutation.
- Unknowns: referenced objects not present locally, hidden triggers, runtime tables, environment-specific behavior.

## Diagram Patterns

Choose the smallest set that explains the procedure:

- Main flow: left-to-right numbered phases from invocation to outputs.
- Data lineage: inputs and source tables on the left, temp/staging objects in the middle, target tables and result sets on the right.
- Branch map: decision diamonds for validation, mode flags, error paths, and optional writes.
- Transaction rail: vertical side rail showing begin, savepoint, commit, rollback, and exception handling.
- Call graph: target procedure in the center with nested procedure/function calls grouped by phase.
- Loop detail: collapse repetitive cursor/loop behavior into one loop block with entry condition, repeated work, and exit condition.

## Output Location

Save the final generated diagram in the same directory as the stored procedure source file by default.

- If the procedure was found in a project file, use that file's parent directory as the output directory.
- If the procedure is inside a package body or multi-procedure SQL file, still use the parent directory of the file containing the target definition.
- If the user supplied a code snippet with no source file, use the current workspace directory unless the user provides a destination.
- If the user explicitly names a different destination, follow the user's destination.
- Use a descriptive filename such as `<procedure-name>-diagram.png`, lowercased and sanitized for the filesystem when needed.
- Do not overwrite an existing diagram unless the user explicitly asks; create a sibling version such as `<procedure-name>-diagram-v2.png`.
- Report the absolute saved path in the final response.

## Accuracy Guardrails

- Do not infer table semantics from names unless comments or code support it.
- Do not claim row counts, performance behavior, trigger effects, or foreign-key cascades unless visible in source or provided by the user.
- Represent dynamic SQL separately and mark unknown target tables when they are not statically knowable.
- Keep exact identifiers in the written legend when the image would become too dense.
- Prefer "appears to", "likely", or "unknown" for inferred business intent.

## Prompt Ingredients

For image generation, convert the analysis into:

- Title: `<schema>.<procedure>` plus dialect.
- Subtitle: one-sentence purpose from source-supported evidence.
- Swimlanes: Inputs, Validation, Read/Data Prep, Branch/Transform, Write/Side Effects, Outputs.
- Nodes: 6 to 14 high-signal blocks. Merge trivial assignments and obvious boilerplate.
- Data stores: use table-shaped nodes; color reads and writes differently.
- Decisions: use diamonds with short condition labels.
- Errors: use red/orange path into catch/exception/rollback/output error.
- Legend: read, write, temp, decision, call, transaction, error.
