# Katie — System Integration

## Integration Overview

I wanted to add a conversational interface without replacing the existing Equipment Fault & Maintenance Management System.

The integration therefore follows a simple principle:

> **Katie understands and collects the information. The existing system remains responsible for recording and processing the fault.**

The current flow is:

```text
Employee
   ↓
Katie
   ↓
Natural-language fault report
   ↓
Clarification and structured information
   ↓
Employee confirmation
   ↓
Submit Confirmed Equipment Fault Agent Flow
   ↓
SharePoint Faults list
   ↓
Existing maintenance process
```

This allowed me to introduce the conversational capability while preserving the existing Power Apps, SharePoint and Power Automate solution.

## Why I Kept the Existing Process

I deliberately avoided rebuilding the maintenance process inside Copilot Studio.

The existing solution already contains business rules for:

- Equipment criticality.
- Fault priority.
- Assigned maintenance team.
- Notifications.

Moving these decisions into the AI layer would have duplicated existing functionality and introduced another place where business rules could diverge.

Instead, Katie acts as an additional entry point into the existing process.

This also means that employees can continue to use the original Equipment Fault Reporter Power Apps application.

## Confirmed Fault Submission

Katie does not submit a fault as soon as she has collected enough information.

The employee first receives a complete proposed report containing:

- Location.
- Equipment.
- Fault category.
- Description.
- Operational impact.

Only after the employee explicitly confirms the report does Katie call the submission flow.

This confirmation step creates a clear boundary between **conversation** and **official record creation**.

The values passed to the submission process are the confirmed business values:

```text
Location
Equipment
Fault category
Description
Operational impact
```

Priority, criticality and assigned team are not collected from the employee or passed as part of the conversational submission.

## Copilot Studio Agent Flow

I implemented the submission step as a native **Copilot Studio Agent Flow**.

The flow is triggered when the agent calls it and is responsible for taking the confirmed fault information and creating the corresponding Fault record in SharePoint.

The current MVP flow is intentionally simple:

```text
When an agent calls the flow
        ↓
Get Location
        ↓
Get Equipment
        ↓
Create Fault item
        ↓
Respond to the agent
```

The SharePoint lookups resolve the relevant Location and Equipment records needed to create the relationships in the Fault record.

In the current MVP, Katie performs the conversational checking of the configured Location, Equipment and relationship before submission. The Agent Flow does not yet implement the full runtime relationship-validation design.

I kept the implementation focused on the core integration rather than adding unnecessary workflow complexity to the MVP.

## SharePoint Integration

The existing SharePoint **Faults** list remains the system of record.

When the confirmed submission is processed, the Agent Flow creates the Fault record using the information collected through the conversation.

The Fault record includes the confirmed:

- Location.
- Equipment.
- Fault category.
- Description.
- Operational impact.

The existing status mechanism is preserved rather than being redesigned as part of the conversational extension.

This keeps the new capability aligned with the data model already used by the original solution.

## Existing Maintenance Automation

Creating the Fault record is only the first stage of the existing maintenance process.

Once the Fault exists in SharePoint, the existing **Process New Equipment Fault** Power Automate flow remains responsible for the downstream processing.

This includes:

1. Retrieving the selected equipment record.
2. Reading equipment type and criticality.
3. Determining the responsible team.
4. Determining fault priority.
5. Updating the Fault record.
6. Sending an email notification.
7. Posting a fault alert to Microsoft Teams.

This separation was important to me because it means the AI layer does not need to reproduce business rules that already exist in the original solution.

## Handling Success and Failure

I also wanted the conversation to reflect the actual result of the integration.

A successful Agent Flow execution means that the Fault record has been created.

It does **not** mean that all downstream maintenance processing has completed.

Similarly, if the Agent Flow fails to create the Fault, Katie must not tell the employee that the fault was successfully submitted.

The agent is therefore instructed to treat the Agent Flow result as authoritative.

This creates a simple but important distinction:

```text
Fault created
      ≠
Maintenance processing completed
```

## Current MVP Implementation

The current MVP implements the core integration between Katie and the existing system.

The implemented path is:

```text
Employee
   ↓
Katie
   ↓
Explicit confirmation
   ↓
Agent Flow
   ↓
SharePoint Faults list
   ↓
Existing maintenance process
```

For the current MVP, Katie uses the configured fault-reporting information to check locations, equipment, relationships and valid reporting values during the conversation.

The Agent Flow then resolves the relevant SharePoint records and creates the Fault record. The current Thin MVP keeps the conversational checking of configured locations, equipment, relationships and reporting values primarily within Katie.

I deliberately did not expand the Agent Flow into a more complex runtime validation layer at this stage. Dynamic validation directly against SharePoint remains a potential future refinement.

## Integration Testing

I tested the integration using a confirmed fault rather than treating the conversational response itself as evidence of successful submission.

The happy-path test demonstrated that:

- Katie collected the required information.
- The employee explicitly confirmed the proposed report.
- The Agent Flow was called after confirmation.
- The flow successfully created a Fault record in SharePoint.

The evidence therefore confirms the **Katie → Agent Flow → SharePoint Fault creation** path.

The downstream maintenance process is treated as a separate outcome and is not used as evidence for successful Fault creation.

## What the Integration Demonstrates

For me, the main value of this integration is that it demonstrates how a conversational AI capability can be introduced without replacing an existing business solution.

The architecture keeps responsibilities clear:

| Component | Responsibility |
|---|---|
| **Katie** | Understand, collect, clarify and obtain confirmation |
| **Agent Flow** | Submit the confirmed fault |
| **SharePoint** | Store the official Fault record |
| **Process New Equipment Fault** | Apply the existing maintenance processing and notifications |
| **Power Apps** | Continue to provide the original structured reporting route |

This approach allowed me to extend the user experience while preserving the existing data model, automation and business rules.
