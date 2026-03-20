# Epic 4.3: Playback
**Parent Topic**: Topic 4: VMS (Video Management System)
**User Objective**: To enable operators to review historical recorded footage using a rich timeline interface, extract video clips and snapshots as evidence, and track the AI processing status of footage to accelerate incident investigation via the VSS Video Assistant.
**Pain Point**: Investigating incidents in recorded video requires manually scrubbing through hours of footage with no ability to quickly understand where motion occurred, extract evidence clips, or directly route footage to the AI engine for analysis — all of which compound investigation time dramatically.

---

## Feature: Recorded Video Playback & Timeline

**Why:** Without this feature, there is no way for operators to review historical footage — the entire value of recorded video data becomes inaccessible and the product cannot support any post-incident investigation workflow.
**Scope Type:** View / Control

### US-04.3.1: Review Recorded Video with Timeline Navigation
**Priority:** Must Have
**Priority Reason:** Core footage review capability — the foundational requirement of the entire Playback Epic.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to switch to recorded video playback mode and navigate a scrollable timeline to find the exact moment I need to review,
So that I can rapidly investigate historical incidents without scrubbing through raw footage manually.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Accessibility**
```gherkin
Given the operator is in the Playback view
When the page loads
Then the video player, timeline, and all playback controls are immediately visible in the main content region
And no navigation bar interaction is required to access any playback controls
```

**Acceptance criteria scenarios: Happy Path - Switching from Live to Playback Mode**
```gherkin
Given the operator is watching a live camera stream
When they click the "Live/Playback" toggle switch above the video window
Then the view switches to recorded footage mode
And the timeline appears below the video showing the available recording history
```

**Acceptance criteria scenarios: Happy Path - Timeline Navigation Controls**
```gherkin
Given the operator is in recorded video playback mode
When they interact with the timeline
Then they can:
  - Scale the timeline by scrolling the mouse wheel over the left portion
  - Scrub rapidly through recordings by scrolling the mouse wheel over the right portion (time-lapse effect)
  - Navigate frame-by-frame by clicking and dragging the timeline left or right
  - Jump to a specific date and time using the Calendar picker
  - Control playback speed between 0.125x and 16x
```

**Acceptance criteria scenarios: Happy Path - Motion Marks on Timeline**
```gherkin
Given the operator is reviewing the timeline in playback mode
When motion detection events exist in the current time range
Then the timeline highlights motion event markers visually
And hovering over a motion mark shows a snapshot preview at that moment in time
```

---

## Feature: Evidence Management (Extraction, Archive & Recovery)

**Why:** Without this feature, operators cannot capture, recover, or manage specific segments of video as evidence. All investigative value is lost if footage cannot be extracted, archived, and seamlessly transitioned into the AI search or summary workflows.
**Scope Type:** Create / View / Action

### US-04.3.2: Take a Snapshot
**Priority:** Must Have
**Priority Reason:** Snapshots are the fastest form of visual evidence capture required by security teams.
**Story Points:** 2

**Story:**
As a Security Operator,
I want to take a snapshot of the current video frame during playback or live view,
So that I can instantly capture a still image as evidence without downloading the full video clip.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Snapshot Capture**
```gherkin
Given the operator is viewing a camera in single camera mode (live or recorded)
When they click the "Snapshot" icon in the upper menu
Then the current video frame is captured and saved to the Archive
And a confirmation notification confirms "Snapshot saved to Archive"
```

---

### US-04.3.3: Extract a Video Clip
**Priority:** Must Have
**Priority Reason:** Clip extraction is the primary evidentiary output required by incident investigators.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to select a time range on the timeline and extract a video clip to the Archive,
So that I can preserve a specific incident segment for reporting, review, or further AI analysis.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Extracting a Clip**
```gherkin
Given the operator is in recorded video (Playback) mode
When they select a start and end time range on the timeline
And they click "Extract Clip"
Then the system packages that time range as a video file
And saves it to the Archive section for later access
```

**Acceptance criteria scenarios: Alternative Path - Create a Time-Lapse**
```gherkin
Given the operator is in recorded video mode
When they use the "Create Time-lapse" option from the upper menu
Then the system generates an accelerated composite video of the selected time range
And saves the time-lapse to the Archive
```

---

### US-04.3.4: Manage Extracted Clips in Archive
**Priority:** Must Have
**Priority Reason:** Essential for converting raw evidence clips into searchable AI context.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to view, filter, and select one or more extracted clips within the Archive folder and add them to the AI processing queue,
So that I can eventually interrogate these specific evidence files using the Video Assistant.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Batch Add to AI Processing**
```gherkin
Given the operator is viewing the "Archive" folder containing multiple extracted clips
When they select one or more video files from the list
And they click the "Add to AI Process" action button
Then the system submits all selected clips to the VSS AI analysis queue
And each selected item displays a "🟡 Processing" status indicator in the Archive list
```

**Acceptance criteria scenarios: Happy Path - Viewing AI Status in Archive List**
```gherkin
Given the operator is browsing the Archive list
When the list renders
Then each video clip displays its current AI status badge (🔴 Not Yet, 🟡 Processing, 🟢 Ready)
And clips marked as "🟢 Ready" provide a direct "Search with AI" button next to the filename
```

**Acceptance criteria scenarios: Happy Path - Open Archive Clip in Video Assistant**
```gherkin
Given a clip in the Archive has the "🟢 Ready" status
When the operator clicks "Search with AI" or the Video Assistant shortcut
Then they are navigated to the Video Assistant (Epic 5.1) with that specific clip loaded as the primary context
```

---

### US-04.3.5: Recover Missing Footage from Camera SD Card
**Priority:** Should Have
**Priority Reason:** Network outages cause gaps in cloud recording; SD card recovery closes footage continuity.
**Story Points:** 5

**Story:**
As a Site Administrator,
I want to recover footage recorded to the camera's local SD card during a cloud recording gap,
So that no incident footage during a network outage is permanently lost.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Initiating SD Card Backup**
```gherkin
Given the operator identifies a time interval on the timeline where no cloud recordings are available
When they click the SD card icon and set a start time, end time, and backup speed (max 8x)
And click "Start New Backup"
Then the system begins recovering video from the camera SD card in the background
And the operator can safely close the dialog — the backup continues
```

**Acceptance criteria scenarios: Happy Path - Recovery Complete**
```gherkin
Given an SD card backup has been initiated
When the recovery process completes
Then the timeline is updated to show the newly recovered recordings in the previously empty time gap
And the backup status reads "Done"
```

**Acceptance criteria scenarios: Performance - Bandwidth Warning**
```gherkin
Given the operator selects a recovery speed higher than 1x
When they configure the backup
Then the system displays an inline warning: "Higher backup speeds consume proportionally more bandwidth. 8x speed uses approximately 8x the bandwidth of a normal live stream."
```

---

## Feature: AI Processing Status & Shortcuts

**Why:** Without this feature, operators have no visibility into whether footage has been processed by the VSS AI Engine — they cannot know if they can immediately use the Video Assistant to search within a clip or if they need to wait, which creates a dead-end investigative workflow.
**Scope Type:** View / Action

### US-04.3.6: View AI Processing Status of Playback Footage
**Priority:** Must Have
**Priority Reason:** Essential bridge connecting recorded footage review to the VSS AI search workflow; without it, operators have no signal to drive AI-assisted investigation.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to see the AI processing status of the footage I am currently reviewing during playback—knowing that footage is stored in predefined chunks (15m/30m/1hr) configured in Epic 3.1—so that I can decide to wait, trigger analysis, or move to the Video Assistant.

> **AI Processing Logic:** Playback footage is analyzed on a per-file (chunk) basis. The status badge reflects the state of the specific file currently under the timeline playhead.

> **AI Processing Status Values:**
> - 🔴 **Not Yet** — Footage has not been submitted to the AI processing queue.
> - 🟡 **Processing** — Footage is currently being analyzed by the VSS AI engine (generating semantic embeddings).
> - 🟢 **Ready** — Footage has been fully analyzed and is searchable via the Video Assistant.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Viewing AI Status Indicator on Playback**
```gherkin
Given the operator is in playback mode reviewing a recorded video segment
When the playback view renders
Then a visible AI Status badge is displayed on the video player (e.g., "🔴 Not Yet", "🟡 Processing", or "🟢 Ready")
And the status reflects the actual current state of that footage segment in the AI engine queue
```

**Acceptance criteria scenarios: Happy Path - "Ready" Status: Shortcut to Video Assistant**
```gherkin
Given the operator is viewing footage with a "🟢 Ready" AI status
When they click the status badge or a nearby shortcut button "Open in Video Assistant"
Then they are navigated directly to the Video Assistant (Epic 5.1) with the relevant footage pre-loaded as the conversation context
```

**Acceptance criteria scenarios: Interactive Action - "Not Yet" Status: Quick Add to AI Queue**
```gherkin
Given the operator is viewing footage with a "🔴 Not Yet" AI status
When they click the status badge or the "Add to AI Queue" quick action button
Then the current footage segment is submitted to the VSS AI processing queue
And the badge immediately updates to "🟡 Processing"
And a notification confirms "Footage submitted to AI processing queue"
```

**Acceptance criteria scenarios: Edge Case - "Processing" Status: No Action Available**
```gherkin
Given the operator is viewing footage with a "🟡 Processing" AI status
When they hover over the badge
Then a tooltip informs: "AI analysis in progress. This footage will be searchable in Video Assistant when complete."
And neither the "Open in Video Assistant" nor the "Add to AI Queue" actions are shown
```

