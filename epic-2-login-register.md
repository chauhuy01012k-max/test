# Epic 2.1: User Registration
**Parent Topic**: Topic 2: Login & Register
**User Objective**: To securely create an account and authenticate into the platform.
**Pain Point**: Complex or insecure onboarding flows prevent users from adopting the platform.

---

## Feature: Account Creation & Email Verification

**Why:** Without this feature, new users have no way to enter the platform — there is no account to authenticate against, making all subsequent epics inaccessible.
**Scope Type:** Create

### US-02.1.1: User Registration (Email/Password)
**Priority:** Must Have
**Priority Reason:** Standard entry point into the application for users without third-party SSO.
**Story Points:** 5

**Story:**
As an unregistered user,
I want to create an account using my email and a secure password,
So that I can securely access the system's features.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Successful Registration**
```gherkin
Given the user navigates to the registration page
When they submit a valid email
And they submit a strong password (minimum 8 characters, 1 number, 1 special character)
Then a new account is provisioned in a pending state
And a verification email is dispatched
```

**Acceptance criteria scenarios: Error Handling - Duplicate Email Rejection**
```gherkin
Given the user is on the registration page
When they submit an email that is already registered
And they click "Register"
Then an inline error message displays "This email is already registered. Please log in."
```

**Acceptance criteria scenarios: Happy Path - Account Verification**
```gherkin
Given the user has received a verification email
When they click the verification link
Then their account status changes to active
And they are prompted to set up MFA
```

---

# Epic 2.2: User Login
**Parent Topic**: Topic 2: Login & Register
**User Objective**: To securely authenticate existing users into the platform using Password or SSO.
**Pain Point**: Security vulnerabilities during login and cumbersome authentication steps for frequent users.

---

## Feature: Secure Authentication (Password / SSO)

**Why:** Without this feature, registered users have no path to access the platform — authentication is the gateway to every other feature in the product.
**Scope Type:** Authenticate

### US-02.2.1: User Login (Email/Password)
**Priority:** Must Have
**Priority Reason:** Core pathway for non-SSO users to access their authorized data.
**Story Points:** 3

**Story:**
As a registered user,
I want to log in using my email and password,
So that I can securely authenticate into my account.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Successful Primary Authentication**
```gherkin
Given the user has an active registered account
When they enter the correct email and password
And they submit the form
Then they are successfully authenticated 
And they are redirected to the MFA verification step
```

**Acceptance criteria scenarios: Error Handling - Invalid Credentials Handling**
```gherkin
Given the user is on the login page
When they enter an incorrect password or unregistered email
And they submit the login form
Then a generic "Invalid email or password" error is displayed to prevent account enumeration
```

**Acceptance criteria scenarios: Security - Account Lockout after Repeated Failures**
```gherkin
Given the user has made 5 consecutive failed login attempts
When they attempt to log in again
Then their account is temporarily locked for 15 minutes
And an error message is displayed
```

### US-02.2.2: Single Sign-On (SSO) Login
**Priority:** Must Have
**Priority Reason:** Enterprise adoption requires SSO integrations.
**Story Points:** 8

**Story:**
As a user,
I want to log in using a recognized third-party SSO provider (e.g., Google, Microsoft),
So that I can access the system quickly without remembering an additional password.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Redirect to Provider**
```gherkin
Given the user is on the login view
When they click the "Continue with [Provider]" button
Then they are safely redirected to the SSO provider's authentication portal
```

**Acceptance criteria scenarios: Happy Path - Successful SSO Authentication**
```gherkin
Given the user has previously linked their internal account with the SSO provider
When they successfully authenticate via the provider
Then they are redirected back to the system's MFA verification step
```

**Acceptance criteria scenarios: Error Handling - SSO Provider Error**
```gherkin
Given the user attempts to log in via an SSO provider
When the SSO provider returns an authentication error or timeout
And the user is redirected back
Then a user-friendly error message "Authentication failed via SSO provider. Please try again." is displayed
```

### US-02.2.3: Auto-Generate and Link Account on First-Time SSO
**Priority:** Must Have
**Priority Reason:** Eliminates onboarding friction for enterprise users.
**Story Points:** 5

**Story:**
As a new user logging in via SSO for the first time,
I want the system to automatically generate and link an internal account for me,
So that I experience a seamless and immediate onboarding process without manual data entry.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Automatic Account Generation**
```gherkin
Given the user has never logged into the system before
When they successfully authenticate via an SSO provider
Then the system automatically captures their email and basic profile data
And generates a new internal account
```

**Acceptance criteria scenarios: Happy Path - Permanent Identity Linking**
```gherkin
Given the new internal account is generated successfully
When the system completes the background provisioning
Then the SSO identity is permanently linked to this new internal account
```

**Acceptance criteria scenarios: Validation - Prompt Initial MFA**
```gherkin
Given the account linking is completed
When the user is redirected back to the app
Then they are immediately prompted to perform their initial MFA setup
```

---

## Feature: Multi-Factor Authentication (MFA)

**Why:** Without this feature, the login flow is incomplete — MFA is a mandatory security requirement and users are blocked from accessing the system until it is configured and verified on every login.
**Scope Type:** Security / Setup

### US-02.2.4: Initial MFA Setup
**Priority:** Must Have
**Priority Reason:** Mandatory security requirement for the VSS Agent.
**Story Points:** 5

**Story:**
As a newly registered user or first-time SSO user,
I want to set up Multi-Factor Authentication (MFA),
So that my account is protected with a mandatory extra layer of security.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Display Setup Details**
```gherkin
Given the user has completed registration or first-time SSO login
When they are redirected to the MFA setup screen
Then a unique QR code is displayed
And a manual setup key is displayed for their authenticator app
```

**Acceptance criteria scenarios: Happy Path - Successful MFA Enablement**
```gherkin
Given the user has scanned the QR code with their authenticator app
When they enter the correct 6-digit Time-based One-Time Password (TOTP)
And they submit the form
Then MFA is permanently enabled for their account
```

**Acceptance criteria scenarios: Validation - Invalid Setup Attempt**
```gherkin
Given the user is trying to set up MFA
When they enter an incorrect or expired TOTP code
And they submit the form
Then an error message "Invalid verification code. Please try again." is displayed
And MFA remains unconfigured
```

### US-02.2.5: MFA Verification during Login
**Priority:** Must Have
**Priority Reason:** Essential requirement to enforce the MFA setup.
**Story Points:** 3

**Story:**
As a returning user with MFA enabled,
I want to verify my identity using an MFA code during every login attempt,
So that unauthorized persons cannot access my account even if my primary credentials are compromised.

**Acceptance criteria scenarios:**

**Acceptance criteria scenarios: Happy Path - Successful Verification**
```gherkin
Given the user has successfully passed primary authentication (Password or SSO)
When they enter the correct current 6-digit MFA code
And they submit the form
Then they are fully logged into the system
And they are redirected to their dashboard
```

**Acceptance criteria scenarios: Validation - Invalid MFA Code**
```gherkin
Given the user is on the MFA verification step
When they enter an incorrect or expired 6-digit code
And they submit the form
Then an error "Invalid authentication code" is displayed
And they are not logged in
```

**Acceptance criteria scenarios: Alternative Path - Lost Authenticator Fallback**
```gherkin
Given the user is on the MFA verification step
When they click "Lost access to my authenticator app"
Then they are guided to an account recovery workflow
```
