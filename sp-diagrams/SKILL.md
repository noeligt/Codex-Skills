---
name: sp-diagrams
description: Analyze SQL stored procedures and generate detailed visual diagrams, especially when the user asks for an image, infographic, or explanation of a stored procedure from an IntelliJ project, SQL file, package, procedure name, or code snippet. Use for T-SQL, PL/SQL, PL/pgSQL, MySQL, DB2 SQL PL, and similar database procedure logic; identify inputs, outputs, table dependencies, branches, loops, transactions, dynamic SQL, errors, and call flow, then use imagegen for a polished raster diagram when requested.
---

# SP Diagrams

## Overview

Use this skill to explain and visualize a stored procedure from a project file, selected snippet, or procedure name. Produce an accurate technical analysis first, then use `$imagegen` for a polished raster diagram when the user asks for an image, visual, infographic, or "incredible detailed diagram."

## Workflow

1. Locate the procedure.
   - If the user provides a file path or code snippet, use it directly.
   - If the user provides only a procedure name, search the current workspace or IntelliJ project with `rg` across SQL-oriented files.
   - Ask only when the procedure cannot be found or several plausible definitions remain.

2. Read enough local context.
   - Read the full procedure body, including package body context for PL/SQL when relevant.
   - Search for locally defined procedures/functions called by the target when they materially affect the flow.
   - Do not connect to a database, use credentials, or inspect production data unless the user explicitly asks and approves.

3. Analyze the procedure using `references/procedure-analysis.md`.
   - Identify dialect, purpose, parameters, outputs, data stores, temp objects, branches, loops, transactions, error handling, dynamic SQL, called routines, and side effects.
   - Treat source code as the authority. Mark unknowns instead of inventing business meaning.

4. Build a concise diagram spec before generating the image.
   - Procedure name and dialect
   - Inputs, outputs, and return channels
   - Data reads, writes, temp tables, and external dependencies
   - Main execution phases in order
   - Decision points, loops, transaction boundaries, and error paths
   - Notable assumptions or unresolved references

5. Create an accuracy anchor.
   - Provide a Mermaid flowchart, sequence diagram, or compact textual outline when the logic is complex or the user will need exact labels.
   - Use this anchor to verify the image prompt, not as a replacement for the requested image.

6. Generate the raster diagram with `$imagegen` when requested.
   - Use the `infographic-diagram` use case.
   - Prompt from the diagram spec, not from raw SQL alone.
   - Keep in-image text short and structural; include exact SQL identifiers in the response or a companion legend because generated raster text can distort dense labels.
   - Prefer a clean technical architecture style: swimlanes, numbered phases, decision diamonds, data-store columns, transaction/error side rail, and color-coded read/write/control flows.

7. Deliver the result.
   - Include the image inline when preview-only.
   - Save or move the final diagram into the same directory as the stored procedure source file by default.
   - Use `references/procedure-analysis.md` for output path and filename rules.
   - Report the source file(s), assumptions, generated prompt summary, and final image path if saved.

## Search Patterns

Use `rg` first. Helpful patterns:

```powershell
rg -n --glob "*.sql" --glob "*.pls" --glob "*.pkb" --glob "*.pks" --glob "*.pkg" --glob "*.prc" "CREATE|ALTER|PROCEDURE|PROC|FUNCTION|PACKAGE BODY|<procedure_name>"
```

For case-insensitive procedure-name lookup:

```powershell
rg -n -i --glob "*.sql" --glob "*.pls" --glob "*.pkb" --glob "*.pks" --glob "*.pkg" --glob "*.prc" "<procedure_name>"
```

## Image Prompt Shape

Use this structure when calling `$imagegen`:

```text
Use case: infographic-diagram
Asset type: detailed stored procedure architecture diagram
Primary request: Create a polished technical diagram explaining stored procedure <name> based strictly on this supplied diagram spec.
Subject: <dialect> stored procedure execution flow, data dependencies, decisions, transactions, and outputs.
Style/medium: crisp database engineering infographic, professional architecture diagram, high readability.
Composition/framing: left-to-right main execution flow with swimlanes for inputs, validation, data reads, transformations/branches, writes, outputs; side rail for transaction and error handling.
Color palette: restrained high-contrast palette with distinct colors for reads, writes, decisions, errors, and external calls.
Text: short labels only; use exact names only for the most important procedures/tables.
Constraints: follow the supplied spec; do not add invented tables, services, business rules, or data paths.
Avoid: tiny unreadable labels, decorative filler, vague cloud shapes, random database icons, hallucinated identifiers.
```

## Reference Map

- `references/procedure-analysis.md`: stored-procedure dialect cues, analysis checklist, and diagram patterns.
