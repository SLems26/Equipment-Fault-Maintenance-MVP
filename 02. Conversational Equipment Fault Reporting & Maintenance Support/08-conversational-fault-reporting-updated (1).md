# Conversational Equipment Fault Reporting & Maintenance Support

## Overview

I extended my Equipment Fault & Maintenance Management System with a
conversational interface called Katie.

The aim was not to replace the existing solution, but to explore how a
conversational assistant could make the front end of the process more
natural for restaurant staff.

Katie gives employees two ways to get help. They can describe an
equipment fault in their own words and be guided through the reporting
process, or they can ask questions about how the existing maintenance
process works.

The underlying Power Apps, SharePoint and Power Automate solution
remains in place.

## Why I Added Katie

The original solution provides a structured way for employees to report
equipment faults.

I wanted to explore a different interaction: instead of requiring an
employee to start with a form, they can begin with a natural description
such as:

> "The main freezer at Croydon Central isn't working and the kitchen is
> completely down."

Katie can identify the information that has already been provided, ask
for anything that is missing, clarify anything that is ambiguous, and
build the structured fault report.

This creates a conversational front end while keeping the existing
system and maintenance process underneath it.

## What Katie Does

I designed Katie around two capabilities.

### Conversational Fault Reporting

An employee can describe a fault in natural language rather than
entering each field separately.

Katie collects five required pieces of information:

-   Location
-   Equipment
-   Fault category
-   Description
-   Operational impact

Information can be provided in any order and across multiple messages.

Katie does not simply accept incomplete or ambiguous information. Where
information is missing or unclear, she asks the employee to clarify.

Once all five fields have been collected, Katie presents the complete
proposed report and asks the employee to confirm it. This confirmation
step is demonstrated in the captured evidence: [Evidence 06](Images/06-katie-confirmation.png).

Only after explicit confirmation does the submission process begin.

### Maintenance Support

I also wanted Katie to be useful when an employee is not trying to
report a fault.

Staff can ask questions about the existing maintenance process, such as:

-   How is priority determined?
-   Which team handles refrigeration equipment?
-   What happens after I submit a fault?
-   What happens when a fault is resolved?
-   What does an operational impact such as "Outage" mean?

Katie uses the available maintenance knowledge to answer these
questions. [Evidence 11](Images/11-katie-maintenance-support.png)

Where the available information does not define an answer, Katie is
instructed to say so rather than inventing one.

## How It Integrates with the Existing System

A key design decision was to keep the existing system and introduce
Katie around it.

The current process is:

Employee\
↓\
Katie\
↓\
Natural-language fault description\
↓\
Clarification and structured information\
↓\
Proposed fault report\
↓\
Explicit employee confirmation\
↓\
Copilot Studio Agent Flow\
↓\
SharePoint Faults list\
↓\
Existing maintenance process

The existing maintenance process remains responsible for the downstream
business rules, including equipment criticality, priority, assigned team
and notifications.

This means the conversational layer does not replace those existing
processes.

## Controls and Boundaries

I deliberately kept Katie's role narrow. [Evidence 12](Images/12-katie-support-reporting-boundary.png)

Katie is a reporting and maintenance-support assistant, not a
maintenance engineer.

She does not:

-   Diagnose the technical cause of a fault.
-   Recommend repairs or repair instructions.
-   Determine equipment criticality.
-   Determine priority during fault reporting.
-   Determine the assigned maintenance team.
-   Ask employees for equipment criticality.
-   Submit a fault without explicit employee confirmation.

These boundaries are important because the purpose of the conversational
interface is to make reporting and access to existing information
easier, not to allow the assistant to make maintenance decisions.

## Current MVP

For the current MVP, I configured Katie with a separate fault-reporting
knowledge source containing the available locations, equipment,
location/equipment relationships and valid fault-reporting values.
[Evidence 03](Images/03-katie-knowledge-sources.png) and [configuration source](Images/03-katie-fault-reporting-configuration.md)

I kept this configuration separate from the maintenance knowledge so
that the information Katie uses for fault reporting is distinct from the
information she uses to explain the existing maintenance process.

The current MVP has been tested for:

-   Complete fault reporting. [Evidence 05](Images/05-katie-fault-reporting.png)
-   Missing information.
-   Invalid equipment/location combinations. [Evidence 10](Images/10-katie-clarification-and-validation.png)
-   Unknown equipment.
-   Changes made after confirmation.
-   Maintenance-support questions. [Evidence 11](Images/11-katie-maintenance-support.png)
-   The boundary between maintenance support and fault reporting. [Evidence 12](Images/12-katie-support-reporting-boundary.png)

The confirmed fault is submitted through a native Copilot Studio Agent
Flow, which creates the Fault record in the existing SharePoint system.


A successful submission means that the Fault record has been created. It
does not mean that the subsequent maintenance processing has been
completed.

## What I Have Learned

The main learning from this extension has been that adding AI to an
existing business process is not simply about making the interaction
conversational.

The assistant needs clear boundaries around:

-   What information it can collect.
-   What it can infer and what requires confirmation.
-   When it should ask for clarification.
-   When it is allowed to take an action.
-   Which existing business rules should remain outside the AI layer.
-   How to distinguish a successful record creation from successful
    downstream processing.

Testing also showed the importance of designing for the conversation
around the process, not just the successful reporting path. For example,
Katie needs to distinguish between an employee asking for maintenance
guidance and an employee who actually wants to submit a fault.

## What This Extension Demonstrates

For me, the value of this extension is not simply that it uses AI.

It demonstrates how I approached adding a conversational capability to
an existing business process without replacing the underlying solution.

The work brings together:

-   Business process analysis.
-   Conversational requirements.
-   AI behaviour and instruction design.
-   Structured data requirements.
-   Confirmation and validation controls.
-   Workflow integration.
-   Testing and evidence.

The result is a conversational front end that makes the existing
equipment fault process more accessible while keeping the established
system and downstream maintenance process in place.
