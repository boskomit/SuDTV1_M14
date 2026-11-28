# Version control and rollback

Use main for reviewed baselines and spec/* branches for draft changes. Open a PR per phase; the team merges after review. Prefer merge commits (not squash) when retaining iteration history is important. Do not force-push shared history. Use git revert for rollback through a reviewed PR.

Historical imports: v0.1 draft (8cd89c7), v0.2 draft (f43c923), approved v0.2.0 (140f9fa). These are reconstructed Git commits from the existing Markdown snapshots and team approval; their Git timestamps reflect import, not original drafting.

Proposed tags after approval/merge: spec-v0.2.0 (HLR), spec-v0.3.0 (FR/NFR), spec-v0.4.0 (TR), spec-v0.5.0 (design), spec-v0.6.0 (test), spec-v1.0.0 (final). Tags are not yet created.

Phase gates: US → HLR → FR/NFR (separate categories) → TR → separate design/test specifications → final PDF and handoff. Changes to approved requirements require team approval and impact analysis.

FR milestone: 0.3.0-fr approved on 2026-11-28; combined FR/NFR milestone spec-v0.3.0 remains pending NFR approval and merge. Preserve this milestone in branch history; no tag is claimed created.

NFR draft 0.3.0-nfr-dev.1 follows approved FR milestone; combined baseline and PR remain pending review.

Combined FR/NFR baseline 0.3.0 approved on 2026-11-28. PR to main is pending team merge; tag creation remains pending merge.

TR draft 0.4.0-dev.1 on spec/tr based on merged 0.3.0 main. TR baseline/PR pending team review; no tag claimed.
