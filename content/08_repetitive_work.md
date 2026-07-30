# Repetitive work

How to apply the same change many times, and when to write a script instead.

## Outline

- Repeating your own work
  - Do the first instance by hand, then have the agent apply the pattern to the rest
  - Example: one hand-written pickle-safe class, then the same idea across the other classes
- Repetitive maintenance tasks
  - Dropping an old language version everywhere it appears, plus modernizing (e.g. pattern matching)
  - Combine with linters (Ruff `UP`) and CI so nothing is missed
- Adding static types: a special case
  - Slow by hand, and non-AI tools do a poor job
  - Weak models handle the first 80%; a strong model then hunts down the `Any`s
  - Also works across languages (JavaScript → TypeScript)
- Capture the process: `SKILL.md`
  - An open standard: human- and AI-readable instructions, invoked by `/skills` or auto-matched
  - Examples: drop-a-Python-version, add a minimum-version CI job, apply repo-review
  - Use the AI to help write skills; teach one your changelog style
- When to write a script instead
  - Deterministic, high-volume transforms: have the agent write the script, then run it —
    fewer tokens and more reliable than the agent doing the repetition directly
- Exercise: write a small `SKILL.md` for a task you repeat, and apply it to `agentic-ai-example`
