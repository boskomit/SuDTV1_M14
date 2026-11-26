# Functional Requirements — Draft 0.3.0-dev.1

Status: **Proposed — team review required**. Parent: approved HLR baseline v0.2.0. All statements in this document are draft requirements; entries explicitly marked Proposed introduce behavior needing additional approval. No acceptance criterion contains illustrative examples. No related-requirements field is used.

Terminology: service = selectable IPTV channel; content position = location on the service's broadcast timeline; live position = current position on that timeline; requested rewind target = position at confirmation minus selected displacement; effective target = requested target adjusted for historical availability as agreed in OQ-12. Playback success requires actual presentation, not merely request acceptance. Timing accuracy, latency, tolerances and measurable limits are deferred to NFR. No implementation protocol beyond the assignment's MPEG-TS over RTP and REST is selected here.

Architecture basis: course lectures 2–7, especially Lecture 7 Figure 2 (Live TV App, TV Input Manager, TV Provider, IPTV TV Input, IPTV client and platform), Lecture 3 PAT/PMT, Lecture 4 managed IPTV media/control separation. Application handles viewer interaction, DTV middleware handles service state and playback control, platform provides media presentation. This is an architectural constraint from course material, not a detailed design or commitment to a particular player, API, language, transport control protocol or native implementation. Broadcast/OTT example project requirements are not imported into M14.


## HLR-01 — TV Service Catalogue

### FR-01.01 — Service Catalogue Retrieval

**Parent Requirement:** HLR-01

**Requirement Statement**

The receiver shall obtain the IPTV service catalogue from the OTT MW REST server.

**Acceptance Criteria**

- The catalogue is requested from the designated server.
- The received service records are available to the receiver for service selection.

### FR-01.02 — Service Identification

**Parent Requirement:** HLR-01

**Requirement Statement**

The receiver shall associate each available service with its identity, channel number and display name.

**Acceptance Criteria**

- Each selectable service resolves to the corresponding server-provided identity.
- Its channel number and name correspond to the catalogue.

### FR-01.03 — Service List Presentation

**Parent Requirement:** HLR-01

**Requirement Statement**

The viewer-facing application shall present the available IPTV services in a selectable list.

**Acceptance Criteria**

- The list contains the available services and their channel numbers and names.
- The viewer can select a listed service.

### FR-01.04 — Catalogue Synchronization

**Parent Requirement:** HLR-01

**Review status:** Proposed — OQ-17

**Requirement Statement**

The receiver shall reconcile repeated successful catalogue retrievals with the installed IPTV services without creating duplicate service entries.

**Preconditions**

A valid catalogue response has been obtained.

**Acceptance Criteria**

- Repeated retrieval of unchanged data does not create duplicates.
- Successfully retrieved changes are reflected in the installed catalogue.

## HLR-02 — Live TV Service Selection

### FR-02.01 — Next Service Selection

**Parent Requirement:** HLR-02

**Requirement Statement**

The system shall allow the viewer to select the next service in the service order.

**Preconditions**

A current service and a successor exist.

**Acceptance Criteria**

- The selected service is the successor of the current service according to the service order.

### FR-02.02 — Previous Service Selection

**Parent Requirement:** HLR-02

**Requirement Statement**

The system shall allow the viewer to select the previous service in the service order.

**Preconditions**

A current service and a predecessor exist.

**Acceptance Criteria**

- The selected service is the predecessor of the current service according to the service order.

### FR-02.03 — Direct Channel Number Selection

**Parent Requirement:** HLR-02

**Requirement Statement**

The system shall allow the viewer to select a service by entering its channel number.

**Acceptance Criteria**

- A valid entered number resolves to the corresponding service.

### FR-02.04 — Service List Selection

**Parent Requirement:** HLR-02

**Requirement Statement**

The system shall allow the viewer to select a service from the service list.

**Acceptance Criteria**

- Selecting a listed service resolves to that service.

### FR-02.05 — Live Service Activation

**Parent Requirement:** HLR-02

**Requirement Statement**

Upon a valid service selection, the receiver shall activate live playback of the selected service regardless of the selection method.

**Acceptance Criteria**

- All supported selection methods activate the same service for the same selection.
- Switching from rewound content to another service starts its live content.

### FR-02.06 — Service Information Display

**Parent Requirement:** HLR-02

**Requirement Statement**

The viewer-facing application shall display a service information dialog when the selected service changes.

**Acceptance Criteria**

- The dialog identifies the selected service using its catalogue information.
- Information for the previous service is not presented as belonging to the new service.

### FR-02.07 — Superseding Service Selection

**Parent Requirement:** HLR-02

**Review status:** Proposed — OQ-20

**Requirement Statement**

The receiver shall ensure that an earlier pending service activation cannot supersede a more recent valid service selection.

**Preconditions**

A service activation is pending.

**Acceptance Criteria**

- After successive valid selections, only the latest selection can become the active service.
- An obsolete completion cannot replace it.

## HLR-03 — Electronic Programme Guide

### FR-03.01 — Programme Schedule Retrieval

**Parent Requirement:** HLR-03

**Requirement Statement**

The receiver shall obtain the programme schedule for the selected service from the OTT MW REST server.

**Preconditions**

A service is selected.

**Acceptance Criteria**

- The obtained programme records belong to the selected service and originate from the REST server.

### FR-03.02 — Selected-Service TV Guide

**Parent Requirement:** HLR-03

**Requirement Statement**

The viewer-facing application shall allow the viewer to open a TV Guide displaying only the selected service's programme schedule.

**Acceptance Criteria**

- The guide identifies the selected service and displays its programme schedule.
- Programmes of other services are excluded.

### FR-03.03 — Programme Information

**Parent Requirement:** HLR-03

**Review status:** Proposed — OQ-22

**Requirement Statement**

The TV Guide shall present the programme title and scheduled start and end times for each displayed programme.

**Preconditions**

Schedule data is available.

**Acceptance Criteria**

- Displayed titles and times correspond to the retrieved schedule.

## HLR-04 — User Input and Audio Control

### FR-04.01 — Keyboard Navigation

**Parent Requirement:** HLR-04

**Requirement Statement**

The viewer-facing application shall support navigation and activation of the required controls using the keyboard without a mouse.

**Acceptance Criteria**

- All mandatory viewer-facing operations are accessible using keyboard input.
- Arrow keys, Enter, digits and letter-based dialog controls are supported as required by the assignment.

### FR-04.02 — Volume Adjustment

**Parent Requirement:** HLR-04

**Requirement Statement**

The system shall allow the viewer to adjust the volume of the active A/V session.

**Preconditions**

An A/V session is active.

**Acceptance Criteria**

- Increasing or decreasing volume changes the audible output accordingly within its supported range.

### FR-04.03 — Mute Control

**Parent Requirement:** HLR-04

**Requirement Statement**

The system shall allow the viewer to mute and unmute the active A/V session.

**Preconditions**

An A/V session is active.

**Acceptance Criteria**

- Muting suppresses audible output.
- Unmuting restores audible output when the configured volume is nonzero.

## HLR-05 — Rewind Control

### FR-05.01 — Rewind Displacement Selection

**Parent Requirement:** HLR-05

**Requirement Statement**

The system shall allow the viewer to select a backward playback displacement in positive multiples of 30 seconds and adjust it in 30-second increments in either direction.

**Preconditions**

A TV service is being played.

**Acceptance Criteria**

- Selectable displacements are positive multiples of 30 seconds.
- Increasing and decreasing the selection changes it by 30 seconds.

### FR-05.02 — Explicit Rewind Confirmation

**Parent Requirement:** HLR-05

**Requirement Statement**

The system shall initiate a rewind operation only after the viewer explicitly confirms the selected backward displacement.

**Preconditions**

A displacement has been selected.

**Acceptance Criteria**

- Selecting or modifying a displacement does not initiate rewind.
- Explicit confirmation initiates a rewind operation using the selected displacement.

### FR-05.03 — Rewind Target Position Determination

**Parent Requirement:** HLR-05

**Requirement Statement**

Upon explicit rewind confirmation, the system shall determine the requested position by subtracting the selected backward displacement from the content position being presented at confirmation.

**Acceptance Criteria**

- The requested position is relative to the playback position at confirmation, not the start of displacement selection.

### FR-05.04 — Rewind Playback Initiation

**Parent Requirement:** HLR-05

**Requirement Statement**

Following confirmation, the receiver shall present the currently selected service at the effective earlier content position in the active quality, subject to content availability and the playback accuracy requirement.

**Preconditions**

An effective playable position has been determined.

**Acceptance Criteria**

- Previously broadcast content is actually presented.
- Service and quality remain unchanged.
- Actual start position conforms to the accuracy requirement to be defined in NFR.

### FR-05.05 — Repeated Rewind Operations

**Parent Requirement:** HLR-05

**Requirement Statement**

The system shall support subsequent rewind operations during playback of previously broadcast content.

**Preconditions**

Previously broadcast content is being played.

**Acceptance Criteria**

- Another rewind is available without first returning to live.
- Each confirmation uses the content position at that confirmation.

### FR-05.06 — Playback During Selection

**Parent Requirement:** HLR-05

**Requirement Statement**

The system shall continue current playback while the viewer selects a rewind displacement.

**Acceptance Criteria**

- Opening or adjusting the rewind selection does not pause or reposition playback.

### FR-05.07 — Rewind Selection Cancellation

**Parent Requirement:** HLR-05

**Requirement Statement**

The system shall allow the viewer to cancel an unconfirmed rewind selection without modifying current playback.

**Acceptance Criteria**

- Cancellation discards the unconfirmed selection.
- No rewind operation is initiated.

### FR-05.08 — Historical Availability Boundary

**Parent Requirement:** HLR-05

**Requirement Statement**

When a confirmed rewind target precedes the earliest available content for the current service and quality, the system shall use the earliest available playable position as the effective target.

**Preconditions**

Earlier content exists but the requested target precedes its availability boundary.

**Acceptance Criteria**

- The effective target is the earliest available playable position rather than the unavailable requested position.
- Playback accuracy is assessed against that effective target.

### FR-05.09 — Initial Rewind Displacement

**Parent Requirement:** HLR-05

**Requirement Statement**

The system shall initialize each new rewind selection to 30 seconds.

**Acceptance Criteria**

- A newly opened selection starts at 30 seconds independently of the previous confirmed displacement.

### FR-05.10 — Pending Rewind Confirmation

**Parent Requirement:** HLR-05

**Requirement Statement**

The system shall prevent confirmation of another rewind while a previously confirmed rewind operation remains pending.

**Acceptance Criteria**

- A second confirmation cannot initiate another rewind until the previous operation has completed or failed.

## HLR-06 — Playback Quality Selection

### FR-06.01 — Available Quality Selection

**Parent Requirement:** HLR-06

**Requirement Statement**

The viewer-facing application shall allow the viewer to select an available quality variant of the currently selected service.

**Acceptance Criteria**

- The selectable quality options correspond to variants available for that service.
- Quality selection is available during live and rewind playback.

### FR-06.02 — Quality Change Request

**Parent Requirement:** HLR-06

**Requirement Statement**

The receiver shall request playback of the selected service in the newly selected quality without requiring a rewind operation.

**Acceptance Criteria**

- The requested variant corresponds to the selected quality.
- The service remains unchanged.

### FR-06.03 — Content Position Preservation

**Parent Requirement:** HLR-06

**Requirement Statement**

The receiver shall preserve the content position across a quality change in live and rewind playback.

**Acceptance Criteria**

- Playback after switching corresponds to the same content timeline and does not return to the live edge solely because of a quality change.
- Continuity is assessed against a tolerance to be defined in NFR.

### FR-06.04 — Active Quality Indication

**Parent Requirement:** HLR-06

**Requirement Statement**

The viewer-facing application shall indicate the quality variant actually being presented.

**Acceptance Criteria**

- The indicator changes to the requested quality only after actual playback of that variant begins.
- A pending or failed request is not indicated as successful.

## HLR-07 — Playback Position and Return to Live

### FR-07.01 — Live Delay Determination

**Parent Requirement:** HLR-07

**Requirement Statement**

The receiver shall determine the delay of presented content relative to the current live position of the same service.

**Acceptance Criteria**

- The delay represents the difference between the live content position and the presented content position on the same timeline.

### FR-07.02 — Live Delay Display

**Parent Requirement:** HLR-07

**Requirement Statement**

The viewer-facing application shall display a single time indicator representing the delay behind live playback.

**Acceptance Criteria**

- The displayed value corresponds to the determined delay and reflects changes in the playback position.
- It is not an absolute broadcast timestamp.

### FR-07.03 — Return to Live

**Parent Requirement:** HLR-07

**Requirement Statement**

The system shall allow the viewer to return from delayed playback to current live content of the selected service.

**Acceptance Criteria**

- The Go Live action activates current live content of the same service.
- The delay indicator reflects the resulting live playback position.

## HLR-08 — Error Reporting and Playback Recovery

### FR-08.01 — Playback Failure Recognition

**Parent Requirement:** HLR-08

**Requirement Statement**

The receiver shall distinguish unsuccessful playback operations from successful playback completion.

**Acceptance Criteria**

- A failed activation, rewind or quality change is not reported as successful.
- Acceptance of a request alone is not treated as successful presentation.

### FR-08.02 — Playback Error Notification

**Parent Requirement:** HLR-08

**Requirement Statement**

The viewer-facing application shall communicate playback failures to the viewer using a message that identifies the problem.

**Acceptance Criteria**

- A failure produces a comprehensible error indication appropriate to the failed operation.
- The message does not claim successful playback.

### FR-08.03 — Rewind Failure Recovery

**Parent Requirement:** HLR-08

**Requirement Statement**

The system shall keep the Go Live action available following a failed rewind.

**Acceptance Criteria**

- The viewer can invoke Go Live for the same service after the rewind failure.
- A successful recovery presents current live content.

### FR-08.04 — Invalid Service Selection Handling

**Parent Requirement:** HLR-08

**Review status:** Proposed — OQ-19

**Requirement Statement**

The system shall reject a channel number that does not identify an available service without activating an unrelated service.

**Preconditions**

A channel number has been submitted.

**Acceptance Criteria**

- Invalid input does not select another service.
- The viewer is informed of the invalid selection.

### FR-08.05 — Metadata Failure Handling

**Parent Requirement:** HLR-08

**Review status:** Proposed — OQ-18

**Requirement Statement**

The system shall distinguish unavailable or invalid catalogue or schedule data from a successful metadata retrieval.

**Preconditions**

A metadata request fails or returns unusable data.

**Acceptance Criteria**

- Unavailable or invalid metadata is not presented as newly retrieved valid data.
- The viewer receives an appropriate indication.

## HLR-09 — On-Demand Media Delivery

### FR-09.01 — Service Metadata Provision

**Parent Requirement:** HLR-09

**Requirement Statement**

The OTT MW REST server shall provide service catalogue information to the receiver on request.

**Acceptance Criteria**

- The response identifies available services and supplies the information needed to present and select them.

### FR-09.02 — Programme Metadata Provision

**Parent Requirement:** HLR-09

**Requirement Statement**

The OTT MW REST server shall provide programme schedule information associated with a requested service.

**Acceptance Criteria**

- The response contains schedule data associated with the requested service.

### FR-09.03 — Simulated Live Timeline

**Parent Requirement:** HLR-09

**Requirement Statement**

The production side shall expose a progressing simulated live content position for each available service.

**Acceptance Criteria**

- The live position advances with simulated broadcasting independently of an individual viewer starting playback.

### FR-09.04 — Live Content Delivery

**Parent Requirement:** HLR-09

**Requirement Statement**

The RTP streaming server shall deliver the requested service's current live content in the requested available quality using MPEG-TS over RTP upon a client request.

**Acceptance Criteria**

- Received media corresponds to the requested service and quality and the current live content.

### FR-09.05 — Historical Content Delivery

**Parent Requirement:** HLR-09

**Requirement Statement**

The RTP streaming server shall deliver previously broadcast content of the requested service in the requested available quality using MPEG-TS over RTP from the effective requested position.

**Acceptance Criteria**

- Received media corresponds to the service, quality and effective earlier content position.

### FR-09.06 — Quality Variant Availability

**Parent Requirement:** HLR-09

**Requirement Statement**

The production side shall make the available quality variants of each service identifiable to the receiver.

**Acceptance Criteria**

- The receiver can determine which variants can be requested for the selected service.

### FR-09.07 — Cross-Variant Content Alignment

**Parent Requirement:** HLR-09

**Requirement Statement**

The production side shall make the same service content position addressable across its available quality variants.

**Acceptance Criteria**

- Requests for the same service and content position in different qualities refer to the same underlying content.
- Alignment accuracy is subject to NFR.

### FR-09.08 — Unavailable Media Request Handling

**Parent Requirement:** HLR-09

**Requirement Statement**

The RTP streaming server shall communicate inability to satisfy a media request rather than report successful delivery of unrelated content.

**Acceptance Criteria**

- An unavailable service, quality or playable content position is distinguishable from successful delivery.
- The historical boundary rule in FR-05.08 applies when an earlier playable position exists.

### FR-09.09 — Stream Termination

**Parent Requirement:** HLR-09

**Review status:** Proposed — OQ-30

**Requirement Statement**

The RTP streaming server shall cease delivery associated with a client playback request when that delivery is no longer required by the client.

**Preconditions**

A delivery has been established.

**Acceptance Criteria**

- A terminated delivery does not continue sending media for the terminated request.

## HLR-10 — Transport Stream Metadata Acquisition

### FR-10.01 — PAT Acquisition and Parsing

**Parent Requirement:** HLR-10

**Requirement Statement**

The receiver shall acquire and parse the PAT from the received MPEG-TS using project-owned parsing logic.

**Acceptance Criteria**

- The parser identifies PAT program entries and their corresponding PMT PIDs from the received stream.

### FR-10.02 — PMT Acquisition and Parsing

**Parent Requirement:** HLR-10

**Requirement Statement**

The receiver shall acquire and parse the PMT of the selected program from the received MPEG-TS using project-owned parsing logic.

**Acceptance Criteria**

- The PMT is located using the PAT result.
- The parser identifies the program's PCR PID, elementary stream PIDs and stream types.

### FR-10.03 — Parsed Component Information

**Parent Requirement:** HLR-10

**Requirement Statement**

The receiver shall make the parsed program component information available to its IPTV playback control logic.

**Acceptance Criteria**

- The receiver can access the parsed audio/video component identifiers and timing reference for the selected program.
- The output corresponds to the received stream independently of a player library's parser.

### FR-10.04 — Invalid Table Handling

**Parent Requirement:** HLR-10

**Requirement Statement**

The receiver shall distinguish incomplete or invalid PAT/PMT data from valid parsed program information.

**Acceptance Criteria**

- Incomplete or invalid table data is not reported as a successful parse.
- Unusable program information does not activate an unrelated program.

### FR-10.05 — Current Stream Metadata

**Parent Requirement:** HLR-10

**Requirement Statement**

The receiver shall acquire program information for each newly activated media stream rather than assume that the previous stream's information is valid.

**Acceptance Criteria**

- Program information used for the newly activated stream corresponds to that stream after a service, rewind or quality change.

## HLR-11 — Android TV Integration

### FR-11.01 — IPTV Input Registration

**Parent Requirement:** HLR-11

**Requirement Statement**

The receiver shall expose a project-owned IPTV TV Input through the Android TV Input Framework.

**Acceptance Criteria**

- The IPTV input is registered and discoverable in the target Android TV environment.

### FR-11.02 — Channel Publication

**Parent Requirement:** HLR-11

**Requirement Statement**

The receiver shall publish its IPTV service catalogue to the Android TV Provider under its own TV Input identity.

**Acceptance Criteria**

- Published channels are associated with the project IPTV input and retain the corresponding service identity.
- Repeated publication does not create duplicate channels.

### FR-11.03 — Programme Publication

**Parent Requirement:** HLR-11

**Requirement Statement**

The receiver shall publish retrieved programme schedule information to the Android TV Provider associated with its corresponding IPTV channel.

**Acceptance Criteria**

- Published programme records refer to the correct IPTV channel and correspond to the retrieved schedule.

### FR-11.04 — TIF Service Activation

**Parent Requirement:** HLR-11

**Requirement Statement**

The IPTV TV Input shall accept service activation requests from the TV Input Framework and resolve them to the corresponding IPTV service.

**Acceptance Criteria**

- A valid activation request from the Live TV application results in activation of the corresponding IPTV service through TIF.

### FR-11.05 — Session Presentation

**Parent Requirement:** HLR-11

**Requirement Statement**

The IPTV TV Input shall present decoded video through the presentation surface supplied by TIF.

**Preconditions**

A valid presentation surface and playback session are available.

**Acceptance Criteria**

- Video for the activated service appears on the assigned presentation surface.

### FR-11.06 — TIF Audio Control

**Parent Requirement:** HLR-11

**Requirement Statement**

The IPTV TV Input shall apply audio control requests from the TV Input Framework to the active playback session.

**Acceptance Criteria**

- TIF volume and mute-related requests affect the active session's audible output.

### FR-11.07 — Playback Availability Reporting

**Parent Requirement:** HLR-11

**Requirement Statement**

The IPTV TV Input shall report actual playback availability and unavailability to the TV Input Framework.

**Acceptance Criteria**

- Video availability is reported only once video is actually presented.
- Unavailable playback is not reported as available.

### FR-11.08 — Session Release

**Parent Requirement:** HLR-11

**Requirement Statement**

The IPTV TV Input shall terminate playback associated with a released TIF session.

**Acceptance Criteria**

- A released session no longer presents media or retains active delivery on its behalf.

### FR-11.09 — Viewer Application Integration

**Parent Requirement:** HLR-11

**Requirement Statement**

The receiver shall expose the required M14 viewer-facing capabilities within the selected TIF-integrated Live TV application.

**Acceptance Criteria**

- All approved user-facing functions are demonstrable in the target TV environment through TIF integration.
- A project-owned application is considered only if the preferred system application fails feasibility verification.

## Open decisions

See [Open Questions](../management/OPEN_QUESTIONS.md). Draft behavior subject to an unresolved decision must not be treated as approved. No NFR or TR baseline is established.
