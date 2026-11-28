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

## FR review feedback — 0.3.0-dev.2
- D-25 (OQ-17; Partial): Catalogue refresh at startup and every 24 h; EPG at startup and 6–12 h intervals. Removal/manual refresh and exact EPG cadence/scope remain open.
- D-26 (OQ-18; Resolved): Retain last valid data with stale/unavailable indication; empty/error state if absent; retry.
- D-27 (OQ-19; Resolved): Ascending channel number, wrap at boundaries, Enter confirms; invalid number is silently ignored.
- D-28 (OQ-20; Resolved at FR level): Latest valid selection wins; minimum interval required between channel activation initiations; interval in NFR and rate-limit handling open.
- D-29 (OQ-21; Resolved): First available service at initial startup; info displays number/name/current programme if available; automatic/manual dismissal.
- D-30 (OQ-22; Open): Team requested explanation of coverage window; proposed past 24 h and future 24 h, programme title/start/end, empty state; awaiting approval.
- D-31 (OQ-23; Resolved): Volume/mute retained across playback operations; persistence across restarts not required.
- D-32 (OQ-24; Partial): Exactly two quality variants; preserve quality when available; defined default remains to be selected.
- D-33 (OQ-25; Resolved): Selection initiates change without further confirmation; same quality no-op; retain/restore prior playback if feasible; otherwise error and Go Live.
- D-34 (OQ-26; Resolved): LIVE in live mode; negative HH:MM:SS delay in rewind mode; ordinary live latency not displayed; Go Live preserves quality.
- D-35 (OQ-27; Resolved): Zap and Go Live supersede rewind/quality; block rewind/quality during pending transitions; Go Live not applicable during zap; obsolete completions ignored.
- D-36 (OQ-28; Resolved): Clamp if earlier playable content exists; otherwise no additional rewind; retain playback if possible; brief nonblocking notice.
- D-37 (OQ-29; Partial): Cyclic scheduled playlist approved; repeat after final programme with continuous broadcast timeline. Historical retention open.
- D-38 (OQ-30; Resolved): One receiver sufficient; terminate obsolete deliveries.
- D-39 (OQ-31; Resolved): SPTS demonstration; PAT parser exposes all program mappings; MPTS playback not required.
- D-40 (OQ-32; Partial): Instructor confirmed parser-driven playback unnecessary. Independently inspectable correct results used or retained for verification; continuous table updates open.
- D-41 (OQ-33; Resolved): Retain/restore prior playback where feasible; otherwise unavailable, error and retry/service selection. Timeout in NFR.
- D-42 (OQ-34; Resolved as constraint): All M14 functions remain mandatory; OQ-08 feasibility remains deferred.
- D-43 (OQ-35; Partial): Arrows channel/volume, Enter confirmation, Backspace adds 30-second rewind step, digits select services, Space Go Live. Remaining mapping pending.

## FR baseline approval — 2026-11-28
- D-44 (OQ-22; confirmed): Selected-service EPG covers preceding and following 24 hours, with programme titles and start/end times; empty state; no direct EPG historical playback requirement.
- D-45 (OQ-36; confirmed): Reconcile additions, updates and removals after valid complete catalogue; manual refresh optional. Resolves D-25 catalogue scope.
- D-46 (OQ-37; confirmed): Retain latest valid selection during minimum channel-change interval and execute at expiry. Interval deferred to NFR. Completes D-28.
- D-47 (OQ-38; confirmed): Six-hour EPG refresh; startup and missing/stale selected-service data on selection. Completes D-25.
- D-48 (OQ-39; confirmed): Complete keyboard mapping from review, including dialog navigation and Space in normal viewing only. See FR-04.05. Completes D-43.
- D-49 (OQ-40; confirmed): High and Low labels, High default, Low fallback. Completes D-32.
- D-50 (OQ-41; confirmed): All simulated broadcast history since startup available, no additional retention cap. Completes D-37.
- D-51 (OQ-42; confirmed): Team selected Hybrid RTSP + server-side media seeking after consultation with instructor. RTSP control, MPEG-TS over RTP media, REST metadata, server-side seek resolution; specific PCR/PTS/index/library choices and session mechanics deferred to TR. Custom control alternative not selected. No PoC or compatibility verification claimed.
- D-52 (OQ-42; FR closure): Parse PAT/PMT on each new stream; no mandatory continuous table-version monitoring for stable SPTS; reacquire upon relevant structure changes. Presented position tracking remains required. Completes D-40.
- D-53 (confirmed): Team authorized closure of FR baseline on 2026-11-28. All 75 FR IDs retained; NFR is next, TR/design/test not authored. OQ-08 deferred, not a waiver of mandatory functionality. Supersedes draft status in D-24.
