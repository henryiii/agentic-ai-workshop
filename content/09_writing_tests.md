# Writing tests

How to add tests, increase coverage, and confirm that a test finds the bug.

## Outline

- Coverage-driven test writing
  - Tests give the agent the clear pass/fail signal it works best with
  - "Add tests for the change I just made"
  - Produce a coverage report; the agent iterates over missing lines and writes tests
  - Fitting new tests into your suite's existing structure and style
- Model choice matters here
  - Weak models write meaningless unit tests; you want a model that understands what is needed
- Turning examples into tests
  - Issue reproductions and bug examples become regression tests in the proper place
  - Regression-test-first: confirm the test fails without the fix
- Typing and testing the test suite itself
  - Annotating tests — a task rarely done by hand, now cheap
- Caution: generated tests are not always correct
  - A test that asserts current behavior is not the same as a test that asserts *correct* behavior
  - Review what property each test actually pins down
- Exercise: raise coverage of `agentic-ai-example` and inspect which generated tests are worth keeping
