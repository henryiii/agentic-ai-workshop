# AGENTS.md

How to write project instructions that tell an agent how to work in your repository.

## Outline

- Why it matters: the agent does not know your best practices unless you tell it
  - Example failures: `pip install --user` instead of `uv run`, outdated standards
- Start with `/init`
  - Generates a project file; rename to `AGENTS.md` if the harness picks another name
  - The cross-tool standard is `AGENTS.md`; Claude Code is the exception —
    `ln -s AGENTS.md CLAUDE.md` for a single source of truth
  - Steer it: ``/init prioritize `uv run`, include architecture design``
- What belongs in it: what is *not* obvious from the code
  - How to run tests, which tools to prefer, where generated files live, traps and gotchas
  - Style rules, things the agent keeps getting wrong
  - Treat it as documentation you maintain, not a dumping ground
  - Edit by hand, or ask the AI to edit it — use the AI to set up the AI
- To commit or not: three options
  - Commit it (shared context, enforce conventions; gitignore `CLAUDE.md`/`.claude/`)
  - Gitignore it (expected but personal)
  - Leave it out entirely (personal copies via `.git/info/exclude`)
- User-level configuration
  - A file loaded in every session (`~/.claude/CLAUDE.md`, `~/.config/opencode/AGENTS.md`,
    `~/.pi/agent/APPEND_SYSTEM.md`)
  - Template: OS/system notes, GitHub username, `uv run`, lint command,
    conventional commits + `Assisted-by:` trailer, AI-text disclaimer line for PRs
  - Relative-paths hint if you use local/small models; `CLAUDE.local.md` caveat
- Related configuration
  - Harness settings (models, permission auto-approvals) — the AI can write these too
  - Skills: reusable instructions in `.agents/skills` / `~/.agents/skills`
    (`gh skills`, skills.sh; Claude Code needs a symlink to `.claude/skills`) —
    detail in the repetitive-work chapter
- Exercise: run `/init` on `agentic-ai-example`, review and refine the result;
  write your own user-level config file
