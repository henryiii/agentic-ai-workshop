# Refactors

How to make large structural changes safely.

## Outline

- Write the plan, let the agent implement it
  - It will very likely work and pass tests — then evaluate whether it is worth keeping
- Example refactors
  - Restructuring files and breaking up modules
  - Removing a dependency (click → argparse), converting decorators to fixtures
  - Language migrations (JavaScript → TypeScript), packaging a webapp properly
- Safety nets make refactoring possible
  - A solid test suite, linters, and a type checker let the agent (and you) trust the change
  - Simpler code is easier for the AI too — refactoring pays forward
- Staged approaches
  - Multi-phase changes that failed by hand can succeed with an agent (e.g. two-phase framework upgrade)
  - Verify and fix issues at each stage instead of one giant leap
- Working in parallel
  - Several refactors at once via separate repos or `git worktree`
- Know your git
  - Rebase, safe force-push, history rewriting, reflog — you must be able to direct the agent
    or do it yourself; harnesses shy away from history manipulation
- Spare subscription time? Point it at cleanups
  - Hunt for simplifications and bugs; compare documentation against implementation
- Exercise: run a prepared refactor plan on `agentic-ai-example` and validate it with the test suite
