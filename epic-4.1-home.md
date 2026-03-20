# Epic 4.1: Home
**Parent Topic**: Topic 4: VMS (Video Management System)
**User Objective**: To provide a unified, role-aware home dashboard that gives operators an at-a-glance overview of all managed sites, camera statuses, storage, and recent events via an interactive map and a customizable information display panel.
**Pain Point**: Security operators managing multiple dispersed sites must switch between separate tools to understand what is happening spatially. Without a unified map-based overview combined with live metrics, critical site outages and storage issues are found reactively rather than proactively.

---

## Feature: Interactive Site Map (OpenStreetMap)

**Why:** Without this feature, operators have no spatial awareness of where their physical sites are located relative to each other — managing multiple sites is reduced to a disconnected list with no geographic context.
**Scope Type:** View / CRUD

### US-04.1.1: View Sites on Interactive Map
**Priority:** Must Have
**Priority Reason:** The map is the primary situational awareness interface for multi-site security operators.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to view all my accessible sites as icons pinned on an interactive OpenStreetMap,
So that I can immediately understand the geographic distribution of my surveillance network.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Accessibility**
```gherkin
Given the operator is on the Home dashboard
When the page loads
Then the interactive site map is displayed prominently in the main content region
And all site management actions are accessible directly from the map without navigating to a separate section
```

**Acceptance criteria scenarios: Happy Path - Viewing Site Icons on Map**
```gherkin
Given the operator has at least one active site registered
When the Home dashboard loads
Then each site is represented by a unique icon pinned at the correct geographic coordinates on the OpenStreetMap base layer
And each site icon displays a badge showing the total number of cameras registered at that site
```

**Acceptance criteria scenarios: Permission - Role-Filtered Map View**
```gherkin
Given a user with scoped site access logs into the Central Software
When the Home map renders
Then only the sites the user is authorized to access are displayed as icons
And sites outside their authorization are not shown — not even as grayed-out placeholders
```

---

### US-04.1.2: View Site Details on Hover
**Priority:** Must Have
**Priority Reason:** Operators need key site stats at a glance without opening a full panel.
**Story Points:** 3

**Story:**
As a Security Operator,
I want to hover over a site icon on the map and see a preview with an edit option,
So that I can quickly assess site health and access editing without navigating away.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Hover Tooltip with Edit Option**
```gherkin
Given the operator moves the mouse pointer over a site icon on the map
When the hover state activates
Then a tooltip/popover appears displaying a snapshot of site details
And an "Edit" action button is visible within the popover
```

---

### US-04.1.3: Edit Site Details
**Priority:** Must Have
**Priority Reason:** Site metadata (name, location, coordinates) must be correctable after initial setup when a site is relocated or renamed.
**Story Points:** 5

**Story:**
As a Site Administrator,
I want to click "Edit" from the site hover popover and update the site's configuration,
So that the site data in the system always reflects real-world operational changes.

> **Suggested Site Detail Fields:**
> - **Name**: Human-readable site label (e.g., "HQ North Building")
> - **Address**: Street/City/Country (used for geocoding)
> - **Coordinates**: Lat/Long (auto-filled from address lookup or manually overridden)
> - **Timezone**: Local timezone for accurate event timestamps
> - **Status**: Active / Inactive / Under Maintenance
> - **Contact Person**: On-site responsible contact name and phone
> - **Description / Notes**: Free text notes for special instructions or site context
> - **Assigned Mini PCs**: List of registered devices at this site

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Editing Site Metadata**
```gherkin
Given the admin selects "Edit" from the site hover popover
When the site edit panel slides open
Then it displays all current site parameters (Name, Address, Coordinates, Timezone, Status, Contact, Description, Assigned Mini PCs)
And the admin can modify any editable field and click "Save Changes" to apply
```

**Acceptance criteria scenarios: Happy Path - Address Auto-Geocoding**
```gherkin
Given the admin updates the "Address" field in the site edit panel
When they submit or blur the address input
Then the system automatically queries a geocoding service and updates the Latitude/Longitude coordinate fields
And the site icon repositions on the map to the new coordinates
```

**Acceptance criteria scenarios: Error Handling - Coordinate Out of Bounds**
```gherkin
Given the admin manually enters Latitude/Longitude coordinates
When the values are outside valid geographic bounds (e.g., lat > 90 or lat < -90)
Then the system highlights the field with an inline validation error
And the save action is blocked until valid coordinates are entered
```

---

### US-04.1.4: Add a New Site
**Priority:** Must Have
**Priority Reason:** Without the ability to add new sites from the dashboard, the product cannot grow with the operator's physical surveillance network.
**Story Points:** 5

**Story:**
As a Site Administrator,
I want to click the (+) overlay button in the lower right corner of the map and create a new site,
So that I can expand my surveillance network with a new physical location directly from the dashboard.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Opening the Add Site Panel**
```gherkin
Given the admin is viewing the Home dashboard map
When they click the (+) floating action button in the lower right corner of the map section
Then a "New Site" creation panel opens as a form overlay or slide-in drawer
```

**Acceptance criteria scenarios: Happy Path - Creating a Site**
```gherkin
Given the admin completes the New Site form (Name, Address, Timezone, Contact Person, Description)
When they click "Create Site"
Then a new site icon immediately appears on the map at the geocoded coordinates
And the site is available for Mini PC device registration in Epic 3.2
```

**Acceptance criteria scenarios: Error Handling - Duplicate Site Name**
```gherkin
Given the admin enters a site name that already exists in their account
When they attempt to submit the form
Then an inline error appears "A site with this name already exists. Please choose a unique name."
And the form is not submitted
```

---

## Feature: Information Display Panel

**Why:** Without this feature, the dashboard is limited to a map with no quantified operational metrics — operators cannot answer basic questions like "how many cameras are offline?" or "how full is my storage?" without navigating to separate screens.
**Scope Type:** View

### US-04.1.5: View Dashboard Information Panels
**Priority:** Must Have
**Priority Reason:** Operators require quick-scan metrics to maintain situational awareness without deep navigation.
**Story Points:** 5

**Story:**
As a Security Operator,
I want to view key operational metrics in a structured information display panel below or beside the map,
So that I can monitor the overall health of my surveillance network at a glance.

> **Panel Display Modes** (user-selectable):
> - **All Cameras View**: Total cameras, Online count, Offline count, Cameras with storage errors.
> - **Specific Site View**: Cameras per selected site (filter by site/Mini PC), their individual online/offline status.
> - **Storage Capacity View**: Total storage allocated, used, remaining, per tier (Camera SD, Mini PC, Central DB).
> - **Event List View**: Chronological list of recent alerts from all cameras and Mini PCs (links to Epic 3.4 event detail).

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Default All Cameras View**
```gherkin
Given the operator is on the Home dashboard
When the information display panel loads
Then it defaults to showing "All Cameras" including a total camera count, online count, and offline count
And cameras in an error or disconnected state are highlighted prominently
```

**Acceptance criteria scenarios: Happy Path - Switching to Site-Specific View**
```gherkin
Given the operator selects a specific site from the panel's filter dropdown
When the filter is applied
Then the panel updates to show only the cameras belonging to that site's associated Mini PCs
And each camera is listed with its name and current online/offline status
```

**Acceptance criteria scenarios: Happy Path - Storage Capacity View**
```gherkin
Given the operator switches the panel mode to "Storage Capacity"
When the panel renders
Then it displays storage utilization across all three tiers: Camera SD, Mini PC Local, and Central DB
And each tier shows used, available, and total capacity with a visual percentage bar
```

**Acceptance criteria scenarios: Happy Path - Recent Event List View**
```gherkin
Given the operator switches the panel mode to "Event List"
When the panel renders
Then a chronological list of the most recent alarms and alerts from all accessible cameras and Mini PCs is displayed
And each event entry shows: source hardware, event type, and timestamp
And clicking an event navigates to the corresponding Event Management detail in Epic 3.4
```
