# AGENTS.md

## Project Context

This repository contains custom skills for the Kilo AI CLI. It is intended to grow into a general collection of AI skills for everyday software engineering, with the current starting set focused on Laravel workflows.

## Behavior

- Use the appropriate skill for the task rather than hardcoding one workflow for everything.
- Follow each skill's `SKILL.md` before implementing.
- Use the references bundled with each skill for technical details; do not invent or guess configuration.
- When discussing code, use clear absolute or relative paths, such as `skils/...`.
- Do not modify existing skill structure without explicit discussion.
- When a skill targets a specific framework or domain, follow its conventions; otherwise follow general software engineering best practices.

## Adding New Skills

- Place new skills under `skils/<skill-name>/SKILL.md`.
- Keep skill instructions and references self-contained.
- Update `README.md` and `kilo.json` when adding or renaming skills.

## File References

- `skils/stitch-laravel-converter/SKILL.md`
- `skils/stitch-laravel-functionalizer/SKILL.md`
- `kilo.json`
- `AGENTS.md`
- `CLAUDE.md`
- `README.md`
