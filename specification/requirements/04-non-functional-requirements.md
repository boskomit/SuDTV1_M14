# Non-Functional Requirements — Draft 0.3.0-nfr-dev.1

Status: **Proposed — team review required**. Based on approved US/HLR/FR baseline 0.3.0-fr. No FR wording is changed. All new numeric thresholds and test sample sizes below are proposed engineering targets for the student demonstration, not measured results, instructor mandates or production SLAs. Existing approved refresh/coverage and protocol constraints are restated for verification. No TR/design implementation decisions are made here.

## Reference conditions and measurement conventions

- One Android TV receiver, both project servers reachable on a controlled local IP network (physical or virtual), valid prepared SPTS in High and Low, cyclic scheduled playlist, and documented runtime/host configuration. No deliberate network impairment or competing heavy workload in normal performance tests. Exact hardware, image, media encoding and tools are deferred to TR; record them before testing. Fault tests intentionally depart from these conditions.
- Use at least two services and content with identifiable timeline markers, independently verified metadata and synchronized A/V cues. Include programme/cycle boundaries and both qualities. This is a proposed verification fixture, not an extra user feature.
- Operation latency starts at the relevant valid user action and ends at actual first presentation of the requested content, not request acknowledgement. A superseded or invalid command is not a successful eligible operation. For each latency sample set, attempt 20 eligible operations; any failed non-superseded attempt counts against the 18/20 target and is also a functional failure under normal reference conditions. Do not discard failures or retry to improve the sample. Use both modes where applicable.
- Time position accuracy is measured on the common service broadcast timeline with independent content identification, not raw PCR, RTP arrival time, or the same unverified receiver estimate. Effective rewind target includes the approved earliest-available clamp. Quality reference is the presented position at selection, as proposed for review.
- Stable playback excludes explicitly initiated transitions and fault injection, not unexplained stalls. All thresholds apply to the reference environment only. No results have been measured.
- Requirements retain stable NFR IDs, parent HLR references and FR traceability (not a Related Requirements field). Acceptance criteria are verification conditions, not illustrative examples or full test cases.

## Performance and responsiveness

### NFR-01 — Keyboard Response

**Parent Requirement:** HLR-02, HLR-04, HLR-05, HLR-06

**Functional Traceability:** FR-04.01, FR-04.05, FR-05.01

**Requirement Statement**

The viewer-facing application shall visibly acknowledge a valid non-blocked keyboard action within 250 ms when the action has a visible effect.

**Acceptance Criteria**

Measure from key activation to first corresponding UI response; all 20 valid actions under the reference conditions meet the limit. Playback completion is measured separately.

### NFR-02 — Channel Activation Latency

**Parent Requirement:** HLR-02

**Functional Traceability:** FR-02.05, FR-02.07, FR-02.09, FR-11.07

**Requirement Statement**

At least 90% of valid channel changes shall begin actual video presentation within 3 s of the final valid selection in the reference environment, including any channel rate-limit wait.

**Acceptance Criteria**

At least 18 of 20 successful eligible selections meet the target; measure from confirmation or navigation command to first frame of the requested service. No superseded selection is counted as a completed change.

### NFR-03 — Minimum Channel Change Interval

**Parent Requirement:** HLR-02

**Functional Traceability:** FR-02.07, FR-02.09

**Requirement Statement**

Successive channel activation initiations shall be separated by at least 500 ms.

**Acceptance Criteria**

For a burst of valid selections, initiation timestamps are at least 500 ms apart; the last valid queued selection is initiated after the interval expires, subject to no newer selection. The wait is included in NFR-02.

### NFR-04 — Rewind Completion Latency

**Parent Requirement:** HLR-05

**Functional Traceability:** FR-05.02, FR-05.04, FR-05.10

**Requirement Statement**

At least 90% of eligible confirmed rewind operations shall begin presentation of the requested historical content within 5 s.

**Acceptance Criteria**

At least 18 of 20 successful eligible operations meet the target, measured from explicit confirmation to first actual frame of the new playback; selection time is excluded.

### NFR-05 — Quality Switch Latency

**Parent Requirement:** HLR-06

**Functional Traceability:** FR-06.02, FR-06.03, FR-06.04

**Requirement Statement**

At least 90% of eligible quality changes shall begin presentation of the newly selected variant within 5 s.

**Acceptance Criteria**

At least 18 of 20 successful eligible switches meet the target; measure from quality selection to first frame of the new variant, in both live and rewind modes.

### NFR-06 — Go Live Completion Latency

**Parent Requirement:** HLR-07

**Functional Traceability:** FR-07.03, FR-08.03

**Requirement Statement**

At least 90% of eligible Go Live operations shall begin live presentation within 5 s.

**Acceptance Criteria**

At least 18 of 20 successful eligible operations meet the target, measured from Go Live command to actual live presentation.

### NFR-07 — Cached Metadata UI Latency

**Parent Requirement:** HLR-01, HLR-03

**Functional Traceability:** FR-01.03, FR-03.02

**Requirement Statement**

A service list or TV Guide with valid locally available metadata shall become visible within 1 s of its open command.

**Acceptance Criteria**

All 10 openings of each control meet the limit. Network retrieval time is excluded only when valid local data is already present.

### NFR-08 — Metadata Request Deadline

**Parent Requirement:** HLR-01, HLR-03, HLR-08

**Functional Traceability:** FR-01.01, FR-03.01, FR-08.05

**Requirement Statement**

A metadata retrieval attempt shall reach a successful or failed terminal result within 5 s.

**Acceptance Criteria**

Measure from request initiation through validated response or failure recognition; unreachable and nonresponding servers reach the failure state by the deadline. Retry remains a separate attempt.

## Temporal accuracy and media quality

### NFR-09 — Rewind Position Accuracy

**Parent Requirement:** HLR-05, HLR-09

**Functional Traceability:** FR-05.03, FR-05.04, FR-05.08, FR-09.05

**Requirement Statement**

The first presented video frame after a successful rewind shall lie within 2 s of the effective target position on the service broadcast timeline.

**Acceptance Criteria**

For each tested eligible rewind, absolute error |actual first presented content position − effective target| is at most 2 s; clamp is applied before calculating error. The reference position is the content presented at confirmation, not the latest received packet.

### NFR-10 — Quality Switch Position Accuracy

**Parent Requirement:** HLR-06, HLR-09

**Functional Traceability:** FR-06.03, FR-09.07

**Requirement Statement**

The first presented frame of a successfully selected quality shall differ by at most 2 s from the content position presented when the quality selection was made.

**Acceptance Criteria**

Measure absolute difference on the shared broadcast timeline in both live and rewind modes; do not add switch interruption duration to the reference. The permitted discontinuity is not permission to return to live during historical playback.

### NFR-11 — Live Delay Indicator Accuracy

**Parent Requirement:** HLR-07

**Functional Traceability:** FR-07.01, FR-07.02

**Requirement Statement**

In stable rewind playback, the displayed delay magnitude shall differ from the independently determined live-minus-presented content position by no more than 2 s.

**Acceptance Criteria**

Compare displayed seconds against the same service broadcast timeline during stable playback, including repeated rewind and quality change. In live mode display LIVE rather than physical transport latency.

### NFR-12 — Live Delay Indicator Update

**Parent Requirement:** HLR-07

**Functional Traceability:** FR-07.02

**Requirement Statement**

The rewind delay indicator shall have one-second display resolution and shall be refreshed at least once per second during stable playback.

**Acceptance Criteria**

The indicator uses negative HH:MM:SS formatting with one-second resolution; observed refresh intervals do not exceed 1 s; once actual live playback begins it displays LIVE within 1 s.

### NFR-13 — Live Edge Accuracy

**Parent Requirement:** HLR-02, HLR-07, HLR-09

**Functional Traceability:** FR-02.05, FR-07.03, FR-09.03, FR-09.04

**Requirement Statement**

Under reference conditions, stable live playback shall present content no more than 5 s behind the current simulated live position.

**Acceptance Criteria**

After a 5 s settling interval following actual playback start, observe live-minus-presented position for 30 s; it remains between 0 and 5 s. This is a measured physical delay, not the user-facing rewind indicator.

### NFR-14 — A/V Synchronization

**Parent Requirement:** HLR-02, HLR-09, HLR-11

**Functional Traceability:** FR-02.05, FR-09.04, FR-09.05, FR-11.05

**Requirement Statement**

During stable playback of valid supported test media, absolute audio/video presentation skew shall not exceed 150 ms.

**Acceptance Criteria**

Use independently identifiable synchronized audio/video cues; measure at least 10 cues across live and historical playback in both qualities. Each measured absolute skew is at most 150 ms. Exclude declared transition intervals only.

### NFR-15 — Cross-Variant Timeline Alignment

**Parent Requirement:** HLR-06, HLR-09

**Functional Traceability:** FR-06.03, FR-09.07, FR-09.10, FR-09.11

**Requirement Statement**

Corresponding content events in the High and Low prepared variants shall map to the shared programme timeline within 100 ms of each other.

**Acceptance Criteria**

Independently compare identifiable content events in both variants at programme start, middle and end for every demonstration programme; maximum difference is 100 ms. This is media alignment, not switch latency or seek error.

## Reliability, recovery and resources

### NFR-16 — Playback Operation Deadline

**Parent Requirement:** HLR-02, HLR-05, HLR-06, HLR-07, HLR-08

**Functional Traceability:** FR-02.05, FR-05.10, FR-06.06, FR-07.03, FR-08.01, FR-08.06

**Requirement Statement**

Each non-superseded playback activation, rewind, quality change or Go Live operation shall reach success or a recognized failure within 10 s of the initiating user command, including rate-limit delay where applicable.

**Acceptance Criteria**

Success requires actual presentation. If no success is possible by the deadline, the operation leaves pending state and reports failure; obsolete completion cannot later mark it successful. Recovery attempts are separately bounded by NFR-17.

### NFR-17 — Recovery Completion Deadline

**Parent Requirement:** HLR-08

**Functional Traceability:** FR-06.06, FR-08.02, FR-08.03, FR-08.07

**Requirement Statement**

Following recognized playback failure, automatic restoration, if attempted, shall finish or enter the unavailable state within 10 s.

**Acceptance Criteria**

Measure from failure recognition to actual restored playback or unavailable state with applicable recovery controls. No unbounded recovery loop; manual retry or Go Live follows NFR-16.

### NFR-18 — Loss of Media Detection

**Parent Requirement:** HLR-08, HLR-09

**Functional Traceability:** FR-08.01, FR-08.02, FR-08.07

**Requirement Statement**

During otherwise stable playback, complete loss of usable incoming media shall be recognized within 5 s after the last usable received media data, under a deliberate complete-loss test.

**Acceptance Criteria**

The receiver reports a playback failure and starts the agreed recovery behavior within the deadline; temporary packet loss that does not interrupt playback is not equivalent to complete loss.

### NFR-19 — Obsolete Delivery Termination

**Parent Requirement:** HLR-09, HLR-11

**Functional Traceability:** FR-09.09, FR-11.08

**Requirement Statement**

Once an established delivery is explicitly superseded or its session is released, the server shall cease sending media for that obsolete delivery within 2 s under reference network conditions.

**Acceptance Criteria**

Server-side observation shows no further media transmission for the obsolete delivery after the deadline; any already-in-flight packets do not become active playback. Abrupt network disconnection cleanup policy is deferred to TR.

### NFR-20 — Session and Resource Stability

**Parent Requirement:** HLR-08, HLR-09, HLR-11

**Functional Traceability:** FR-08.06, FR-09.09, FR-11.08

**Requirement Statement**

The reference system shall complete 100 mixed sequential and superseding playback operations without process crash, unrecoverable deadlock or accumulation of abandoned active media sessions.

**Acceptance Criteria**

Exercise zap, rewind, quality change and Go Live, including superseding operations; after final release and cleanup deadline, no active media delivery or session-owned playback resource remains. Record resource observations; no numerical heap limit is imposed.

### NFR-21 — Continuous Playback Stability

**Parent Requirement:** HLR-09, HLR-11

**Functional Traceability:** FR-09.10, FR-09.11, FR-11.05

**Requirement Statement**

The system shall sustain 30 minutes of playback under reference conditions without unexpected termination or unrecovered stall, including at least one programme boundary and one playlist-cycle boundary.

**Acceptance Criteria**

Use a suitable demonstration playlist; verify presentation advances through boundaries, with no unplanned interruption exceeding 3 s. Planned user-triggered transitions and injected failures are excluded.

### NFR-22 — Parser Correctness

**Parent Requirement:** HLR-10

**Functional Traceability:** FR-10.01, FR-10.02, FR-10.03, FR-10.04, FR-10.05

**Requirement Statement**

Project-owned PAT/PMT parsing shall produce correct mandatory fields for every valid reference transport stream and shall not report invalid or incomplete tables as valid.

**Acceptance Criteria**

All reference cases agree with independent inspection for program-to-PMT mappings, PCR PID, elementary stream PIDs and stream types; incomplete and malformed reference cases do not produce false success. Actual received TS, not only local source files, is verified.

### NFR-23 — Parser Result Availability

**Parent Requirement:** HLR-10

**Functional Traceability:** FR-10.01, FR-10.02, FR-10.03, FR-10.05

**Requirement Statement**

For valid demonstration media, inspectable PAT/PMT results shall be available within 2 s after receipt of the complete valid PAT and corresponding PMT required for parsing.

**Acceptance Criteria**

Measure from receipt of the last required complete table to availability of the correct inspectable result; no claim that the sender repeats tables within this deadline. New stream results must be attributable to the correct stream.

## Usability and data freshness

### NFR-24 — Information Dialog Duration

**Parent Requirement:** HLR-02

**Functional Traceability:** FR-02.06

**Requirement Statement**

The service information dialog shall remain visible for 3 s after it is shown unless dismissed earlier or replaced by a newer service selection.

**Acceptance Criteria**

Automatic dismissal occurs within 3 s ± 0.5 s; manual dismissal remains available; a replacement dialog receives its own duration.

### NFR-25 — Boundary Notice Duration

**Parent Requirement:** HLR-05, HLR-08

**Functional Traceability:** FR-05.08, FR-05.11

**Requirement Statement**

Nonblocking rewind boundary notices shall be visible for 2 s unless replaced or dismissed earlier.

**Acceptance Criteria**

Notice disappears automatically within 2 s ± 0.5 s and does not require confirmation or block mandatory controls. Actual playback failure notifications are governed separately.

### NFR-26 — Metadata Freshness

**Parent Requirement:** HLR-01, HLR-03

**Functional Traceability:** FR-01.05, FR-03.04, FR-08.05

**Requirement Statement**

Successful metadata retrieval age shall be tracked against the approved 24 h service catalogue and 6 h selected-service EPG refresh periods.

**Acceptance Criteria**

Startup triggers retrieval; during operation the catalogue is due at 24 h and selected-service EPG at 6 h; selection with missing or stale EPG triggers retrieval. Failed retrieval does not reset data age or mark stale data fresh. Verification may use controlled time advancement.

### NFR-27 — EPG Coverage Consistency

**Parent Requirement:** HLR-03, HLR-09

**Functional Traceability:** FR-03.03, FR-09.02, FR-09.10

**Requirement Statement**

The selected-service EPG shall cover the approved preceding and following 24 h schedule interval as of the successful retrieval time, with schedule entries consistent with the simulated broadcast timeline.

**Acceptance Criteria**

Compare server-provided schedule and receiver-visible records with independently derived playlist schedule; no missing intersecting entries within the available schedule interval, no wrong-service entries, and correct start/end times. Scheduled pre-start entries, if supplied, do not imply historical playback availability.

## Architecture, interoperability and reproducibility

### NFR-28 — Protocol and Container Conformance

**Parent Requirement:** HLR-09, HLR-10

**Functional Traceability:** FR-09.01, FR-09.02, FR-09.04, FR-09.05, FR-10.01

**Requirement Statement**

The system shall preserve the approved separation of RTSP playback control, MPEG-TS over RTP media delivery and OTT MW REST metadata delivery.

**Acceptance Criteria**

Independent network inspection demonstrates each path and valid MPEG-TS RTP payload for live and historical playback; separate elementary-stream RTP alone is not acceptable. Exact RTSP version and protocol subset are deferred to TR.

### NFR-29 — Layer and Platform Separation

**Parent Requirement:** HLR-10, HLR-11

**Functional Traceability:** FR-10.01, FR-10.02, FR-11.01, FR-11.04, FR-11.05

**Requirement Statement**

Project-owned DTV logic shall be separated from viewer UI and platform-specific playback/network integration through documented interfaces consistent with the course layered model.

**Acceptance Criteria**

Design/code review identifies application, DTV middleware, platform and relevant HAL boundaries; project-owned parsing and service/timeline logic are identifiable independently of UI and player APIs. Platform adaptation is documented; a second target port is not required.

### NFR-30 — Target Environment Compatibility

**Parent Requirement:** HLR-11

**Functional Traceability:** FR-11.01, FR-11.02, FR-11.03, FR-11.09

**Requirement Statement**

All mandatory viewer functions shall be demonstrable in one documented Android TV runtime with functioning TIF, TV Provider and a compatible media pipeline.

**Acceptance Criteria**

Record system image, Android version, ABI and actual input registration, channel/programme publication and playback evidence; verify all mandatory controls. System Live TV app remains preferred, with approved conditional alternative if infeasible.

### NFR-31 — Reproducible Build and Deployment

**Parent Requirement:** HLR-09, HLR-11

**Functional Traceability:** FR-09.04, FR-11.01, FR-11.09

**Requirement Statement**

The project shall provide a documented reproducible build, installation and launch procedure for the receiver and both server components from a clean checkout with stated prerequisites.

**Acceptance Criteria**

A team member following the documented procedure can build/install/start the system and execute the demonstration without undocumented configuration edits; tool/library versions and development versus runtime environments are recorded. No exact tools are selected by this NFR.

### NFR-32 — Verification Evidence and Observability

**Parent Requirement:** HLR-01–HLR-11

**Functional Traceability:** All approved FRs

**Requirement Statement**

The project shall provide independently reviewable evidence for mandatory functional and non-functional verification.

**Acceptance Criteria**

Record requirement/test identifiers, inputs, actual results and pass/fail/not executed/not verifiable status; record relevant logs, versions, media identity/checksum and independently inspected expected results. Build success alone is not functional proof; own parser results remain inspectable.

### NFR-33 — Server Replaceability Boundary

**Parent Requirement:** HLR-09, HLR-11

**Functional Traceability:** FR-09.01, FR-09.02, FR-09.04, FR-09.05

**Requirement Statement**

The receiver shall keep production-side connection configuration and service integration boundaries separate from viewer-facing logic.

**Acceptance Criteria**

A compatible server endpoint can be configured without modifying viewer UI or parser logic; interface assumptions and supported protocol subset are documented. This does not claim compatibility with an arbitrary commercial head-end or require a real provider demonstration.

## Scope exclusions and deferred decisions

No mandatory multi-client load capacity, production uptime SLA, DRM/authentication, public Internet exposure, second platform port, persistent user settings or fixed CPU/RAM/bitrate limits are introduced. The controlled demonstration network is the proposed operating scope, not a security assurance. TR must define compatible media formats, libraries, RTSP version, PCR/PTS and seek mechanism, session cleanup on disconnect, runtime image and resource ownership. OQ-08 remains open. If measurements or feasibility show a proposed/approved target is impractical, seek team approval for a documented revision rather than silently relaxing it.

Sources: approved FR baseline; original M14 task and instructor project instructions (measurable criteria, layered architecture, signal/metadata paths, asynchronous operations and reproducible verification); course lectures 2–7. The 150 ms A/V skew target is motivated by Lecture 3's approximate perceptual discussion, not a normative course limit. Other new thresholds are engineering proposals.
