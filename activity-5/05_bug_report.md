# BUG-SEC-001 – Sensitive User Information Stored in Session Storage as Plain Text

## Bug Information

| Field | Value |
|------|------|
| Bug ID | BUG-SEC-001 |
| Title | Sensitive user information is stored in Session Storage in plain text |
| Severity | High |
| Priority | P1 |
| Status | Open |
| Module | Authentication / User Profile |
| Environment | Staging |
| Browser | Chrome 126 / Chrome 150 |
| Operating System | Windows 11 |

---

# Environment

- Application: Booking Appointment System
- Environment: Staging
- Browser: Chrome 126 / Chrome 150
- OS: Windows 11
- Tools:
  - Chrome DevTools
  - Application Tab
  - Network Tab
  - Sources Tab
  - Console

---

# Preconditions

- User has a valid account.
- User can log into the system successfully.
- Browser Session Storage is empty before login.

---

# Steps to Reproduce

1. Open the Booking Appointment System.
2. Log in using a valid user account.
3. Press F12 to open Chrome DevTools.
4. Open the **Application** tab.
5. Navigate to **Session Storage**.
6. Select the current website.
7. Locate the key **auth_state_v1**.
8. Inspect its stored value.

---

# Expected Result

Only the minimum authentication information required for the current session should be stored in Session Storage.

Sensitive personal information (PII), such as email address or unnecessary business data, should not be stored in plain text.

---

# Actual Result

The key **auth_state_v1** stores the complete API response directly in Session Storage.

The stored JSON contains sensitive information including:

- id
- email
- name
- role
- bookingCount
- fetchedAt

These values are stored in plain text and can be accessed directly through the browser.

---

# Evidence

- Session Storage contains `auth_state_v1`
- API `/bookings/me` returns full user information
- React Context directly executes

```javascript
sessionStorage.setItem("auth_state_v1", JSON.stringify(response))
```

without filtering unnecessary fields.

Supporting evidence:

- Session Storage screenshot
- Sources search result
- Network response
- Verification Log (Activity 4)

---

# Severity

**High**

### Reason

Sensitive Personally Identifiable Information (PII) is exposed in browser storage.

Any script running in the same browser context or a user with local machine access can read the stored information.

Although no direct privilege escalation was observed, exposing PII creates a significant security risk.

---

# Priority

**P1**

### Reason

The issue affects all authenticated users and should be fixed as soon as possible because it violates secure data storage practices.

---

# Root Cause (Verified)

Human verification confirmed that the issue is caused by two design problems:

### Primary Root Cause

Frontend React Context directly stores the entire API response into Session Storage using

```javascript
sessionStorage.setItem()
```

without applying any mapping, filtering, or data sanitization.

### Contributing Cause

Backend API `/bookings/me` returns more user information than the UI actually requires, including:

- email
- bookingCount

These unnecessary fields are subsequently persisted by the frontend.

---

# Suggested Fix

## Frontend

- Create a dedicated Persist Model containing only the minimum fields required.
- Remove email and bookingCount before saving data.
- Never persist the raw API response.
- Store only authentication/session information.

Example

```javascript
const authState = {
    id: response.id,
    name: response.name,
    role: response.role
};

sessionStorage.setItem(
    "auth_state_v1",
    JSON.stringify(authState)
);
```

## Backend

Review the `/bookings/me` API response and remove unnecessary PII that is not required by the UI.

Return only the minimum fields needed by the client.

---

# Trace

## Related Test Case

TC-LOGIN-PII-001

## Related Requirement

US2 – Login

---

# Verification Result

| Hypothesis | Result |
|------------|---------|
| H1 – Custom sessionStorage.setItem | Confirmed |
| H2 – Zustand / Redux Persist | Rejected |
| H3 – Cache / Service Layer | Rejected |
| H4 – Backend returns unnecessary fields | Confirmed |
| H5 – Redux Persist Storage | Rejected |

---

# References

- Activity 3 – Root Cause Hypotheses
- Activity 4 – Human Verification