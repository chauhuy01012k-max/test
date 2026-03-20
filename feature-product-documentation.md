# Product Hierarchy context
- **Product Goal**: VSS AI Agent
- **Topic 1**: Landing Page
- **Epic 1.1**: Landing Page Overview

## Feature: Product Documentation (Guide / Manual portal)

**Why:** To provide unauthenticated visitors, prospective customers, and existing users with detailed resources, technical specifications, and user manuals to help them evaluate, implement, and use the VSS AI Agent.
**Scope Type:** View / Navigate

---

### US-01: Top Bar Documentation Link Opening in Separate Page
**Priority:** Must Have
**Priority Reason:** Essential critical path for users to access product guides directly from initial entry without losing the homepage context.
**Story Points:** 2

**Story:**
As an unauthenticated visitor,
I want to see a clear link to the Product Documentation directly on the top navigation bar,
So that I can easily access technical details in a dedicated document page without navigating away from the main landing page.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Open Documentation from Top Bar**
```gherkin
Given the user is viewing the landing page
When they examine the top navigation bar
Then a distinct link titled "Documentation" or "Guide / Manual" is clearly visible
And clicking the link opens the separate documentation portal securely in a new browser tab or window
```

---

### US-02: Search Documentation Top-Level Topics
**Priority:** Nice to Have
**Priority Reason:** Reduces friction for users who arrive with a specific technical question or troubleshooting need, allowing them to jump straight to the answer.
**Story Points:** 3

**Story:**
As an unauthenticated visitor or returning user,
I want to be able to search for documentation topics directly from a search bar or quick-link section,
So that I can quickly find specific answers without manually navigating through nested documentation menus.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Search and Redirect**
```gherkin
Given the user interacts with the documentation search bar on the landing page or resources section
When they enter a valid search query (e.g., "camera setup") and submit
Then they are redirected to the documentation portal's search results page for that specific query
```

**Acceptance criteria scenarios: Negative Path - Empty Search Query**
```gherkin
Given the user interacts with the documentation search bar
When they submit an empty search query
Then the system prevents the search submission
And highlights the search bar prompting the user to enter a query
```
