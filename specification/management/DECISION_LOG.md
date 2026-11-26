# Decision Log

- D-01 (confirmed): Android TV + TIF architecture.
- D-02 (confirmed): English specification; Serbian team discussion; gated US → HLR → FR/NFR → TR → separate design and test specifications.
- D-03 (confirmed): New specification; no inherited technical decisions.
- D-04 (confirmed): Modular Markdown handoff and a shorter unified formal PDF; changelog, decision log and rollback support.
- D-05 (confirmed): Team supplied seven user stories and authorized progression to HLR. English wording normalized without intentional scope changes.
- D-06 (superseded by D-13): HLR-01–HLR-11, pending team approval.

- D-07 (confirmed): Rewind selection in backward 30-second increments with explicit confirmation; team reports agreement with instructor.
- D-08 (confirmed): Quality changes in both live and rewind modes preserve content position.
- D-09 (confirmed): Single indicator represents delay behind live.
- D-10 (confirmed): Team implements both servers and simulated live. Replacement by a real head-end with minimal receiver changes is a design objective, not established compatibility.
- D-11 (confirmed): Guide limited to selected service.
- D-12 (conditional): Prefer system Live TV application; project-owned TIF-integrated application may be considered if feasibility checks fail.
- D-13 (confirmed): Revised HLR baseline approved by the team on 2026-11-26, with the OQ-07 resolution recorded in D-14.
- D-14 (confirmed): Rewind reference is the playback position at explicit confirmation (option B).

## FR phase — confirmed pilot feedback
- D-15: FR identifiers FR-HLR.sequence, one functional responsibility per requirement, parent HLR, normative statement, optional preconditions and acceptance criteria. Omit Related Requirements and illustrative examples from acceptance criteria.
- D-16 (OQ-09): Current playback continues during rewind selection.
- D-17 (OQ-10): Displacement can increase or decrease in 30-second increments.
- D-18 (OQ-11): Unconfirmed selection can be cancelled without affecting playback.
- D-19 (OQ-12): Clamp a confirmed request before available history to earliest available playable content (option C). Edge behavior when no earlier content exists remains open. Finite history is possible even in real deployments; no production guarantee is assumed.
- D-20 (OQ-13): Playback start deviation is expected and permitted; numerical accuracy will be defined in NFR.
- D-21 (OQ-14): Each new rewind selection defaults to 30 seconds.
- D-22 (OQ-15): No second rewind confirmation while prior rewind is pending.
- D-23 (OQ-16): Rewind preserves active quality.
- D-24: Team authorized a complete FR draft, not a final FR baseline. NFR and TR remain unauthored.
