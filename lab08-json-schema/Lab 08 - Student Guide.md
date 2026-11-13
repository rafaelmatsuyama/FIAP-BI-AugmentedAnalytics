# Lab 08 - Refinement & JSON Schema (Google AI Studio)

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Google AI Studio (Web Console)  
* **Language / Stack:** Gemini 3.8 Flash (`gemini-3.8-flash`), Structured Outputs, JSON Schema, Enums  
* **Estimated Duration:** 20 to 25 minutes  

---

## 🎯 Lab Objectives

The objective of this laboratory is to enforce strict data contracts and deterministic output governance for Large Language Models in Google AI Studio. By eliminating conversational ambiguity, students force LLMs to deliver strictly validated JSON Schema payloads with primitive typing and controlled Enums.

Upon completing this laboratory, you will be able to:
1. Formulate executive *System Instructions* that eliminate subjective opinion and ground brand tone.
2. Activate and configure native *Structured Output* schemas within Google AI Studio.
3. Declare rigid data contracts using primitive types (`string`, `number`, `boolean`) and arrays.
4. Enforce categorical boundaries using Enumerations (*Enums*) to prevent vocabulary drift and hallucinated labels.
5. Stress-test schemas against ambiguous, qualitative prompts to verify JSON output invariance and production resilience.

---

## 📋 Prerequisites & Materials

1. **Google AI Studio:** Access to the official web console at [aistudio.google.com](https://aistudio.google.com).
2. **Google Account:** Standard authenticated session.
3. **Validation Telemetry:** Dataset sample from `PesquisaClientes.csv`.

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Console Access & Model Selection
1. Navigate to [aistudio.google.com](https://aistudio.google.com).
2. Click **"Create New Prompt"** $\rightarrow$ select **"Chat prompt"** (or **"Freeform prompt"**).
3. Select the official course model: **Gemini 3.8 Flash** (`gemini-3.8-flash`).

### Step 2: Formulating the System Directive ("Constitution")
In the **System Instructions** panel, define executive governance constraints:

```text
You are a Senior Strategic Advisory Partner for the FIAP MBA in Business Intelligence.
Your tone is strictly analytical, pragmatic, and focused on measurable ROI and quantitative evidence.

Non-Negotiable Directives:
1. Never begin responses with subjective conversational phrases such as "I think" or "In my opinion".
2. Every business assertion must cite empirical data points from the provided telemetry.
3. Each surfaced opportunity must include an estimated business impact categorized strictly as Low, Medium, or High.
4. Deliver strictly the declared data contract with zero conversational preamble, code block markdown backticks, or closing pleasantries.
```

### Step 3: Declaring the Data Contract (Structured Output)
1. In the right configuration panel, locate the **"Structured Output"** block (or **"Response Format"**).
2. Toggle the switch to **ON** and click **"Edit Schema"** (JSON Schema mode).
3. Declare the root object properties:
   * `titulo_insight`: Type `STRING` (Description: "Short executive summary title of the finding").
   * `metrica_chave`: Type `STRING` (Description: "Quantitative metric or critical indicator").
   * `impacto_estimado`: Type `STRING` with an **ENUM** constraint: `["Low", "Medium", "High"]`.
   * `recomendacao_estrategica`: Type `STRING` (Description: "Prescriptive business action recommended").
   * `acoes_imediatas`: Type `ARRAY` of `STRING` (Description: "Tactical execution milestones").

* **Why Enums are Vital:** Enforcing Enums prevents the model from generating synonyms like "Severe", "Moderate", or "Very High", guaranteeing that downstream microservices and front-end charts ingest records without parsing crashes.

### Step 4: Empirical Testing with Tabular Records
1. Open `PesquisaClientes.csv` and copy a sample of 5 to 10 rows.
2. Paste the snippet into the user message prompt bar:
   ```text
   Analyze the customer interaction records below and deliver the core operational diagnosis:
   [PASTE_CSV_ROWS_HERE]
   ```
3. Submit the inference request and inspect the response.
* **Expected Output:** The engine returns strictly a valid JSON object without leading or trailing markdown text.

### Step 5: Ambiguity Stress-Testing (Schema Invariance Audit)
1. Submit an intentionally unstructured, informal, and subjective inquiry:
   ```text
   I feel our customers are unhappy with our response times, what should we do about this?
   ```
2. Submit the query and audit the result.
3. Observe that despite the absence of structured tabular data, the engine is constrained by Structured Output to populate every required JSON attribute (`titulo_insight`, `metrica_chave`, `impacto_estimado`, `recomendacao_estrategica`), binding `impacto_estimado` strictly to a valid Enum entry.

---

## 🧪 Validation & Acceptance Criteria

- [ ] Delivered payload passes standard JSON parsers (`json.loads`) without pre-processing or regex workarounds.
- [ ] The `impacto_estimado` field strictly honors the authorized Enum entries (`Low`, `Medium`, or `High`).
- [ ] Output is devoid of conversational noise ("Sure, here is your JSON" or "Hope this helps").

---

## 🧹 Cleanup

1. Close the tab or click **Clear Chat** if not saving.
2. To retain for enterprise portfolio reference, click **Save** in the top bar and name it `FIAP-MBA-Contract-Gov`.

---

## 💡 Strategic Extension Challenges

* **Nested Object Schemas:** Expand the schema to include a child dictionary `kpis_afetados` containing `metric_name` (string) and `variance_delta` (number).
* **SDK Code Export:** Click **"Get Code"** in the top bar, select **Python**, and examine how the JSON Schema translates directly into parameters for the `google-genai` SDK.

