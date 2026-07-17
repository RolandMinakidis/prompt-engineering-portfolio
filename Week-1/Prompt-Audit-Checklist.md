# 🔍 Prompt Optimization & Quality Control Audit

## Overview
Writing a functional prompt is only step one. Production-grade prompt engineering requires rigorous audit protocols to ensure safety, cost-efficiency, and output consistency across different model providers (OpenAI, Anthropic, Google Gemini). 

This document outlines the **5-Point Quality Control Checklist** used to audit all prompt architectures in this portfolio.

---

## 📋 The 5-Point Prompt Quality Checklist

### 1. Token Efficiency Audit 🪙
* **Goal:** Reduce unnecessary token consumption to minimize API costs.
* **Checks:**
  * [ ] Removed polite/filler phrases ("Please", "Thank you", "Can you help me").
  * [ ] Replaced wordy explanations with explicit bulleted constraints.
  * [ ] Checked that system instructions use direct, imperative commands (e.g., "Extract" instead of "I would like you to extract").

### 2. Output Determinism & Formatting 🎯
* **Goal:** Ensure the LLM output can be cleanly parsed by automated code pipelines.
* **Checks:**
  * [ ] Explicitly forbidden conversational intro/outro text.
  * [ ] Output format strictly bound to JSON, Markdown Table, or CSV.
  * [ ] Defined clear schema requirements (e.g., specific JSON keys).

### 3. Hallucination & Fallback Prevention 🛑
* **Goal:** Prevent the model from making up facts when data is missing or ambiguous.
* **Checks:**
  * [ ] Included an explicit fallback instruction (e.g., *"If data is missing, return 'DATA_NOT_FOUND'"*).
  * [ ] Constrained the model to **only** use provided context for factual claims.
  * [ ] Avoided open-ended questions where speculative answers are possible.

### 4. Edge Case Handling ⚡
* **Goal:** Ensure the prompt handles weird, malicious, or unexpected user inputs gracefully.
* **Checks:**
  * [ ] Tested with empty inputs or incomplete text.
  * [ ] Included basic safety boundaries (ignoring prompt injection attempts inside input text).
  * [ ] Defined handling for multi-language or non-standard characters.

### 5. Few-Shot Efficiency 📚
* **Goal:** Verify if examples are necessary, and if so, keep them concise.
* **Checks:**
  * [ ] Evaluated Zero-Shot performance before adding examples.
  * [ ] If Few-Shot is used, examples represent diverse inputs and edge cases.
  * [ ] Ensured example formatting matches the requested evaluation schema exactly.

---

## 🧪 Audit Case Study: Optimizing a Flawed Prompt

### Original (Unaudited) System Prompt:
> "Hello! Please read this customer message and tell me if they are mad or happy. Also try to figure out what product they bought if they mention it. Thanks!"

### Identified Audit Issues:
1. **Token Waste:** Contains polite fillers ("Hello!", "Please", "Thanks!").
2. **Ambiguous Output:** "Mad or happy" is subjective; no structured format specified.
3. **Missing Fallback:** No instruction on what to do if the product isn't mentioned.

---

### Refactored (Audited) System Prompt:
```text
[ROLE]
You are an Automated Customer Sentiment Classifier.

[TASK]
Classify the sentiment of the input message and extract any mentioned product name.

[CONSTRAINTS]
1. Sentiment MUST be classified as strictly: `POSITIVE`, `NEUTRAL`, or `NEGATIVE`.
2. Product Name MUST be extracted verbatim. If no product is mentioned, output `NONE`.
3. Output MUST be a single raw JSON object with keys: `sentiment` and `product_name`.
4. Do NOT include introductory text, explanations, or conversational filler.

[INPUT]
{{USER_MESSAGE}}
