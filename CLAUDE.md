# CLAUDE.md

## Project Overview

This repository stores custom skills for the Kilo AI CLI. It is meant to become a general collection of AI skills for everyday software engineering, with the initial set focused on Laravel workflows.

## How To Use These Skills

- Choose the skill that matches the task instead of defaulting to a single workflow.
- Follow each skill's `SKILL.md`, including its bundled references.
- Do not add new libraries or patterns unless the active skill or task explicitly requires it.
- Keep implementations aligned with the conventions defined in the relevant skill.

## General Conventions

- Prefer DRY, maintainable, and accessible solutions.
- Avoid duplicating global structure across files.
- Use framework idioms when a skill targets a specific stack.
- Do not place data access or business logic in presentation files when a backend skill applies.
- Define clear boundaries between skills so they can be used independently or together.

## Notes

- This file provides additional guidance for Claude when working in this repository.
- For implementation details, see the relevant `SKILL.md` files.
- For Kilo configuration, see `AGENTS.md` and `kilo.json`.
