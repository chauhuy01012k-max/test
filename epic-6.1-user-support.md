# Epic 6.1: User Support
**Parent Topic**: Topic 6: Help and Feedback
**User Objective**: To provide operators and administrators with quick, contextual access to help resources and a searchable knowledge base from anywhere in the application, without interrupting their active workflow.
**Pain Point**: Users who encounter problems mid-workflow are forced to leave the application, search external documentation sites, and lose their current context — significantly delaying resolution and reducing platform adoption.

> **Scope Note**: This epic is globally accessible from Topics 2, 3, 4, and 5. The help widget persists across all major views and opens as an overlay without navigating away.

---

## Feature: Contextual Help Widget

**Why:** Without this feature, users have no in-app path to resolve issues — they must abandon their workflow to find help externally, increasing drop-off rates and support ticket volume.
**Scope Type:** View / Navigate

### US-06.1.1: Access the Help Widget from Any View
**Priority:** Must Have
**Priority Reason:** The help widget is the primary first-line support mechanism — it must be universally accessible to reduce support friction across all user personas.
**Story Points:** 3

**Story:**
As any authenticated user,
I want to access a help widget from any page in the application,
So that I can get quick guidance without leaving my current work context.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: UI Layout - Persistent Accessibility**
```gherkin
Given the user is viewing any page within Topics 2, 3, 4, or 5
When the page renders
Then a persistent "Help" icon or button is visible in the global navigation or as a floating action element
And clicking it opens the help widget as an overlay without navigating away or losing the current page state
```

**Acceptance criteria scenarios: Happy Path - Contextual Help Suggestion**
```gherkin
Given the user opens the help widget while on a specific feature page (e.g., Camera Management)
When the widget loads
Then it pre-populates with help articles relevant to the current feature context
And the user can read the suggested content without leaving the page
```

**Acceptance criteria scenarios: Alternative Path - Browsing All Help Topics**
```gherkin
Given the user opens the help widget
When they clear the contextual suggestion and browse manually
Then a categorized list of all available help topics is displayed
And selecting any category expands a list of related articles
```

---

### US-06.1.2: Search the Knowledge Base
**Priority:** Must Have
**Priority Reason:** Contextual suggestions cover common cases — a search capability handles long-tail questions that pre-loaded context cannot anticipate.
**Story Points:** 3

**Story:**
As any authenticated user,
I want to search the knowledge base by keyword from within the help widget,
So that I can quickly find answers to specific questions without browsing through every help category.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Keyword Search**
```gherkin
Given the user has the help widget open
When they type a keyword in the search field (e.g., "SD card recovery")
Then the widget displays a ranked list of matching knowledge base articles
And each result shows the article title and a short excerpt
```

**Acceptance criteria scenarios: Alternative Path - No Results Found**
```gherkin
Given the user searches for a keyword that has no matching articles
When the search completes
Then the widget displays a clear "No results found for '[keyword]'" message
And provides a link to "Submit a Support Request" via the Feedback feature (Epic 6.2)
```

**Acceptance criteria scenarios: Happy Path - Reading an Article**
```gherkin
Given the user selects a search result article
When the article opens
Then it renders the full help content within the widget panel
And a "Was this helpful?" thumbs up/down prompt is displayed at the bottom of the article
```
