# High-Level Requirements

Version: 0.1 — proposed, pending team approval. Sources: M14 assignment, project instructions, US-01–US-07, and lectures 1–7 as supporting context. No FR/NFR/TR decisions are made here.

| ID | Requirement | Traceability | Verification intent |
|---|---|---|---|
| HLR-01 | The system shall provide a TV service catalogue based on service information obtained from the OTT MW REST server. | M14; US-01 | Compare available services with known server data. |
| HLR-02 | The system shall support live service selection through next/previous navigation, direct channel number entry and a service list, and display service information when changing services. | M14; US-01 | Exercise all three selection methods. |
| HLR-03 | The system shall provide an Electronic Programme Guide displaying the selected service's schedule obtained from the OTT MW REST server. | M14; US-02 | Compare displayed schedule with known server data. |
| HLR-04 | The system shall enable keyboard-based operation of the viewer-facing application, including volume adjustment and muting of the active A/V session. | M14; US-03 | Operate without a mouse and verify audio controls. |
| HLR-05 | The system shall enable repeated 30-second rewind operations relative to the current playback position of the selected service. | M14; US-04 | Demonstrate consecutive rewind operations on known content. |
| HLR-06 | The system shall allow the viewer to select an available playback quality independently of rewind and display the quality of the variant actually being played. | M14; US-05 | Change quality independently and compare displayed and actual variant. |
| HLR-07 | The system shall display a single time-related value representing the current viewing position relative to live content and provide a Go Live action for the same service. | M14; US-06 | Demonstrate delayed playback, the time indicator and return to live. |
| HLR-08 | The system shall communicate playback failures to the viewer and provide a Go Live recovery action following a failed rewind. | US-07 | Induce rewind failure and verify message and recovery action. |
| HLR-09 | The server side shall supply the requested live or previously broadcast content in the corresponding quality variant using MPEG-TS over RTP on client demand. | M14; US-01, US-04, US-05, US-06 | Inspect network media delivery and correspondence to requests. |
| HLR-10 | The receiver shall parse PAT and PMT from the received MPEG-TS using project-owned parsing logic. | M14; project instructions | Independently compare parser output with known TS contents. |
| HLR-11 | The receiver shall operate within the Android TV + TV Input Framework architecture, integrating its IPTV input and service/programme information with the Android TV environment. | Team platform decision; project instructions; lecture 7 | Demonstrate registered input, channel/programme integration and playback in the target environment. |

HLR-09 describes the required server responsibility, not a confirmed decision that the team must implement every server component. HLR-06 does not yet resolve whether quality changes preserve the playback position. HLR-07 wording follows the user goal; exact time representation remains open. HLR-05 does not yet settle archive limits or interval-selection coverage.
