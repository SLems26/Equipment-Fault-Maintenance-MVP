# Katie — Copilot Agent

## Overview

I designed Katie as the conversational front door to the existing Equipment Fault & Maintenance Management System.

The objective was to make equipment fault reporting more natural without moving the existing maintenance decisions into the AI layer.

Katie understands what an employee tells her, collects the information needed for a fault report, asks for clarification where necessary, and requires explicit confirmation before a fault can be submitted.

## Designing the Conversation

I wanted employees to be able to start with the problem rather than with a form field.

For example, an employee might say:

> "The main freezer isn't working and the kitchen is completely down."

Katie should recognise the information that has already been provided and then ask for anything that is still missing.

The five required fields are:

- Location
- Equipment
- Fault category
- Description
- Operational impact

The information does not have to be provided in a particular order, and employees can provide it across multiple messages.

This was an important part of the design because a conversational interface should not simply turn a form into a series of rigid questions.

## Fault Reporting Configuration

I kept the information Katie uses for fault reporting separate from the general maintenance knowledge.

The fault-reporting configuration contains:

- Available locations.
- Available equipment.
- Valid location/equipment relationships.
- Fault categories.
- Operational impact values.

This gives Katie a defined set of values to use when collecting and checking the information provided by the employee, rather than relying on general knowledge or assumptions.

For the current MVP, this configuration is maintained separately from the existing SharePoint data rather than being retrieved dynamically at runtime.

## Clarification Rather Than Guessing

A key behaviour I configured was that Katie should clarify rather than guess.

If required information is missing, she asks for it.

If information is ambiguous, she asks the employee to clarify rather than selecting an answer herself.

I also configured the agent to use the defined fault-reporting values rather than inventing alternative values.

For example, if an employee describes a fault in a way that suggests an outage, Katie can propose **Outage**, but the employee still needs to confirm that value.

This keeps the conversational experience flexible while maintaining structured data for the existing system.

## Confirmation Before Submission

The most important control in the conversation is explicit confirmation.

I designed the interaction around a simple state:

```text
Collecting information
        ↓
Complete report
        ↓
Waiting for confirmation
        ↓
Confirmed
        ↓
Submit
```

Once all five fields have been collected, Katie presents the complete proposed report.

The employee must explicitly confirm that the report is correct before submission.

If the employee changes a material value after confirmation, Katie returns to the confirmation step rather than continuing with the previous version.

This gives the employee a clear opportunity to catch mistakes before an official Fault record is created.

## Keeping Katie's Role Narrow

I deliberately kept Katie's responsibilities narrow.

Katie is not a maintenance engineer and does not attempt to diagnose the technical cause of a fault or recommend repairs.

She also does not:

- Determine equipment criticality.
- Determine priority.
- Determine the assigned maintenance team.
- Ask the employee for equipment criticality.
- Automatically select ambiguous equipment.
- Create the official Fault record directly.
- Submit a fault before explicit confirmation.

These boundaries were important because the existing maintenance process already contains the relevant business rules, so I kept those decisions outside the conversational layer.

I did not want the conversational layer to duplicate or compete with those rules.

## Maintenance Support

I also configured Katie to support employees when they are not actually trying to report a fault.

For example, an employee can ask how priority is determined, which team handles refrigeration equipment, or what happens after a fault is submitted.

Katie uses the available maintenance knowledge for these questions.

If the knowledge does not define an answer, she is instructed to say that the information is not currently defined rather than inventing one.

This gives Katie a second useful capability without turning her into a general-purpose maintenance adviser.

## Personality and Interaction Style

I wanted Katie to feel like a capable colleague rather than a formal form-filling system.

I therefore configured her to use:

- Natural first-person language.
- Brief acknowledgements where useful.
- Conversational clarification questions.
- Reassuring language when correcting misunderstandings.
- A slightly warmer response when a submission has been completed.

At the same time, I deliberately kept the personality restrained. The task is still operational reporting, so clarity and accuracy take priority over conversation for its own sake.

## Integration with the Existing Process

The separation between Katie and the existing maintenance process is deliberate.

The current interaction is:

```text
Employee
   ↓
Katie
   ↓
Understand + collect + clarify
   ↓
Employee confirms
   ↓
Confirmed report
   ↓
Agent Flow
   ↓
SharePoint Fault
   ↓
Existing maintenance process
```

Katie does not create the official Fault record directly. After confirmation, she passes the confirmed report to the Agent Flow, which performs the submission and creates the Fault record in SharePoint.

The existing maintenance automation continues to handle downstream processing.

This means a successful Fault creation should not be confused with completed maintenance processing.

## Testing the Agent

I tested the agent against more than just the happy path.

The tests included:

- Natural-language fault descriptions.
- Missing information.
- Information provided across multiple messages.
- Configured fault categories and operational impacts.
- Complete report presentation.
- Explicit confirmation.
- Changes after confirmation.
- Invalid equipment and location combinations.
- Unknown equipment.
- Maintenance-support questions.
- The boundary between asking for maintenance help and actually reporting a fault.

Testing also showed that natural-language interpretation needed to remain controlled against the structured values required by the existing system.

For example, Katie initially used an informal description such as "freezer unavailable" rather than the configured **Operational Impact** value. I refined the instructions so that Katie could interpret natural language while using only the configured values when proposing the structured field.

The subsequent test correctly produced **Outage**.

This was a useful reminder that conversational flexibility still needs clear boundaries when the output ultimately feeds a structured business process.

## Current Implementation and Evidence

The core conversational behaviour has been configured and tested.

The current MVP demonstrates:

- Natural-language fault reporting.
- Collection of the five required fields.
- Clarification of missing or ambiguous information.
- Conversational validation against the configured fault-reporting information.
- Explicit confirmation before submission.
- Maintenance-support conversations using the available knowledge.
- Submission of confirmed reports through the Copilot Studio Agent Flow.

The current implementation deliberately keeps the conversational layer separate from the existing maintenance business rules, so Katie collects and confirms information while the existing solution continues to determine priority, assignment and notifications.

The confirmed report is passed to the Agent Flow, which creates the Fault record in the existing SharePoint system.

A successful submission means that the Fault record has been created. It does not mean that the subsequent maintenance processing has been completed.

I then tested the hand-off between Katie, the Agent Flow and SharePoint to verify that a confirmed report could result in a Fault record being created.
