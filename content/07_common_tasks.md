# Common tasks

Everyday jobs that an agent does well, such as small edits, docs, and release chores.

## Outline

- The theme: AI is good at things you do not want to spend time on
- Small edits
  - Config tweaks, adopting a new tool, release chores — fire off and do something else
- Merge conflicts and rebases
  - "rebase this PR" — repetitive conflicts do not need a strong model or your hands
  - With `AGENTS.md` set up, it tests and fixes the result too
- Conversion tasks
  - CI provider migrations, config format changes (`tox.ini` → `tox.toml`),
    docs system moves, language ports, citation formats
  - Tip: give it a link to the schema or a description of the target format
- Fixing lints
  - Enable a lint with no autofix and let the agent iterate over the report
  - More lint rules → better agent output overall: a win-win
- Better docs
  - Converting code into readable text that pairs with the code
  - Matching existing style, moving text inline, filling missing details
- One-off scripts
  - Plots, data analysis, Dockerfiles from shell history, quick tooling —
    output you use once and do not need to review as production code
- Small web work: pages, JavaScript/TypeScript when you are rusty
- Release chores
  - Draft release notes or a changelog from the git log between two tags — it mimics existing style
  - "What's new since last release? Changelog style."
- Reviving old work: ask it to bring an outdated PR up to date with the current codebase
- Tasks you never had time for: small cleanups, dependency swaps, restructuring
  - Also things you *wouldn't* have done: test the top downstream projects before and after a change
- Low-friction mixing: leave `TODO` comments while writing, then ask the AI to find and fix them
- Exercise: pick two chores in `agentic-ai-example` and run them in parallel with an agent
