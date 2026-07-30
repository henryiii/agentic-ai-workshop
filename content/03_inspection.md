# Inspection

How to use an agent to explore and explain an unfamiliar codebase.

## Outline

- The best first task: ask questions about a codebase, no write access needed
  - "How do I run the tests?" — "Who wrote the majority of the CI?"
  - Where is X defined (works even across languages, e.g. C in CPython)?
  - Can this return value be `None`? The agent traces the logic and shows why
  - Does this code handle this edge case (e.g. multi-digit version numbers)?
  - "Write an `ARCHITECTURE.md` describing how this project works"
  - The agent looks up git history, webpages, and more on its own — no need to spell it out
- Watch it work: enable thinking display — it greps, reads, and searches the way you would
- Beyond code
  - Look over failing logs; compare two archives that should be identical
  - Explain confusing code (even your own old code) before you edit it
- From inspection to small edits
  - Once it can find where and what to edit, small changes are fast — fire and do something else
- Contributing to an unfamiliar project
  - The agent learns their conventions, gets tests and linters running
  - Run `/init` if the project has no `AGENTS.md`
  - Etiquette: human-written PR description, mention AI use, respect `AI_POLICY.md`,
    do not open PRs a maintainer could trivially do themselves
- Exercise: explore `agentic-ai-example` — answer three questions about it,
  then have the agent write an `ARCHITECTURE.md`
