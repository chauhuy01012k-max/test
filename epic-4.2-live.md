# Epic 4.2: Live
**Parent Topic**: Topic 4: VMS (Video Management System)
**User Objective**: To enable security operators to monitor multiple live camera streams simultaneously on one screen using flexible multi-view layouts that can be saved and shared across the team.
**Pain Point**: Monitoring multiple cameras in separate tabs or sequential individual views creates dangerous blind spots and operator fatigue. Without a configurable multi-camera grid with saveable layouts, operators waste time reconstructing the same surveillance arrangement every session.

---

## Feature: Multi-Camera Live View (Layout Management)

**Why:** Without this feature, operators must monitor cameras one at a time — a single operator cannot maintain situational awareness across multiple cameras or sites simultaneously, making multi-site surveillance operationally infeasible.
**Scope Type:** View / CRUD

### US-04.2.1: Build a Multi-Camera Surveillance Layout
**Priority:** Must Have
**Priority Reason:** Core surveillance use case. A VMS without multi-view monitoring is functionally incomplete.
**Story Points:** 8

**Story:**
As a Security Operator,
I want to drag and drop cameras or entire sites into a surveillance canvas and select a grid layout,
So that I can monitor multiple camera feeds simultaneously on one screen.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Accessibility**
```gherkin
Given the operator navigates to the "Live" section
When the page loads
Then the Sites/Cameras source panel and the surveillance canvas are both immediately visible in the main UI region
And the operator does not need to navigate through any sub-menus to start adding cameras
```

**Acceptance criteria scenarios: Happy Path - Adding Cameras via Drag and Drop**
```gherkin
Given the operator is on the Live monitoring page
When they open the Sites panel and drag an individual camera or an entire site's camera group into the surveillance canvas
Then the camera feed(s) appear as live video tiles in the canvas
```

**Acceptance criteria scenarios: Happy Path - Selecting a Grid Layout**
```gherkin
Given the operator has added cameras to the surveillance canvas
When they click the "Layout" button and choose a grid option (e.g., 1x1, 2x2, 3x3, 1+5, 2+8)
Then the canvas reorganizes all camera tiles into the selected grid configuration
And the operator can drag each camera tile to reorder its position within the grid
```

**Acceptance criteria scenarios: Alternative Path - Single Camera Mode**
```gherkin
Given the operator is viewing a multi-camera layout
When they double-click on a specific camera tile
Then the view expands that camera to single camera mode with full live view controls visible
And a "<" back button is displayed to return to the multi-camera layout
```

---

### US-04.2.2: Save a Surveillance Layout
**Priority:** Must Have
**Priority Reason:** Without saved layouts, operators must rebuild their multi-view arrangement every session — a critical time loss in an emergency.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to save my configured multi-camera layout with a name and sharing settings,
So that I can reload my surveillance arrangement instantly in future sessions.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Saving a Layout**
```gherkin
Given the operator has configured a multi-camera surveillance arrangement on the canvas
When they click "Create Layout" and provide a layout name
Then the layout (camera selection, grid configuration, and tile positions) is saved to the "Layouts" library
```

**Acceptance criteria scenarios: Happy Path - Auditor Shareable Layout (Single Site)**
```gherkin
Given the operator saves a layout using cameras from only one site
When the save dialog opens
Then the "Auditor Shareable Layout" checkbox is automatically enabled
And if left checked, any Auditor-role user with access to that site can view this layout in the Sites Tree tab
```

**Acceptance criteria scenarios: Happy Path - Private vs. Public Layout (Multi-site)**
```gherkin
Given the operator saves a layout using cameras from multiple sites or unchecks "Auditor Shareable"
When the save dialog presents visibility options
Then the operator can choose "Private" (visible only to themselves) or "Public" (visible to all Operator-role users in the company)
```

---

### US-04.2.3: Load and Manage Saved Layouts
**Priority:** Must Have
**Priority Reason:** Layouts are only valuable if they can be retrieved, renamed, and deleted efficiently.
**Story Points:** 3

**Story:**
As a Security Operator,
I want to browse, load, rename, and delete saved surveillance layouts,
So that my layout library stays organized and I can switch between different monitoring configurations instantly.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Loading a Saved Layout**
```gherkin
Given the operator opens the "Layouts" section in the Live view
When they select a previously saved layout name
Then the surveillance canvas instantly restores all camera tiles and grid positions exactly as saved
```

**Acceptance criteria scenarios: Edge Case - Camera No Longer Available**
```gherkin
Given a saved layout includes a camera that has been deleted or deregistered
When the operator loads that layout
Then the missing camera tile displays a placeholder with a clear "Camera Unavailable" message
And all other cameras in the layout continue streaming normally
```

---

## Feature: Single Camera Live Controls

**Why:** Without this feature, operators double-clicking a camera in the multi-view grid are dropped into a full-screen view with no controls — they cannot zoom, adjust audio, or use PTZ, making the single camera mode non-functional for active incident investigation.
**Scope Type:** View / Control

### US-04.2.4: Control a Single Camera in Live Mode
**Priority:** Must Have
**Priority Reason:** Active incident investigation requires PTZ control, audio, and zoom from a single camera view.
**Story Points:** 5

**Story:**
As a Security Operator,
I want access to full live view controls when I enter single camera mode from the multi-view grid,
So that I can actively investigate an incident using all available hardware capabilities of that camera.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Available Live View Controls**
```gherkin
Given the operator is in single camera mode for a selected camera
When the live view loads
Then the control panel exposes all controls the camera hardware supports:
  - PTZ (Pan-Tilt-Zoom joystick with speed control, if camera supports it)
  - Microphone (backward/two-way audio, if supported)
  - Streaming format selector (HLS/DASH, WebRTC, JPEG polling)
  - Digital Zoom
  - Dewarping toggle (for fisheye lens cameras)
  - Audio Volume Control
```

**Acceptance criteria scenarios: Interactive Validation - PTZ Control**
```gherkin
Given the camera supports Pan-Tilt-Zoom
When the operator uses the PTZ joystick controls in the live view
Then the physical camera begins moving in the corresponding direction
And the live stream updates in real time to reflect the camera's new position
```

**Acceptance criteria scenarios: Edge Case - Unsupported Controls Hidden**
```gherkin
Given the selected camera does not support PTZ or two-way audio
When the operator enters single camera mode
Then the PTZ joystick and Microphone controls are visually disabled or hidden
And a tooltip on hover reads "This camera does not support [feature]"
```
