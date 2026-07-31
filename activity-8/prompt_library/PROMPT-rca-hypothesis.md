## PROMPT_ID: RCA-HYPOTHESIS-001

### Purpose

Sinh ít nhất 5 giả thuyết Root Cause Analysis (RCA), đánh giá xác suất, evidence ủng hộ/phản bác và đề xuất cách xác thực.

### Prompt template

You are a Senior QA performing Root Cause Analysis.

Using ONLY the evidence below, generate at least 5 possible root cause hypotheses.

For each hypothesis provide:

- Hypothesis
- Probability (High / Medium / Low) with explanation
- Supporting evidence
- Refuting evidence
- Verification experiment
- Expected result if the hypothesis is correct

Rules:

- Use ONLY available evidence.
- Clearly distinguish evidence from assumptions.
- If evidence is insufficient, explicitly state so.
- Do NOT force probabilities to sum to 100%.
- Multiple hypotheses may have similar probabilities.

## EVIDENCE
{{evidence}}

### Known limitations

- AI may overlook refuting evidence unless explicitly required.
- AI should not assume hidden implementation details.
- AI must not invent logs or system behavior.
- AI must not force probability totals to equal 100%.

### Version history

- v1.0 Initial version
- v1.1 Added mandatory refuting evidence and no forced 100% probability rule
