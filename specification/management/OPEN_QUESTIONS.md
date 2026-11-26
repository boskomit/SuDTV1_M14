# Open Questions

OQ-01 through OQ-05 and OQ-07 resolved; OQ-06 resolved as a conditional platform preference. See Decision Log.

- OQ-07 (resolved): Option B — use the playback position at explicit confirmation, not at selection start.
- OQ-08 (deferred to feasibility verification): Can the chosen system Live TV application expose all required viewer controls and selected-service guide while integrating with TIF? If not, seek team approval for a project-owned application.

Deferred to later phases: archive limits, quality defaults, precise continuity tolerance, failure semantics, keyboard mapping, stream control, runtime image, detailed architecture and test cases.

## FR pilot questions — resolved
OQ-09–OQ-16: see D-16–D-23 in Decision Log. OQ-13 numerical tolerance deferred to NFR. OQ-12 boundary edge cases remain in OQ-28.

## FR review — open questions
### OQ-17 — Catalogue lifecycle

When is the catalogue loaded or refreshed, and should successfully retrieved changes remove services no longer listed?

**Proposed answer (not approved):** Load at startup; support explicit refresh; reconcile additions, changes and removals only after a valid complete response.

### OQ-18 — Metadata failure

On catalogue or guide retrieval failure, should previously retrieved data remain visible?

**Proposed answer (not approved):** Keep last valid data with an unavailable/stale indication; if no valid data exists show an empty/error state; retry available.

### OQ-19 — Channel ordering and invalid input

What is the order, boundary behavior, and confirmation rule for direct numeric entry?

**Proposed answer (not approved):** Ascending channel number; wrap at ends; Enter confirms; invalid number leaves playback unchanged and shows an error.

### OQ-20 — Rapid channel changes

Should a newer channel selection supersede pending activation?

**Proposed answer (not approved):** Latest valid selection wins; obsolete completion ignored.

### OQ-21 — Startup and service information

What is selected at startup; which fields and visibility duration are required in the info dialog?

**Proposed answer (not approved):** First available service on initial startup; channel number, service name and current programme if available; automatic dismissal with manual close.

### OQ-22 — EPG scope and empty schedule

Which schedule fields, time range and empty state are needed?

**Proposed answer (not approved):** Title and start/end times; display all records supplied for selected service within server-provided range; indicate no schedule when none is available.

### OQ-23 — Audio persistence

Should volume and mute persist across service, rewind and quality changes?

**Proposed answer (not approved):** Retain settings across playback changes; no persistence across app restarts unless requested.

### OQ-24 — Quality catalogue and default

How many variants, labels and default selection? Is quality global or per service?

**Proposed answer (not approved):** At least two manually selectable variants; retain current quality across operations when available; choose a documented default otherwise. Exact labels and encoding deferred.

### OQ-25 — Quality switching semantics

Should selecting quality immediately request it or require confirmation? What happens on same-quality selection and failure?

**Proposed answer (not approved):** Explicit selection activates change; same active quality is a no-op; retain or restore previous quality/position if feasible, otherwise report error with Go Live recovery.

### OQ-26 — Live position semantics

Should Go Live preserve quality, and what is shown for ordinary live latency? How often should indicator update?

**Proposed answer (not approved):** Preserve active quality; show zero or LIVE for live mode and actual delay for delayed mode; update during playback, exact precision/cadence in NFR. Clarify whether physical live latency counts.

### OQ-27 — Cross-operation conflicts

What happens when Go Live, zap or quality change arrives while rewind or quality switch is pending?

**Proposed answer (not approved):** Zap and Go Live supersede pending playback operations; block other rewind/quality confirmations while pending. Keep latest valid command semantics and ignore obsolete results.

### OQ-28 — Rewind boundary and failure

If already at earliest content or no historical content exists, what should happen? Should clamping be communicated?

**Proposed answer (not approved):** Clamp when earlier playable content exists; show an informative notice; if no earlier playable content exists, retain playback if possible and explain. Do not falsely claim success.

### OQ-29 — Simulated live lifecycle

Should simulated broadcast loop, stop at media end, or follow a scheduled sequence? How much history is retained?

**Proposed answer (not approved):** Common server timeline independent of receiver; choose explicit end-of-content behavior and retention before baseline. Do not assume infinite history.

### OQ-30 — Server delivery scope

Is one receiver/session enough for demonstration or must concurrent independent clients be supported? Should old delivery stop on switch/release?

**Proposed answer (not approved):** One independently controlled receiver is sufficient for required demonstration; stop obsolete delivery. Multiple-client scalability is not assumed.

### OQ-31 — Transport program scope

Will each stream contain one program (SPTS) or must multiple programs be supported (MPTS)? How is selected service associated with program?

**Proposed answer (not approved):** SPTS for demonstration, with parser able to expose PAT program mappings; explicit service-to-program association if MPTS is required.

### OQ-32 — PAT/PMT change behavior

Must receiver handle table updates during uninterrupted playback as well as on new streams? Must parsed PIDs drive playback configuration or is independent verification plus availability to control logic sufficient?

**Proposed answer (not approved):** Require correct independent parsing and use of results in service validation; dynamic table changes and player integration scope need team confirmation.

### OQ-33 — Failure and timeout semantics

On failed zap, lost stream or malformed TS, should previous playback remain/restore or show unavailable state? Should retry be offered?

**Proposed answer (not approved):** Do not mark unsuccessful request as active; preserve valid prior playback where feasible, otherwise show unavailable state and offer recovery. Timeout thresholds in NFR.

### OQ-34 — TIF feasibility

Can preferred system Live TV application support confirmed rewind, quality UI, selected-service EPG and error actions?

**Proposed answer (not approved):** Keep OQ-08 deferred to explicit feasibility verification; do not silently weaken requirements or assume a custom UI is approved.

### OQ-35 — Keyboard and dialogs

Should exact key mapping and dialog close behavior be agreed at FR level?

**Proposed answer (not approved):** Define user-facing mapping for required actions in FR once approved; implementation event handling remains TR. No mouse dependency.
