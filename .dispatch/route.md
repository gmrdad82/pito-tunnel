# The route

A dispatched implementation owns its validated landing. Keep implementation in
its worktree. Run integration through `dispatch land -- <program> [arguments]`,
which serializes landings across this repository's worktrees.

1. Inspect the diff for secrets and unrelated changes; stage explicit paths and
   the runbook, then commit signed with a plain card-prefixed message.
2. Fetch and integrate the current main line; resolve conflicts within the card's
   scope. A decision or signing blocker keeps the worktree and a precise record.
3. Run the repository's full local gate on the integrated result. Rust work checks
   formatting, binary compilation, warning-free clippy and relevant tests; other
   projects use their native build and validation commands. Add tests only for
   consequential behavior and security boundaries.
4. Push normally to main. On rejection, refresh, reconcile and validate again;
   never force-push, amend, discard changes or bypass a failing gate.
5. After the validated changes reach main, append `## Landing`, commit the record
   signed and push under the same lock. Keep the worktree until plan purge.

No review, fix or merge agent follows. Existing landed records remain unchanged.
No deployment, bucket mutation or tag is authorized by this route; releases
follow the repository's explicit release request and production restrictions.
Never modify the owner's checkout or stage unnamed files. Observe any pipeline
that starts itself; never trigger CI just to prove a main landing.
