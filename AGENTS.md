# AGENTS.md

This repository is a public example of spec-driven development with
[OpenSpec](https://github.com/Fission-AI/OpenSpec) 1.14.1. The code is a small
Mule 4 API; the point is how it gets built.

## Rules

- Work comes from an approved OpenSpec change in `openspec/changes/<name>/`.
  Implement what its tasks say; do not re-plan an approved change. If a task
  needs a decision the change does not make, stop and ask.
- Work on a branch, never on `main`. Merge is the owner's decision.
- Keep artifacts and code short and readable: readers study this history.
- No MUnit. Prove each spec scenario with a curl call against the running
  application and record the call and the real response in `README.md`.
- No secrets, credentials, employer or client material.
- Run `openspec validate --all --strict` before opening a pull request.

## Build

Java 17 and Maven. Mule runtime 4.12.x.
