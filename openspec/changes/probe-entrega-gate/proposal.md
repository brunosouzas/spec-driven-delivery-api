## Why

Throwaway probe for the SDD-8 proof of concept (BRU-132, issue BRU-151). It
exercises the delivery gate of `entrega.py` on a real repository: a change with
an open task must not get a pull request, and a tracker failure must not
duplicate the pull request or the tracker comment on a rerun.

## What Changes

- Nothing in the API. This change adds no behaviour and no spec delta.

## Out of Scope

- Any change to `src/`, `README.md` or `openspec/specs/`.
- Merging: the pull request is closed without merge and the branch deleted.

## Impact

- None on the application.
