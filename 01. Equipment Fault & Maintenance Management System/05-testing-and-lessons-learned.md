# Testing and Lessons Learned

## Overview

I tested the main user journey and the key business rules across the MVP.

My testing focused on whether a fault could be reported, processed automatically, displayed correctly, and then managed through the Power Apps interface.

## End-to-end process

The main process I tested was:

```text
Report fault
    |
    v
Create SharePoint record
    |
    v
Power Automate processes fault
    |
    +--> Assign team
    +--> Calculate priority
    +--> Update fault
    +--> Send Teams notification
    +--> Send Outlook email
    |
    v
Review and manage fault in Power Apps
```

I used this process to check that the different components worked together rather than testing each component in isolation.

**Evidence:** [E11 — Successful flow run](Images/11-successful-flow-run.png)

## Power Apps testing

I tested the main functions of the Power Apps interface, including:

- Selecting a restaurant location
- Selecting equipment associated with that location
- Entering fault details
- Submitting a new fault
- Viewing the fault list
- Searching for faults
- Filtering by status
- Filtering by category
- Opening a fault detail record
- Updating the fault status
- Adding resolution notes
- Preventing a fault from being marked as Resolved without resolution notes
- Returning to the fault list after an update

I also checked the priority indicator on the fault detail screen.

**Evidence:** [E04 — Fault submission](Images/04-report-fault.png) · [E06 — Faults list](Images/06-faults-list.png) · [E07 — Fault detail](Images/07-fault-detail.png)

## Power Automate testing

I tested the automation using different combinations of equipment type, equipment criticality and operational impact.

I checked that:

- The correct maintenance team was assigned
- P1, P2 and P3 priorities were calculated according to the business rules
- The SharePoint fault record was updated
- A Teams notification was generated
- An Outlook email notification was generated

**Evidence:** [E10 — Assignment & priority logic](Images/10-assignment-priority-logic.png) · [E11 — Successful flow run](Images/11-successful-flow-run.png) · [E12 — Teams notification](Images/12-teams-notification.png) · [E13 — Outlook notification](Images/13-outlook-notification.png)

## Issues found during testing

### Stale data in Power Apps

One issue I found was that a fault could be successfully updated in SharePoint while the Faults screen continued to show the previous value.

I initially checked the underlying SharePoint record and confirmed that the update had been successful.

The problem was therefore not the database update. It was that the Power Apps screen was still displaying its previous data.

I resolved this by refreshing the `Faults` data source when the Faults screen becomes visible:

```powerapps
Refresh(Faults)
```

This ensured that the latest SharePoint data was loaded when I returned to the fault list.

### Priority indicator

I also tested the visual priority indicator on the fault detail screen.

The final mapping is:

| Priority | Indicator |
|---|---|
| P1 | Red |
| P2 | Amber |
| P3 | Green |
| Other/unrecognised value | Grey |

I kept the priority text visible as well, so the meaning is not dependent on colour alone.

**Evidence:** [E07 — Fault detail](Images/07-fault-detail.png)

### Fault title issue

During testing, I identified an intermittent issue where a newly submitted fault could sometimes display an ID value as its Title rather than the expected fault title.

This remains an item for investigation before I consider the end-to-end testing complete.

**Evidence:** [E06 — Faults list](Images/06-faults-list.png)

## What I learned

Testing the solution showed me that integration problems are not always caused by the component that appears to be failing.

The stale-data issue was a useful example. The SharePoint record had been updated correctly, but the Power Apps interface had not refreshed its displayed data.

This reinforced the importance of testing the complete path:

```text
Business action
    ↓
Application
    ↓
Data
    ↓
Automation
    ↓
Notification
    ↓
Application display
```

I also learned that business rules need to be tested with different combinations of inputs rather than only with a single successful scenario.

## Current testing position

The main Power Apps functionality, automation rules, SharePoint updates and notification routes have been tested. I have also completed an end-to-end test of the original Power Apps fault-reporting process. The remaining testing work is focused on investigating the intermittent Title-as-ID issue and completing the final UI refinements.
