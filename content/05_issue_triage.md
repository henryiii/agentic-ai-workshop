# Issue triage

How to sort, label, and reproduce reported issues with an agent.

## Outline

- Ask the agent to survey open issues
  - "Categorize all open issues. Highlight ones that are easy to close,
    and bugs that you can reproduce"
  - Which are easiest to close, with reasons — then `gh issue close` by hand
  - Which are best to work on first; which are duplicates or already fixed
  - "Is #234 still broken?"
- Scaling up: "launch subagents to fix the reproduced bugs in worktrees, open a PR for each"
- Reproduce before you fix
  - Point the agent at an issue; ask it to construct a minimal reproduction
  - A good reproduction becomes the regression test (see the tests chapter)
- Keep the human in the loop
  - Fully automated triage is too easy to attack — malicious issue titles have
    carried injected instructions to triage bots
  - The agent proposes; you decide and you respond
  - Never let a reporter talk to an AI without knowing it
- Exercise: triage the open issues of `agentic-ai-example` and produce a ranked list with reasons
