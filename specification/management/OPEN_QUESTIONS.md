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
- TR/design: RTSP version and compatible client/server libraries, MP2T support and access to received TS for project parser, seekable live behavior, global timeline mapping, PCR/PTS and random-access point handling, RTP/session transitions, exact media preparation and verification. Hybrid approach is approved but no implementation or PoC is verified.

## NFR review — closed

OQ-43–OQ-52 approved on 2026-11-28. OQ-45 selects option B for live quality switching (preserve live mode/current live edge); rewind mode preserves the historical position within ±2 s. The remaining proposed numerical thresholds and reference conditions are approved as written. OQ-52 evaluates EPG coverage at successful retrieval time; valid cached data need not maintain a continuously rolling +24 h horizon. See Decision Log and approved NFR baseline. OQ-08 and technical feasibility remain deferred.
