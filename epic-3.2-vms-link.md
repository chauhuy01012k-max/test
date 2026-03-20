# Epic 3.2: VMS Link (Mini PC Installation & Auth)
**Parent Topic**: Topic 3: Site (Mini PC & Edge Server Management)
**User Objective**: To securely install, register, and restore Edge Software on a Mini PC connected to the Central VMS.
**Pain Point**: Edge deployments fail if physical hardware cannot securely authenticate back to the central domain or if hardware replacements require manual reconfiguration from scratch.

---

## Feature: Mini PC Software Setup & Domain Binding

**Why:** Without this feature, the Mini PC cannot identify or connect to any Central VMS tenant — neither camera streams, event syncs, nor user logins are possible until this domain binding is established.
**Scope Type:** Setup / Installation

### US-03.2.1: Initial Software Installation
**Priority:** Critical
**Priority Reason:** Essential requirement; the product physically cannot function without this binding.
**Story Points:** 5

**Story:**
As a Site Installer,
I want the mini PC installation wizard to strictly mandate an active internet connection and the input of the Central VMS Domain,
So that the device is cryptographically bound to the correct central software instance during setup.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Domain Registration**
```gherkin
Given the installer powers on a brand new Mini PC with the Edge software loaded
When the installation wizard boots
And the Mini PC detects an active outbound internet connection
Then the installer is prompted to input the exactly configured URL/Domain of the Central Software
And submitting a valid domain successfully establishes a handshake with the central tenant
```

**Acceptance criteria scenarios: Error Handling - Offline Installation Attempt**
```gherkin
Given the installer is setting up the Mini PC
When the Edge software attempts to initialize but detects no outbound internet connectivity
Then the installation halts immediately
And displays a blocking error "Installation requires an active internet connection. Please connect network cable."
```

### US-03.2.5: Change Central VMS Domain
**Priority:** Must Have
**Priority Reason:** Operational requirement when migrating a Mini PC between tenants or when the Central Software URL changes.
**Story Points:** 3

**Story:**
As a Site Administrator,
I want to update the Central VMS Domain that the Mini PC is bound to,
So that the edge device can continue operating after a domain migration or tenant reassignment.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Successful Domain Change**
```gherkin
Given the Mini PC is already installed and operational
When an authorized admin opens the "System Settings > Central Domain" configuration
And they input a new valid Central VMS domain URL and confirm the change
Then the Mini PC disconnects from the old domain
And re-establishes a handshake with the new domain
And all subsequent data syncs are directed to the new Central VMS tenant
```

**Acceptance criteria scenarios: Security - Unauthorized Domain Change Attempt**
```gherkin
Given the Mini PC is operational
When a user without "System_Admin" privileges attempts to access the "Central Domain" setting
Then the field is disabled or hidden entirely
And the system displays "Permission denied: Only system administrators can change the Central VMS domain."
```

**Acceptance criteria scenarios: Error Handling - New Domain Unreachable**
```gherkin
Given the admin is updating the Central VMS domain
When the Mini PC cannot establish a connection to the newly entered domain URL
Then the system cancels the change and rolls back to the previous domain configuration
And displays an inline error "Cannot connect to the new domain. Verify the URL and network connection before retrying."
```

---

## Feature: Device Registration Authorization

**Why:** Without this feature, any user could add rogue hardware to the network or access an active site's cameras — there is no security perimeter protecting the physical edge layer.
**Scope Type:** Security / Authentication

### US-03.2.2: Add a Completely New Mini PC
**Priority:** Must Have
**Priority Reason:** Without restricting new provisions, rogue hardware could inject local footage into the tenant.
**Story Points:** 5

**Story:**
As an Authorized Administrator,
I want the system to require a verified login before allowing a brand new Mini PC to be fully added to the central software,
So that I prevent unauthorized or rogue hardware from joining the network.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: System Validation - Authorized Addition**
```gherkin
Given the Mini PC has successfully bound to the Central Domain
When the installer is prompted to log in through the unified UI (deployed on the mini PC)
And the installer logs in using an account with explicit `Install_Device` privileges
Then the Mini PC is successfully registered to the Central DB as an active site node
```

**Acceptance criteria scenarios: Security Restriction - Unauthorized Addition**
```gherkin
Given the Mini PC has successfully bound to the Central Domain
When a user logs in to the Mini PC UI using an account lacking `Install_Device` privileges
Then the system blocks the registration
And displays "Permission Denied: You do not have the authorization required to add a new edge device."
```

### US-03.2.3: Access a Working Mini PC
**Priority:** Must Have
**Priority Reason:** Protects local console access of the Edge software.
**Story Points:** 3

**Story:**
As an Authorized User,
I want to log into an already working Mini PC to access its local features,
So that I can manage the edge device securely without risking unauthorized access.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Site Navigation**
```gherkin
Given the user logs into the local working Mini PC or the Central Software node
When the interface renders
Then the core site information, camera lists, and operational states for the Mini PC are prominently displayed in the main UI workspace
And the user does not have to hunt through the Topic/Epic framework navigation bars to uncover the active feature options
```

**Acceptance criteria scenarios: Access Control - Working Device Login**
```gherkin
Given a Mini PC is fully registered and actively working
When a user attempts to access the local edge software via the device's UI
Then they are forced to log in
And only users with specific site-level access privileges (e.g., `Site_Admin` or `Operator`) can view the local camera feeds and event logs
```

---

## Feature: Device Restore Pipeline

**Why:** Without this feature, a failed Mini PC hardware unit requires complete manual re-configuration from scratch — reinstalling all camera registrations, storage rules, and event triggers can take hours, creating a critical surveillance blackout window during hardware replacement.
**Scope Type:** Create / Restore

### US-03.2.4: Install via Active-Backup Restore
**Priority:** Should Have
**Priority Reason:** Massively reduces RTO (Recovery Time Objective) during hardware failure replacements.
**Story Points:** 8

**Story:**
As a Site Installer,
I want to choose to install a new Mini PC by restoring configuration data cloned from a previously saved, inactive Mini PC,
So that I do not have to manually re-configure ONVIF rules, Kerberos pods, and event alarms upon hardware replacement.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Seamless Edge Restore**
```gherkin
Given the installer is setting up a new Mini PC attached to the Central Domain
When they authenticate as an authorized installer
Then the system presents an option to "Restore from Inactive Device Backup"
And selecting a specific inactive Mini PC profiles flawlessly clones all local DB configuration (Cameras, storage rules, events) onto the new hardware
```

**Acceptance criteria scenarios: Validation - Restoring from an Active Device**
```gherkin
Given the installer is in the Restore configuration wizard
When they attempt to restore data from a Mini PC that is currently marked as "Active" or connected
Then the system prevents the selection
And displays a warning "Cannot restore from a device currently active on the network. Select an inactive backup."
```
