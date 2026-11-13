# Lab 05 - Native White-Box AutoML with Scikit-Learn and Gemini API

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Google Colab ([colab.research.google.com](https://colab.research.google.com/))  
* **Language / Stack:** Python 3.10+ / Google GenAI SDK (`google-genai`) / Scikit-Learn / Pandas / Gemini Flash 3.8 (`gemini-3.8-flash`)  
* **Estimated Duration:** 25 to 30 minutes  

---

## 🎯 Lab Objectives

The objective of this laboratory is to construct and operate an autonomous Machine Learning agent. This agent interprets high-level business goals, designs, and executes an end-to-end predictive pipeline (*White-Box AutoML*) in Scikit-Learn. By doing so, organizations bypass black-box vendor lock-in and generate explainable AI (*XAI*) metrics to mitigate customer churn.

Upon completing this laboratory, you will be able to:
1. Configure a cognitive AutoML agent in Google Colab utilizing **Gemini Flash 3.8** (`gemini-3.8-flash`) via the official `google-genai` SDK.
2. Delegate Feature Engineering, categorical encoding, and stratified train/test partitioning (80/20) autonomously to the LLM.
3. Train, optimize, and evaluate classification pipelines based on Decision Tree ensembles (*Random Forest Classifier*).
4. Interpret Confusion Matrices from an executive risk perspective, weighing the business costs of False Positives against False Negatives.
5. Extract and communicate Feature Importance charts to substantiate C-Level retention interventions.

---

## 📋 Prerequisites & Materials

* Active account on [Google Colab](https://colab.research.google.com/).
* API Key generated in [Google AI Studio](https://aistudio.google.com/).
* Laboratory Courseware:
  * [`Lab 05 - Colab Notebook.ipynb`](./Lab%2005%20-%20Colab%20Notebook.ipynb): Structured execution notebook with self-healing data ingestion.
  * [`Lab 05 - EvasaoClientesClassificados.xlsx`](./Lab%2005%20-%20EvasaoClientesClassificados.xlsx): Insurance customer dataset containing policy details, coverage tiers, activity levels, and target churn status (`Perdido?`: 0 = Retained, 1 = Churned).

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Open Notebook in Google Colab
1. Navigate to [Google Colab](https://colab.research.google.com/).
2. Select the **"GitHub"** tab.
3. Enter the repository: `rafaelmatsuyama/FIAP-BI-AugmentedAnalytics`.
4. Open the notebook: `lab05-automl-scikit/Lab 05 - Colab Notebook.ipynb`.

### Step 2: Dataset Ingestion (Self-Healing Runtime)
* The updated notebook includes a **self-healing download routine**: if `Lab 05 - EvasaoClientesClassificados.xlsx` is not found locally, it retrieves it automatically from the official GitHub branch.
* Execute the package installation cell (`google-genai openpyxl tabulate`).

### Step 3: Configure Authentication Credentials
In the setup cell, provide your Google AI Studio API key:
```python
from google import genai
from google.genai import types

API_KEY = 'YOUR_API_KEY_HERE'
client = genai.Client(api_key=API_KEY)
```

### Step 4: Data Ingestion & Schema Inspection
Execute the loading cell to inspect the target variable `Perdido?` and associated insurance attributes:
```python
df = pd.read_excel('Lab 05 - EvasaoClientesClassificados.xlsx')
```

### Step 5: Autonomous White-Box AutoML Execution
Invoke `executar_automl_nativo()` with the strategic business question:
```python
executar_automl_nativo("Identify the top 3 features driving customer attrition and evaluate overall model precision.")
```
* **Mechanics:** **Gemini 3.8 Flash** ingests the schema and sample records, synthesizes a complete Scikit-Learn script (drops identifiers, encodes categoricals with `LabelEncoder`, splits 80/20, trains `RandomForestClassifier`), executes the code in memory, and outputs Accuracy, Classification Reports, and visual charts.

### Step 6: Executive Risk Interpretation (Confusion Matrix & XAI)
Inspect the generated model artifacts:
1. **Confusion Matrix Audit:**
   * *False Positive:* Predicting a loyal customer will churn (Risk: unnecessary retention discount expenditure).
   * *False Negative:* Predicting a churning customer will stay (Critical Risk: irreversible loss of recurring customer lifetime value).
2. **Feature Importance (XAI):**
   * Identify features at the top of the importance ranking (typically **Coverage Level** and **Age**).
   * Synthesize the executive takeaway: sub-par coverage tiers accelerate policy cancellations.

---

## 🧪 Validation & Acceptance Criteria

- [ ] Ingested `Lab 05 - EvasaoClientesClassificados.xlsx` successfully without Excel engine failures.
- [ ] Confirmed inference execution via `google-genai` SDK using `gemini-3.8-flash`.
- [ ] Verified that Scikit-Learn training executed dynamically in memory without syntax exceptions.
- [ ] Model achieved testing accuracy within the **78% to 88%** benchmark range.
- [ ] Rendered the *Feature Importance* plot identifying prime churn drivers.
- [ ] Documented the asymmetrical financial cost of False Negatives vs. False Positives.

---

## 🧹 Cleanup

Upon finishing:
1. Navigate to **Runtime** -> **Disconnect and delete runtime** to terminate execution memory and release Colab resources.

