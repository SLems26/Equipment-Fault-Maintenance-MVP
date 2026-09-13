# Equipment Fault & Maintenance Management MVP

I built this self-directed MVP around a fictional multi-site food-service organisation.

## Why I built it

Equipment faults can be reported through different channels. This can lead to inconsistent information, unclear ownership, manual follow-up and limited visibility of maintenance activity.

I wanted to build a small working solution that gives staff a consistent way to report faults and gives maintenance users a clearer way to manage them.

## What I built

I used Microsoft Power Apps, SharePoint, Power Automate, Microsoft Teams and Outlook.

```text
                 ┌───────────────┐
                 │   Power Apps  │
                 └───────┬───────┘
                         │
                         v
                 ┌───────────────┐
                 │   SharePoint  │
                 └───────┬───────┘
                         │
                    new fault
                         │
                         v
                 ┌───────────────┐
                 │ Power Automate │
                 └───────┬───────┘
                         │
                  ┌──────┴──────┐
                  v             v
           ┌───────────┐   ┌───────────┐
           │   Teams   │   │  Outlook  │
           │notification│  │   email   │
           └───────────┘   └───────────┘
```

I used Power Apps for reporting and managing faults, SharePoint for the underlying data, and Power Automate to process new faults, apply assignment and priority rules, update the fault record and send notifications through Teams and Outlook.

## What the MVP can do

### Power Apps

I built the app so that users can:

- Report an equipment fault
- Select a location and then the relevant equipment
- Record the fault category
- Record a description
- Record the operational impact
- View reported faults
- Sort faults with the newest first
- Search faults by title
- Filter faults by status
- Filter faults by category
- Open a fault and view its details
- Update the fault status
- Add resolution notes
- Require resolution notes when a fault is marked as Resolved
- Show priority using a simple RAG indicator

The main screens are:

- Home
- Report a Fault
- Faults
- Fault Detail

The status options are:

`New` | `Assigned` | `In Progress` | `Awaiting external support` | `Resolved` | `Closed`

I chose not to enforce a rigid status-transition model because the maintenance situation may change and a fault may need to move back to an earlier status.

### Power Automate

I built the **Process New Equipment Fault** flow to process a fault when a new fault record is created.

It:

1. Looks up the relevant equipment
2. Determines the assigned team
3. Determines the priority
4. Updates the fault record
5. Sends a notification to the maintenance team in Microsoft Teams
6. Sends an email notification through Outlook

#### Assignment rules

| Equipment Type | Assigned team |
|---|---|
| Refrigeration | Refrigeration Contractor |
| POS | IT Support |
| Digital Display | IT Support |
| HVAC | Facilities |
| Other/default | Internal Maintenance |

#### Priority rules

- **P1** — Critical equipment and an Outage impact
- **P2** — Outage or Major impact, where the fault is not P1
- **P3** — Remaining faults

### Microsoft Teams and Outlook

I used two notification routes:

- **Microsoft Teams** — maintenance notification in the **Equipment Maintenance** team's **Fault Alerts** channel
- **Outlook** — email notification to the relevant recipients

## Data model

I used three SharePoint lists:

```text
Locations
    |
    v
Equipment
    |
    v
Faults
```

This gives me a simple relationship between a restaurant location, its equipment and the faults associated with that equipment.

## Project Documentation

- [01 — Business Problem](01-business-problem.md)
- [02 — Solution Overview](02-solution-overview.md)
- [03 — Power Apps](03-power-apps.md)
- [04 — Power Automate](04-power-automate.md)
- [05 — Testing and Lessons Learned](05-testing-and-lessons-learned.md)
- [06 — Project Status](06-project-status.md)
- [07 — Evidence Index](07_evidence.md)

## Screenshots

The `images` folder contains screenshots from my current implementation.

### Home

![Home screen](images/03-home.png)

### Report a Fault

![Report a Fault screen](images/04-report-fault.png)

### Faults

![Faults screen](images/06-faults-list.png)

### Fault Detail

![Fault Detail screen](images/07-fault-detail.png)

## A problem I had to troubleshoot

While testing the app, I found that a status could be successfully updated in SharePoint while the Faults screen continued to show the previous value.

The SharePoint record was correct, but the app was displaying stale data.

I resolved this by refreshing the `Faults` data source when the Faults screen becomes visible:

```powerapps
Refresh(Faults)
```

This ensured that the latest SharePoint data was loaded when I returned to the fault list.

This was an important lesson for me: successfully updating the underlying data source does not necessarily mean that the application is immediately displaying the latest value.

## Current status

The core MVP functionality is implemented.

The remaining work before I finalise the MVP is:

- Complete end-to-end testing
- Investigate an intermittent issue where a fault Title can sometimes appear as an ID after submission
- Make any final minor UI adjustments

## About the project

I built this as a self-directed project using a fictional multi-site food-service organisation and synthetic data.

The wider project is based on approximately 25 restaurants, while this MVP is deliberately limited to 3 restaurants.

Through the project, I have developed practical experience of taking a business process, designing a solution and building it across several Microsoft technologies.
