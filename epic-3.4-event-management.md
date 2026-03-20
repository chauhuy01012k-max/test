# Epic 3.4: Event Management
**Parent Topic**: Topic 3: Site (Mini PC & Edge Server Management)
**User Objective**: To manage events, alarms, and alerts collected from physical cameras or the Mini PC itself, and configure automated responses based on time frames.
**Pain Point**: Raw security alarms are useless if they don't trigger immediate, automated physical and virtual responses to mitigate incidents without requiring human intervention.

---

## Feature: Automated Event Actions & Responses

**Why:** Without this feature, collected alarms are dead-end notifications — operators cannot see them or respond, and no automated physical response occurs, making the entire Event Management Epic functionally passive and useless in an active threat.
**Scope Type:** Create, Read, Update, Delete (CRUD)

### US-03.4.1: View and Manage Collected Events
**Priority:** Must Have
**Priority Reason:** Operators must clearly see the centralized list of alerts originating from the cameras or Mini PC hardware to maintain situational awareness.
**Story Points:** 3

**Story:**
As a Security Operator,
I want to view a centralized log of all events, alarms, and alerts collected from my connected cameras and Mini PC hardware,
So that I can monitor and acknowledge security incidents continuously.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Accessibility**
```gherkin
Given the operator accesses the Event Management feature
When the dashboard renders
Then the chronological event log and the buttons to configure automated action rules are fully exposed front-and-center within the main UI region
And the operator does not need to manipulate the parent navigation headers (Topic, Epic, or Feature nav) to access the underlying workflows
```

**Acceptance criteria scenarios: Happy Path - Viewing the Event Log**
```gherkin
Given the operator navigates to the "Event Management" dashboard
When the page loads
Then a chronological list of all collected alarms and alerts is displayed
And each entry explicitly details the source hardware (e.g., "Camera 1", "Mini PC CPU") and the trigger initialization timestamp
```

---

### US-03.4.2: Configure Automated Action Rules by Time Frame
**Priority:** Must Have
**Priority Reason:** Core Edge intelligence required to automate incident response without latency.
**Story Points:** 8

**Story:**
As a Site Administrator,
I want to create rules that link specific trigger events to automated actions based on active time frames,
So that the edge system acts automatically without requiring immediate human intervention from the central cloud.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Creating a Time-gated Action Rule**
```gherkin
Given the admin is creating a new Event Rule in the configuration UI
When they select a specific trigger event (e.g., "Motion Detected on Camera 1")
And they select the active time frame (e.g., "Every day between 22:00 and 06:00")
And they assign multiple execution actions (e.g., "Cut 30s video clip", "Play alarm tone through Mini PC speaker", "Move Camera 1 PTZ to Preset Position 1")
Then the system saves the rule
And the local edge Kerberos agent automatically begins listening for the trigger within the defined time range
```

**Acceptance criteria scenarios: Validation - Executing the Automated Actions**
```gherkin
Given an active event rule is fully configured
When the specified trigger event occurs physically during the active time frame
Then the Kerberos agent immediately executes all mapped actions simultaneously (Cutting video, Sounding Audio, PTZ manipulation)
And a definitive log of the execution success/failure is recorded in the Event Management dashboard
```

**Acceptance criteria scenarios: Edge Case - Trigger Occurs Outside Time Frame**
```gherkin
Given an active event rule is fully configured for a "Night Mode" time frame
When the specified trigger event occurs outside of the active time frame bounds
Then the kerberos agent ignores the trigger entirely
And no mapped actions are executed
```
