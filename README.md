# Equipment Fault & Maintenance Management MVP

## Project Overview

This is a self-directed Technical Business Analyst portfolio project based on a fictional multi-site food-service organisation and synthetic data.

The project explores the design and implementation of an equipment fault and maintenance management process using Microsoft Power Platform and Microsoft 365.

I started with a structured equipment fault and maintenance solution and later extended it with **Katie**, a Copilot Studio agent that provides a conversational route for reporting faults and accessing maintenance information.

## Business Problem

In a multi-site food-service environment, equipment faults need to be reported consistently so that the right information is available for maintenance processing.

A useful reporting process needs to capture:

- Location
- Equipment
- Fault category
- Description
- Operational impact

The project therefore focuses on creating a structured fault-reporting process and connecting reported faults to a defined maintenance workflow.

## Solution

The original solution uses:

- **Microsoft Power Apps** for structured fault reporting and fault management
- **SharePoint** for equipment and fault data
- **Power Automate** for maintenance processing and notifications
- **Microsoft Teams** for maintenance notifications
- **Outlook** for email notifications

The core data structure is:

```text
Locations
    ↓
Equipment
    ↓
Faults
```

The **Equipment Fault Reporter** Power Apps application provides the structured reporting route and functionality for viewing and managing reported faults.

A fault report captures:

- Location
- Equipment
- Fault category
- Description
- Operational impact

Once a fault is created, the existing **Process New Equipment Fault** Power Automate flow processes it according to the defined maintenance rules, including priority, team assignment and notifications.

## Katie Extension

Katie was added to explore how conversational AI could provide another way of interacting with the same business process.

Rather than replacing the existing application, Katie provides an alternative conversational route.

An employee can describe an equipment problem in natural language. Katie collects the required fault information, clarifies missing or ambiguous information, presents the proposed report and requires explicit confirmation before submission.

Katie can also provide information about the existing maintenance process.

For the Katie route, the confirmed fault information is passed to the **Submit Confirmed Equipment Fault** Agent Flow, which creates the Fault record in the existing SharePoint system.

The existing solution remains responsible for the underlying data structure and downstream maintenance process.

Katie's role is deliberately limited. It does not diagnose faults, recommend repairs or take over the existing maintenance decisions around priority, criticality or team assignment.

## Testing and Current Status

The original solution is at working MVP stage.

The core Katie conversational experience and confirmed submission path have been configured and tested. Wider integration scenarios, downstream processing and full end-to-end regression testing remain outstanding.

The detailed testing results, evidence and current project status are documented separately.

## What This Project Demonstrates

This project demonstrates my approach to taking a business problem through analysis, solution design, implementation and testing.

It covers:

- Business process analysis
- Requirements and business rules
- Data modelling
- Power Platform solution design
- Workflow automation
- Application design
- Conversational AI
- Integration between components
- Testing and evidence
- Documenting implementation decisions and limitations

The project also demonstrates how an existing business solution can be extended with a new technology while keeping the original process and responsibilities intact.

## Project Documentation

### Equipment Fault & Maintenance Management System

- [01 — Business Problem](01.%20Equipment%20Fault%20%26%20Maintenance%20Management%20System/01-business-problem.md)
- [02 — Solution Overview](01.%20Equipment%20Fault%20%26%20Maintenance%20Management%20System/02-solution-overview.md)
- [03 — Power Apps](01.%20Equipment%20Fault%20%26%20Maintenance%20Management%20System/03-power-apps.md)
- [04 — Power Automate](01.%20Equipment%20Fault%20%26%20Maintenance%20Management%20System/04-power-automate.md)
- [05 — Testing and Lessons Learned](01.%20Equipment%20Fault%20%26%20Maintenance%20Management%20System/05-testing-and-lessons-learned.md)
- [06 — Project Status](01.%20Equipment%20Fault%20%26%20Maintenance%20Management%20System/06-project-status.md)
- [07 — Evidence](01.%20Equipment%20Fault%20%26%20Maintenance%20Management%20System/07_evidence.md)

### Katie — Conversational Extension

- [08 — Conversational Fault Reporting](02.%20Conversational%20Equipment%20Fault%20Reporting%20%26%20Maintenance%20Support/08-conversational-fault-reporting.md)
- [09 — Katie Copilot Agent](02.%20Conversational%20Equipment%20Fault%20Reporting%20%26%20Maintenance%20Support/09-katie-copilot-agent.md)
- [10 — Katie System Integration](02.%20Conversational%20Equipment%20Fault%20Reporting%20%26%20Maintenance%20Support/10-katie-system-integration.md)
- [11 — Katie Testing and Lessons Learned](02.%20Conversational%20Equipment%20Fault%20Reporting%20%26%20Maintenance%20Support/11-katie-testing-and-lessons-learned.md)
- [12 — Katie Project Status](02.%20Conversational%20Equipment%20Fault%20Reporting%20%26%20Maintenance%20Support/12-katie-project-status.md)

## Repository Structure

```text
Equipment-Fault-Maintenance-MVP/
│
├── 01. Equipment Fault & Maintenance Management System/
│   ├── Images/
│   ├── 01-business-problem.md
│   ├── 02-solution-overview.md
│   ├── 03-power-apps.md
│   ├── 04-power-automate.md
│   ├── 05-testing-and-lessons-learned.md
│   ├── 06-project-status.md
│   └── 07_evidence.md
│
├── 02. Conversational Equipment Fault Reporting & Maintenance Support/
│   ├── Images/
│   ├── 08-conversational-fault-reporting.md
│   ├── 09-katie-copilot-agent.md
│   ├── 10-katie-system-integration.md
│   ├── 11-katie-testing-and-lessons-learned.md
│   └── 12-katie-project-status.md
│
├── README.md
└── ROADMAP.md
```

**[Project Roadmap](ROADMAP.md)**
