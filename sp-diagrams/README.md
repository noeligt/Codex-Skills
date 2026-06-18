# SP Diagrams

A Codex skill for analyzing SQL stored procedures and generating detailed technical diagrams.

## What it does

- Finds stored procedures in SQL-oriented files.
- Analyzes procedure inputs, outputs, table dependencies, branches, loops, transactions, dynamic SQL, and errors.
- Creates a concise diagram specification before image generation.
- Uses `$imagegen` for polished raster diagrams when the user asks for an image or infographic.

## Install

Copy this `sp-diagrams` folder into your Codex skills directory:

```powershell
Copy-Item -Recurse .\sp-diagrams "$env:USERPROFILE\.codex\skills\sp-diagrams"
```

Restart Codex, then use:

```text
$sp-diagrams
```

## Example prompt

```text
Use $sp-diagrams to analyze dbo.usp_ProcessOrders and generate a detailed image diagram.
```

## Files

- `SKILL.md`: main skill instructions.
- `references/procedure-analysis.md`: dialect cues, analysis checklist, diagram patterns, and output rules.
- `agents/openai.yaml`: display metadata for the OpenAI/Codex interface.
