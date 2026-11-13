# Lab 03 - M&A Synergy Challenge: Mergers, Acquisitions & Cross-Dataset Intelligence

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Google AI Studio ([aistudio.google.com](https://aistudio.google.com/))  
* **Language / Stack:** Google Gemini Flash (`gemini-3.8-flash`) / Cross-Dataset Reasoning / Strategic Analytics  
* **Estimated Duration:** 25 to 30 minutes  

---

## 🎯 Lab Objectives

The objective of this laboratory is to evaluate the reasoning capacity of Google Gemini Flash when performing semantic cross-referencing across heterogeneous, un-normalized enterprise datasets (without pre-existing foreign keys or relational joins), simulating an executive Mergers & Acquisitions (M&A) Investment Committee.

Upon completing this laboratory, you will be able to:
1. Execute simultaneous multi-dataset inference within a single multimodal context window.
2. Isolate customer friction gaps in a CX dataset and map strategic capabilities in a portfolio of investable startups.
3. Formulate synergy hypotheses linking customer dissatisfaction with capital allocation in R&D and Marketing.
4. Author an executive M&A Investment Memorandum supported by cross-dataset empirical evidence.
5. Stress-test corporate investment theses using dialectical "Devil's Advocate" prompting backed by **Native Reasoning**.

---

## 📋 Prerequisites & Materials

* Active account on [Google AI Studio](https://aistudio.google.com/).
* Laboratory Datasets (located in the previous module folders):
  * [`../lab01-cx-analytics/Lab 01 - PesquisaClientes.csv`](../lab01-cx-analytics/Lab%2001%20-%20PesquisaClientes.csv): Customer satisfaction and demographic survey.
  * [`../lab02-financial-analytics/Lab 02 - 50_Startups.csv`](../lab02-financial-analytics/Lab%2002%20-%2050_Startups.csv): Startup accounting data, operating expenditures, and net profits.

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Environment Setup in Google AI Studio
1. Navigate to [https://aistudio.google.com/](https://aistudio.google.com/).
2. In the navigation sidebar, click **"Create New"** -> **"Chat Prompt"**.
3. Select the model: **Gemini 3.8 Flash** (`gemini-3.8-flash`).
4. In the right sidebar (**Run settings**), configure the reasoning engine:
   * **Thinking level:** Select **`High`**.
   * *Architectural Rationale:* Reconciling disparate datasets lacking relational keys demands multi-hop deductive reasoning. Setting `Thinking: High` provides the model with internal thinking tokens to deliberate on market complementarity before formulating the acquisition thesis.

### Step 2: Configure M&A Committee System Instructions
In the **"System instructions"** panel (right sidebar), enter the governance directive:

> *"You are a Senior Mergers & Acquisitions (M&A) and Corporate Venture Capital Advisory Partner. Your mandate is to reconcile two disparate corporate domains: current customer demographic frictions and candidate startup operating capabilities. Your strategic recommendation must demonstrate product synergy, customer churn reduction, and return on invested capital (ROIC)."*

### Step 3: Multi-Dataset Ingestion
In the chat prompt input bar, use the upload icon (**`+`**) to attach both files simultaneously:
1. `Lab 01 - PesquisaClientes.csv` (from the `lab01-cx-analytics` directory)
2. `Lab 02 - 50_Startups.csv` (from the `lab02-financial-analytics` directory)

### Step 4: The Strategic Acquisition Challenge
Submit the structured analytical prompt:

```text
Based on the two provided datasets, perform the following acquisition diligence:

1. Demand Diagnostic (PesquisaClientes): Identify the demographic cluster displaying the highest resistance and friction toward our core offering.
2. Operational Capability Mapping (50_Startups): Audit the candidate startups and determine which company (or R&D vs. Marketing cost profile) possesses the optimal product capability to resolve this specific customer friction.
3. Executive M&A Memorandum: Draft an Investment Memorandum recommending the prime acquisition target, proving operational and financial complementarity across both datasets.
```

### Step 5: Stress-Testing the Thesis (The "Devil's Advocate")
In corporate boardrooms, consensus theses frequently obscure post-merger integration risks. Utilizing the *Native Reasoning* architecture, provoke the engine to ruthlessly audit its own conclusions.

Submit the dialectical stress-test prompt:

```text
Adopt the persona of a conservative, skeptical Investment Committee Member ('Devil's Advocate'):

1. Detail the top 3 operational and cultural integration risks that could cause this acquisition to fail within the first 12 months post-closing.
2. Evaluate whether an alternative startup in the portfolio—featuring higher R&D intensity or lower valuation multiples—offers an asymmetric, superior risk-adjusted return over a 3-year horizon.
```

*Expected Output:* Gemini Flash activates its internal reasoning trace to deconstruct the initial thesis, assessing real-world integration friction, revenue cannibalization, and downside risks.

---

## 🧪 Validation & Acceptance Criteria

To confirm mastery of this module:
- [ ] Both tabular files were successfully ingested into the AI Studio context window under `Gemini 3.8 Flash`.
- [ ] The committee operated with `Thinking level: High`, surfacing multi-hop deductive traces.
- [ ] The M&A memorandum explicitly grounded conclusions on empirical data points from both CX and Financial sources.
- [ ] The Devil's Advocate stress-test yielded a balanced evaluation of operational trade-offs and portfolio alternatives.

---

## 💡 Strategic Extension Challenges

* **Deterministic Valuation via Code Execution:** Enable **Code execution** under Tools and prompt: *"Calculate the exact operating margin and Marketing-to-Revenue ratio for each startup to validate the target's valuation multiple."* Observe the model writing and executing Python routines in real time.
* **Earn-Out Deal Structuring:** Ask the model to propose an M&A deal structure detailing upfront cash considerations vs. milestone-contingent earn-outs tied to customer retention KPIs.

