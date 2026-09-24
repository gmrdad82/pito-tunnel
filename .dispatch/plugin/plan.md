# pito-tunnel — plan `plugin`

Prefix: SH
State: parked
Parked: 2026-09-20 — waits on pito-work/WK-20, with Paper.

## Decisions
1. A plan is bound to one repository; a two-repository card of the split became one card per repository, the other half waited on as `<repo>/<ID>`.
2. Dependencies on retired or landed cards are dropped; IDs are kept.
3. One implementing agent owns each card through validation and landing; the route serializes integration per repository while worktrees implement in parallel.
4. Cards contain 1–4 checkable deliverables; tests protect consequential behavior and security boundaries, never every item.
5. Only a Landing entry completes a card. Preserve landed cards, board rows and runbooks; resume unfinished work in its existing worktree.
6. Model and effort are command arguments, not card metadata. Use --model and --effort with native model IDs.

## Left open
- A session re-reads the tree at resume and notes what moved.
