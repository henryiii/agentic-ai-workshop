# New features

How to plan and build a new feature with an agent.

## Outline

- Plan first
  - Use plan mode to discuss the change; a good agent finds weak points and asks questions
  - Most harnesses generate a plan file you can edit before it starts
  - Mix tiers: a smart model makes the plan, a cheaper model implements it
  - Save the plan as a comment on the issue or PR
- Implementing from a specification
  - Point it at the spec (a PEP, an issue, a design document), add your extra details, let it go
  - Real examples: PEP implementations, build-backend plugin features
- Prototype to decide
  - Test-drive a feature before committing to it: does it work, is it fast enough, do you like it?
  - Keep the AI version, polish it, or rewrite it yourself — the prototype answered the question either way
- Model choice: use a good model here — you want the highest chance of usable code
- Validate beyond your own repo
  - Adapt downstream projects to the new feature and collect their pain points
  - Have the AI try to break the feature; have it follow your tutorial and report gaps
- The real cost is after generation
  - Working through and understanding the changes is the time sink; it can overwhelm you with code to review
  - Iterate — do not try to one-shot it; talk to the agent while it works
    (you do not need to hand-edit, but do iterate until it meets your standards)
- Exercise: take a small written spec, plan in plan mode, implement it in `agentic-ai-example`,
  and review the result
