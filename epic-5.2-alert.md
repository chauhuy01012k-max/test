# Epic 5.2: Alert
**Parent Topic**: Topic 5: VSS (Video Search and Summary Streams)
**User Objective**: To give security operators a centralized notification hub that surfaces AI-generated alert messages and automation task execution reports without requiring them to continuously watch camera streams.
**Pain Point**: Users of surveillance systems miss critical anomalies because they cannot monitor all camera feeds simultaneously. Without a centralized notification view driven by natural-language AI scenarios, operators only discover incidents reactively — long after they occur.

---

## Feature: Notification

**Why:** Without this feature, all AI-generated alerts and automation task executions are invisible to the operator — the entire AI detection and automation pipeline produces results with no observable output, making it functionally worthless.
**Scope Type:** Read / Update

### US-05.2.1: View the Notification Dashboard (Default Mode)
**Priority:** Must Have
**Priority Reason:** The dashboard summary view is the entry point to all alert and task monitoring — without it, operators have no overview of the system's alerting health.
**Story Points:** 3

**Story:**
As a Security Operator,
I want to see a consolidated notification dashboard showing alert message counts and automation task counts at a glance,
So that I can immediately understand the state of my surveillance AI pipeline without drilling into individual lists.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Accessibility**
```gherkin
Given the operator navigates to the "Alert" section
When the page loads
Then the notification dashboard is displayed prominently in the main content region
And the Alert summary counters (Total / Active / Inactive) and Task summary counters (Total / Active / Inactive) are both immediately visible
```

**Acceptance criteria scenarios: Happy Path - Alert Summary Counters with Shortcut**
```gherkin
Given the operator is viewing the notification dashboard in default mode
When the page renders
Then they see the total number of configured Alerts broken down as Total, Active, and Inactive counts
And clicking any of these count numbers navigates the operator directly to the Alert Setting Page
```

**Acceptance criteria scenarios: Happy Path - Automation Task Summary Counters with Shortcut**
```gherkin
Given the operator is viewing the notification dashboard in default mode
When the page renders
Then they see the total number of configured Automation Tasks broken down as Total, Active, and Inactive counts
And clicking any of these count numbers navigates the operator directly to the Automation Page
```

---

### US-05.2.2: View and Manage Alert Message List
**Priority:** Must Have
**Priority Reason:** Without a detailed, filterable view of AI-generated alerts, operators cannot triage, prioritize, or acknowledge incidents.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to expand the alert message list and control how alerts are displayed, filtered, and marked,
So that I can efficiently triage all AI-generated anomaly notifications from configured alerts.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Expanding the Alert List**
```gherkin
Given the operator is on the notification dashboard in default mode
When they select to view the alert list in detail
Then the system transitions to the expanded alert message list view
And the full table of alert messages generated from all configured Alerts is displayed
```

**Acceptance criteria scenarios: Interactive Validation - Configurable Auto-Refresh**
```gherkin
Given the operator is viewing the expanded alert list
When they change the refresh frequency via the dropdown (options: 5s / 15s / 30s)
Then the system updates the auto-reload interval of the list to the selected frequency
And new alerts that arrive within that interval are automatically populated without a manual page reload
```

**Acceptance criteria scenarios: Interactive Validation - Time Range Filter**
```gherkin
Given the operator is viewing the expanded alert list
When they change the display time range via the dropdown (options: 1 week / 1 month / 3 months)
Then the system filters the alert message list to only show entries within the selected time range
```

**Acceptance criteria scenarios: Interactive Validation - Pagination Control**
```gherkin
Given the operator is viewing the expanded alert list
When they change the number of items per page via the dropdown (options: 10 / 20 / 50)
Then the system updates the table to display the selected number of rows per page
And the pagination controls update accordingly
```

**Acceptance criteria scenarios: Happy Path - Search by Time and Keyword**
```gherkin
Given the operator is viewing the expanded alert list
When they enter a time range and/or keyword in the search field
Then the system filters the alert list to display only entries matching the search criteria
And the result count updates to reflect the filtered set
```

**Acceptance criteria scenarios: Happy Path - Mark Alerts**
```gherkin
Given the operator is viewing the expanded alert list
When they select individual or all alerts and apply a status action (Mark as Read / Mark as Important / Mark as Unread)
Then the system updates the status of all selected alerts immediately
And the table reflects the updated status indicators for each affected row
```

---

### US-05.2.3: View and Manage Automation Task Execution Reports
**Priority:** Must Have
**Priority Reason:** Without visibility into task execution reports, operators cannot verify whether automated responses (e.g., PTZ triggers, video clips, speaker alarms) actually fired correctly when an alert was detected.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to expand the automation task execution report list and control how reports are displayed, filtered, and marked,
So that I can audit the execution history of all automated responses triggered by AI alert conditions.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Expanding the Task Execution Report List**
```gherkin
Given the operator is on the notification dashboard in default mode
When they select to view the task execution report list
Then the system transitions to the expanded task execution report view
And the full table of automation task execution reports is displayed
```

**Acceptance criteria scenarios: Interactive Validation - Configurable Auto-Refresh**
```gherkin
Given the operator is viewing the expanded task execution list
When they change the refresh frequency via the dropdown (options: 5s / 15s / 30s)
Then the system updates the auto-reload interval for the task list to the selected frequency
```

**Acceptance criteria scenarios: Interactive Validation - Time Range Filter**
```gherkin
Given the operator is viewing the expanded task execution list
When they change the display time range (options: 1 week / 1 month / 3 months)
Then the system filters and updates the task execution list to reflect the selected range
```

**Acceptance criteria scenarios: Interactive Validation - Pagination Control**
```gherkin
Given the operator is viewing the expanded task execution list
When they change the number of tasks per page (options: 10 / 20 / 50)
Then the system updates the table pagination to display the selected number of rows per page
```

**Acceptance criteria scenarios: Happy Path - Search by Time and Keyword**
```gherkin
Given the operator is viewing the expanded task execution list
When they enter a time range and/or keyword in the search field
Then the system displays only the task execution reports matching the search criteria
```

**Acceptance criteria scenarios: Happy Path - Mark Task Reports**
```gherkin
Given the operator is viewing the expanded task execution list
When they select individual or all reports and apply a status action (Mark as Read / Mark as Important / Mark as Unread)
Then the system updates the status of all selected task reports immediately
And the table reflects the updated status indicators for each affected row
```

---

## Feature: Alert Setting

**Why:** Without this feature, the Notification dashboard has no data to surface — alerts cannot be generated if no alert configurations exist. This is the configuration backbone of the entire alerting pipeline.
**Scope Type:** Create / Read / Update / Delete (CRUD)

### US-05.2.4: View and Manage Alert Configurations
**Priority:** Must Have
**Priority Reason:** Alert configurations are the prerequisite for any AI-generated notification — without them, the Notification feature has nothing to display.
**Story Points:** 5

**Story:**
As a Site Administrator,
I want to view, create, edit, activate/deactivate, and delete alert configurations,
So that I can define which AI-detected anomaly scenarios should generate alert messages for my surveillance network.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Alert Setting Page**
```gherkin
Given the operator navigates to the Alert Setting Page (via the shortcut or direct navigation)
When the page loads
Then a table listing all configured alerts is displayed with columns: Name, Status (Active/Inactive), Cameras in scope, and Last Triggered timestamp
And a "Create Alert" button is clearly visible to add a new configuration
```

**Acceptance criteria scenarios: Happy Path - Create a New Alert**
```gherkin
Given the admin clicks "Create Alert"
When the creation form opens
Then they can configure:
  - Alert name
  - Natural-language scenario description (e.g., "A person enters the restricted zone after 10 PM")
  - Cameras / camera groups to monitor
  - Active time frame (always on / scheduled hours)
  - Alert severity level (Low / Medium / High / Critical)
  - Notification recipients (user list or role group)
And saving the form creates the alert in Active status
```

**Acceptance criteria scenarios: Happy Path - Edit an Existing Alert**
```gherkin
Given the admin selects an existing alert from the table
When they click "Edit"
Then all current configuration fields are loaded into the edit form
And the admin can modify any field and save the updated configuration
```

**Acceptance criteria scenarios: Happy Path - Toggle Alert Active / Inactive**
```gherkin
Given the admin is viewing the alert configuration table
When they toggle the Active/Inactive status switch for a specific alert
Then the system immediately updates the alert's operational status
And the Notification dashboard counters (Active / Inactive) reflect the change
```

**Acceptance criteria scenarios: Error Handling - Delete Alert with Active History**
```gherkin
Given the admin attempts to delete an alert that has previously generated alert messages
When they click "Delete" and confirm
Then the system removes the alert configuration
And deletes no historical alert messages — those remain accessible in the Notification history
```

---

## Feature: Automation Task Management

**Why:** Without this feature, the Automation Task counters in the Notification dashboard are static placeholders — operators cannot create, modify, or control the automated responses that should fire when AI alerts are triggered, making the entire automation pipeline non-configurable.
**Scope Type:** Create / Read / Update / Delete (CRUD)

### US-05.2.5: View and Manage Automation Task Configurations
**Priority:** Must Have
**Priority Reason:** Automation tasks define the automated physical and virtual responses to AI alerts — without them, alerts are passive notifications with no actionable consequence.
**Story Points:** 8

**Story:**
As a Site Administrator,
I want to view, create, edit, activate/deactivate, and delete automation task configurations that link alert triggers to automated responses,
So that the system can respond to security incidents immediately and autonomously without requiring human intervention.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Automation Page**
```gherkin
Given the operator navigates to the Automation Page (via the shortcut or direct navigation)
When the page loads
Then a table listing all configured automation tasks is displayed with columns: Task Name, Status (Active/Inactive), Linked Alert, Actions configured, and Last Executed timestamp
And a "Create Task" button is clearly visible
```

**Acceptance criteria scenarios: Happy Path - Create a New Automation Task**
```gherkin
Given the admin clicks "Create Task"
When the creation form opens
Then they can configure:
  - Task name
  - Trigger: linked Alert configuration (select from active Alert list)
  - Active time frame for the task (always on / scheduled hours)
  - One or more automated actions to execute on trigger:
    - Cut a video clip (configurable duration before/after event)
    - Play alarm tone through the Mini PC speaker
    - Move a camera PTZ to a saved preset position
    - Send an in-app notification or email to specified recipients
And saving the form creates the task in Active status
```

**Acceptance criteria scenarios: Happy Path - Edit an Existing Automation Task**
```gherkin
Given the admin selects an automation task from the table
When they click "Edit"
Then all current task parameters are loaded into the edit form
And the admin can modify the trigger, time frame, or action list and save the updated task
```

**Acceptance criteria scenarios: Happy Path - Toggle Task Active / Inactive**
```gherkin
Given the admin is viewing the automation task table
When they toggle the Active/Inactive status switch for a specific task
Then the system immediately enables or disables the task's trigger listener
And the Notification dashboard counters (Active / Inactive) for Automation Tasks reflect the change
```

**Acceptance criteria scenarios: Validation - Executing a Task on Alert Trigger**
```gherkin
Given an active automation task is configured with a valid trigger alert and action list
When the linked alert condition is detected by the VSS AI engine during the task's active time frame
Then all configured actions execute simultaneously (clip cut, speaker alarm, PTZ move, notification)
And a task execution report entry is created in the Notification Automation Task report list with execution status (Success / Partial / Failed)
```

**Acceptance criteria scenarios: Edge Case - Trigger Outside Active Time Frame**
```gherkin
Given an automation task is configured with a specific active time frame (e.g., 22:00–06:00)
When the linked alert is triggered outside that time frame
Then the task does not execute
And no execution report entry is generated for this event
```

