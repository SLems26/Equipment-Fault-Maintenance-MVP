# Solution Overview

## Overview

I designed the Equipment Fault & Maintenance Management MVP to provide a consistent process for reporting and managing equipment faults across 3 restaurants.

I used Microsoft Power Apps, SharePoint, Power Automate, Microsoft Teams and Outlook.

## Actors

I identified three main actors in the MVP:

- **Restaurant staff** — report equipment faults
- **Maintenance users** — review and manage reported faults
- **Automated processes** — apply assignment and priority rules and send notifications

## Inputs

The fault-reporting process captures:

- Restaurant/location
- Equipment
- Fault category
- Description
- Operational impact

Equipment criticality is held as part of the equipment data and is used by Power Automate when determining priority.

## Outputs and outcomes

The solution produces:

- A fault record stored in SharePoint
- An assigned maintenance team
- A calculated priority
- Notifications through Microsoft Teams and Outlook

Once a fault has been reported, maintenance users can manage its status and add resolution information through Power Apps.

## Overall process

I designed the main process as:

```text
Restaurant staff
       |
       v
Power Apps
Report fault
       |
       v
SharePoint
Store fault
       |
       v
Power Automate
Process new fault
       |
       +--------------------+
       |                    |
       v                    v
Microsoft Teams          Outlook
Fault alert              Email notification
```

The fault remains stored in SharePoint and can then be managed through the Power Apps interface.

## Solution components

### Power Apps

I used Power Apps as the user interface for reporting and managing faults.

The app allows users to:

- Report a new equipment fault
- Select a restaurant location
- Select equipment associated with that location
- Record fault information
- View existing faults
- Search and filter faults
- Open individual fault records
- Update status
- Add resolution notes

### SharePoint

I used SharePoint as the underlying data store for the MVP.

I created three related lists:

```text
Locations
    |
    v
Equipment
    |
    v
Faults
```

This structure lets me associate equipment with a specific restaurant location and faults with the relevant equipment.

### Power Automate

I used Power Automate to process a new fault after it is created.

The flow:

- Identifies the relevant equipment
- Determines the assigned team
- Determines the fault priority
- Updates the fault record
- Sends notifications

### Microsoft Teams

I used Teams to provide a notification channel for the maintenance team.

New fault notifications are sent to the **Fault Alerts** channel within the **Equipment Maintenance** team.

### Outlook

I also added an Outlook email notification route for new faults.

This gives the process both a Teams-based notification and an email notification.

## Interfaces

The main interfaces between the solution components are:

```text
Power Apps ↔ SharePoint
SharePoint → Power Automate
Power Automate → Microsoft Teams
Power Automate → Outlook
```

I used these interfaces to move fault information between the main components of the MVP.

## Assignment and priority

I used simple business rules to support initial fault triage.

### Assignment

| Equipment Type | Assigned team |
|---|---|
| Refrigeration | Refrigeration Contractor |
| POS | IT Support |
| Digital Display | IT Support |
| HVAC | Facilities |
| Other/default | Internal Maintenance |

### Priority

I determine priority using equipment criticality and operational impact:

- **P1** — Critical equipment and Outage impact
- **P2** — Outage or Major impact, where the fault is not P1
- **P3** — Remaining faults

These rules provide a consistent starting point for assigning and prioritising new faults.

## Fault status

The MVP supports:

```text
New | Assigned | In Progress | Awaiting external support | Resolved | Closed
```

I chose not to enforce a rigid sequence between these statuses. This allows maintenance users to update the status when circumstances change.

When a fault is marked as **Resolved**, the app requires resolution notes.

## Scope

I deliberately limited the MVP to 3 restaurants.

The wider project is based on a fictional organisation with approximately 25 restaurants, but the smaller MVP scope allowed me to demonstrate the core process and technical design without introducing unnecessary complexity.

## Design approach

I separated the main responsibilities across the technologies:

- **Power Apps** — user interface and fault management
- **SharePoint** — data storage
- **Power Automate** — automated processing and business rules
- **Teams and Outlook** — notifications

This gave me a straightforward way to connect several Microsoft technologies around one business process.
