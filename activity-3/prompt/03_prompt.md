Role: You are a Senior Security QA Engineer and Software Architect specializing in web application debugging.
Context:
I am investigating a security bug where PII (Personally Identifiable Information) such as email, name, and role is being stored in plain text in the browser's sessionStorage under the key auth_state_v1.
Input Evidence from Activity 1:

URL: https://booking-appointment-system-psi.vercel.app/
Behavior: After login, the key auth_state_v1 appears in Session Storage containing a raw JSON object: {"user": {"id": "...", "email": "...", "name": "...", "role": "..."}, "bookingCount": 0}.
Network Discovery: The API GET /bookings/me returns the exact same JSON structure in its response body (Status 200 OK).
Console Log: No errors or warnings.
Environment: Next.js + PostgreSQL, hosted on Vercel.
Your Task (Activity 3):
Generate exactly 5 hypotheses for the root cause of this vulnerability. Format the output as a Markdown table with the following columns:

Rank: Priority of the hypothesis.
Hypothesis: Technical explanation of the potential cause.
Prob: Probability (High/Medium/Low) with a percentage.
Evidence FOR: Supporting facts from the evidence above.
Evidence AGAINST: Facts that might contradict this hypothesis.
Verify Method: A specific, manual experiment to confirm or reject the hypothesis.
Constraints & Rules:

DO NOT speculate without technical logic.
Focus on: State management libraries (Zustand/Redux), API Response design, Middleware, and Security utilities.
Ensure the "Verify Method" is actionable via Chrome DevTools or Source code review.
Follow the "Quality Gate #2" standard: No purely "emotional" hypotheses, all must be based on the provided evidence.
Output Language: Vietnamese.