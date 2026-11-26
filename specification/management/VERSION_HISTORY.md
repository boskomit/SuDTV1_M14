# Version control and rollback

Use main for reviewed baselines and spec/* branches for draft changes. Open a PR per phase; the team merges after review. Prefer merge commits (not squash) when retaining iteration history is important. Do not force-push shared history. Use git revert for rollback through a reviewed PR.

Historical imports: v0.1 draft (8cd89c7), v0.2 draft (f43c923), approved v0.2.0 (140f9fa). These are reconstructed Git commits from the existing Markdown snapshots and team approval; their Git timestamps reflect import, not original drafting.

Proposed tags after approval/merge: spec-v0.2.0 (HLR), spec-v0.3.0 (FR/NFR), spec-v0.4.0 (TR), spec-v0.5.0 (design), spec-v0.6.0 (test), spec-v1.0.0 (final). Tags are not yet created.

Phase gates: US → HLR → FR/NFR (separate categories) → TR → separate design/test specifications → final PDF and handoff. Changes to approved requirements require team approval and impact analysis.
