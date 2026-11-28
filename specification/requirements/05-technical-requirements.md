# Technical Requirements — Draft 0.4.0-dev.1

Status: **Proposed — team review required**. Parent: approved baseline 0.3.0 (7 US, 11 HLR, 75 FR, 33 NFR). Mandatory new instructor constraint: project-owned receiver middleware in C++. Android TV + TIF and hybrid RTSP/server-side seeking remain mandatory. Other selections are delegated engineering decisions, subject to feasibility verification. No implementation or PoC has been executed. This document defines technical obligations and interface contracts; full component diagrams, class contracts and test cases remain separate Design and Test Specifications.

## Architecture basis and selected profile

Course Lectures 5–7 define application/middleware/platform/HAL separation, producer-consumer buffering, platform glue and TIF service activation; Lecture 7 Figure 2 places IPTV TV Input and IPTV client in DTV middleware, with TV Input Manager and TV Provider. Lecture 4 separates RTP media, RTSP control and REST metadata. Lecture 3 defines TS/PAT/PMT and media timing. The concrete GStreamer, C++17, Java/JNI, FastAPI, SQLite and RTSP resource-URI profile below are project engineering choices, not technologies prescribed by lectures. Comedia/Nexus/Tuner SDK examples are not imported into managed IPTV.

Primary stack: C++17 native receiver core and C++ head-end streaming process; Java Android/TIF glue and UI; GStreamer native media platform; RTSP 1.0 TCP control, RTP/RTCP UDP unicast MP2T; Python FastAPI metadata server; libavformat for indexed source reads and FFmpeg/ffprobe preparation; libcurl/nlohmann/json/SQLite native adapters; CMake/CTest, Android Gradle/NDK. Exact compatible versions locked at feasibility gate. System TV app preferred conditionally; project-owned TvView application remains fallback subject to team approval.

Important implementation profile: each confirmed seek creates a new RTSP presentation resource identified by the target on the common broadcast timeline. This avoids requiring a generic RTSP library to implement arbitrary seekable-live Range across a cyclic playlist. RTSP remains the control protocol; actual server-side seeking is mandatory. The resource URI profile is project-specific and must be documented for replaceability. PCR is useful for clock/TS validation; PTS and verified random access index are the primary seek and presentation mapping references. No PCR-only assumption.

## Requirements

## Environment and architecture

### TR-01 — Native middleware

**Requirement Traceability:** FR-01.01–01.05, FR-02.05–02.09, FR-05.01–05.11, FR-06.01–06.06, FR-07.01–07.03, FR-08.01–08.07, FR-10.01–10.05; NFR-29

**Requirement Statement**

Implement the receiver's service model, metadata cache policy, playback state machine, command arbitration, timeline arithmetic and PAT/PMT parser in portable C++17. Java shall contain UI, TIF and Android integration only. Python is confined to the server-side metadata service and preparation tooling. Use CMake for native targets and CTest for host tests.

**Acceptance Criteria**

A host build exercises the core without Android, JNI or GStreamer headers; Android links the same core into its JNI library.

### TR-02 — Reference development and runtime

**Requirement Traceability:** FR-11.01–11.09; NFR-30,31

**Requirement Statement**

Use Linux Ubuntu 24.04 LTS for head-end and native host development; Android Studio/Gradle, JDK 17, Android NDK with CMake for receiver. Initial runtime candidate is an Android TV API 28 x86_64 AVD with hardware acceleration; select an available TIF-capable image by the feasibility gate, not by API number alone. Pin exact compatible SDK/NDK/AGP/GStreamer versions and ABI in a checked-in environment manifest. Use emulator host alias 10.0.2.2 for host servers, configurable LAN IP on devices; verify UDP reachability rather than assume emulator NAT permits it.

**Acceptance Criteria**

Record actual image fingerprint, feature declarations, packages, ABI, network route, tool versions and successful launch; an ordinary phone image is not acceptable.

### TR-03 — Layer contracts and platform glue

**Requirement Traceability:** FR-09.04–09.05, FR-10.01–10.05, FR-11.04–11.08; NFR-28,29,33

**Requirement Statement**

Use application (Live TV UI), application layer/DTV middleware (TIF adapter + native IPTV core), platform glue (network, storage, GStreamer, Android surface adapters), platform/PAL/OS (Android/Linux and media libraries), underlying HAL/drivers. Define C++ ports IPlayback, IMetadataClient, ICache, IClock, IEventSink with standard value types; no JNI/GStreamer types cross into the portable core. TIF is the Android service boundary, not the decoder; no broadcast tuner SDK or custom HAL is required.

**Acceptance Criteria**

Dependency review and host tests demonstrate platform independence; media, metadata and C&C interfaces remain distinct.

### TR-04 — Build and repository organization

**Requirement Traceability:** FR-09.01–09.11, FR-11.09; NFR-31,32

**Requirement Statement**

Use receiver/android, receiver/native/{core,platform,tests}, servers/streaming, servers/metadata, media-tools, config, tests, scripts, specification. Use Gradle wrapper and pinned dependency manifests, CMake targets for host and Android, Python virtual environment with pinned requirements. Keep generated media, binaries, secrets and local SDK paths out of Git; include licensed reproducible synthetic media generation scripts and hashes.

**Acceptance Criteria**

Clean checkout builds host tests, server and APK using documented commands; exact dependencies and launch order are recorded.

### TR-05 — Feasibility gate and UI selection

**Requirement Traceability:** FR-04.05, FR-11.01–11.09; NFR-28,30,31

**Requirement Statement**

Before full implementation, verify system Live TV app registration/setup, channel publishing, actual MP2T playback, native surface, custom key routing, selected-service EPG and all M14 controls. Prefer existing system Live TV app only if all mandatory functions are achievable without privileged modifications. Otherwise propose the already conditional project-owned Java Live TV activity using TvView and the same registered TvInputService; obtain team approval for that fallback. Never bypass TIF or replace media with HLS or elementary RTP. Confirm actual first-render and timestamp observability in the same spike.

**Acceptance Criteria**

Save ADB/logcat, packet capture and frame/time evidence. If no candidate satisfies the gate, report blocker and seek decision; do not mark requirements fulfilled.

## Receiver application and TIF

### TR-06 — TIF registration and setup

**Requirement Traceability:** FR-11.01, FR-11.02, FR-11.09; NFR-30

**Requirement Statement**

Declare an exported TvInputService protected by android.permission.BIND_TV_INPUT, with android.media.tv.TvInputService action and android.media.tv.input metadata XML plus setup activity. Declare INTERNET and appropriate TV features/permissions for the chosen image; request only allowed permissions. Discover input using TvInputManager and verify actual provider access and channel visibility.

**Acceptance Criteria**

Installed APK exposes its own input; setup and channel publication succeed under actual non-root application permissions.

### TR-07 — Session and surface lifecycle

**Requirement Traceability:** FR-11.04–11.08; NFR-19,20,30

**Requirement Statement**

Implement Java Session callbacks onTune, onSetSurface, onSetStreamVolume, onRelease. Resolve channel URI through owned provider rows to stable service ID; onTune returns acceptance only, queues native command. Convert Surface with ANativeWindow_fromSurface in platform glue; retain/release native reference on pipeline owner thread after sink detachment. Null surface stops presentation and cancels pending presentation success; release invalidates generation and stops delivery. Reattach resumes according to active mode when feasible.

**Acceptance Criteria**

Tune, surface replacement/null and release are exercised; notifyVideoAvailable only after verified rendering, notifyVideoUnavailable on tune/loss; no callback after release.

### TR-08 — JNI ownership and event delivery

**Requirement Traceability:** FR-08.01, FR-08.06, FR-11.04–11.08; NFR-16,20,29

**Requirement Statement**

Use opaque jlong handles backed by registry-owned shared session objects with explicit create/release; JNI entry points copy Java strings/arrays before return, validate handle and generation, never retain local references. Worker callbacks attach/detach JVM as needed; post to main Handler through weak/managed Java references. Expose nativeTune, nativeRewind, nativeSelectQuality, nativeGoLive, nativeSetVolume, nativeSetSurface, nativeRelease and immutable snapshots/events. Release joins or asynchronously drains owned work without blocking UI indefinitely.

**Acceptance Criteria**

Repeated creation/release and stale callback tests show no invalid handles, leaked references, or UI-thread network waits.

### TR-09 — Viewer commands and UI model

**Requirement Traceability:** FR-01.03, FR-02.01–02.06, FR-03.02–03.03, FR-04.01–04.05, FR-05.01–05.10, FR-06.01–06.04, FR-07.02–07.03, FR-08.02–08.04; NFR-01,07,24,25

**Requirement Statement**

Use a thin Java View/Presenter: approved keyboard mapping in FR-04.05, modal focus, numeric buffer committed by Enter, invalid number silently ignored, first Backspace selects 30 s and subsequent presses increment. Render immutable native playback snapshot; never infer success from button press. Info dialog 3 s, boundary notice 2 s, error remains actionable. In custom activity use TvView.tune for channel selection and sendAppPrivateCommand with namespaced actions for rewind/quality/live; session handles onAppPrivateCommand. Use same-app explicitly scoped event bridge for custom snapshots (not hidden TIF APIs); system app feasibility must demonstrate equivalent controls. UI must not invoke a second bypass player.

**Acceptance Criteria**

All approved keys and dialog contexts work without mouse; selection alone never seeks; actual quality and LIVE/delay indicators match core state.

### TR-10 — Audio settings

**Requirement Traceability:** FR-04.02–04.04, FR-11.06; NFR-14

**Requirement Statement**

Keep session volume scalar [0,1] and mute flag in native model; effective gain = mute ? 0 : volume times framework gain where separately applicable. TIF onSetStreamVolume maps framework gain to adapter; custom app sends volume via TvView.setStreamVolume or namespaced command with one canonical owner, avoiding double attenuation. Apply GStreamer volume element before audio sink, retain settings across new pipelines.

**Acceptance Criteria**

Volume/mute survive zap, rewind, quality and Go Live; no duplicate gain multiplication and audible output follows controls.

### TR-11 — TV Provider synchronization

**Requirement Traceability:** FR-01.02–01.05, FR-03.01–03.04, FR-11.02–11.03; NFR-26,27,30

**Requirement Statement**

Use ContentResolver and TvContract.Channels/Programs; map immutable serviceId to owned row using input ID and internal provider data; channel number/name from catalogue. Reconcile validated full snapshot by stable key, not delete-all/reinsert. Publish EPG by channel row and stable occurrence ID, UTC start/end epoch milliseconds; remove obsolete owned entries only after valid snapshot. Use applyBatch where supported, serialize provider writes, and never mutate other inputs' rows.

**Acceptance Criteria**

Repeated synchronization creates no duplicates; channel URI remains stable across metadata updates; provider matches native cache and selected-service EPG.

## Metadata and simulated head-end

### TR-12 — Shared broadcast clock and configuration

**Requirement Traceability:** FR-03.03, FR-09.01–09.03, FR-09.06–09.07, FR-09.10–09.11; NFR-15,27,33

**Requirement Statement**

Run independent C++ RTSP server and Python FastAPI REST server from a shared immutable validated configuration: schemaVersion, broadcastInstanceId, epochUtcMs, service IDs/numbers/names, ordered programme assets, durations, two variants and paths. Launcher generates one epoch/instance and passes it to both; use steady-clock elapsed time anchored to epoch on streaming server, UTC for EPG; clock changes do not reset running media timeline. REST derives schedule from the same epoch/config; document clock synchronization assumptions. Restart creates a new instance and invalidates old playback mapping; catalogue revision separate from instance ID.

**Acceptance Criteria**

Servers agree on timeline, occurrence IDs and schedule; receiver rejects stale instance anchors after restart.

### TR-13 — Global timeline mapping

**Requirement Traceability:** FR-05.03, FR-05.08, FR-07.01, FR-09.03, FR-09.10–09.11; NFR-09–13

**Requirement Statement**

Represent content position as signed 64-bit microseconds since broadcast epoch. For each service compute prefix sums of programme durations and total cycle D; for t>=0, cycle=floor(t/D), local=t mod D, find half-open programme interval [prefix_i,prefix_i+duration_i), asset offset=local-prefix_i. No reset at cycle boundary; history [0,live] only; reject future historical targets. Clamp only when an earlier playable point exists; no additional rewind at earliest position. Schedule IDs include service, cycle and programme index.

**Acceptance Criteria**

Boundary, large-time, repeated-cycle, clamp and restart unit tests produce deterministic positions; no duplicated physical media per cycle.

### TR-14 — REST contract

**Requirement Traceability:** FR-01.01–01.05, FR-03.01–03.04, FR-08.05, FR-09.01–09.02, FR-09.06; NFR-08,26,27,28,33

**Requirement Statement**

FastAPI HTTP JSON API v1: GET /v1/services returns schemaVersion, complete=true, revision, broadcastInstanceId, epochUtcMs, serverUtcMs and services[{id,number,name,variants:[{id, label,rtspUri}]}]; GET /v1/services/{id}/epg returns instance, coverageStartUtcMs, coverageEndUtcMs, generatedAtUtcMs, programmes[{occurrenceId,title,startUtcMs,endUtcMs}]; GET /v1/clock returns instance, epochUtcMs, livePositionUs, serverUtcMs. 200 valid full data; 404 unknown service; 503 temporarily unavailable; invalid schema treated as failed. Clock endpoint is timing metadata only, not stream control. EPG covers intersecting entries in ±24 h at retrieval, including scheduled pre-start entries if configured; no implied history. OpenAPI generated and checked in.

**Acceptance Criteria**

Independent requests verify schema, stable IDs, UTC time, selected-service coverage and error behavior.

### TR-15 — Native metadata client and cache

**Requirement Traceability:** FR-01.01–01.05, FR-03.01–03.04, FR-08.05; NFR-07,08,26,27

**Requirement Statement**

Use libcurl and nlohmann/json in C++ platform adapter (pin versions, verify Android ABI); SQLite via native adapter for last valid snapshots, or bundled SQLite if system access unsuitable. Atomic transactions after full validation, monotonic in-process age and persisted UTC retrieval time with clock-anomaly stale handling. Boot fetch catalogue and selected EPG; 24 h catalogue, 6 h selected EPG, missing/stale on selection, no mandatory all-channel prefetch. HTTP total timeout 5 s including validation budget; cancel obsolete requests, retain valid cache on failure; provider is Android projection, not authoritative cache.

**Acceptance Criteria**

Offline startup, invalid JSON, complete-list removal, stale cache and controlled clock tests verify policies without duplicate rows.

### TR-16 — Clock correlation

**Requirement Traceability:** FR-07.01–07.02, FR-09.03; NFR-11–13

**Requirement Statement**

Sample /v1/clock at startup and during playback (initial proposed every 10 s, independent of EPG refresh); estimate server live anchor at client monotonic midpoint of request/response, extrapolate with steady clock; reject stale instance or high RTT samples (>500 ms initial engineering setting). Track anchor age and uncertainty, never fabricate exact position if invalid; retry within metadata deadline and expose unavailable/error if position cannot be established. Clock sync is not PCR recovery.

**Acceptance Criteria**

Compare derived live position with independent server timeline; validate under reference network and clock changes; tune interval only within approved NFRs.

## Media preparation and RTSP head-end

### TR-17 — Prepared media profiles

**Requirement Traceability:** FR-09.04–09.07, FR-09.10–09.11, FR-10.01–10.05; NFR-09,10,14,15,21,22

**Requirement Statement**

Generate at least two services with cyclic programmes, H.264 AVC 8-bit yuv420p, AAC-LC stereo 48 kHz, 25 fps, High 1280x720 ~2.5 Mbps and Low 640x360 ~0.8 Mbps; same source timeline and duration, closed GOP/IDR every 1 s, repeated SPS/PPS, consistent SPTS program/PIDs across assets and variants. Use FFmpeg/ffprobe and synthetic visible timecode plus audio sync cues. These are initial profiles, adjust only after decoder/accuracy verification. Store one TS per programme/quality, not HLS segments.

**Acceptance Criteria**

Verify codec, GOP spacing, actual durations, PAT/PMT and cross-variant alignment before serving; reject mismatched assets.

### TR-18 — Random access index

**Requirement Traceability:** FR-05.04, FR-06.03, FR-09.05, FR-09.07; NFR-09,10,15

**Requirement Statement**

Offline media preparation records per-asset duration, source hash, video random-access PTS and seek location via FFmpeg/ffprobe or libavformat. Normalize PTS to asset timeline; PCR assists validation and optional coarse seek but is not sole seek key. Store sorted index with timeUs and demux seek timestamp/packet offset when verified. Choose greatest valid random-access time <= local target; handle start and wrap/discontinuity explicitly. No raw byte cut without complete codec/table context.

**Acceptance Criteria**

Index points verified against decoded frames; binary search gives bounded backward error with prepared 1 s GOP; index invalidated on asset hash change.

### TR-19 — Server source and remux

**Requirement Traceability:** FR-09.04–09.05, FR-09.10–09.11; NFR-09,10,14,21,28

**Requirement Statement**

Use C++ streaming service with libavformat/libavcodec utility APIs for source demux and indexed seek (no re-encoding at runtime), feed parsed H.264/AAC buffers through GStreamer appsrc branches into h264parse/aacparse, mpegtsmux, rtpmp2tpay name=pay0 pt=33. Maintain one continuous session timeline by rescaling and rebasing PTS/DTS across programme/cycle boundaries, keep A/V relative offsets, handle preroll and codec config, mux repeated PAT/PMT/PCR. Use bounded appsrc queues and clock-paced output, never burst entire file. Stop or hold at live edge rather than send unbroadcast media.

**Acceptance Criteria**

Packet capture proves MP2T; decoded sequence crosses boundaries without timestamp reset, A/V drift or unexpected EOS; inspect PAT/PMT and decoder entry.

### TR-20 — RTSP profile and session start seeking

**Requirement Traceability:** FR-05.04, FR-06.02–06.03, FR-07.03, FR-09.04–09.09; NFR-02,04–06,09,10,19,28

**Requirement Statement**

Use RTSP 1.0 over TCP, RTP/RTCP UDP unicast, SDP MP2T/90000 payload 33. Use gst-rtsp-server non-shared media per playback generation. Simplify seekable-live integration by using a new RTSP presentation URI per operation: /v1/play/{serviceId}/{quality}/{requestId}/live or /v1/play/{serviceId}/{quality}/{requestId}/at/{targetUs}. DESCRIBE/SETUP/PLAY start a presentation whose NPT zero is the server-selected historical random-access start (or latest live start); no arbitrary in-session Range support required by this profile. Server resolves target on session preparation and validates again at PLAY, sends standard RTSP response or error; use TEARDOWN on old session. Use standard Range npt=0- if client sends it; do not falsely advertise arbitrary live NPT seeking. URI selector is an explicit project-specific RTSP resource contract, not a standardized timeshift URI. No REST command is used to control playback. In-session PLAY Range on global timeline is an optional alternative only after feasibility and team review.

**Acceptance Criteria**

Packet trace shows RTSP session lifecycle and server-side seek on each confirmed rewind; historical and live deliveries use actual RTP MP2T. Invalid targets or unavailable resources never silently select unrelated content.

### TR-21 — Start anchor metadata

**Requirement Traceability:** FR-05.03–05.04, FR-06.03, FR-07.01, FR-09.05; NFR-09–13

**Requirement Statement**

Expose read-only GET /v1/playback-anchors/{requestId} on streaming server HTTP companion (or shared metadata service via read-only registry), returning instanceId, serviceId, quality, requestId, requestedUs, effectiveUs, actualStartUs, firstVideoPts90k, clock mapping and status. This is observation only; RTSP alone creates/controls sessions. Publish anchor before first media or allow bounded polling within operation deadline; 404 pending/unknown distinguished by client context; expire after session cleanup. Bind anchor to actual mux output timestamp domain, not original file PTS. If GStreamer remux changes offsets, measure first emitted video PES PTS from output or configure and verify mapping. No assumption that RTP timestamp equals video PTS.

**Acceptance Criteria**

Anchor corresponds to independently inspected emitted TS and correct generation; no successful presentation position is reported without valid mapping.

### TR-22 — Server lifecycle and failure

**Requirement Traceability:** FR-08.06–08.07, FR-09.08–09.09; NFR-16–20

**Requirement Statement**

Each requestId owns session source, mux, transport and cancellation token; one active delivery per viewer in steady state. TEARDOWN and explicit supersession cancel producer, remove media transport and release resources within 2 s; disconnect/RTSP session timeout cleanup bounded and documented (initial 10 s idle cleanup; keepalive configured below timeout where feasible). Reject unsupported quality/service, invalid timestamps and invalid instance with protocol errors; log reason and requestId. Do not share a seekable source among unrelated sessions.

**Acceptance Criteria**

Repeated switching and teardown show no orphan sender; injected failures reach terminal state. Verify actual library timeout/cleanup behavior.

### TR-23 — Media and metadata evidence separation

**Requirement Traceability:** FR-09.01–09.11, FR-10.03; NFR-28,32

**Requirement Statement**

Maintain distinct production services and TCP/UDP endpoints; log server selected asset, cycle, target/effective/actual time, RTP stream identity, requestId and start/end events. Use read-only anchor/clock metadata without substituting HTTP media delivery. Store packet capture and test content hashes separately.

**Acceptance Criteria**

Network trace distinguishes REST JSON, RTSP control and RTP MPEG-TS; logs correlate requested and served media.

## Native playback, timing and parser

### TR-24 — RTP reception and native pipeline

**Requirement Traceability:** FR-09.04–09.05, FR-10.01–10.05, FR-11.05–11.07; NFR-02,04–06,14,28,30

**Requirement Statement**

Use GStreamer Android SDK, rtspsrc protocols=udp with initial jitter latency 150 ms, dynamic pad caps MP2T/90000 -> rtpmp2tdepay -> tee; branch A queue -> appsink for own TS parser, branch B queue -> tsdemux -> h264parse/aacparse -> decodebin (prefer Android MediaCodec, software fallback if verified) -> synchronized video/audio sinks. Video sink implements GstVideoOverlay with ANativeWindow; audio branch volume then audio sink. Explicitly package required plugins including RTSP, RTP, MPEG-TS, app, codecs and sinks for chosen ABI. Do not rely on Media3 RTSP for MP2T without proof.

**Acceptance Criteria**

Actual Android runtime displays and audibly plays both variants, captures raw TS after depay, and preserves A/V sync; check required elements at startup.

### TR-25 — Producer-consumer and bounded buffers

**Requirement Traceability:** FR-08.01, FR-10.01–10.04, FR-11.08; NFR-18,20,21,23,29

**Requirement Statement**

GStreamer owns reception/decode scheduling; parser branch uses separate bounded queue and worker, no blocking streaming callbacks on UI or disk. Start with 2 MiB parser queue and 500 ms playback queue limits (tunable after measurements); parser overflow resets partial sections and records discontinuity, waits for next tables; never silently drop playback media to meet a queue target. Use ref-counted GstBuffer or copied bounded chunks with explicit ownership. Stop/flush/wake consumers on cancellation and join worker before freeing.

**Acceptance Criteria**

Stress and malformed input do not deadlock; parser backlog is bounded, playback stays independent; no stale buffer crosses generation.

### TR-26 — Project-owned PSI section assembly

**Requirement Traceability:** FR-10.01–10.05; NFR-22,23

**Requirement Statement**

Implement portable C++ TS framing and PSI assembly: sync 0x47, 188-byte packets, header PID, TEI, PUSI, adaptation field bounds, continuity counter for payload, discontinuity reset, pointer_field including tail of prior section, multiple sections per packet, section_length bounds, MPEG-2 CRC32, current_next_indicator, version/section numbers. Reject truncated/invalid data, ignore duplicates, assemble complete current table before publishing. PAT PID 0, ignore program 0 as PMT mapping; retain all program mappings. PMT selected program from SPTS and PAT mapping; parse PCR PID, program/ES descriptor lengths, stream_type and ES PID safely. No library PSI parser substitutes for own code.

**Acceptance Criteria**

Unit fixtures cover split/multiple sections, pointer, CRC, loss, duplicates, invalid lengths, version and multiple PAT entries; compare received TS results with independent tool.

### TR-27 — Parser result lifecycle

**Requirement Traceability:** FR-10.03–10.05; NFR-22,23,32

**Requirement Statement**

Reset PSI state per new generation and relevant programme structure change; stable prepared SPTS requires no mandatory continuous version monitoring. Keep parsing until complete valid PAT/PMT, publish immutable structured record with requestId, service, quality, program number, mapping, PCR PID, ES list and validation status to logs/JSON evidence; retain latest result. If unexpected structure change is signaled, reset/reacquire; parser branch may remain listening at low cost. Invalid/incomplete tables never publish valid results.

**Acceptance Criteria**

Results observable within 2 s of complete required tables, attributable to correct stream; playback library parsing is independently separate.

### TR-28 — Presented position measurement

**Requirement Traceability:** FR-05.03, FR-06.03, FR-07.01–07.02; NFR-09–13

**Requirement Statement**

Correlate server anchor actualStartUs/firstVideoPts90k with decoded video presentation timestamps and pipeline segment/running-time mapping. Normalize 33-bit PTS unwrap and reset per generation; track last actually rendered frame timestamp via sink render instrumentation or a verified clock-scheduled sink callback with presentation evidence. A pad probe before sink, received PCR, RTP timestamp or query_position alone is not proof of actual presentation. If selected sink cannot expose sufficient presentation timing, implement platform adapter instrumentation (e.g. custom clock-synchronized render sink) and validate against visible timecode; do not fake position from wall clock. Maintain thread-safe last-presented snapshot and reject stale generation.

**Acceptance Criteria**

Independently measure actual first frame and current presented position against timecode; demonstrate ±2 s targets and correct reference at confirmation. Gate on feasibility.

### TR-29 — Playback start and completion

**Requirement Traceability:** FR-06.04, FR-08.01, FR-11.07; NFR-02,04–06,16

**Requirement Statement**

Treat RTSP success, pipeline PLAYING and first received/decoded buffer as intermediate events only. Mark operation success on confirmed presentation of expected generation and valid anchor; update active service/quality/mode atomically, notify TIF video available and UI snapshot. For surface absent do not declare success. Bus ERROR/EOS unexpected, invalid anchor or deadline produces typed failure.

**Acceptance Criteria**

Trace demonstrates command acceptance precedes actual presentation; stale render cannot complete a newer request.

### TR-30 — Rewind and quality control algorithm

**Requirement Traceability:** FR-05.01–05.11, FR-06.01–06.06, FR-07.03; NFR-04–06,09,10,13

**Requirement Statement**

At confirmation sample presented position P, compute target=P-30s*n using checked integer arithmetic, clamp to earliest playable only if earlier position exists; request new RTSP generation at target. Rewind preserves active quality. Quality selection in live requests live URI, in rewind requests captured presented position; same quality no-op. Go Live requests live URI preserving quality. Record server effective/actual start, notify clamp/no-history nonblocking. Keep old playback snapshot for recovery; stop old pipeline before activating new one as default; visible transition permitted.

**Acceptance Criteria**

Repeated rewind uses fresh rendered reference; both quality modes and Go Live conform to FR and NFR; no local recording or implicit return to live.

### TR-31 — State machine and arbitration

**Requirement Traceability:** FR-02.07–02.09, FR-05.10, FR-08.01–08.07; NFR-01–06,16,17,20

**Requirement Statement**

Use single C++ control executor with monotonically increasing generation, explicit states Idle, Starting, Live, Rewind, Switching, Recovering, Unavailable, Released and separate UI selection state. Each pending operation stores kind, deadline, intended service/quality/mode and captured position; callbacks carry generation. Zap latest valid wins, 500 ms minimum initiation gap, retain last desired selection and use desired channel as rapid navigation reference; Go Live supersedes rewind/quality, not pending zap; block additional rewind/quality confirmations during transition. Cancel superseded work and ignore late events. Use steady-clock timers, not UI sleeps.

**Acceptance Criteria**

Deterministic host tests cover races, priority, rate-limit expiry, timeout and release; no obsolete completion changes current state.

### TR-32 — Error detection and recovery

**Requirement Traceability:** FR-08.01–08.07, FR-06.06; NFR-08,16–19

**Requirement Statement**

Use typed error codes for RTSP rejection, network timeout, no usable media, invalid TS/anchor, decoder/surface failure and cancellation; distinguish expected cancellation from error. Operation deadline 10 s from user command including rate limit; media loss watchdog 5 s from last usable media, recovery at most 10 s; one restoration attempt using previous mode/quality and historical position when feasible, else Unavailable with retry/zap/Go Live as applicable. On timeout invalidate generation before teardown; no unbounded retries.

**Acceptance Criteria**

Fault injection demonstrates terminal states and actionable messages; no false success, stale events or blocked controls.

### TR-33 — Resource ownership and shutdown

**Requirement Traceability:** FR-09.09, FR-11.08; NFR-19,20,29

**Requirement Statement**

Use RAII for native sessions, sockets/library handles, GstObject references, worker threads, ANativeWindow and JNI references. One pipeline owner GLib context; marshal state transitions to it; stop producers, flush queues, detach callbacks/sinks, set pipeline NULL, release references in order. Use cancellation and bounded wait; log active session/resource counts. No direct UI-thread blocking teardown.

**Acceptance Criteria**

100-operation stability and final release show no orphan active delivery or leaked session-owned resources.

## Verification and implementation gates

### TR-34 — Independent test instrumentation

**Requirement Traceability:** FR-10.03, FR-11.07; NFR-01–28,32

**Requirement Statement**

Instrument monotonic timestamps for command, dispatch, RTSP response, first actual render, parser completion, error and cleanup; use requestId/generation correlation. Prepare visible source timeline/cycle markers and synchronized audio cues, independently inspect packets/metadata with ffprobe, TSDuck or Wireshark. Keep test oracle independent of receiver estimates. Capture actual results, logs, source checksum, environment and test IDs; do not mark unexecuted tests passed.

**Acceptance Criteria**

Evidence supports approved 20-attempt metrics, accuracy, 100-operation stability and 30-minute continuity; detailed cases remain Test Specification.

### TR-35 — Implementation compatibility gates

**Requirement Traceability:** FR-09.04–09.05, FR-11.01–11.09; NFR-09–15,28–32

**Requirement Statement**

First validate native GStreamer ABI/plugins, actual MP2T reception and parser access, TIF surface and first-render observability; then server RTSP resource routing and remux anchor mapping, programme/cycle continuity and quality changes; finally integrate metadata and full UI. If a dependency fails, investigate alternative compatible implementation while preserving C++ middleware/TIF/RTSP/MPEG-TS/RTP; document change and get approval before changing approved behavior. No fallback to ordinary Android player, HLS or non-TIF app.

**Acceptance Criteria**

Record each spike as NOT EXECUTED until real evidence; no production compatibility or NFR achievement is claimed by specification.

## External technical references (consulted, not execution evidence)
- Android TIF: https://developer.android.com/training/tv/tif/tvinput ; https://developer.android.com/reference/android/media/tv/TvInputService ; https://developer.android.com/reference/android/media/tv/TvView
- GStreamer RTSP and RTP: https://gstreamer.freedesktop.org/documentation/rtsp/rtspsrc.html ; https://gstreamer.freedesktop.org/documentation/rtp/rtpmp2tdepay.html ; https://gstreamer.freedesktop.org/documentation/rtp/rtpmp2tpay.html ; https://gstreamer.freedesktop.org/documentation/gst-rtsp-server/rtsp-media.html
- GStreamer Android surface: https://gstreamer.freedesktop.org/documentation/tutorials/android/video.html ; https://gstreamer.freedesktop.org/documentation/application-development/advanced/pipeline-manipulation.html
- Media3 RTSP supported sample formats: https://developer.android.com/media/media3/exoplayer/rtsp
- RFC 2326, RFC 2250, RFC 3550; course all-lectures(1).pdf Lectures 3–7.

## Review decisions
OQ-53: Approve per-operation RTSP resource URI + new session profile rather than requiring in-session global NPT Range. Recommended for bounded project complexity; preserves server-side seek and RTSP/RTP.
OQ-54: Approve pre-authorized fallback to project-owned TIF-integrated TvView app only if system app fails documented feasibility (or retain explicit team approval at gate). Recommended pre-authorization.
OQ-55: Confirm C++ mandate applies to receiver DTV middleware, not the independent OTT MW REST server (Python FastAPI). Recommended yes; C++ streaming server also selected.

All other stack/profile choices are delegated draft decisions. Do not silently weaken approved FR/NFR if a spike fails. No source code authored and no verification result claimed.
