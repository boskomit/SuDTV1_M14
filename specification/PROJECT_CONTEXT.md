# Project context

M14 — Broadband Managed IPTV Receiver: Rewind and Quality Selection. Group V. Android TV + TIF. Media delivery: MPEG-TS over RTP on demand. PAT/PMT: project-owned parsing from received MPEG-TS. Service and EPG information: OTT MW REST. Both server components are in project scope, with simulated live. See approved requirements for normative scope.

Sources: M14 assignment, instructor project instructions, team-approved decisions, and course lectures as supporting context. Instructor materials are not redistributed here. Production head-end substitutability is a design objective, not a compatibility guarantee.

Approved architecture direction (2026-11-28): Hybrid RTSP + server-side media seeking. RTSP controls playback, MPEG-TS over RTP carries media, OTT MW REST provides catalogue and EPG. The server resolves seek targets against the broadcast timeline. Exact timing/index implementation and library feasibility remain open in TR/design.

Instructor constraint confirmed by team in TR phase: project-owned receiver DTV middleware must be C++. Android TV + TIF remains mandatory.
