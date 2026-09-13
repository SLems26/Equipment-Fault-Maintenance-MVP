# Katie — Project Status

## Project Overview

I started this project as an Equipment Fault & Maintenance Management System for a fictional multi-site food-service organisation.

The original solution provides structured fault reporting through Power Apps, with SharePoint as the data layer and Power Automate handling the maintenance processing.

I then extended the solution with Katie, a conversational interface for equipment fault reporting and maintenance support.

Katie is an extension of the existing solution rather than a replacement for it. The original Power Apps reporting route remains available.

## Current Position

The core Katie conversational experience has now been configured and tested.

The current implementation covers:

- Natural-language fault reporting.
- Collection of the five required fault fields.
- Information provided across multiple messages.
- Missing-information handling.
- Clarification of invalid or unknown equipment.
- Configured Fault Category and Operational Impact values.
- Complete report presentation.
- Explicit confirmation before submission.
- Reconfirmation when a material value changes.
- Maintenance-support questions.
- Boundaries around criticality, priority, assignment, diagnosis and repairs.

These conversational behaviours have been tested through actual Katie conversations.

## Current Integration

The confirmed submission path has also been implemented.

The current flow is:

```text
Employee
   ↓
Katie
   ↓
Explicit confirmation
   ↓
Submit Confirmed Equipment Fault
   ↓
SharePoint Faults list
   ↓
Existing maintenance process
```

The Agent Flow takes the confirmed business values, resolves the relevant SharePoint records and creates the Fault.

A successful test has demonstrated the Katie → Agent Flow → SharePoint Fault creation path.

The existing **Process New Equipment Fault** flow remains responsible for the downstream maintenance processing rather than moving those rules into Katie.

## What Is Implemented

| Area | Status |
|---|---|
| Katie Copilot agent | **Implemented** |
| Natural-language fault reporting | **Implemented / Tested** |
| Five-field collection | **Implemented / Tested** |
| Clarification and missing information | **Implemented / Tested** |
| Configured reporting values | **Implemented / Tested** |
| Confirmation before submission | **Implemented / Tested** |
| Change after confirmation | **Implemented / Tested** |
| Maintenance support | **Implemented / Tested** |
| Submit Confirmed Equipment Fault Agent Flow → SharePoint Fault creation | **Implemented / Tested** |

The Agent Flow currently uses the Thin MVP approach. Katie performs the conversational checking of configured locations, equipment and relationships, while the flow resolves the corresponding SharePoint records and creates the Fault.

## What Has Not Yet Been Completed

There are still areas from the wider design and test plan that I have not marked as implemented or tested.

These include:

- Full runtime validation within the Agent Flow.
- Duplicate protection.
- Failure and retry handling.
- Verification of the existing downstream maintenance flow.
- Verification of priority and assigned-team processing after an AI-created Fault.
- Verification of email and Teams notifications.
- Full end-to-end regression testing.

These scenarios are part of the wider test plan, but I have not yet tested them.

## Design Decisions

A few decisions shaped the current implementation.

### Keep AI focused on the conversation

Katie understands what the employee is saying, collects the required information, clarifies it and obtains confirmation.

She does not determine equipment criticality, priority or assigned team. Those responsibilities remain with the existing solution.

### Keep the existing maintenance process

I did not rebuild the existing Power Automate maintenance logic inside Copilot Studio.

This keeps the AI extension connected to the existing system rather than creating a second version of the business process.

### Use confirmation before creating the record

The employee confirms the proposed report before the Agent Flow is called.

This gives the conversation a clear point between collecting information and creating an official Fault.

### Keep the MVP implementation focused

The original design included more extensive validation within the submission flow.

For the current MVP, I kept the Agent Flow simpler and relied on Katie's configured conversational checking for locations, equipment and their relationships.

A more extensive runtime validation approach can be added later if required.

## Future Refinements

The next improvements would focus on strengthening the integration rather than expanding Katie's role.

Potential refinements include:

- More robust Agent Flow validation.
- Duplicate protection.
- Better handling of submission failures and retries.
- Testing the existing maintenance automation against AI-created Faults.
- Full end-to-end testing from conversation through to notifications.
- Expanding the evidence set around the integration.

## Overall Project Status

The original Equipment Fault & Maintenance Management System remains the foundation. Katie now provides a working conversational reporting route through to confirmed Fault creation in SharePoint.

The current position is:

```text
Original system
      ↓
Katie conversational layer
      ↓
Confirmed submission
      ↓
Agent Flow
      ↓
SharePoint Fault creation
      ↓
Existing maintenance processing
```

The core conversational experience and confirmed SharePoint submission path are now in place. The remaining work is primarily around broader integration testing, validation and the refinements identified above.
