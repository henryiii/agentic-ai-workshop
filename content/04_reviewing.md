# Reviewing

How to review changes and pull requests with an agent.

## Outline

- Text review: paste anything (non-confidential!) and ask for a review
  - Assign a role; it can check references (e.g. does your citation match the PEP?)
  - Local models for private material
- PR review with an agent
  - Ask the agent to review a diff, a branch, or a PR
  - Automated reviewers (e.g. Copilot reviewer) have become genuinely good
    - Real examples: catching binary pickle strings that did not match their comments
  - Limits: misses context; suggested replacements are often not worth taking
- Adversarial review
  - "You are an adversarial reviewer for this branch. Any problems? Can you break it?"
  - "Review this project for bugs, performance, simplifications, and modernizations" —
    with a good model that validates its findings
  - Docs too: look for typos and coverage gaps; have it follow your tutorial and report what is missing
- AI reviewing AI: rubber duck
  - Use a *different model family* to review — same-family review self-praises
  - Feed the review back to the implementer and iterate; roughly 70% of the way
    to the next model class in tests (Copilot ships this as a mode, but you can do it by hand)
- Humans still matter
  - AI writes code faster than you can read it — review is the bottleneck
  - Human review is better; AI review complements it and catches the little stuff
  - Little to lose by running it on nearly every PR
- Exercise: run an adversarial review of `agentic-ai-example` (or a prepared PR on it);
  compare with your own review
