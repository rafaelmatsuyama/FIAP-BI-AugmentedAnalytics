# Lab 02 - The Programmable Analyst with Python and Gemini API

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Google Colab ([colab.research.google.com](https://colab.research.google.com/))  
* **Language / Stack:** Python 3.10+ / Google GenAI SDK (`google-genai`) / Pandas / Gemini Flash 3.8 (`gemini-3.8-flash`)  
* **Estimated Duration:** 25 to 30 minutes  

---

## 🎯 Lab Objectives

The objective of this laboratory is to automate large-scale financial portfolio analysis for Venture Capital (VC) allocations, integrating the Google Gemini API directly into Python runtimes to convert tabular accounting metrics into Boardroom-ready investment memoranda ("White-Box Analytics") programmatically.

Upon completing this laboratory, you will be able to:
1. Generate and configure secure API credentials in **Google AI Studio**.
2. Integrate the official `google-genai` SDK within a Jupyter Notebook on **Google Colab**.
3. Implement Markdown serialization (`to_markdown`) for efficient, token-optimized tabular ingestion into LLM context windows.
4. Program inference calls targeting **Gemini Flash 3.8** (`gemini-3.8-flash`) with constrained token budgets (`max_output_tokens=10000`) and temperature calibration.
5. Evaluate non-linear capital efficiency dynamics (R&D intensity vs. Administration burn vs. Net Profitability) across 50 tech startups.

---

## 📋 Prerequisites & Materials

* Active account on [Google Colab](https://colab.research.google.com/).
* API Key generated in [Google AI Studio](https://aistudio.google.com/) ("Get API key" menu).
* Laboratory Courseware:
  * [`Lab 02 - Colab Notebook.ipynb`](./Lab%2002%20-%20Colab%20Notebook.ipynb): Structured Python execution notebook with self-healing dataset ingestion.
  * [`Lab 02 - 50_Startups.csv`](./Lab%2002%20-%2050_Startups.csv): Corporate financial dataset containing R&D Spend, Administration, Marketing Spend, State, and Profit.

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Obtain Google AI Studio API Credentials
1. Navigate to [https://aistudio.google.com/](https://aistudio.google.com/).
2. In the left navigation bar, select **"Get API key"**.
3. Click **"Create API key"** (associate with a new or existing Cloud Project).
4. Copy the generated alphanumeric key and store it securely.

### Step 2: Launch Google Colab Notebook
1. Open [Google Colab](https://colab.research.google.com/).
2. In the notebook launcher modal, switch to the **"GitHub"** tab.
3. In the repository URL input, enter: `rafaelmatsuyama/FIAP-BI-AugmentedAnalytics`.
4. Select the target file: `lab02-financial-analytics/Lab 02 - Colab Notebook.ipynb`.

### Step 3: Dataset Ingestion (Self-Healing Runtime)
* The updated notebook includes a **self-healing download routine**: if `Lab 02 - 50_Startups.csv` is not present in `/content/`, it fetches it automatically from the official GitHub branch.
* Alternatively, upload [`Lab 02 - 50_Startups.csv`](./Lab%2002%20-%2050_Startups.csv) manually via the left sidebar (**Files** panel).

### Step 4: Configure Authentication, Language & Runtime Libraries
In Cell 1 of the notebook, configure your API credential and set `OUTPUT_LANGUAGE` to `'English'`:
```python
API_KEY = 'COLE_SUA_API_KEY_AQUI'
OUTPUT_LANGUAGE = 'English' # @param ['Portuguese (Brazil)', 'English']
client = genai.Client(api_key=API_KEY)
```
Execute the cell to install required dependencies (`google-genai`, `tabulate`) and initialize the authenticated GenAI client session.

### Step 5: Execute Automated Portfolio Evaluation (Gemini Flash 3.8)
Execute subsequent cells sequentially:
1. **Ingest & Inspect:** Pandas loads the tabular dataset, confirming 50 records.
2. **Tabular Serialization:** The `df.head(15).to_markdown()` routine transforms structured records into dense Markdown preserving column semantics.
3. **Agentic Invocation:** The model `gemini-3.8-flash` processes the executive VC prompt via `client.models.generate_content` with an extended output budget (`max_output_tokens=10000`).
4. **Boardroom Memorandum:** Inspect the synthesized investment thesis identifying the top startup candidate for Series A capital deployment.

---

## 🧪 Validation & Acceptance Criteria

- [ ] Notebook executed cleanly without authentication exceptions (`403 Forbidden` or invalid credentials).
- [ ] Ingested 50 startup records with R&D, administrative, and profit telemetry.
- [ ] Invocations target the official `gemini-3.8-flash` engine.
- [ ] Delivered an algorithmic investment ranking justifying the selected startup based on empirical R&D-to-Profit correlations.

---

## 💡 Strategic Extension Challenges

* **Automated Batch Ingestion:** Modify the script to chunk startups in batches of 10, iterating programmatically and concatenating outputs into a unified `portfolio_audit.md` dossier.
* **Risk Tolerance Tuning:** Increase the `temperature` parameter from `0.3` to `0.8` to evaluate whether the agent shifts recommendations toward higher-variance, moonshot startup profiles.

