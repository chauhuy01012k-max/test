# Epic 5.1: Video Assistant
**Parent Topic**: Topic 5: VSS (Video Search and Summary Streams)
**User Objective**: To enable security operators and investigators to manage analyzed video assets in a structured catalog, conduct natural language conversations with the VSS AI Agent over selected footage, and configure advanced AI analysis behaviors to suit specific investigation use cases.
**Pain Point**: Traditional VMS tools require investigators to manually scrub hours of footage with no AI assistance. Without a centralized video catalog, a conversational AI interface, and configurable analytics pipelines, operators spend hours on investigations that should take minutes — and critical context relationships between objects and events in video are never surfaced.

---

## Feature: Video Catalog Management

**Why:** Without this feature, users have no unified view of what has been analyzed, what is in progress, and what is waiting — making the entire AI pipeline invisible and unmanageable. Operators cannot route footage to the Video Assistant if they don't know its processing status.
**Scope Type:** Read / Update / Action

### US-05.1.1: View Video Catalog by AI Processing Status
**Priority:** Must Have
**Priority Reason:** Operators must have a complete, classified view of all AI-processed and unprocessed video assets before any conversation or investigation can begin.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to view all videos stored in the system in a thumbnail grid classified by their AI processing status,
So that I can quickly understand which footage is ready for AI-assisted investigation and which needs to be queued or monitored.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Accessibility**
```gherkin
Given the operator navigates to the "Video Assistant" section
When the page loads
Then the Video Catalog thumbnail grid is displayed prominently in the main content region
And the AI status filter tabs (All / Analyzed / Processing / Not Yet Analyzed) are visible without any additional navigation
```

**Acceptance criteria scenarios: Happy Path - Catalog by Classification**
```gherkin
Given the operator is viewing the Video Catalog
When the catalog renders
Then all videos are displayed as thumbnail cards organized into classification groups: "Analyzed", "Processing", and "Not Yet Analyzed"
And each thumbnail card shows a visible AI status badge (🟢 Analyzed / 🟡 Processing / 🔴 Not Yet)
```

**Acceptance criteria scenarios: Happy Path - Hover Tooltip per Status**
```gherkin
Given the operator hovers the mouse pointer over a video thumbnail card
When the hover state activates
Then a tooltip popover appears displaying status-specific information:
  - 🟢 "Analyzed": Shows the AI-generated summary content of the video
  - 🟡 "Processing": Shows the estimated remaining processing time and any available initial/partial summary
  - 🔴 "Not Yet Analyzed": Shows the source (camera/archive/clip), recording duration, and date
```

---

### US-05.1.2: View Analyzed Video in Detail
**Priority:** Must Have
**Priority Reason:** The detailed view is where analysts consume AI output — without it, the catalog is a status list with no actionable investigative value.
**Story Points:** 8

**Story:**
As a Security Investigator,
I want to open an analyzed video to review its AI-generated transcript, edit annotations, and access any ongoing or historical conversations about this video,
So that I can deeply interrogate a specific piece of evidence using all available AI context.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Opening the Detail View**
```gherkin
Given the operator selects a video card with "🟢 Analyzed" status from the catalog
When the detail panel opens
Then it displays:
  - A video player with synchronized AI transcript highlights (transcript text highlights as the corresponding video moment plays)
  - The full AI-generated transcript panel beside/below the player
  - An "Ongoing Conversations" section showing all existing dialogue sessions linked to this video
  - A "Start New Conversation" button to open a fresh dialogue context with this video
```

**Acceptance criteria scenarios: Happy Path - Transcript Edit & Version Control**
```gherkin
Given the analyst is reviewing the AI-generated transcript in the detail view
When they click on a transcript segment to edit
Then the segment becomes an editable text field
And saving the edit creates a new version entry in the transcript version history
And the analyst can view all previous versions and roll back to any prior version at any time
```

**Acceptance criteria scenarios: Alternative Path - Re-open an Existing Conversation**
```gherkin
Given the detail view shows a list of "Ongoing Conversations" linked to this video
When the analyst selects an existing conversation entry
Then the conversation thread opens, displaying all prior messages, AI responses, and context references
And the analyst can continue the conversation from where it was left off
```

---

### US-05.1.3: Manage Processing Videos (Pause / Cancel)
**Priority:** Must Have
**Priority Reason:** Operators must be able to control the AI processing queue to prioritize urgent footage or free up processing resources.
**Story Points:** 3

**Story:**
As a Security Operator,
I want to select processing videos and pause or cancel their analysis,
So that I can manage the AI processing queue and prioritize critical footage over less urgent content.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Pause AI Processing**
```gherkin
Given the operator selects one or more video cards with "🟡 Processing" status
When they click the "Pause" action from the selection toolbar
Then the AI analysis for those videos is suspended
And the video cards update to show a "⏸️ Paused" status with the percentage completed at time of pause
```

**Acceptance criteria scenarios: Happy Path - Cancel AI Processing**
```gherkin
Given the operator selects one or more "🟡 Processing" or "⏸️ Paused" video cards
When they click "Cancel Analysis"
Then the system terminates the analysis job
And the video reverts to "🔴 Not Yet Analyzed" status in the catalog
```

---

### US-05.1.4: Queue Unanalyzed Videos for AI Processing
**Priority:** Must Have
**Priority Reason:** Without this, there is no user-initiated path to submit footage into the AI engine — the catalog would be read-only with no mechanism to grow the analyzed asset pool.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to select unanalyzed videos and choose an analysis type to submit them to the VSS AI processing queue,
So that I can build the pool of searchable AI-analyzed footage available in the Video Assistant.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Default Analysis Submission**
```gherkin
Given the operator selects one or more "🔴 Not Yet Analyzed" video cards
When they click "Analyze" and choose the "Default" analysis type
Then the system enqueues all selected videos using the default analysis pipeline
And each card immediately updates to "🟡 Processing" status
```

**Acceptance criteria scenarios: Happy Path - Advanced Analysis Submission**
```gherkin
Given the operator selects unanalyzed video cards and clicks "Analyze"
When they open the "Advanced" analysis type dropdown
Then a list of previously saved advanced analysis scripts/configurations is shown
And selecting one and confirming enqueues the videos with that specific analysis pipeline
```

---

## Feature: Conversations with VSS Agent

**Why:** Without this feature, all AI analysis remains static metadata — operators cannot interact with, interrogate, or extract investigative intelligence from the analyzed footage in real time, making the VSS AI engine a passive pipeline with no interactive value.
**Scope Type:** Create / Read / Action

### US-05.1.5: Start a Conversation with Selected Videos
**Priority:** Must Have
**Priority Reason:** Core interactive use case of the entire Video Assistant feature — the primary way operators query the AI.
**Story Points:** 8

**Story:**
As a Security Investigator,
I want to select one or more analyzed videos, clips, or live camera feeds and start a natural language conversation with the VSS Agent about their content,
So that I can interactively interrogate the footage without manually reviewing hours of recordings.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Chat Interface**
```gherkin
Given the operator opens the "Conversations" interface in the Video Assistant
When the page renders
Then it presents a chatbot-style conversation panel with a message input field at the bottom
And a side panel showing the currently selected video/clip/camera context artifacts
And the conversation history is displayed in a scrollable thread above the input
```

**Acceptance criteria scenarios: Happy Path - Selecting Context and Starting a Conversation**
```gherkin
Given the operator selects one or more "🟢 Analyzed" videos, clips, or camera feeds from the catalog or sidebar
When they open a new conversation
Then the selected assets are shown as context tags at the top of the conversation
And the operator can type a natural language question and receive an AI-generated response grounded in the selected footage
```

**Acceptance criteria scenarios: Alternative Path - Start Conversation First, Get Suggestions**
```gherkin
Given the operator opens a new conversation without pre-selecting any videos
When they type their question or describe what they want to investigate
Then the VSS Agent analyzes the query intent
And suggests relevant videos, clips, or camera feeds that are already "🟢 Analyzed" and match the investigation context
And the operator can accept one or more suggestions to add them to the conversation as context
```

---

### US-05.1.6: Smart Context Expansion During Conversation
**Priority:** Must Have
**Priority Reason:** Without this, conversations hit dead ends when questions exceed the scope of currently loaded context — the operator has no guided path to expand the investigation.
**Story Points:** 8

**Story:**
As a Security Investigator,
I want the VSS Agent to proactively suggest adding new footage or triggering analysis when my questions go beyond the current conversation context,
So that I can seamlessly expand the investigation scope without manually searching for related assets.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Suggesting Analyzed Content Outside Current Context**
```gherkin
Given the operator asks a question that references events or objects not covered by the currently loaded context videos
When the VSS Agent determines the question requires footage outside the current context
Then it replies with a suggestion: "This question may be relevant to [Video Name / Camera / Clip]. Would you like to add it to our conversation?"
And the operator can accept the suggestion to expand the conversation context
```

**Acceptance criteria scenarios: Alternative Path - Suggesting Analysis for Unprocessed Footage**
```gherkin
Given the operator's question references footage that exists in the system but is "🔴 Not Yet Analyzed"
When the VSS Agent identifies the relevant unanalyzed asset
Then it replies: "I found [Video Name] which may answer your question, but it hasn't been analyzed yet. Would you like to submit it for analysis?"
And the operator can confirm to immediately queue the video for AI processing
```

**Acceptance criteria scenarios: Alternative Path - Suggesting Re-analysis with a Different Script**
```gherkin
Given the operator's question requires a different type of AI analysis than what was originally performed on the video in context (e.g., object tracking instead of motion detection)
When the VSS Agent detects the mismatch
And a saved analysis script matching the required type is available
Then it replies: "This question requires a different analysis approach. Would you like to re-analyze [Video Name] using the '[Script Name]' analysis script?"
And the operator can confirm to re-queue the video with the new script
```

**Acceptance criteria scenarios: Alternative Path - Suggesting Creation of New Analysis Script**
```gherkin
Given the operator's question requires an analysis type that no existing saved script can satisfy
When the VSS Agent identifies no matching analysis script in the library
Then it replies: "Answering this question requires a custom analysis configuration. Would you like to create a new analysis scenario to process this footage appropriately?"
And the operator can be guided to the Advanced Analysis feature to create a new script
```

---

### US-05.1.7: Query and Review Previous Conversations
**Priority:** Should Have
**Priority Reason:** Investigators often revisit prior conversation threads when new evidence surfaces; without this, all historical AI dialogue is lost.
**Story Points:** 3

**Story:**
As a Security Investigator,
I want to search and browse my previous VSS Agent conversations,
So that I can revisit prior conclusions, continue interrupted investigations, and avoid duplicating conversational queries.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Browsing Conversation History**
```gherkin
Given the operator opens the "Conversations" section
When they access the conversation history sidebar or tab
Then all prior conversations are listed in reverse-chronological order
And each entry displays: the conversation title/date, the context videos used, and a preview of the first exchange
```

**Acceptance criteria scenarios: Happy Path - Resuming a Previous Conversation**
```gherkin
Given the operator selects a prior conversation from the history list
When the conversation thread opens
Then the full message history is restored
And the conversation's original video/clip/camera context is reloaded
And the operator can continue sending new messages seamlessly
```

---

## Feature: Advanced Analysis Configuration

**Why:** Without this feature, all video analysis uses a single default pipeline — operators investigating specialized scenarios (e.g., vehicle tracking, facial pattern detection, object relationship mapping) cannot customize the AI behavior, severely limiting the investigative depth of the VSS platform.
**Scope Type:** Create / Read / Update / Delete

### US-05.1.8: Create and Manage Analysis Scripts
**Priority:** Must Have
**Priority Reason:** The entire smart suggestion flow in conversations depends on a library of saved analysis scripts — without them, re-analysis suggestions cannot be offered.
**Story Points:** 8

**Story:**
As a Security Investigator,
I want to create and save custom analysis configurations that define how the VSS AI engine processes video content and links detected objects to each other,
So that I can tailor AI behavior to specific investigation use cases and reuse configurations across multiple videos.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Creating a New Analysis Script**
```gherkin
Given the analyst opens the "Advanced Analysis" configuration section
When they click "Create New Script"
Then they are presented with a configuration form that allows:
  - Selecting the AI detection behaviors to enable (e.g., motion detection, object classification, person re-identification, vehicle tracking, anomaly detection)
  - Defining object relationship rules (e.g., "Link Person ID across cameras", "Associate Vehicle to Entry Gate events")
  - Setting the analysis sensitivity and confidence thresholds
  - Naming and saving the script to the library
```

**Acceptance criteria scenarios: Happy Path - Reusing a Saved Script**
```gherkin
Given the analyst is queuing videos for analysis in the catalog or through a conversation suggestion
When they open the analysis type dropdown
Then all previously saved advanced scripts appear as selectable options
And selecting a script applies its full configuration to the queued footage
```

**Acceptance criteria scenarios: Happy Path - Edit and Version a Script**
```gherkin
Given the analyst opens an existing script from the library
When they modify a configuration parameter and save
Then the system creates a new version of the script
And prior versions remain accessible and can be applied independently
```
