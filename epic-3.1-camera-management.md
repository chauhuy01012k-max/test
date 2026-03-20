# Epic 3.1: Camera Management
**Parent Topic**: Topic 3: Site (Mini PC & Edge Server Management)
**User Objective**: To provision and manage Kerberos Agent pods for individual physical cameras connected to the Mini PC edge server.
**Pain Point**: Traditional Rstp gw model cannot transmit events from camera to VMS or perform tasks for AI. The per-camera microservice (Kerberos Agent) ensures isolated reliability, advanced PTZ control, and flexible edge-to-cloud storage tiers without bottlenecking the Mini PC.

---

## Feature: Add, edit, remove, and monitor status of local cameras attached to the site

**Why:** Without this feature, no camera data feeds into the VSS AI Agent — all other features in this Epic become non-functional as they depend on at least one registered and active camera stream.
**Scope Type:** Create, Read, Update, Delete (CRUD) / Infrastructure

### US-03.1.1: Register New IP Camera to Site
**Priority:** Must Have
**Priority Reason:** Essential critical path; the agent cannot perform video analytics without active camera streams.
**Story Points:** 5

**Story:**
As a Site Administrator,
I want to add a new local IP camera to the site's edge gateway (Mini PC),
So that its video stream can be ingested and analyzed by the VSS AI Agent.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Registering ONVIF Camera via Network Scan**
```gherkin
Given the admin is viewing the "Add Camera" dashboard for a specific site
When they initiate an automated local network scan via the Mini PC
Then the system displays a list of discovered ONVIF-compliant IP cameras
And selecting a camera and providing the correct device credentials successfully registers it to the site
```

**Acceptance criteria scenarios: Alternative Path - Manual RTSP URL Entry**
```gherkin
Given the admin is on the "Add Camera" modal
When they select the "Manual Entry" option
And they input a valid RTSP stream URL and authentication credentials
Then the system verifies the connection and saves the camera to the site registry
```

**Acceptance criteria scenarios: Error Handling - Authentication Failure during Registration**
```gherkin
Given the admin is attempting to register a camera manually or via scan
When they provide incorrect device username or password credentials
Then the system prevents the registration from completing
And displays an inline error message stating "Connection failed: Invalid camera credentials or camera offline."
```

---

## Feature: View Camera Stream & Hardware Controls

**Why:** Without this feature, operators have no way to verify a camera is working or to actively investigate incidents. A camera that cannot be viewed is operationally useless regardless of how well it is registered or configured.
**Scope Type:** View / Control

### US-03.1.2: Advanced Live Stream Control
**Priority:** Must Have
**Priority Reason:** Core surveillance operational requirement for active monitoring.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to view the live video stream alongside display configurations and PTZ controls,
So that I can digitally adjust the image or physically move supported cameras to investigate incidents.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Live View with Display Configs**
```gherkin
Given the operator opens a specific camera's live view
When the stream successfully connects from the kerberos agent
Then the live video is displayed
And the operator can apply dynamic digital display configurations such as "Inverse (flip)" or "Digital Zoom" instantly
```

**Acceptance criteria scenarios: Interactive Validation - PTZ and Audio Controls**
```gherkin
Given the camera hardware natively supports Pan-Tilt-Zoom (PTZ) and two-way audio
When the operator interacts with the PTZ joystick elements or the "Mic/Speaker" function keys
Then the underlying kerberos agent translates and sends the corresponding hardware commands to the physical camera
And the video/audio stream updates to reflect the physical movement or broadcast
```

---

## Feature: ONVIF-based Camera Configuration

**Why:** Without this feature, cameras default to factory settings (often wrong resolution or framerate), wasting bandwidth and producing footage of quality insufficient for AI-based analysis or legal evidence.
**Scope Type:** Update

### US-03.1.3: Configure Camera Parameters via ONVIF Profile
**Priority:** Must Have
**Priority Reason:** Proper camera configuration is required before reliable streams can be ingested by the VSS AI Agent.
**Story Points:** 8

**Story:**
As a Site Administrator,
I want to select a camera, see its detected ONVIF profile, and configure all parameters supported by that profile,
So that each camera is optimized for bandwidth, image quality, and AI analysis before it is actively monitored.

> **ONVIF Profile Reference:**
> - **Profile S** (Live Streaming): Codec (H.264/MJPEG), Resolution, Bitrate, Framerate, GOP interval, Quality, Network settings.
> - **Profile T** (Advanced Streaming): Adds H.265/HEVC codec, Motion Detection Zones, Bidirectional Audio, OSD (Onscreen Display), HTTPS Streaming, Metadata configuration.
> - **Profile G** (Edge Storage): Recording modes (Continuous, Scheduled, Event-based), Playback configuration.
> - **Imaging Service** (All profiles): Brightness, Contrast, Sharpness, Color Saturation, White Balance, Exposure (Auto/Manual), Focus (Auto/Manual), IR Cut Filter, WDR, BLC, Noise Reduction, Image Stabilization, Defogging, Tone Compensation.
> - **PTZ Service** (Profile S/T): Absolute/Relative/Continuous Move, Speed control, Presets, Home position, PTZ Limits, E-Flip, Preset Tours.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Display Camera's Detected ONVIF Profile**
```gherkin
Given the admin selects a camera from the site camera list
When they open the "Configuration" panel
Then the system queries and displays the camera's detected ONVIF profile(s) (e.g., "Profile S + Imaging Service")
And only the configurable parameter groups supported by that profile are shown
```

**Acceptance criteria scenarios: Happy Path - Configure Video Encoder Parameters (Profile S)**
```gherkin
Given the selected camera supports Profile S
When the admin opens the "Video Encoder" configuration
Then the system exposes the editable parameters: Codec (H.264, MJPEG), Resolution, Bitrate, Framerate, GOP interval, and Quality
And submitting the form applies these settings directly to the camera via ONVIF
```

**Acceptance criteria scenarios: Happy Path - Configure Imaging Parameters**
```gherkin
Given the selected camera exposes the ONVIF Imaging Service
When the admin opens the "Image" configuration section
Then the system dynamically renders only the image parameters the camera actively supports (e.g., Brightness, Contrast, Sharpness, WDR, IR Cut Filter, White Balance, Exposure, Noise Reduction)
And each parameter includes allowable min/max bounds as reported by the camera's ONVIF capabilities
```

**Acceptance criteria scenarios: Batch Action - Multi-Camera Configuration**
```gherkin
Given the admin selects multiple cameras that share at least one identical ONVIF profile
When they apply a common parameter configuration (e.g., Framerate, Codec)
Then the system applies the bulk update to all selected cameras simultaneously via ONVIF
And cameras with incompatible profiles are automatically excluded with a clear warning listing the excluded devices
```

---

## Feature: Multi-tier Storage Configuration

**Why:** Without this feature, all video data is written to a single storage destination with no fallback. If that destination fills or becomes unavailable, footage is permanently lost — breaking the core data continuity promise of the Camera Management epic.
**Scope Type:** Update

### US-03.1.4: Configure Video Storage Destinations

**Priority:** Critical
**Priority Reason:** Video storage is the most expensive operational cost; precise routing is critical for cloud SaaS margins.
**Story Points:** 5

**Story:**
As a Site Administrator,
I want to configure the storage destination, capacity limits, and retention duration for each camera's video data,
So that I can balance local edge storage with centralized cloud archiving.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Setting Storage Tiers**
```gherkin
Given the admin opens the storage configuration for a specific camera
When they define the storage routing rules
Then they can explicitly choose to save video to the "Camera Memory Card", the "Mini PC Local Storage", or the "Central Software DB"
And they can define the retention policy by absolute time duration (e.g., "7 days") or storage capacity limits (e.g., "Maximum 50GB limit")
And they can select the predefined length for the recorded video files (chunks) to be stored: "15m", "30m", or "1hr"
```

**Acceptance criteria scenarios: Error Handling - Storage Full Cascade**
```gherkin
Given the primary storage tier (e.g., "Camera Memory Card") is full
When new video data needs to be written from the kerberos agent
Then the system automatically cascades the write operation to the next configured storage tier (e.g., "Mini PC Local Storage")
And an alert is raised on the dashboard notifying the admin that the primary storage tier has reached its capacity limit
And if all configured tiers are simultaneously full, new footage is queued and the admin is immediately notified to take corrective action
```

**Acceptance criteria scenarios: Edge Case - Simultaneous Duration and Capacity Limit**
```gherkin
Given the admin configures a storage rule with both a time duration ("7 days") and a capacity limit ("50GB")
When stored footage begins accumulating
Then the system enforces whichever limit is reached first
And footage exceeding the earliest-triggered limit is automatically purged in oldest-first order
```
