# 📘 The RTCE Framework Guide

## Overview
The **RTCE Framework** (Role, Task, Context, Evaluation) is a structured approach to prompt engineering designed to produce predictable, high-accuracy outputs from Large Language Models (LLMs). By explicitly defining each component, we eliminate ambiguity and reduce hallucinations.

---

## 🧱 The 4 Pillars of RTCE

### 1. Role (Who is the AI?)
Defining a persona sets the behavioral boundaries, tone, and domain expertise of the model.
* *Example:* "Act as a Senior Cyber Security Auditor with 10+ years of enterprise experience."

### 2. Task (What must the AI do?)
A clear, action-oriented instruction defining the core objective.
* *Example:* "Analyze the provided system logs and identify potential security vulnerabilities."

### 3. Context (What background information does the AI need?)
Essential details, constraints, target audience, or raw input data needed to fulfill the task accurately.
* *Example:* "Focus only on SSH login attempts during off-peak hours (11 PM - 5 AM). Ignore successful logins."

### 4. Evaluation (How should the output be formatted/tested?)
Strict output constraints and quality criteria to ensure deterministic results.
* *Example:* "Format your findings as a Markdown table with columns: `Timestamp`, `IP Address`, `Risk Level`, and `Recommended Action`. Do not include conversational filler."

---

## 🔬 Practical Demonstration: Naive Prompt vs. RTCE Prompt

### ❌ Bad (Naive Prompt)
> "Check these server logs for bad stuff and tell me if anything looks wrong."

**Why it fails:** 
* No role to set technical depth.
* Vague definition of "bad stuff".
* Unpredictable, unstructured response format.

---

### ✅ Good (RTCE Structured Prompt)
```text
[ROLE]
You are a Lead Cloud Security Engineer specializing in incident response.

[TASK]
Review the provided system log snippet and highlight suspicious activity.

[CONTEXT]
- Target environment: Linux Web Server
- Expected traffic: Standard HTTP/HTTPS requests
- Raw Logs:
  2024-05-12 02:14:11 - IP 192.168.1.50 - GET /admin/login.php 200
  2024-05-12 02:14:12 - IP 192.168.1.50 - POST /admin/login.php 401
  2024-05-12 02:14:13 - IP 192.168.1.50 - POST /admin/login.php 401
  2024-05-12 02:14:14 - IP 192.168.1.50 - POST /admin/login.php 200

[EVALUATION]
1. Output MUST be formatted as a bulleted security alert summary.
2. Flag brute-force attempt evidence explicitly.
3. Keep the entire response under 100 words.
4. Exclude pleasantries (e.g., "Sure, here is your analysis").
