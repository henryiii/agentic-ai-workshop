# Setup

Instructions to set up your environment for the workshop.

## Outline

- Pick a harness (all handle similarly, with common commands)
  - Three flavors: TUI (works anywhere, incl. clusters), GUI apps, editor plugins
    - Most harnesses come in all three; pick the one you like — some favor one
      (Copilot → editor first; Claude Code, OpenCode → TUI)
  - The system prompt is the biggest difference between harnesses running the same model
    - Some providers (Anthropic) require their harness for subscription use; prompt caching
  - Trade-offs: permission models, system prompt size, configurability
- Pick a model
  - Model tiers: frontier / workhorse / simple / local — with a task-to-tier breakdown
    (questions and throwaway scripts → local; triage and lint fixes → simple;
    review, bugfixes, conversion → workhorse; refactors, features, profiling → frontier)
  - Start with a good model so first attempts succeed
  - Effort levels; a strong model on high effort can overengineer simple tasks
  - Cheaper/faster and local models come later, once you can judge task difficulty
- Accounts and access
  - Copilot Pro / free tiers, education accounts
  - Optional: local models (memory requirements, quantization in brief)
- Install and sign in
  - Download the harness, sign in or set the API key
  - Create a user-level config file (`~/.claude/CLAUDE.md`, `~/.config/opencode/AGENTS.md`,
    `~/.pi/agent/APPEND_SYSTEM.md`) — template covered in the AGENTS.md chapter
- Security basics before you start
  - Permission modes: manual approve vs. auto-approve; containers and codespaces as isolation
  - Do not give the agent access to anything it can destroy (databases, tokens)
- Clone the workshop example repository (`agentic-ai-example`, a small plotting package)
  and verify your harness runs in it
- Optional: usage tracking across harnesses (`uvx agentsview`, `npx ccusage`)
