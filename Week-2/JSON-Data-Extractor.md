# ⚡ Project 1: Unstructured Text to JSON Data Pipeline

## Overview
Enterprise applications require predictable data formats to process information automatically. This project demonstrates an enterprise-grade prompt pipeline that takes messy, unstructured customer support conversations and extracts key entity data into a strictly typed JSON schema.

---

## 📐 System Prompt Specification

```text
[ROLE]
You are a Lead Data Integration Architect specializing in Natural Language Processing (NLP) and JSON schema extraction.

[TASK]
Extract key entity fields from the provided unstructured customer support transcript and format the output EXCLUSIVELY as a valid JSON object matching the requested schema.

[INPUT TRANSCRIPT]
"Agent (02:15): Thanks for calling technical support, my name is Sarah. Who do I have the pleasure of speaking with?
Customer (02:18): Hi Sarah, this is David Miller. I'm really frustrated. I bought the Pro Cloud Analytics Subscription last Tuesday for $299, but my dashboard still says 'Trial Mode'. My account ID is ACCT-99482.
Agent (02:35): I completely understand your frustration David, let me look into that for you right now. Was this charged to your Visa ending in 4021?
Customer (02:40): Yes, exactly. I need this fixed today because my team has a presentation tomorrow morning."

[EXTRACTION SCHEMA & RULES]
1. `customer_name`: Full name of the customer.
2. `account_id`: The account ID provided (Format: ACCT-XXXXX).
3. `product_purchased`: Name of the software/product mentioned.
4. `amount_paid`: Numerical value only (e.g., 299.00).
5. `issue_category`: Must be classified as one of: [`BILLING`, `ACCESS_CONTROL`, `BUG`, `FEATURE_REQUEST`].
6. `customer_sentiment`: Must be classified as: [`HIGHLY_FRUSTRATED`, `NEUTRAL`, `SATISFIED`].
7. `urgency_level`: Must be classified as: [`CRITICAL`, `MEDIUM`, `LOW`].

[CONSTRAINTS & EVALUATION]
- Output MUST be a single raw JSON object.
- DO NOT wrap the output in markdown code fences unless explicitly requested.
- DO NOT include conversational filler, notes, or intros (e.g., "Here is your JSON:").
- If any required field is missing from the transcript, set its value to `null`.
{
  "customer_name": "David Miller",
  "account_id": "ACCT-99482",
  "product_purchased": "Pro Cloud Analytics Subscription",
  "amount_paid": 299.00,
  "issue_category": "ACCESS_CONTROL",
  "customer_sentiment": "HIGHLY_FRUSTRATED",
  "urgency_level": "CRITICAL"
}
💡 Engineering Highlights
Zero-Hallucination Guardrails: Explicit null fallback rule prevents the model from fabricating missing account details.

Enum Constraining: Restricting issue_category and customer_sentiment to fixed arrays enables seamless database indexing.

Software Integration Ready: This prompt can be directly integrated into Python (json.loads()) or JavaScript (JSON.parse()) backend pipelines without syntax errors.
