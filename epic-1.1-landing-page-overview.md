# Epic 1.1: Landing Page Overview
**Parent Topic**: Topic 1: Landing Page
**User Objective**: To provide a compelling introduction to the VSS AI Agent and offer a persistent gateway for authentication.
**Pain Point**: Prospective users quickly bounce if the value proposition and login flow are not immediately obvious and easily accessible.

---

## Feature 1: Product introduction, services, customer logos, and visual key functions

**Why:** Without this feature, visitors cannot understand what the product is or does — the landing page provides no value and all other features of the epic have no context to present.
**Scope Type:** View

### US-01.1.1: View Hero Introduction and Value Proposition
**Priority:** Must Have
**Priority Reason:** Without a clear, immediate statement of what the product is (VSS AI Agent), visitors will bounce.
**Story Points:** 5

**Story:**
As an unauthenticated prospective customer,
I want to see a clear product introduction (hero section) summarizing the AI video search and site management capabilities,
So that I understand immediately how the product solves my video investigation pain points.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Main Region Accessibility**
```gherkin
Given the user navigates to the landing page
When the page loads
Then the hero introduction, core services, and customer trust signals are all visible in the main content region without requiring any navigation bar interaction
```

**Acceptance criteria scenarios: Happy Path - Successful View of Hero Section**
```gherkin
Given the user navigates to the landing page root URL
When the page loads successfully
Then the hero banner is displayed with the primary value proposition "Search, Verify, and Summarize Video with AI"
And a prominent Call-to-Action (CTA) button to "Register" or "Log In" is clearly visible above the fold
```

**Acceptance criteria scenarios: Performance - Load Time Expectation**
```gherkin
Given normal operating conditions
When the user requests the landing page
Then the hero section and initial visual content must display within 1.5 seconds to prevent user drop-off
```

**Acceptance criteria scenarios: Alternative Path - Mobile Responsiveness**
```gherkin
Given a user is accessing the landing page from a device with a mobile viewport
When the hero section renders
Then the introduction text and CTA buttons adapt to stack vertically
And the content remains fully legible without any horizontal scrolling required
```

### US-01.1.2: View Core Services and Visual Key Functions
**Priority:** Must Have
**Priority Reason:** Users need to know exactly *how* the AI helps them (e.g., Natural Language Search, Alert Verification, Mini PC edge device).
**Story Points:** 5

**Story:**
As an unauthenticated prospective customer,
I want to view the core services and visual demonstrations of the key functions,
So that I can evaluate if the platform's specific capabilities meet my security and operational needs.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Viewing Key Function Cards**
```gherkin
Given the user is scrolling down the landing page
When they reach the "Core Services" section
Then they see distinct cards summarizing key features including "AI Video Search", "Automated Summaries", and "Secure Edge Bridging"
```

**Acceptance criteria scenarios: Interactive Validation - Triggering Visual Demonstrations**
```gherkin
Given the user is inspecting a specific key function (e.g., "AI Video Search")
When the corresponding visual asset (video snippet or GIF) comes into the viewport
Then the animation demonstrating the Natural Language search plays automatically
And playback controls (pause/play) or text descriptions are accessible
```

**Acceptance criteria scenarios: Permission / Access Control - Attempting Restricted Actions**
```gherkin
Given the user is interacting with the visual demonstration of the AI Video Search
When the user clicks on the search bar or attempts to input a query in the demo
Then the system displays a friendly modal prompting them to "Register" or "Log In" to access the live search environment
And no actual queries are executed against the backend
```

### US-01.1.3: View Customer Logos and Trust Signals
**Priority:** Should Have
**Priority Reason:** Enterprise security buyers require deep social proof before adopting a cloud-based VMS solution.
**Story Points:** 3

**Story:**
As an unauthenticated prospective customer,
I want to see logos of existing enterprise customers or partners (e.g., NVIDIA VSS integration),
So that I feel confident in the product's reliability and market adoption.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Displaying the Trust Banner**
```gherkin
Given the user is on the landing page
When they scroll below the hero section or services area
Then a horizontal carousel or grid of customer/partner logos is displayed
```

**Acceptance criteria scenarios: Empty State - No Logos Configured Yet**
```gherkin
Given the platform is in early launch and no customer logos have been configured in the CMS
When the user navigates to the landing page
Then the trust banner section is safely hidden altogether
And the surrounding sections collapse smoothly without empty gaps
```

**Acceptance criteria scenarios: Error Handling - Graceful Degradation on Missing Assets**
```gherkin
Given the landing page cannot load one or more customer logo images due to a network timeout or missing asset link
When the trust banner attempts to render
Then the missing image gracefully falls back to the customer's text name or is safely hidden
And the rest of the page layout remains unbroken
```

---

## Feature 2: Navigation portal to Login/Register

**Why:** Without this feature, users who are ready to log in or register have no visible entry point — they are trapped on the landing page with no conversion path, making the entire epic commercially useless.
**Scope Type:** View

### US-01.1.4: View Persistent Navigation Header
**Priority:** Must Have
**Priority Reason:** Users navigating through deep product details must always have an immediate way to log in without needing to scroll back to the top.
**Story Points:** 2

**Story:**
As an unauthenticated visitor,
I want to see a persistent navigation header with login and registration buttons,
So that I can easily access the authentication portal from anywhere on the page.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Header Visibility on Load**
```gherkin
Given the user navigates to the landing page
When the page loads initially
Then a navigation header is displayed at the top of the viewport
And it contains distinct "Log In" and "Register" buttons
```

**Acceptance criteria scenarios: Alternative Path - Sticky Header on Scroll**
```gherkin
Given the user is on the landing page
When they scroll down past the initial hero section
Then the navigation header becomes sticky and remains fixed at the top of the viewport
And its background becomes slightly opaque to ensure the buttons remain legible over scrolling content
```

**Acceptance criteria scenarios: Alternative Path - Mobile Hamburger Menu**
```gherkin
Given a user is accessing the landing page from a device with a mobile viewport
When the navigation header renders
Then the distinct buttons are consolidated into a tappable "Hamburger" menu icon
And tapping the icon reveals the "Log In" and "Register" options
```

### US-01.1.5: Navigate to Authentication Portals
**Priority:** Must Have
**Priority Reason:** Essential critical path for user onboarding and system access.
**Story Points:** 1

**Story:**
As an unauthenticated visitor,
I want to click the navigation buttons in the header,
So that I am routed directly to the correct authentication or registration workflow.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Routing to Login**
```gherkin
Given the user is viewing the landing page navigation header
When they click the "Log In" button
Then they are securely redirected to the central login portal (/login)
And the landing page context is safely preserved in the browser history
```

**Acceptance criteria scenarios: Happy Path - Routing to Registration**
```gherkin
Given the user is viewing the landing page navigation header
When they click the "Register" button
Then they are securely redirected to the account creation workflow (/register)
```
