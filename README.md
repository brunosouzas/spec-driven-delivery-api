# spec-driven-delivery-api

A small Mule 4 API built with spec-driven development (SDD) using
[OpenSpec](https://github.com/Fission-AI/OpenSpec).

The interesting part is not the API. It is the path from a one-line request to
code:

1. A short, incomplete request.
2. An OpenSpec change (`proposal.md`, `specs/`, `design.md`, `tasks.md`) that
   makes the missing decisions explicit, reviewed before any code exists.
3. Human approval of one commit of that change.
4. An AI agent implementing from the approved artifacts only.
5. Each spec scenario proven with a real call, below.
6. The change archived into `openspec/specs/`, and a second change that evolves
   the API through a delta.

Follow it in the commit history and in `openspec/`.

## Status

Work in progress. The first change is being prepared.
