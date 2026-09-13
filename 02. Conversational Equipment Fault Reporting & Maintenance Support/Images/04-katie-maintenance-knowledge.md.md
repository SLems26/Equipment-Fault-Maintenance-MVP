# Equipment Maintenance Knowledge

## Purpose

The Equipment Maintenance solution allows restaurant staff to report equipment faults and understand how those faults are processed.

## How to report a fault

Staff can report faults using the Equipment Fault Reporter Power Apps application or you.

The reporter must provide:

- Location
- Equipment
- Fault category
- Description
- Operational impact

These fields are required to submit a fault.

The equipment available for selection is filtered according to the selected location.

## What happens after submission

When a fault is submitted:

1. The fault is saved to the SharePoint Faults list.
2. Its initial Status is New.
3. Its initial Assigned Team is Unassigned.
4. Power Automate retrieves the selected equipment record.
5. The flow reads the equipment type and criticality.
6. The flow determines the responsible team.
7. The flow determines the fault priority.
8. The SharePoint fault record is updated.
9. An email notification is sent.
10. A fault alert is posted to Microsoft Teams.

## Operational impact

Operational impact is selected by the person reporting the fault.

Available values are:

- Outage
- Major
- Minor
- No immediate impact

## Priority rules

Priority is assigned automatically.

### P1 — Critical

A fault is P1 when:

- Equipment Criticality is Critical; and
- Operational Impact is Outage.

### P2 — High

A fault is P2 when it is not P1 and:

- Operational Impact is Outage; or
- Operational Impact is Major.

### P3 — Normal

A fault is P3 when it is not P1 or P2.

This currently covers:

- Minor operational impact
- No immediate impact

These are prototype business rules for this portfolio solution and are not claimed to be industry-standard maintenance rules.

## Assigned team rules

The assigned team is determined automatically from Equipment Type.

- Refrigeration → Refrigeration Contractor
- POS → IT Support
- Digital Display → IT Support
- HVAC → Facilities
- All other equipment types → Internal Maintenance

## Fault status

A reported fault can have one of the following statuses:

- New
- Assigned
- In Progress
- Awaiting external support
- Resolved
- Closed

A newly reported fault starts with:

- Status: New

When a fault is marked as Resolved, resolution notes are required.

## What the Maintenance Support Assistant should help with

The Maintenance Support Assistant is named Katie.

The assistant can explain:

- How to report a fault
- What information is required to report a fault
- What the operational impact options mean
- How P1, P2 and P3 are determined
- How the assigned team is determined
- What the different fault statuses mean
- What happens when a fault is resolved
- What happens after a fault is submitted

The assistant should also help staff describe an equipment fault clearly and summarise the information they provide.

If the available maintenance knowledge does not answer a question, the assistant should say that the information is not currently defined rather than inventing an answer.
