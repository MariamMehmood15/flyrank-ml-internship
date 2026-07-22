# **FL-01: Prompt Iteration Log – SEO Audit & Data Leakage Forensics**

## **1. Task Definition & Audit Context**

**Target Task:** Technical SEO Audit & Machine Learning Data-Leakage Forensics

**Workflow Goal:** Analyze an incoming SEO performance summary log to identify valid performance drivers versus spurious correlations/data leakage, and generate actionable engineering recommendations for content budget allocation.

---

# **2. Iteration Ladder (6 Total Runs)**

## 🏁 Version 0: Naive Baseline

### **Technique Applied**

**None (Naive One-Liner)**

### **Prompt**

```plaintext
Audit this SEO data report and tell me what content changes to make.
```

### **Output Received (Claude 3.5 Sonnet)**

> "It looks like the SEO data report didn't come through — the uploads folder is empty on my end. Could you re-upload the file (or paste the data directly)? Once I can see it, I'll dig into the metrics and flag concrete content changes."

### **Iteration Note**

**Output Observation:** The model hit an immediate operational block. Because the prompt provided no inline context or schema, the model stopped execution and requested an uploaded file, failing to generate any strategic or analytical insights.

---

## 📈 Version 1: Role Assignment

### **Technique Applied**

**Role Assignment**

### **Prompt**

```plaintext
You are a Principal Technical SEO Strategist and Machine Learning Data Auditor specializing in search telemetry diagnostics.

Audit this SEO data report and tell me what content changes to make.
```

### **Output Received (Claude 3.5 Sonnet)**

> "I don't see an SEO data report attached — the uploads folder is empty. Could you upload the file (CSV, spreadsheet, PDF, or paste the data directly)? Once I have it, I'll dig into things like:
>
> • Query-to-page mismatches (impressions high, CTR/rankings low)
> • Cannibalization patterns across similar pages
> • Pages losing rank/traffic vs. gaining, and what might explain the split
> • Content gaps suggested by query clusters with no matching page
>
> Just drop the file in and I'll get started."

### **Iteration Note**

**Output Observation:** Assigning the persona completely transformed the vocabulary. Even though it still asked for a file upload, it stopped using basic conversational filler and proactively listed technical ML/SEO auditing areas (e.g., query-to-page mismatches, cannibalization patterns, and query clustering).

---

## 📈 Version 2: Context and Motivation

### **Technique Applied**

**Context & Motivation**

### **Prompt**

```plaintext
You are a Principal Technical SEO Strategist and Machine Learning Data Auditor specializing in search telemetry diagnostics.

We are evaluating a 30,000-row search telemetry dataset where stagnant pages average 2,909 words and growing pages average 2,848 words. Our corporate goal is to prevent misallocating rewrite budgets on unproven length heuristics.

Audit this SEO data report findings and tell me what content changes to make.
```

### **Output Received (Claude 3.5 Sonnet)**

> "Here's the audit — and the headline finding is that this dataset, as described, cannot support a length-based rewrite decision.
>
> **The core statistical problem:**
>
> 2,909 vs. 2,848 words is a ~2% gap. On its own, that's not evidence of anything... Word count distributions are typically skewed by a handful of very long pillar pages or thin doorway pages...
>
> Bottom line: this data shows stagnant and growing pages have essentially the same word count. If anything, that's evidence against word count as a driver — it's not 'growing pages are shorter, so cut content,' and it's not 'stagnant pages are shorter, so add content.' The near-parity is the finding...
>
> **What to actually check instead:**
>
> Redirect the audit toward variables with real mechanistic ties to ranking movement:
>
> * Search intent alignment
> * Content freshness
> * SERP feature volatility
> * Internal linking / cannibalization..."

### **Iteration Note**

**Output Observation:** Providing inline data findings (~2,909 vs. ~2,848 words) and operational business goals unblocked the model entirely. It stopped asking for an uploaded file and directly critiqued the industry word-count myth, highlighting that a 2% gap represents statistical parity/noise rather than a causal ranking signal.

---

## 📈 Version 3: Few-Shot Examples

### **Technique Applied**

**Few-Shot Examples**

### **Prompt**

```plaintext
You are a Principal Technical SEO Strategist and Machine Learning Data Auditor specializing in search telemetry diagnostics.

We are evaluating a 30,000-row search telemetry dataset where stagnant pages average 2,909 words and growing pages average 2,848 words. Our corporate goal is to prevent misallocating rewrite budgets on unproven length heuristics.

Here are examples of how to analyze audit findings:

Example 1:
- Finding: Pages under 500 words have lower median impressions.
- Analysis: Correlation, not causation. Thin content may lack search intent match, but blindly increasing length creates budget waste.
- Decision: Do not rewrite. Perform intent gap analysis.

Example 2:
- Finding: Target label `is_growing` correlates 0.92 with `impressions_90d`.
- Analysis: Data Leakage. `impressions_90d` includes post-treatment telemetry, meaning it memorizes the target instead of predicting growth.
- Decision: Reject feature from model training.

Audit this SEO data report and tell me what content changes to make.
```

### **Output Received (Claude 3.5 Sonnet)**

> **Audit Finding**
>
> **Finding:** Stagnant pages average 2,909 words; growing pages average 2,848 words (a 61-word / ~2% difference across a 30,000-row sample).
>
> **Analysis:** This is not a meaningful signal — it's noise dressed up as a finding.
>
> * Effect size is negligible.
> * No variance/significance reported.
> * Word count is a proxy, not a mechanism.
>
> **Decision:**
>
> Do not rewrite based on word count. Reject word-count as a standalone feature/heuristic for budget allocation.
>
> **Recommended next steps instead:**
>
> * Run a significance test on word-count distributions.
> * Segment by query intent category.
> * Watch for leakage the same way as Example 2.

### **Iteration Note**

**Output Observation:** Injected few-shot examples forced the model to adopt a strict structural framework. The output transitioned from narrative paragraphs into an explicit, standardized **Finding → Analysis → Decision** triadic breakdown.

---

## 📈 Version 4: Output Structure

### **Technique Applied**

**Output Structure**

### **Prompt**

```plaintext
You are a Principal Technical SEO Strategist and Machine Learning Data Auditor specializing in search telemetry diagnostics.

We are evaluating a 30,000-row search telemetry dataset where stagnant pages average 2,909 words and growing pages average 2,848 words. Our corporate goal is to prevent misallocating rewrite budgets on unproven length heuristics.

Here are examples of how to analyze audit findings:

Example 1:
- Finding: Pages under 500 words have lower median impressions.
- Analysis: Correlation, not causation. Thin content may lack search intent match, but blindly increasing length creates budget waste.
- Decision: Do not rewrite. Perform intent gap analysis.

Example 2:
- Finding: Target label `is_growing` correlates 0.92 with `impressions_90d`.
- Analysis: Data Leakage. `impressions_90d` includes post-treatment telemetry, meaning it memorizes the target instead of predicting growth.
- Decision: Reject feature from model training.

Format Output:
Deliver your response as a Markdown Technical Spec using this structure:

## 1. Executive Summary & Risk Assessment
## 2. Data Leakage & Feature Validity Findings
## 3. Content Resource Allocation Matrix (Table: Segment | Findings | Leakage Risk | Approved Action)
## 4. Pipeline Recommendations
```

### **Output Received (Claude 3.5 Sonnet)**

> **1. Executive Summary & Risk Assessment**
>
> The headline finding — stagnant pages average 2,909 words vs. growing pages averaging 2,848 words — is a 61-word delta (~2.1%) on a base of nearly 3,000 words...
>
> **Risk Assessment:** High risk of stakeholder misinterpretation...
>
> **Verdict:** Word count, as currently measured, shows no meaningful discriminative power between growing and stagnant segments. Do not authorize rewrite budget on this variable alone.
>
> **Data Leakage & Feature Validity Findings**
>
> [Table mapping Effect Size, Directionality, Confounding Variables, and Temporal Alignment]
>
> **Decision:** Reject `word_count` (in isolation) as a standalone modeling feature or business trigger. Flag for temporal-alignment audit before any reuse.
>
> **Content Resource Allocation Matrix**
>
> | Segment                          | Findings                                                | Leakage Risk                                | Approved Action                                               |
> | -------------------------------- | ------------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------- |
> | Stagnant pages (avg 2,909 words) | Marginally longer (+2.1%); no evidence length is causal | Low direct; moderate reverse-causality risk | Hold. No rewrite based on length. Route to intent/SERP audit. |
> | Growing pages (avg 2,848 words)  | Marginally shorter; growth not explained by brevity     | Same as above                               | Hold. Do not use as justification for cutting content.        |
> | Full 30,000-row corpus           | Word count distributions likely overlapping heavily     | N/A (aggregate)                             | Require variance/distribution report before decision          |
>
> **Pipeline Recommendations...**

### **Iteration Note**

**Output Observation:** Enforcing strict section templates eliminated free-form summary text and generated an executive-ready technical specification, including a populated markdown table with concrete resource allocation decisions.

---

## 📈 Version 5: Step Decomposition (Chain-of-Thought)

### **Technique Applied**

**Step Decomposition**

### **Prompt**

```plaintext
You are a Principal Technical SEO Strategist and Machine Learning Data Auditor specializing in search telemetry diagnostics.

We are evaluating a 30,000-row search telemetry dataset where stagnant pages average 2,909 words and growing pages average 2,848 words. Our corporate goal is to prevent misallocating rewrite budgets on unproven length heuristics.

Here are examples of how to analyze audit findings:

Example 1:
- Finding: Pages under 500 words have lower median impressions.
- Analysis: Correlation, not causation. Thin content may lack search intent match, but blindly increasing length creates budget waste.
- Decision: Do not rewrite. Perform intent gap analysis.

Example 2:
- Finding: Target label `is_growing` correlates 0.92 with `impressions_90d`.
- Analysis: Data Leakage. `impressions_90d` includes post-treatment telemetry, meaning it memorizes the target instead of predicting growth.
- Decision: Reject feature from model training.

Execution Steps:
Reason step-by-step inside <thinking> tags before generating the report:

1. Analyze the dataset findings for statistical validity and potential data leakage.
2. Challenge any implicit assumptions regarding content length or traffic metrics.
3. Verify every recommendation directly mitigates budget waste.

Format Output:
Deliver your response as a Markdown Technical Spec starting directly with:

## 1. Executive Summary & Risk Assessment
## 2. Data Leakage & Feature Validity Findings
## 3. Content Resource Allocation Matrix (Table: Segment | Findings | Leakage Risk | Approved Action)
## 4. Pipeline Recommendations
```

### **Output Received (Claude 3.5 Sonnet)**

```plaintext
<thinking>
Statistical validity: The word-count gap between stagnant (2,909) and growing (2,848) pages is only ~61 words, roughly a 2% difference on a base of ~2,900 words. Given a 30,000-row dataset, this is very likely within noise/variance for a metric with high standard deviation...

Implicit assumptions to challenge:
- "Longer content performs better."
- Word count is a causal lever.
- Averages are the right statistic.
- Growth/stagnation labels are exogenous.

Leakage check:
Length isn't inherently post-treatment, but if word count was measured after the growth window closed, editors historically adding words to winning pages creates reverse causation/reinvestment bias...

Budget waste mitigation:
Reject length as a segmentation feature... recommend actual diagnostic steps before budget approval.
</thinking>
```

The final report then included:

* **1. Executive Summary & Risk Assessment**
* **2. Data Leakage & Feature Validity Findings**
* **3. Content Resource Allocation Matrix**
* **4. Pipeline Recommendations**

### **Iteration Note**

**Output Observation:** Adding explicit `<thinking>` tags caused the model to conduct deep statistical checks before drafting the final text. It identified subtle edge risks—such as editorial reinvestment bias (reverse causality where editors add words to pages that are already growing)—and corrected its own reasoning before rendering the final markdown spec.

---

# **3. Cross-Model Comparison: Claude vs. ChatGPT**

Using the exact **Version 5 Prompt**, the prompt was executed in both **Claude 3.5 Sonnet** and **ChatGPT (GPT-4o).**

| **Dimension**            | **Claude 3.5 Sonnet**                                                                                                                                          | **ChatGPT (GPT-4o)**                                                                                                                                                |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Tone**                 | Clinical, highly technical, and engineering-focused. Reads like a principal ML auditor's forensic report.                                                      | Professional and authoritative, but retains a slightly more consultative, editorial tone.                                                                           |
| **Accuracy & Nuance**    | Deeply identified subtle ML data-leakage risks, specifically pointing out editorial reinvestment bias (reverse causality from post-growth edits).              | Correctly flagged data leakage and statistical insignificance, but focused more on standard non-parametric tests (Mann-Whitney U) and generic ML importance (SHAP). |
| **Formatting Structure** | Adhered perfectly to execution instructions: generated explicit `<thinking>` tags first and started the main output directly with **## 1. Executive Summary**. | Generated a structured spec, but transformed the text metrics to medians rather than means and omitted the explicit `<thinking>` block tag rendering.               |
| **Failure Points**       | Highly rigid; strictly limited its analysis to the provided schema and refused to speculate on unmentioned external variables.                                 | Mild boundary shift: modified the prompt's underlying statistical premise (pivoting reported "means" to "medians") without explicit user authorization.             |

---

# **4. Final Parameterized Reusable Template**

```plaintext
You are a Principal Technical SEO Strategist and Machine Learning Auditor specializing in search telemetry diagnostics and data-leakage identification.

### 🎯 Objective
Audit a technical SEO dataset report and evaluate candidate content update strategies to prevent misallocating corporate content budgets on spurious data correlations or leaked features.

### 📋 Task Context & Constraints
* Dataset Summary: [INSERT DATASET DESCRIPTION / ROW COUNT / COLUMNS]
* Business Goal: Allocate content engineering resources only to high-confidence growth targets while eliminating budget waste caused by unproven heuristics (e.g., word count rules).
* Quality Criteria: Rigorously inspect features for target leakage, lookahead bias, or overlapping feature distributions before approving rewrites.

### 📝 Decision Framework Examples

Example 1:
- Finding: Feature [X] correlates with target [Y].
- Analysis: Target leakage identified because Feature [X] captures post-treatment telemetry.
- Decision: Exclude Feature [X] from predictive model.

Example 2:
- Finding: Candidate metric [A] shows overlapping medians across target classes.
- Analysis: Metric lacks predictive power; observed variance is statistical noise.
- Decision: Freeze budget allocation for this metric.

### ⚙️ Execution Instructions

Before generating the final specification, reason step-by-step inside `<thinking>` tags:

1. Identify all potential data leakage risks in the provided schema/findings.
2. Evaluate feature validity and reject unproven heuristics.
3. Verify that all approved actions directly protect technical or financial resources.

### 📦 Required Output Format

Deliver your response strictly as a Markdown Spec starting with the first header (no conversational intro/outro):

## 1. Executive Summary & Operational Risk
## 2. Data Leakage & Feature Forensic Audit
## 3. Content Resource Allocation Matrix (Table columns: Candidate Segment | Findings | Leakage Risk | Approved Action)
## 4. Pipeline & Architectural Recommendations
```
