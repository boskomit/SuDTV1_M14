# Open Questions

OQ-01 through OQ-05 and OQ-07 resolved; OQ-06 resolved as a conditional platform preference. See Decision Log.

- OQ-07 (resolved): Option B — use the playback position at explicit confirmation, not at selection start.
- OQ-08 (deferred to feasibility verification): Can the chosen system Live TV application expose all required viewer controls and selected-service guide while integrating with TIF? If not, seek team approval for a project-owned application.

Deferred to later phases: archive limits, quality defaults, precise continuity tolerance, failure semantics, keyboard mapping, stream control, runtime image, detailed architecture and test cases.

## FR pilot questions — resolved
OQ-09–OQ-16: see D-16–D-23 in Decision Log. OQ-13 numerical tolerance deferred to NFR. OQ-12 boundary edge cases remain in OQ-28.

## FR review — decisions and remaining questions

- OQ-17 (Partial): Catalogue refresh at startup and every 24 h; EPG at startup and 6–12 h intervals. Removal/manual refresh and exact EPG cadence/scope remain open.
- OQ-18 (Resolved): Retain last valid data with stale/unavailable indication; empty/error state if absent; retry.
- OQ-19 (Resolved): Ascending channel number, wrap at boundaries, Enter confirms; invalid number is silently ignored.
- OQ-20 (Resolved at FR level): Latest valid selection wins; minimum interval required between channel activation initiations; interval in NFR and rate-limit handling open.
- OQ-21 (Resolved): First available service at initial startup; info displays number/name/current programme if available; automatic/manual dismissal.
- OQ-22 (Open): Team requested explanation of coverage window; proposed past 24 h and future 24 h, programme title/start/end, empty state; awaiting approval.
- OQ-23 (Resolved): Volume/mute retained across playback operations; persistence across restarts not required.
- OQ-24 (Partial): Exactly two quality variants; preserve quality when available; defined default remains to be selected.
- OQ-25 (Resolved): Selection initiates change without further confirmation; same quality no-op; retain/restore prior playback if feasible; otherwise error and Go Live.
- OQ-26 (Resolved): LIVE in live mode; negative HH:MM:SS delay in rewind mode; ordinary live latency not displayed; Go Live preserves quality.
- OQ-27 (Resolved): Zap and Go Live supersede rewind/quality; block rewind/quality during pending transitions; Go Live not applicable during zap; obsolete completions ignored.
- OQ-28 (Resolved): Clamp if earlier playable content exists; otherwise no additional rewind; retain playback if possible; brief nonblocking notice.
- OQ-29 (Partial): Cyclic scheduled playlist approved; repeat after final programme with continuous broadcast timeline. Historical retention open.
- OQ-30 (Resolved): One receiver sufficient; terminate obsolete deliveries.
- OQ-31 (Resolved): SPTS demonstration; PAT parser exposes all program mappings; MPTS playback not required.
- OQ-32 (Partial): Instructor confirmed parser-driven playback unnecessary. Independently inspectable correct results used or retained for verification; continuous table updates open.
- OQ-33 (Resolved): Retain/restore prior playback where feasible; otherwise unavailable, error and retry/service selection. Timeout in NFR.
- OQ-34 (Resolved as constraint): All M14 functions remain mandatory; OQ-08 feasibility remains deferred.
- OQ-35 (Partial): Arrows channel/volume, Enter confirmation, Backspace adds 30-second rewind step, digits select services, Space Go Live. Remaining mapping pending.

## Remaining functional clarifications

### OQ-36 — Catalogue reconciliation

Should a valid complete catalogue remove absent services, and is manual refresh required? Proposed: reconcile removals after valid complete response; manual refresh optional, not required.

### OQ-37 — Channel rate limiting

Keep latest valid selection during the minimum interval and initiate it when interval expires, rather than dropping the last selection? Proposed: yes; exact interval in NFR.

### OQ-38 — EPG cadence and retrieval scope

Choose an interval within 6–12 h (proposed 6 h). Refresh selected-service EPG on startup, channel selection when missing/stale, and periodically; no requirement to prefetch every channel. Confirm coverage proposal in OQ-22.

### OQ-39 — Complete keyboard mapping

Proposed normal viewing: Up/Down next/previous, Right/Left volume up/down; Enter confirm, Backspace adds rewind step/open selection, digits enter channel, Space Go Live, M mute, G guide, Q quality, L list, I info, Esc cancel/close. In dialogs arrows navigate; in rewind selection Left adds 30 s, Right subtracts 30 s, Backspace adds 30 s, Enter confirms, Esc cancels. Confirm dialog focus and Space global versus normal-viewing only.

### OQ-40 — Default quality

Proposed High at initial startup, retain selected quality when available; Low fallback if High unavailable. Confirm labels High/Low and default.

### OQ-41 — Historical retention

Proposed all simulated history since server start, with no additional retention cap for demonstration; do not allow pre-start content as already broadcast. Confirm or specify rolling window.

### OQ-42 — Continuous PAT/PMT changes

Proposed parse on every new stream; continuous table-version monitoring is not required for stable demonstration SPTS. Confirm.

OQ-22 coverage and EPG fields still require approval. OQ-08 feasibility remains deferred. No NFR thresholds beyond team-provided refresh intervals are established.
