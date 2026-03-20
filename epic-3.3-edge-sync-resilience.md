# Epic 3.3: Edge-to-Cloud Sync & Resilience
**Parent Topic**: Topic 3: Site (Mini PC & Edge Server Management)
**User Objective**: To ensure local DB configuration and activity logs synchronize seamlessly with the Central VMS, and to forcefully alert administrators if the edge hardware loses sync connectivity.
**Pain Point**: Edge servers are prone to network isolation. Without a robust retry protocol and an aggressive alerting system, sites could go blind for hours without operations noticing until an incident occurs.

---

## Feature: DB Configuration & Activity Replication

**Why:** Without this feature, the Central Software and Mini PC operate with independent, eventually divergent data models — configuration changes made locally are invisible centrally, breaking the unified management promise of the product.
**Scope Type:** Infrastructure / System

### US-03.3.1: Synchronize Edge Database
**Priority:** Critical
**Priority Reason:** Essential pipeline connecting the hardware edge to the SaaS cloud.
**Story Points:** 8

**Story:**
As a System Architect,
I want the Mini PC database to aggressively push local configuration data changes upwards, while the Central Software periodically pulls activity logs downwards,
So that data consistency is maintained efficiently over volatile internet links.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Pushing Configuration**
```gherkin
Given a site admin modifies camera parameters locally on the Mini PC UI
When the local configuration database is updated
Then the Mini PC triggers an immediate replica push to the Central Software Database
And the Central DB reflects the updated configuration instantly
```

**Acceptance criteria scenarios: Performance - Pulling Activity Logs**
```gherkin
Given the kerberos agents are generating activity and event logs locally on the Mini PC
When the predefined synchronization period elapses (e.g., every 5 minutes)
Then the Central Software systematically pulls the batch of accumulated activity logs from the Mini PC DB
And safely trims the archived logs from the local Mini PC cache to preserve storage
```

---

## Feature: Edge Disconnect Alert Protocol

**Why:** Without this feature, a disconnected site appears "active" in the Central Software with no indication of failure — the entire site goes dark silently and incidents go undetected for hours.
**Scope Type:** Security / Alerting

### US-03.3.2: 10-Attempt Failure Execution
**Priority:** Critical
**Priority Reason:** Security teams must know explicitly when a site goes completely "dark" on the network.
**Story Points:** 8

**Story:**
As a Site Administrator,
I want the system to aggressively retry failed data updates using a strict session cadence, and alarm me both physically and centrally if the link completely fails,
So that I do not suffer from alert fatigue on minor network drops, but am instantly alerted to critical outages.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Alternative Path - Handling Transient Network Drops**
```gherkin
Given the Mini PC attempts to push a data update to the Central DB
When the update fails due to packet loss
Then the Mini PC software initiates up to 3 rapid sessions (3 seconds apart) constituting "1 Attempt"
And if the connection restores on the 2nd session, the update succeeds and the overall retry counter resets to 0
```

**Acceptance criteria scenarios: Happy Path - Exhausting the Retry Protocol**
```gherkin
Given the connection between the Mini PC and the internet is completely severed
When a data update fails
Then the Mini PC executes 1 attempt (3 sessions 3s apart) every 3 minutes
And when exactly 10 consecutive attempts fail (lasting approximately 30 minutes of total downtime)
Then the sync protocol declares an "Edge Isolation Status"
```

**Acceptance criteria scenarios: Security - Triggering Physical and Central Alerts**
```gherkin
Given an "Edge Isolation Status" is declared
When the 10th consecutive attempt fails
Then the local Edge software triggers a continuous signal to the physical Mini PC speaker
And the Central Software immediately generates an urgent popup newsletter displayed to users precisely when they subsequently log in to the central dashboard
And an urgent alert summarizes the outage via an automated email dispatched to configured administrators
```
