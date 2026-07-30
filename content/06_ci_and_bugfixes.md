# CI and bugfixes

How to find the cause of a failed CI run and fix the bug behind it.

## Outline

- Agents are great at debugging — a clear pass/fail condition is where they shine
  - They rerun tests, add debug statements, trace the failure down, and propose a fix
  - They happily read long CI logs — just say "Fix the failing CI on this PR"
  - Give it the URL of a flaky CI run and ask it to investigate
  - "Bisect this regression" — a tedious mechanical loop the agent is happy to run
- Fixing failing tests
  - Common as a follow-up after the first CI run of another PR
  - Platform-specific failures (Windows permission races, environment leaks) are good examples
- From issue to fix
  - Point the agent at a well-written issue or failing log and let it work
  - It turns reproduction examples into tests that fit your suite's style
  - Old backlog bugs become tractable: a proposed fix and analysis is a great starting point
- Judgment still required
  - The proposed fix may not be the *best* fix — always look it over
  - Once you know what is wrong, the final iteration is the easy part
- Workflow tip: add the regression test first, confirm it fails, then fix
- Exercise: on a failing branch of `agentic-ai-example`, find the cause and land a fix with a test
