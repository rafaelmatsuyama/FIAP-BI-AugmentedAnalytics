# Enterprise Augmented Analytics & Cognitive Decision Intelligence Blueprint
> Reference Architecture, Autonomous Agentic Workflows, and Hands-on Labs for Next-Generation Business Intelligence.

[![FIAP MBA](https://img.shields.io/badge/FIAP-MBA%20BI%20%26%20Strategic%20Insights-ED145B?logo=fiap&logoColor=white)](https://www.fiap.com.br/mba/mba-em-business-intelligence-e-strategic-insights/)
![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![Google Gemini](https://img.shields.io/badge/LLM-Gemini%203.8%20Flash-orange?logo=google)
![Multi-Agent Orchestration](https://img.shields.io/badge/Orchestration-CrewAI%20%7C%20Dify-blueviolet)
![Reactive UI](https://img.shields.io/badge/Frontend-Streamlit-FF4B4B?logo=streamlit)
![Code License](https://img.shields.io/badge/Code-Apache%202.0-green.svg)
![Content License](https://img.shields.io/badge/Content-CC%20BY--NC--SA%204.0-orange.svg)

![Enterprise Augmented Analytics & Cognitive Decision Intelligence Reference Architecture](assets/architecture.jpg)

---

## 🎯 Executive Overview & The Paradigm Shift

Enterprise business intelligence stands at an inflection point. For the past two decades, organizations invested billions into descriptive data warehousing and visual analytics suites. However, modern corporate leadership is now reckoning with the **Reactive Dashboard Obsolescence Paradox**:

```
Traditional Descriptive BI (Lagging)             Augmented Cognitive Decisioning (Leading)
┌──────────────────────────────────────┐         ┌──────────────────────────────────────────────┐
│  • Siloed SQL & Static KPI Cards     │         │  • Dynamic In-Context Semantic Grounding     │
│  • Human Cognitive Overload          │  ───►   │  • Autonomous Auto-EDA & Anomaly Mining      │
│  • Post-Mortem Diagnostic Latency    │         │  • Multi-Agent Chain-of-Thought Deliberation │
│  • Zero Prescriptive Action Capacity │         │  • Contract-Driven Enactment (JSON / APIs)   │
└──────────────────────────────────────┘         └──────────────────────────────────────────────┘
```

Static dashboards have become visual graveyards of historical telemetry. Business stakeholders are burdened with manually filtering cross-tabular views to answer *what happened*, while the critical executive questions—*why did it happen*, *what will happen next*, and *which operational levers should be pulled immediately*—remain unanswered.

This repository serves as the official enterprise reference blueprint and laboratory suite for the **[MBA em Business Intelligence e Strategic Insights](https://www.fiap.com.br/mba/mba-em-business-intelligence-e-strategic-insights/)** at **FIAP**. It bridges modern cognitive technologies (**Google Gemini Flash**, **Multi-Agent Orchestration**, and **Contract-Driven UI**) to engineer proactive, self-driving decision systems.

### The Four Architectural Pillars:

1. **Semantic Context Grounding & In-Context Inference:** Bypassing brittle natural-language-to-SQL (NL2SQL) pipelines through direct multimodal grounding. By ingesting raw operational and behavioral schemas directly into LLM context windows with zero loss of semantic nuances, analysts extract deep diagnostic signals without writing repetitive extraction queries.
2. **White-Box Automated Feature Discovery & Diagnostics:** Eliminating the black-box opacity of traditional AutoML. Utilizing LLMs as autonomous code-generating agents capable of executing statistical profiling, outlier isolation, feature synthesis, and machine learning pipelines in open Python environments (`pandas`, `scikit-learn`, `altair`).
3. **Autonomous Multi-Agent Deliberation & Chain-of-Thought (CoT):** Decomposing complex strategic decisions (e.g., M&A synergy, churn mitigation, risk underwriting) across specialized agentic squads. Roles including Lead Strategic Analyst, Financial Modeler, Customer Success Auditor, and Compliance Officer collaborate asynchronously to deliberate, challenge assumptions, and synthesize consensus.
4. **Contract-Driven Enactment & Reactive Interfaces:** Bridging probabilistic AI generation with deterministic production downstream. Leveraging structured JSON Schemas, strict Pydantic parsing, and reactive interfaces (`Streamlit`) to trigger real-world business workflows, CRM webhooks, and executive-ready decision cockpits.

---

## 🏗️ End-to-End Architectural Flow

![End-to-End Decision Intelligence Lifecycle Flow](assets/architectural_flow.jpg)

### Lifecycle Architecture Matrix

| Stage | Architectural Phase | Domain Responsibility & Core Actions | Standards & Technology Stack | Production Guarantees & Deliverables |
| :---: | :--- | :--- | :--- | :--- |
| **0** | **Operational & Telemetry Sources** | Aggregation of raw enterprise telemetry, tabular survey CSVs, and transactional ledgers. | Pandas, CSV, Enterprise DBs, Data Lakes | Immutable raw source datasets (`PesquisaClientes.csv`, `50_Startups.csv`) |
| **1** | **In-Context Semantic Grounding** (Labs 01 & 03) | Contextual schema injection, persona calibration, and multi-dataset cross-referencing without brittle NL2SQL pipelines. | Google AI Studio, Google Gemini Flash, System Instructions | Grounded semantic extraction, zero-hallucination diagnostic memos |
| **2** | **Statistical Mining & Auto-EDA** (Labs 02, 04 & 05) | Programmatic exploration, automated outlier surfacing, white-box feature synthesis, and machine learning modeling. | Python 3.10+, Pandas Native Agent, Scikit-Learn (Random Forest) | Interpretable White-Box code, SHAP values, portfolio allocation algorithms |
| **3** | **Multi-Agent Squad Deliberation** (Labs 06 & 07) | Collaborative decision synthesis across specialized agent personas (Strategic Lead, Financial Modeler, Risk Auditor). | CrewAI, LiteLLM, Dify.ai, Chain-of-Thought (CoT) | Multi-perspective strategic consensus, proactive retention protocols |
| **4** | **Contract Enforcement & Schemas** (Lab 08) | Constrained decoding and strict schema verification to guarantee deterministic JSON payloads for microservices. | JSON Schema, Pydantic, Constrained Decoding | Type-safe JSON payloads, automated API contract compliance |
| **5** | **Boardroom Enactment & Cockpits** (Labs 09 & 10) | Interactive reactive frontends, dynamic visualization, and human-in-the-loop operational trigger execution. | Streamlit, Plotly, Webhooks, Ngrok Tunneling | Deployable Executive Cockpit with autonomous trigger workflows |

---

## 🧪 Comprehensive 10-Module Enterprise Lab Suite

Each module is self-contained within its respective directory, featuring structured student walkthroughs (`Roteiro Aluno.md`), production datasets, and executable Jupyter Notebooks or Google AI Studio blueprints.

| Module | Lab Directory | Enterprise Challenge / Business Problem | Technical Architecture & Modern Stack | Key Executive Artifact & Delivery | Interactive Launch |
| :---: | :--- | :--- | :--- | :--- | :---: |
| **01** | [`lab01-cx-analytics/`](./lab01-cx-analytics/) | **Customer Experience (CX) Friction Isolation:** Mining unstructured customer sentiment and satisfaction drivers across multi-product portfolios. | Google AI Studio • Gemini Flash • Zero-Shot In-Context Learning | Executive CX Diagnosis Memo & Action Matrix | [Inspect Guide](./lab01-cx-analytics/Lab%2001%20-%20Roteiro%20Aluno.md) |
| **02** | [`lab02-financial-analytics/`](./lab02-financial-analytics/) | **Venture Capital Capital Allocation:** Evaluating operational burn rates, R&D intensity, and profitability across 50 tech startups. | Python 3.10+ • Pandas • Google GenAI SDK (Gemini Flash) | Algorithmic Investment Thesis & Portfolio Ranking | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/blob/main/lab02-financial-analytics/Lab%2002%20-%20Colab%20Notebook.ipynb) |
| **03** | [`lab03-ma-synergy/`](./lab03-ma-synergy/) | **Cross-Context M&A Due Diligence:** Harmonizing disparate customer survey telemetry with financial startup ledger data to identify acquisition targets. | Google AI Studio • Dual-Context Grounding • Top-P/Temp Calibration | M&A Investment Committee Acquisition Memorandum | [Inspect Guide](./lab03-ma-synergy/Lab%2003%20-%20Roteiro%20Aluno.md) |
| **04** | [`lab04-pandas-agent/`](./lab04-pandas-agent/) | **Autonomous Data Exploration:** Replacing rigid ad-hoc SQL inquiries with a self-executing Python code-writing analytics agent. | Python • Google Colab • Pandas Native Agent • Dynamic Code Exec | Interactive Natural-Language Ad-Hoc Analytics Engine | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/blob/main/lab04-pandas-agent/Lab%2004%20-%20Colab%20Notebook.ipynb) |
| **05** | [`lab05-automl-scikit/`](./lab05-automl-scikit/) | **White-Box Churn Predictor:** Automating feature transformation, model selection, and SHAP-based feature importance explainability. | Python • Scikit-Learn • Random Forest • White-Box Profiling | Interpretable Churn Prediction Model & ROC-AUC Report | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/blob/main/lab05-automl-scikit/Lab%2005%20-%20Colab%20Notebook.ipynb) |
| **06** | [`lab06-dify-workflow/`](./lab06-dify-workflow/) | **Low-Code Enterprise Orchestration:** Constructing resilient multi-step analytical pipelines with human-in-the-loop validation checkpoints. | Dify.ai Enterprise • Visual Flow Orchestration • Tool Calling | Production-Grade Visual Analytical Pipeline | [Inspect Guide](./lab06-dify-workflow/Lab%2006%20-%20Roteiro%20Aluno.md) |
| **07** | [`lab07-crewai-squad/`](./lab07-crewai-squad/) | **Autonomous Operation Churn Zero:** Multi-agent autonomous squad orchestrating retention campaigns, discount modeling, and outreach drafting. | CrewAI • LiteLLM • Role-Based Multi-Agent Collaboration | Synthesized Retention Protocol & Task Delegation Flow | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/blob/main/lab07-crewai-squad/Lab%2007%20-%20Colab%20Notebook.ipynb) |
| **08** | [`lab08-json-schema/`](./lab08-json-schema/) | **Deterministic Output Contracts:** Forcing LLMs to comply with strict, production-ready JSON schemas for microservice integration. | Google AI Studio • Constrained Decoding • JSON Schema | Validated Pydantic/JSON Microservice Interface Spec | [Inspect Guide](./lab08-json-schema/Lab%2008%20-%20Roteiro%20Aluno.md) |
| **09** | [`lab09-agentic-dashboard/`](./lab09-agentic-dashboard/) | **Reactive Executive Cockpit:** Building interactive, agent-driven analytical applications that generate insights dynamically on user input. | Streamlit • PyNgrok • Gemini Flash • Plotly Dynamic Charts | Deployable Interactive AI-Powered Executive Dashboard | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/blob/main/lab09-agentic-dashboard/Lab%2009%20-%20Dashboard%20de%20Analytics.ipynb) |
| **10** | [`lab10-integracao-final/`](./lab10-integracao-final/) | **End-to-End Enterprise Solution Capstone:** Unifying ingestion, predictive modeling, multi-agent evaluation, and a reactive executive frontend. | Full Stack: Streamlit • Scikit-Learn • CrewAI • JSON Schema | Enterprise-Grade Decision Intelligence System | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/blob/main/lab10-integracao-final/Lab%2010%20-%20Integração%20Final%20e%20Dashboard%20Estruturado.ipynb)<br>[Inspect Guide](./lab10-integracao-final/Lab%2010%20-%20Roteiro%20Aluno.md) |

---

## 🚀 Operational Execution Tracks

### Track A: Zero-Setup Cloud Execution (Recommended)
All notebook-centric modules (Labs 02, 04, 05, 07, 09, 10) are optimized for **Google Colaboratory**:
1. Click the corresponding **"Open in Colab"** badge in the matrix above.
2. Store your API Key securely in Colab Secrets:
   ```python
   from google.colab import userdata
   api_key = userdata.get('GEMINI_API_KEY')
   ```
3. For semantic and prompt-centric labs (Labs 01, 03, 08), launch directly via [Google AI Studio](https://aistudio.google.com/) using the provided system personas.

### Track B: Local Enterprise Production Setup
For teams running local development workflows or self-hosting dashboards:

```bash
# 1. Clone the repository
git clone https://github.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics.git
cd FIAP-BI-AugmentedAnalytics

# 2. Initialize a dedicated virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\Activate.ps1

# 3. Install core dependencies
pip install --upgrade pip
pip install -r requirements.txt

# 4. Export runtime credentials
export GEMINI_API_KEY="your-gemini-api-key" # On Windows PowerShell: $env:GEMINI_API_KEY="your-gemini-api-key"
```

---

## ⚙️ Key Architectural Patterns & Production Hardening

This reference implementation incorporates critical engineering lessons learned across enterprise deployments and executive MBA cohorts:

* **Deterministic Contracts via Regex Extraction (`re.search`):** Rather than fragile string stripping or raw `json.loads` that breaks when LLMs produce conversational preambles or markdown code fences, analytical pipelines use strict regex extraction (`r'\{.*\}'`) with `re.DOTALL`, guaranteeing type-safe ingestion into downstream services.
* **Controlled Context Sampling & Rate-Limit Shielding:** In high-density enterprise datasets (`PesquisaClientes.csv`, `adult.csv`), transmitting entire transactional tables triggers 429 quota exhaustion and LLM attention dilution. Agents enforce bounded sampling (`df.head(15)`), transmitting high-variance structural snapshots for diagnostic inference while optimizing token consumption.
* **Reactive UI Session State Hydration (`st.session_state`):** Bridging asynchronous multi-agent deliberation with reactive frontends (`Streamlit`) introduces state loss on UI reruns. Pipelines hydrate `st.session_state` with immutable parsed data, preserving strategic metrics, warning cards, and raw JSON payloads across user clicks and parameter updates.
* **White-Box AutoML & Transparent Model Explainability:** Replacing opaque black-box AutoML platforms with open-source Python pipelines (`scikit-learn`, `pandas`). Code-generating agents synthesize Random Forest classification pipelines, computing native Feature Importance to deliver explainable boardroom metrics directly linked to business levers (e.g. Churn mitigation).
* **Anti-Sycophancy System Instructions & Sampling Calibration:** Corporate LLMs exhibit strong conversational agreeableness (sycophancy), validating flawed executive premises. Augmented Analytics workflows enforce adversarial auditor personas calibrated with constrained temperature (0.2) and Top-P controls, ensuring uncompromised analytical rigor in boardroom decisioning.

---

## 🛡️ Governance, Security & API Best Practices

- **Standard Model Architecture:** All laboratories standardize on **Google Gemini 3.8 Flash** (`gemini-3.8-flash`), optimized for ultra-low latency, native structured output compliance, and high throughput.
- **Credential Sanitization:** API keys must **never** be hardcoded or committed to version control. Always retrieve keys via environment variables (`GEMINI_API_KEY`, `GOOGLE_API_KEY`) or secret managers.
- **Academic Guardrails:** Instructor master solutions and answer keys (`*Gabarito*`, `*Solution*`) are strictly protected via enterprise `.gitignore` policies to ensure pedagogical rigor and independent student learning.

---

## 🎓 Academic Validation & Field Hardening

This framework, curriculum, and interactive lab suite were architected, refined, and battle-tested by **Rafael Matsuyama** as part of the **Augmented Analytics & AI-Driven Insights** curriculum for the **Executive MBA in Business Intelligence & Strategic Insights at FIAP** (São Paulo, Brazil).

Over 150+ senior analytics professionals, data managers, solutions architects, and tech leads across multiple enterprise cohorts (including 7BIR, 92BI, 93BI, and 9BIR) have executed, stress-tested, and validated these autonomous agentic workflows against real-world business decision scenarios.

---

## 🏷️ Release Cadence & Versioning Policy

This repository follows a strict **Calendar Versioning with Cycle and Patch semantics (CalVer)** (`vYYYY.CYCLE.PATCH`) to balance enterprise technical stability with annual executive curriculum delivery:

```
  v2026 . 1 . 0
    │     │   │
    │     │   └── PATCH: Hotfixes, dependency bumps & documentation errata
    │     └────── CYCLE: Major content iterations or new lab modules
    └──────────── YEAR:  Annual technology stack baseline & MBA curriculum edition
```

* **Deterministic Reproducibility:** Each tagged release provides an immutable snapshot where all laboratory notebooks, scripts, prompts, and Streamlit dashboards are verified to execute deterministically without runtime drift.
* **Academic Cohort Pinning:** Students from specific cohort years or enterprise teams standardizing on past editions can pin their environment directly to the official release tag:
  ```bash
  git checkout tags/v2026.1.0
  ```
* All milestone releases and automated release notes are available on the official [GitHub Releases](https://github.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/releases) page.

---

## 👨‍💻 Author & Leadership

**Rafael Matsuyama**  
*Solutions Architect & AI/Data Engineering Specialist*  
Engineering graduate from **Escola Politécnica da Universidade de São Paulo (Poli-USP)** with over two decades of experience across Cloud Architecture, Distributed Systems, Mission-Critical Platforms, and Academic Leadership.

* 🌐 **LinkedIn:** [linkedin.com/in/rafaelmatsuyama](https://www.linkedin.com/in/rafaelmatsuyama)
* 🐙 **GitHub:** [github.com/rafaelmatsuyama](https://github.com/rafaelmatsuyama)

---

## 📜 Dual Licensing Model

This repository employs a **dual-licensing structure** to encourage enterprise adoption of the codebase while protecting educational materials from unauthorized commercial resale:

* 💻 **Code, Scripts & Notebooks (`*.ipynb`, `*.py`, `*.yaml`, `*.json`):** Licensed under the **[Apache 2.0 License](LICENSE)**. Permissive and free to be used, adapted, and integrated into enterprise production environments.
* 📚 **Educational Content & Manuals (`*.md` lab guides, prompts, diagrams):** Licensed under **[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/)**. Free for personal learning, study, and educational reference; commercial reproduction, selling, or course packaging by third parties is strictly prohibited.
