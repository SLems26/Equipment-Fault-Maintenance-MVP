# Power Apps

## Overview

Power Apps provides the user interface for reporting and managing equipment faults.

The MVP is a Canvas app connected to the SharePoint data model:

```text
Locations
    |
    v
Equipment
    |
    v
Faults
```

The app provides a simple workflow from reporting a fault through to reviewing and updating the fault record.

## Screens

The app contains four main screens:

- `scrHome`
- `scrReportFault`
- `scrFaults`
- `scrFaultDetail`

### Home

`scrHome` provides the starting point for the application and navigation to the main fault-management functions.

### Report a Fault

`scrReportFault` allows a user to create a new equipment fault.

The form captures:

- Location
- Equipment
- Fault category
- Description
- Operational impact

The Equipment selection is dependent on the selected Location.

![Report a Fault](images/04-report-fault.png)

*Evidence E04 — fault reporting form with the business inputs populated.*

This creates a cascading selection:

```text
Select Location
      |
      v
Show equipment for that location
      |
      v
Select Equipment
```

This helps prevent users from selecting equipment that does not belong to the selected location.

### Faults

`scrFaults` provides a list of reported faults.

The screen supports:

- Newest-first sorting
- Search by fault title
- Status filtering
- Category filtering
- Selecting a fault to view its details

The search and filtering are performed against the SharePoint data source.

![Faults list](images/06-faults-list.png)

*Evidence E06 — fault list with search, filtering, sorting and processed fault information.*

### Fault Detail

`scrFaultDetail` displays the selected fault and allows maintenance users to update it.

The screen supports:

- Viewing fault information
- Viewing priority
- Updating status
- Adding resolution notes
- Returning to the fault list

When a fault is marked as **Resolved**, resolution notes are required before the update can be saved.

![Fault detail](images/07-fault-detail.png)

*Evidence E07 — fault detail, priority, assigned team, status and resolution fields.*

## Application Navigation

![Home screen](images/03-home.png)

*Evidence E03 — application home screen, navigation and fault summary.*

## Navigation

The main navigation is:

```text
Home
  |
  v
Report a Fault
  |
  v
Submit
  |
  v
Faults
  |
  v
Fault Detail
  |
  +----> Back to Faults
  |
  +----> Back to Home
```

The app uses the selected fault record to maintain context when moving from the fault list to the detail screen.

## Fault status

The available status values are:

- New
- Assigned
- In Progress
- Awaiting external support
- Resolved
- Closed

The app does not enforce a rigid sequence between these statuses.

This allows maintenance users to update the status when circumstances change.

## Resolution handling

Resolution notes are captured on the Fault Detail screen.

When the user selects **Resolved**, the app checks that resolution notes have been entered.

If the notes are blank, the save operation is stopped and the user is shown an error message.

This provides a simple validation rule at the application level.

## Priority indicator

The Fault Detail screen displays the fault priority using a small RAG-style visual indicator.

The current mapping is:

| Priority | Indicator |
|---|---|
| P1 | Red |
| P2 | Amber |
| P3 | Green |
| Other/unrecognised value | Grey |

The priority value remains available as text as well as being represented visually, so the colour indicator is not the only way to understand the priority.

The priority itself is determined by the Power Automate process rather than by the Power Apps interface.

## Data interaction

Power Apps uses SharePoint as its data source.

The main relationship is:

```text
Locations
    |
    v
Equipment
    |
    v
Faults
```

The app reads and writes fault information through the SharePoint `Faults` list.

When a fault is updated, the application uses Patch to update the relevant SharePoint record.

## Search and filtering

The Faults screen provides basic operational filtering.

Users can:

- Search by fault title
- Filter by status
- Filter by category

Results are sorted with the newest faults first.

This provides a simple way for maintenance users to find relevant faults without introducing a more complex search solution.

## Troubleshooting

During testing, I found an issue where a fault could be successfully updated in SharePoint but the Faults screen could continue to display the previous value.

The underlying SharePoint data was correct, but the app was displaying stale data.

The solution was to refresh the `Faults` data source when the Faults screen becomes visible:

```powerapps
Refresh(Faults)
```

This ensured that the latest SharePoint data was loaded when returning to the fault list.

### Technical lesson

This demonstrated an important distinction between:

```text
Data source
    ↓
Application data/state
    ↓
Displayed information
```

A successful update to the underlying data source does not necessarily mean that the application is immediately displaying the latest data.

## What I learned from building the app

Building the Power Apps solution gave me practical experience of taking a business process and turning it into a working application.

I worked with related SharePoint data, built a cascading Location → Equipment selection, added validation and filtering, and connected the different screens into a simple user workflow.

One of the more useful lessons came from troubleshooting the stale-data issue. The SharePoint record was being updated successfully, but the app was not immediately showing the updated value. This helped me understand the difference between the underlying data source and the data being displayed by the application.

The project also gave me experience of making decisions about MVP scope. Rather than trying to build a full maintenance management system, I focused on the core process of reporting, prioritising, assigning and managing faults.
