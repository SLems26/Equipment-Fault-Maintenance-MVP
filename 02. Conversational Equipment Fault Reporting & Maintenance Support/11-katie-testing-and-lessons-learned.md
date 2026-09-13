# Katie — Testing and Lessons Learned

## Testing Approach

I treated testing as part of the design rather than as a final check at the end.

The aim was not simply to demonstrate that Katie could complete a successful conversation. I wanted to test how she behaved when information was missing, ambiguous, incorrect or changed, and whether confirmed information could then be passed into the existing system.

I separated the testing into four areas:

1. Conversational behaviour
2. Fault-reporting controls
3. Agent Flow and SharePoint submission
4. Maintenance-support behaviour

This helped me distinguish between the behaviour of the conversational agent, the submission workflow, SharePoint record creation and the existing downstream maintenance process.

## Fault Reporting Tests

I tested the main reporting journey using both successful and deliberately problematic scenarios.

### Complete Fault Report

I started with a natural-language description containing most of the required information.

For example:

> "The Main Freezer at Croydon Central isn't working. The kitchen is completely down."

Katie correctly identified the Location and Equipment, proposed the relevant Fault Category and Operational Impact, and then presented the complete report before asking for confirmation.

**Result: Passed**

This established the basic reporting journey.

[Evidence 5](Images/05-katie-fault-reporting.png.png)

### Missing Information

I then tested a report where required information was missing:

> "The Main Freezer at Croydon Central isn't working."

Katie retained the information already provided and asked for the missing Fault Category rather than attempting to complete the report herself.

**Result: Passed**

This confirmed that the conversation could continue from partial information.

### Invalid Location and Equipment Combination

I deliberately provided an incorrect relationship:

> "The Main Freezer at Camden High Street isn't working."

Katie did not accept the combination. She explained that the Main Freezer was associated with Croydon Central and asked the employee to clarify.

**Result: Passed**

[Evidence 6](Images/06-katie-clarification-and-validation.png.png)

This was important because accepting a valid piece of equipment with an incorrect location could create an inaccurate Fault record.

### Unknown Equipment

I also tested equipment that was not part of the configured reporting information:

> "The ice machine at Reading Central isn't working."

Katie explained that the ice machine was not configured and provided the equipment available for that location rather than inventing an equipment record.

**Result: Passed**

### Change After Confirmation

I tested what happened when the employee changed a material value after confirming the report.

The employee changed the Operational Impact from the previously confirmed value to **Major**.

Katie updated the report and returned to the confirmation step rather than submitting the previously confirmed version.

**Result: Passed**

This demonstrated that confirmation applies to the current version of the report rather than simply acting as a one-time approval.

## Confirmation Testing

Explicit confirmation was one of the most important controls in the design.

I tested that Katie:

- Presents the complete proposed report.
- Clearly asks the employee to confirm it.
- Does not treat an unrelated response as confirmation.
- Does not submit before confirmation.
- Returns to confirmation when a material value changes.

These tests gave me confidence that the conversational layer had a clear boundary before official record creation.

[Evidence 7](Images/07-katie-confirmation.png.png)

## Maintenance Support Tests

I also tested the other side of Katie's role: helping employees understand the existing maintenance process without unnecessarily starting a fault report.

The scenarios included:

- How priority is determined.
- What happens after a fault is submitted.
- Which team handles refrigeration equipment.
- How to report a fault.
- What an Operational Impact such as **Outage** means.
- What happens when a fault is resolved.
- What to do when an equipment problem is described without an explicit request to report it.

Katie correctly used the available maintenance knowledge and remained within its defined scope.

[Evidence 12](Images/12-katie-maintenance-support.png.png)

Where the knowledge did not define an answer, such as a formal definition of **Outage**, Katie did not invent one.

**Result: Passed**

I also tested situations where an employee described an equipment problem but was asking for help rather than trying to submit a fault. Katie remained in maintenance-support mode rather than automatically beginning the reporting process.

[Evidence 13](Images/13-katie-support-reporting-boundary.png)

## Testing Natural-Language Interpretation

One useful issue emerged during testing.

Katie initially used informal descriptions such as "freezer unavailable" rather than the configured **Operational Impact** value.

The problem was not that Katie had misunderstood the employee. The issue was that the conversational interpretation was not yet sufficiently controlled for the structured field required by the existing system.

I refined the instructions so that Katie could interpret natural language while proposing only the configured values.

A subsequent test correctly produced:

**Operational Impact: Outage**

**Result: Passed after refinement**

This was one of the most useful lessons from the testing because it showed the difference between understanding natural language and producing controlled business data.

## Agent Flow and SharePoint Testing

After the conversational tests, I tested the confirmed submission path.

The happy-path test demonstrated that:

- Katie collected the required information.
- The employee explicitly confirmed the report.
- The Agent Flow was called after confirmation.
- The Agent Flow successfully created a Fault record in SharePoint.

The evidence therefore confirms the:

**Katie → Agent Flow → SharePoint Fault creation**

path.

The SharePoint Fault record was created successfully from the confirmed report.

This was important because I did not treat Katie's conversational response as proof that a transaction had taken place.

## Issues Found and Changes Made

The main refinement identified through testing was the handling of natural-language descriptions for structured Operational Impact values.

The initial behaviour was conversationally understandable but did not consistently map the employee's description to the exact configured value required by the existing system.

I refined the agent instructions to make the structured values explicit.

The subsequent test confirmed that Katie proposed **Outage** correctly.

This showed the value of testing actual conversation behaviour rather than assuming that the initial instructions would produce the required structured output.

## What I Have Not Yet Claimed

The project test plan contains additional scenarios beyond the current demonstrated evidence, including duplicate protection, Agent Flow validation, downstream maintenance processing, notifications, failure handling and broader end-to-end testing.

I have deliberately not treated these as passed simply because the core submission test succeeded.

In particular, the current evidence confirms that a confirmed fault can be submitted through the Agent Flow and that the resulting Fault record is created in SharePoint. I have not yet tested the full downstream maintenance process end to end.

## Lessons Learned

### 1. Conversational flexibility still needs structure

Natural language makes the interaction easier for the employee, but the output still needs to fit the structured data model.

The solution therefore needs both:

**flexible input**

and

**controlled output**.

### 2. Confirmation is a business control, not just a conversational feature

The confirmation step provides a clear point where responsibility moves from information gathering to official submission.

It also provides protection when an employee changes their mind or corrects information.

### 3. AI should not duplicate existing business rules unnecessarily

I found it more useful to keep priority, criticality and assignment outside Katie rather than attempting to reproduce those rules in the conversational layer.

This keeps responsibilities clearer and reduces the risk of conflicting logic.

### 4. Testing the boundary cases was more valuable than testing only the happy path

The invalid equipment/location test, unknown equipment test and change-after-confirmation test revealed more about the quality of the design than a successful report alone.

They showed whether Katie behaved safely when the conversation did not follow the expected path.

### 5. Evidence needs to match the claim

One of the strongest lessons from the project has been the importance of distinguishing:

- **Designed**
- **Implemented**
- **Tested**
- **Evidenced**

A feature being described in a specification does not mean it has been implemented, and an implemented feature does not automatically mean it has been tested.

## Current Evidence Position

The current MVP has demonstrated the core conversational fault-reporting behaviour and the confirmed submission path into SharePoint.

The evidence supports:

| Area | Current position |
|---|---|
| Natural-language fault reporting | **Tested** |
| Missing information | **Tested** |
| Multi-message collection | **Tested** |
| Invalid equipment/location combination | **Tested** |
| Unknown equipment | **Tested** |
| Configured reporting values | **Tested** |
| Complete report presentation | **Tested** |
| Explicit confirmation | **Tested** |
| Change after confirmation | **Tested** |
| Maintenance support | **Tested** |
| Agent Flow submission → SharePoint Fault creation | **Tested** |
| Full downstream maintenance processing | **Not yet claimed** |
| Duplicate protection | **Not yet claimed** |
| Complete end-to-end regression evidence | **Not yet claimed** |

This gives the project a clear evidence boundary while leaving a defined path for future refinement.
