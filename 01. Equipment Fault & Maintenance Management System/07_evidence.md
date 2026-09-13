# Evidence Index

This page provides an index of the evidence I captured for the Equipment Fault Maintenance MVP. Each item links the evidence image to the relevant project documentation.

## Evidence

| ID | Evidence | Demonstrates | Related documentation |
|---|---|---|---|
| E01 | [SharePoint Equipment](images/01-sharepoint-equipment-data.png) | Equipment records, locations, equipment types, criticality and active status | [02 Solution Overview](02-solution-overview.md) |
| E02 | [SharePoint Faults](images/02-sharepoint-faults.png) | Fault records and processed priority/status data | [02 Solution Overview](02-solution-overview.md) |
| E03 | [Application Home](images/03-home.png) | Application entry point, navigation and fault summary | [03 Power Apps](03-power-apps.md) |
| E04 | [Fault Report](images/04-report-fault.png) | Fault submission form and business inputs | [03 Power Apps](03-power-apps.md) |
| E06 | [Faults List](images/06-faults-list.png) | Fault list, search, filtering, sorting and priority visibility | [03 Power Apps](03-power-apps.md) |
| E07 | [Fault Detail](images/07-fault-detail.png) | Fault details, priority/RAG indicator, assigned team, status and resolution fields | [03 Power Apps](03-power-apps.md) |
| E09 | [Power Automate Flow Overview](images/09-flow-overview.png) | Overall automation structure from trigger through processing and notifications | [04 Power Automate](04-power-automate.md) |
| E10 | [Assignment and Priority Logic](images/10-assignment-priority-logic.png) | Implemented equipment-type assignment rules and priority decision logic | [04 Power Automate](04-power-automate.md) |
| E11 | [Successful Flow Run](images/11-successful-flow-run.png) | Successful end-to-end execution of the automation | [04 Power Automate](04-power-automate.md) |
| E12 | [Teams Notification](images/12-teams-notification.png) | Automated Teams notification containing processed fault information | [04 Power Automate](04-power-automate.md) |
| E13 | [Outlook Notification](images/13-outlook-notification.png) | Automated email notification containing processed fault information | [04 Power Automate](04-power-automate.md) |

## Evidence not captured as standalone screenshots

Some behaviours are documented and tested but were not included as separate screenshots because the visual evidence would not add significant value:

- **E05 — Location → Equipment cascading selection:** documented in the Power Apps implementation.
- **E08 — Resolution validation:** documented and tested in the Power Apps implementation and testing notes.
- **E14 — Refresh(Faults):** the stale-data fix is documented in the Power Apps implementation and testing notes.

## Testing and known issue

The evidence supports the implemented MVP flow from fault submission through automated assignment, prioritisation and notifications.

During end-to-end testing, I also identified an intermittent issue where a fault Title can be replaced by an ID value after automated processing. I have kept this issue visible in the evidence rather than altering the test data retrospectively. Further investigation is recorded in the testing and project status documentation.

## Evidence principles

The screenshots are intended to show the implementation and actual execution of the MVP. I have used a mixture of application screens, underlying SharePoint data, Power Automate configuration, successful execution and notification outputs so that the evidence covers both how the solution is configured and what it actually produces.
