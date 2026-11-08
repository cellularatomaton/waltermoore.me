## Overview

**Foundry** turns a GitHub Issue into a verified pull request. It is the orchestration and governance layer around a coding agent: it decides when an agent runs, where it runs, what happens to its output, and how a human steers it, all through comments and labels on the issue itself.

Foundry is a **GitHub Action**. There is no server to run; every piece of state lives on GitHub, in labels, a structured claim comment, the pull request, and the preview comment. It is MIT licensed and published to npm as `@joynt/foundry`.

### The loop
1. A person with write access comments `@foundry go` on an issue.
2. Foundry claims the issue and creates an isolated git worktree from the integration branch.
3. The configured agent (Claude Code, aider, or any CLI) works on the brief and ends with a typed report: done, needs input, plan ready, or blocked.
4. Foundry runs the repository's own lint, typecheck, build, and test commands. No pull request is opened until they pass.
5. Foundry opens the pull request, stands up a preview environment, and waits for a human.
6. The reviewer approves, or comments to send the agent back to work. On approval, Foundry rebases, waits for CI, and merges.

Three comments are the whole control surface: `@foundry go`, `@foundry restart`, and `@foundry stop`.

---

## Designed Around Control

- **Human release gates**: merging is always a human decision.
- **Permission checks**: only users with write access can start a run; anyone else's comment is ignored and logged.
- **Untrusted input handling**: issue text reaches the agent through environment variables and a file, never re-evaluated in a shell.
- **Typed agent reports**: control flow branches on a structured report, never on the agent's prose.
- **Verification before review**: nothing reaches a reviewer until it builds and passes tests.
- **Audit trail by default**: every claim, run, verification result, and approval is recorded on GitHub.

---

## My Role: Creator

I designed and built Foundry to run JoYnt's engineering as a one-person organization directing a team of AI developer agents. It applies what I learned leading engineering teams and building trading and regulatory reporting systems: speed is only valuable when you can prove what happened.

- **Code**: [github.com/joyntshops/foundry](https://github.com/joyntshops/foundry)
- **Live demo with real agent pull requests**: [github.com/joyntshops/foundry-demo](https://github.com/joyntshops/foundry-demo)
