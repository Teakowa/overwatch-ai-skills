# overwatch-ai-skills

A curated collection of reusable AI skills for Overwatch Workshop and OverPy workflows.

This repository follows the [Agent Skills Specification](https://agentskills.io/specification), with each skill centered on a canonical `SKILL.md`.

## Available Skills

See [skills/README.md](skills/README.md) for the current catalog.

Current published skills:

- `$ow-workshop-loops`: design and review safe loop logic (`Rule Loop`, `While`, `For`) with server-load safeguards.
- `$ow-workshop-server-stability`: audit and remediate startup crashes, load spikes, and high-frequency event pressure in Workshop rules.

## How To Use

Reference the skill name directly in prompts:

```text
Use $ow-workshop-loops to review this rule for waitless loop risk and suggest safer loop patterns.
```

## Model Compatibility

- Codex: consume the skill via `SKILL.md`; optional UI metadata lives in `agents/openai.yaml`.
- Claude Code: consume the same `SKILL.md`; non-standard metadata files can be ignored safely.
- Any Agent Skills-compatible runtime: read frontmatter + markdown body from `SKILL.md`.

## Repository Layout

```text
overwatch-ai-skills/
  skills/
    <skill-name>/
      SKILL.md
      agents/openai.yaml
      references/
      scripts/        # optional
      assets/         # optional
```
