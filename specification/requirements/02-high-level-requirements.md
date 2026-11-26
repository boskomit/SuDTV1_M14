# High-Level Requirements

Version: 0.2 — proposed, pending team approval. Sources: M14 assignment, project instructions, US-01–US-07, and lectures 1–7 as supporting context. No FR/NFR/TR decisions are made here.

| ID | Requirement | Traceability | Verification intent |
|---|---|---|---|
| HLR-01 | The system shall provide a TV service catalogue based on service information obtained from the OTT MW REST server. | M14; US-01 | Compare available services with known server data. |
| HLR-02 | The system shall support live service selection through next/previous navigation, direct channel number entry and a service list, and display service information when changing services. | M14; US-01 | Exercise all three selection methods. |
| HLR-03 | The system shall provide an Electronic Programme Guide displaying only the selected service's schedule obtained from the OTT MW REST server. | M14; US-02 | Compare the selected service schedule with known server data. |
| HLR-04 | The system shall enable keyboard-based operation of the viewer-facing application, including volume adjustment and muting of the active A/V session. | M14; US-03 | Operate without a mouse and verify audio controls. |
| HLR-05 | The system shall allow the viewer to select a backward displacement in 30-second increments relative to the current playback position of the selected service and execute the rewind only upon explicit confirmation. Repeated confirmed rewind operations shall be supported. | M14; US-04 | Select multiple increments without initiating rewind; confirm and verify playback against known content; repeat. |
| HLR-06 | The system shall allow independent quality selection in both live and rewind playback while preserving the content position, and display the quality of the variant actually being played. | M14; US-05 | Change quality in both modes and verify content continuity and actual quality indication. |
| HLR-07 | The system shall display a single time indicator representing the delay behind live playback and provide a Go Live action for the same service. | M14; US-06 | Compare displayed delay with known live and playback positions; verify return to live. |
| HLR-08 | The system shall communicate playback failures to the viewer and provide a Go Live recovery action following a failed rewind. | US-07 | Induce rewind failure and verify message and recovery action. |
| HLR-09 | The project shall include an RTP streaming server supplying requested live or previously broadcast content in the requested quality using MPEG-TS over RTP on client demand, and an OTT MW REST server providing service and programme information. Live broadcasting shall be simulated for the project demonstration. | M14; US-01, US-04, US-05, US-06 | Demonstrate both servers; inspect requested media and metadata against known test content. |
| HLR-10 | The receiver shall parse PAT and PMT from the received MPEG-TS using project-owned parsing logic. | M14; project instructions | Independently compare parser output with known TS contents. |
| HLR-11 | The receiver shall operate within the Android TV + TV Input Framework architecture, integrating its IPTV input and service/programme information with the Android TV environment. The system Live TV application is the preferred viewer-facing application, subject to feasibility verification; a project-owned TIF-integrated application is a conditional alternative. | Team platform decision; project instructions; lecture 7 | Verify input registration, service/programme integration, playback and required viewer controls in the selected TV application. |


Open interpretation: the exact reference instant for a confirmed rewind (selection start or confirmation) remains to be agreed. Production integration is a design objective, not a verified compatibility claim.
