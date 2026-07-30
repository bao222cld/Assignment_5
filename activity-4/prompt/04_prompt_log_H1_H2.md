# Activity 4 – AI Prompt Log (Lê Quốc Anh)

## Prompt ID

VERIFICATION-H1-H2-001

---

## Version 1

You are a Senior Security QA Engineer and Software Architect.

Help me verify two specific root cause hypotheses (H1 and H2) for a PII leakage bug on a staging React/Next.js web application.

### Bug Context:
- Bug Description: User PII (email, name, role) is stored in plaintext under the key `auth_state_v1` in Session Storage after login.
- Web Staging URL: `https://booking-appointment-system-psi.vercel.app/`

### Hypotheses to Verify:
1. **H1 (Direct Response Mapping / Custom setItem):** A custom React context/function calls `sessionStorage.setItem()` directly with the raw API response object without filter/mapping.
2. **H2 (State Persistence Library - Zustand/Redux Persist):** The app uses a state persistence library (like Zustand or Redux Persist) that automatically syncs the auth state to Session Storage without a partialize/whitelist filter.

### Rules for Verification Guidance:
- Focus only on Black-box analysis using Chrome DevTools (Application, Network, Sources tabs).
- Explain step-by-step how to check the storage structure in the **Application** tab.
- Explain step-by-step how to search for source code in the **Sources** tab using global search (Ctrl+Shift+F).
- Provide a template structure for the verification log `04_verification_log_H1_H2.md` including:
  - System under test and Environment.
  - Experimental steps and metrics (e.g., number of test runs).
  - Concrete evidence filenames (screenshots).
  - Final verdict (Confirmed or Rejected).
- Output in Markdown format.

### Expected Input:
- The user will provide the Chrome DevTools screenshot content/description and search results.
