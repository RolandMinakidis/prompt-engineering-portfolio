# 🧪 Baseline Comparison Test (Naive vs. RTCE)

## Objective
To measure how structural prompting improves output consistency, eliminates unnecessary token usage, and prevents extra conversational fluff in business workflows.

---

## 📋 The Test Scenario
**Task:** Extract key action items and deadline dates from a messy project update email.

**Raw Input Data:**
> "Hey team, quick update from today's sync. Sarah finished the homepage design draft, so Mark needs to review that by Friday at 5 PM. Also, we noticed a critical bug in the login flow—Alex, please patch that before tomorrow morning. Oh, and everyone remember to submit your time tracking logs by end of day Thursday!"

---

## ❌ Test A: Naive Prompt (Unstructured)

### Prompt Used:
> "Find all the tasks and who needs to do them from this email: [Insert Raw Input Data]"

### AI Response Received:
> "Sure thing! Here are the tasks mentioned in the email:
> 
> 1. Mark needs to review the homepage design draft that Sarah finished. He should do this by Friday at 5 PM.
> 2. Alex needs to fix a critical bug in the login flow before tomorrow morning.
> 3. Everyone on the team needs to submit their time tracking logs by the end of the day on Thursday.
> 
> Hope this helps! Let me know if you need anything else."

### Analysis of Test A:
* ⚠️ **Conversational Fluff:** Output contains unnecessary intro ("Sure thing!...") and outro ("Hope this helps!").
* ⚠️ **Token Inefficiency:** Uses extra words to repeat the full context.
* ⚠️ **Non-Deterministic:** Unusable by automated code/parsers due to sentence-based formatting.

---

## ✅ Test B: RTCE Structured Prompt (Optimized)

### Prompt Used:
```text
[ROLE]
You are an Automated Project Management Assistant.

[TASK]
Extract actionable tasks, assigned individuals, and deadlines from the input text.

[CONTEXT]
Input Text:
"Hey team, quick update from today's sync. Sarah finished the homepage design draft, so Mark needs to review that by Friday at 5 PM. Also, we noticed a critical bug in the login flow—Alex, please patch that before tomorrow morning. Oh, and everyone remember to submit your time tracking logs by end of day Thursday!"

[EVALUATION]
1. Output MUST be formatted exclusively as a clean Markdown table.
2. Table headers: `Assignee` | `Action Item` | `Deadline` | `Priority`.
3. Priority level must be inferred as `High`, `Medium`, or `Low` based on urgency.
4. DO NOT include any introductory or concluding text. Output ONLY the table.
