# Codex Skills

Personal Codex skills shared from this repository.

## Available skills

- [`sp-diagrams`](sp-diagrams/): Analyze SQL stored procedures and generate detailed technical diagrams.

## Install a skill

Clone or download this repository, then copy the skill folder into your Codex skills directory:

```powershell
Copy-Item -Recurse .\sp-diagrams "$env:USERPROFILE\.codex\skills\sp-diagrams"
```

Restart Codex, then invoke the skill with:

```text
$sp-diagrams
```

## Notes

`sp-diagrams` uses the built-in `$imagegen` skill when a raster diagram or infographic is requested. It does not connect to databases or inspect production data unless the user explicitly asks and approves.
