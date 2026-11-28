# Open Questions

## Functional review — closed

OQ-01–OQ-07 and OQ-09–OQ-42 are resolved at functional level. Historical discussion and resolutions are preserved in the Decision Log and Git history. OQ-06 remains a conditional system Live TV application preference.

- OQ-22: EPG coverage preceding and following 24 hours; title, start and end times; selected service only.
- OQ-36: Reconcile valid complete catalogue including removals; manual refresh optional.
- OQ-37: Latest valid selection retained during channel rate limit; execute at expiry.
- OQ-38: EPG refresh every 6 hours; startup and service selection when missing/stale.
- OQ-39: Complete keyboard mapping approved; Space only in normal viewing.
- OQ-40: High/Low, High default, Low fallback.
- OQ-41: All simulated history since server startup; no pre-start or future content as history.
- OQ-42: Hybrid RTSP + server-side media seeking selected after instructor consultation. Parse PAT/PMT for each newly activated stream; continuous table-version monitoring not mandatory for stable SPTS; reacquire on relevant structure changes. Track presented playback position independently.

## Deferred (not FR blockers)

- OQ-08: Verify preferred system Live TV application can expose all mandatory M14 functions; consider project-owned TIF-integrated alternative if feasibility fails, with team approval.
- NFR: rewind/quality position accuracy, channel activation minimum interval, playback operation timeouts, indicator precision/cadence, info/notice durations and other measurable limits.
- TR/design: RTSP version and compatible client/server libraries, MP2T support and access to received TS for project parser, seekable live behavior, global timeline mapping, PCR/PTS and random-access point handling, RTP/session transitions, exact media preparation and verification. Hybrid approach is approved but no implementation or PoC is verified.

## NFR review — proposed targets (not approved)

- OQ-43: Approve reference conditions and measurement convention (single receiver, controlled local IP network, at least two services, 20 attempts per playback latency metric, 18/20 target, no excluded failures)?
- OQ-44: Approve responsiveness targets: 250 ms UI acknowledgement, 3 s channel change, 500 ms minimum initiation interval, 5 s rewind/quality/Go Live, 1 s cached metadata UI, 5 s metadata deadline?
- OQ-45: Approve 2 s absolute rewind and quality-position error, measured from effective target and position at quality selection respectively; 100 ms cross-variant media alignment? In live quality switching the selected position is preserved rather than advancing to the live edge; confirm this interpretation if it conflicts with LIVE mode expectations.
- OQ-46: Approve 2 s delay-indicator error, 1 s update/resolution, 5 s measured live lag after settling, 150 ms A/V skew?
- OQ-47: Approve 10 s operation deadline, 10 s recovery deadline, 5 s complete-loss detection and 2 s obsolete-delivery termination?
- OQ-48: Approve 100-operation resource stability and 30-minute continuous playback including programme/cycle boundaries, with no unplanned interruption over 3 s?
- OQ-49: Approve 2 s parser result availability after complete tables and exact correctness on reference corpus?
- OQ-50: Approve 3 s info dialog and 2 s nonblocking boundary notice with ±0.5 s tolerance? Confirm failure messages are not subject to the short notice duration.
- OQ-51: Approve architecture, compatibility, reproducibility and evidence constraints; confirm no fixed CPU/RAM limits, production SLA, authentication/DRM or multi-client capacity requirement for the controlled demo?
- OQ-52: EPG window is evaluated at successful retrieval time with the approved 6 h refresh. Confirm that a cached schedule need not continuously extend to a fresh rolling +24 h horizon between refreshes, and that pre-start scheduled records (if any) do not imply rewind availability.
