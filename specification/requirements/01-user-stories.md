# User Stories

Status: Approved.

| ID | As a viewer, I want to... | Acceptance criteria |
|---|---|---|
| US-01 | Select a TV service using next/previous navigation, direct channel number entry, or the service list. | Each selection method starts live playback of the selected service and displays its information. |
| US-02 | Open the TV Guide and see what is being broadcast. | The TV Guide displays only the schedule of the selected service using data obtained from the OTT MW server. |
| US-03 | Control audio and operate the application using a keyboard. | Volume and mute controls affect the active A/V session, and application controls are accessible without a mouse. |
| US-04 | Select a backward displacement in 30-second increments and explicitly confirm it, repeatedly if desired. | Selection alone does not initiate rewind; each confirmed operation uses the playback position at the moment of explicit confirmation as its reference. |
| US-05 | Select playback quality independently in live and rewind playback while preserving the content position. | Once playback of the selected variant actually begins, the active quality and displayed quality indicator correspond to that variant. |
| US-06 | See how far behind live playback I am and return to the live programme. | A single indicator displays the delay behind live playback. The Go Live command starts playback of the current live content of the same service. |
| US-07 | Understand playback errors and continue watching. | An error message explains the problem. Following a failed rewind operation, the Go Live action remains available. |

