# Project Status

## Current status

The core MVP has been implemented across Power Apps, SharePoint, Power Automate, Microsoft Teams and Outlook.

The core process has been implemented from fault reporting through to automated processing, notification and subsequent fault management.

## Implemented

### SharePoint

- Created the `Locations`, `Equipment` and `Faults` lists
- Established the relationships between locations, equipment and faults
- Added the data required to support assignment and priority rules

### Power Apps

- Built the four main screens:
  - `scrHome`
  - `scrReportFault`
  - `scrFaults`
  - `scrFaultDetail`
- Implemented Location → Equipment cascading selection
- Implemented fault reporting
- Added search and filtering
- Added fault detail and status management
- Added resolution-note validation
- Added the P1/P2/P3 visual priority indicator
- Added data refresh to prevent stale fault information being displayed

**Evidence:** [E03 — Home](images/03-home.png) · [E04 — Fault report](images/04-report-fault.png) · [E06 — Faults list](images/06-faults-list.png) · [E07 — Fault detail](images/07-fault-detail.png)

### Power Automate

- Built `Process New Equipment Fault`
- Implemented equipment lookup
- Implemented automated assignment
- Implemented automated priority calculation
- Updated the SharePoint fault record
- Added Microsoft Teams notifications
- Added Outlook email notifications

**Evidence:** [E09 — Flow overview](images/09-flow-overview.png) · [E10 — Assignment & priority logic](images/10-assignment-priority-logic.png) · [E11 — Successful flow run](images/11-successful-flow-run.png) · [E12 — Teams notification](images/12-teams-notification.png) · [E13 — Outlook notification](images/13-outlook-notification.png)

## Tested and resolved

I have tested the main Power Apps functionality, automation rules, SharePoint updates and notification routes.

I also identified and resolved the stale-data issue in Power Apps.

The underlying SharePoint record was being updated correctly, but the Power Apps Faults screen could continue displaying previous data until the data source was refreshed.

**Evidence:** [E11 — Successful flow run](images/11-successful-flow-run.png)

## Remaining work

Before I consider the MVP complete, I need to:

1. Investigate the intermittent issue where a new fault can sometimes display an ID as its Title
   
   **Evidence:** [E06 — Faults list](images/06-faults-list.png)
2. Complete the remaining minor UI refinements

## Scope

The MVP covers **3 restaurants**.

The wider fictional business context is approximately **25 restaurants**, but I kept the implemented scope smaller so I could focus on the core fault-management process.

## Evidence

The project includes screenshots of the Power Apps interface and Markdown documentation covering the business problem, solution design, implementation, testing and lessons learned. The master evidence index is documented in [`07_evidence.md`](07_evidence.md).

## Current position

The solution has reached a working MVP stage, with the main end-to-end process tested.

My remaining work is primarily troubleshooting the Title issue and completing minor presentation improvements rather than adding major new functionality.
