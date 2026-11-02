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

## Delivery

Stack: Mule 4, Maven, OpenSpec, GitHub Actions
Delivery: E reference
Integration branch: `main`
Data: none
Article: `50-writing/spec-driven-development-in-practice/`

- Build: `mvn -B clean package`
- Test: `openspec validate --all --strict` (CI: `.github/workflows/openspec.yml`), plus the curl calls recorded in `README.md` against the running application
- Run locally: run the packaged application on a Mule 4.12.x runtime and repeat the curl calls in `README.md`
- Deliver and verify: `nothing is deployed; the OpenSpec workflow green on the merge commit, and the links and paths the article cites still work`

This repository was made for the article above and keeps what it published
(code, OpenSpec content and structure): every merge is the owner's.
