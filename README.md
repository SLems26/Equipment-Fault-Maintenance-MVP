# Equipment Fault & Maintenance Management

I built this self-directed project as an Equipment Fault & Maintenance Management System for a fictional multi-site food-service organisation, using Microsoft Power Apps, SharePoint, Power Automate, Microsoft Teams and Outlook.

The original solution provides structured equipment fault reporting and a workflow for processing new faults. I later extended it with **Katie**, a conversational interface for equipment fault reporting and maintenance support.

Katie extends the existing solution rather than replacing it. The original Power Apps reporting route remains available.

## Why I built it

Equipment faults can be reported through different channels. This can lead to inconsistent information, unclear ownership, manual follow-up and limited visibility of maintenance activity.

I wanted to build a small working solution that gives staff a consistent way to report faults and gives maintenance users a clearer way to manage them.

I then explored how a conversational interface could sit alongside the existing solution, allowing employees to describe a fault naturally while keeping the existing data model, automation and maintenance rules in place.

## What I built

The original system uses Power Apps for reporting, SharePoint for the underlying data, and Power Automate to process new faults, apply the existing assignment and priority rules, and send notifications through Microsoft Teams and Outlook.

The solution is structured around:

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

The MVP uses three core SharePoint lists:

```text
Locations
    |
    v
Equipment
    |
    v
Faults
```

The Power Apps application allows users to report and manage faults, including recording the location, equipment, fault category, description and operational impact.

Power Automate processes new faults, determines the assigned team and priority, updates the Fault record and sends notifications.

### Katie

Katie provides a conversational route into the same process.

Employees can describe a fault naturally. Katie collects the required information, clarifies anything missing or ambiguous, presents the proposed report and requires explicit confirmation before submission.

Katie can also answer questions about the existing maintenance process, including reporting, priority, assigned teams, statuses and what happens after submission.

Katie does not determine criticality, priority or assigned team, diagnose equipment faults or recommend repairs. Those responsibilities remain outside the conversational layer.

## How Katie fits into the system

The integration follows a simple principle:

> **Katie understands and collects the information. The existing system remains responsible for recording and processing the fault.**

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
Submit Confirmed Equipment Fault
   ↓
SharePoint Faults list
   ↓
Existing maintenance process
```

The employee can therefore use either:

- Katie
- The original Equipment Fault Reporter Power Apps application

The confirmed conversational submission uses a native Copilot Studio Agent Flow.

The Agent Flow resolves the relevant **Location and Equipment records** and creates the Fault. The existing **Process New Equipment Fault** Power Automate flow remains responsible for the downstream maintenance processing.

A successful Fault creation does not by itself mean that all downstream maintenance processing has completed.

## Current Status

The original system is at working MVP stage, with one known Title issue still under investigation. The core Katie conversational experience has also been configured and tested, with the confirmed submission path connected to SharePoint.

### Implemented and tested

| Area | Status |
|---|---|
| Katie Copilot agent | **Implemented / Tested** |
| Natural-language fault reporting | **Implemented / Tested** |
| Five-field collection | **Implemented / Tested** |
| Clarification and missing information | **Implemented / Tested** |
| Configured reporting values | **Implemented / Tested** |
| Confirmation before submission | **Implemented / Tested** |
| Change after confirmation | **Implemented / Tested** |
| Maintenance support | **Implemented / Tested** |
| Submit Confirmed Equipment Fault Agent Flow → SharePoint Fault creation | **Implemented / Tested** |

The current MVP uses a deliberately simple Agent Flow. Katie performs the conversational checking of configured locations, equipment and relationships, while the flow resolves the corresponding SharePoint records and creates the Fault.

### Not yet completed

The wider design and test plan includes additional scenarios that I have not yet marked as implemented or tested:

- Full runtime validation within the Agent Flow.
- Duplicate protection.
- Failure and retry handling.
- Verification of the existing downstream maintenance flow.
- Verification of priority and assigned-team processing after an AI-created Fault.
- Verification of email and Teams notifications.
- Full end-to-end regression testing.

These scenarios are part of the wider test plan, but I have not yet tested them.

## Project Documentation

### Original System

- [01 — Business Problem](01-business-problem.md)
- [02 — Solution Overview](02-solution-overview.md)
- [03 — Power Apps](03-power-apps.md)
- [04 — Power Automate](04-power-automate.md)
- [05 — Testing and Lessons Learned](05-testing-and-lessons-learned.md)
- [06 — Project Status](06-project-status.md)
- [07 — Evidence Index](07_evidence.md)

### Katie Extension

- [08 — Conversational Fault Reporting](08-conversational-fault-reporting.md)
- [09 — Katie Copilot Agent](09-katie-copilot-agent.md)
- [10 — Katie System Integration](10-katie-system-integration.md)
- [11 — Katie Testing and Lessons Learned](11-katie-testing-and-lessons-learned.md)
- [12 — Katie Project Status](12-katie-project-status.md)

## Screenshots

The `images` folder contains screenshots from the current implementation.

### Home

![Home screen](images/03-home.png)

### Report a Fault

![Report a Fault screen](images/04-report-fault.png)

### Faults

![Faults screen](images/06-faults-list.png)

### Fault Detail

![Fault Detail screen](images/07-fault-detail.png)

## Project Roadmap

This project is evolving as a practical Technical Business Analyst learning portfolio.

| Stage | Focus | Outcome |
|---|---|---|
| **1. Project 1** | Power Platform + Microsoft 365 | Equipment Fault Maintenance MVP |
| **2. APIs & Integration** | REST APIs, JSON, HTTP, authentication | Practical API integration |
| **3. Data & SQL** | Relational data, SQL, data quality | Operational data analysis |
| **4. Cloud & Architecture** | Azure/AWS, architecture, security, scalability | Technical architecture and options assessment |
| **5. AI & Automation** | GenAI, AI APIs, workflows, evaluation | AI Operations Assistant prototype |
| **6. Technical BA Delivery** | Technical requirements, NFRs, integrations, data flows, testing | Technical BA requirements/design pack |
| **7. Capstone** | Bringing the capabilities together | End-to-end Technical BA case study and portfolio |
| **Candidate Extension** | Predictive maintenance | Explore predictive analytics using equipment and fault data |

The wider direction is:

**Power Platform → APIs & Integration → Data & SQL → Cloud & Architecture → AI & Automation → Technical BA Capstone**

Katie is part of the project's AI & Automation development.

## About the Project

I built this as a self-directed project using a fictional multi-site food-service organisation and synthetic data.

Through the project, I have developed practical experience of taking a business process, designing a solution and building it across Microsoft technologies. I then extended the solution with a conversational interface, keeping the existing data model, business rules and maintenance process in place.
