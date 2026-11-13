# Lab 07 - Operation Churn Zero: Autonomous Squads with CrewAI

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Google Colab (Cloud Runtime)  
* **Language / Stack:** Python 3.10+, CrewAI, Gemini 3.8 Flash (`gemini-3.8-flash`)  
* **Estimated Duration:** 25 to 30 minutes  

---

## 🎯 Lab Objectives

The objective of this laboratory is to construct and orchestrate an autonomous Multi-Agent AI Squad in pure Python using CrewAI. Moving beyond the limitations of generalist "jack-of-all-trades" prompts, students automate triage, strategic negotiation, and personalized C-Suite communication to reverse high-value customer churn in real time.

Upon completing this laboratory, you will be able to:
1. Initialize the CrewAI ecosystem connected to the Google Gemini API using secure environment variables.
2. Define specialized corporate agent personas bounded by explicit mandates (`Role`, `Goal`, and `Backstory`).
3. Construct sequential task pipelines with deterministic output contracts (`Expected Output`) and automated context propagation.
4. Audit the Multi-Agent Chain-of-Thought (CoT) trace through verbose execution telemetry.
5. Quantify corporate productivity gains (ROI) achieved by replacing manual churn desk operations with autonomous squads.

---

## 📋 Prerequisites & Materials

1. **Google Colab:** Active session at [colab.new](https://colab.new).
2. **Google AI Studio API Key:** Authentication key generated at [aistudio.google.com](https://aistudio.google.com).
3. **Operational Dataset:** `Lab 05 - EvasaoClientesClassificados.xlsx` (available with self-healing auto-download in the notebook).
4. **Starter Notebook:** `Lab 07 - Colab Notebook.ipynb`.

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Environment Preparation & Dependency Setup
In your Google Colab notebook, execute the initial setup cell:

```python
# Silent installation of CrewAI and formatting utilities
!pip install -q crewai tabulate openpyxl
print("✅ Multi-agent environment successfully provisioned!")
```

### Step 2: Configure Environment Variables & LLM Provider
Configure API credentials targeting the Google Gemini backend:

```python
import os
import pandas as pd
from crewai import Agent, Task, Crew, Process

API_KEY = "PASTE_YOUR_API_KEY_HERE"
os.environ["GOOGLE_API_KEY"] = API_KEY
os.environ["GEMINI_API_KEY"] = API_KEY

# Standardized model string in canonical LiteLLM/CrewAI format
llm_config = "gemini/gemini-3.8-flash"
print(f"✅ Cognitive Engine Configured: {llm_config}")
```

### Step 3: Churn Telemetry Ingestion (Self-Healing Runtime)
Ingest the classified customer attrition dataset:

```python
import os
import urllib.request

DATASET_NAME = 'Lab 05 - EvasaoClientesClassificados.xlsx'
RAW_URL = 'https://raw.githubusercontent.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/main/lab05-automl-scikit/Lab%2005%20-%20EvasaoClientesClassificados.xlsx'

if not os.path.exists(DATASET_NAME):
    print(f"📥 Downloading {DATASET_NAME} from official repository...")
    urllib.request.urlretrieve(RAW_URL, DATASET_NAME)

df = pd.read_excel(DATASET_NAME)
amostra_clientes = df.head(5).to_markdown()
print("✅ VIP Customer Churn Sample Ingested:")
print(amostra_clientes)
```

### Step 4: Assembling the Specialized Agent Squad
Initialize the three distinct agent personas with non-overlapping corporate responsibilities:

```python
investigador = Agent(
    role='Risk and Churn Data Detective (The Investigator)',
    goal='Uncover dissatisfaction patterns across customer records and isolate the single most critical churn case.',
    backstory='Senior Churn Analytics investigator with forensic attention to customer contract tenure and coverage deficits.',
    allow_delegation=False,
    llm=llm_config
)

arquiteto = Agent(
    role='Commercial Retention Strategist (The Architect)',
    goal='Formulate a customized, economically viable retention offer tailored to the critical customer pain points.',
    backstory='Master contract negotiator experienced in enterprise B2B policy restructuring and lifetime value optimization.',
    allow_delegation=False,
    llm=llm_config
)

comunicador = Agent(
    role='Executive Customer Success Writer (The Communicator)',
    goal='Draft an empathetic, high-touch outreach memorandum ready for immediate dispatch to the customer decision-maker.',
    backstory='Executive communication director adept at converting raw data and pricing concessions into genuine partnership value.',
    allow_delegation=False,
    llm=llm_config
)

print("✅ Agent Squad Initialized: Investigator, Architect, and Communicator deployed.")
```

### Step 5: Defining the Sequential Task Pipeline
Bind the deliverables of each agent into an orchestrated dependency chain:

```python
tarefa_analise = Task(
    description=f"Perform forensic analysis on the customer dataset below, identifying who exhibits the highest cancellation risk and why: {amostra_clientes}",
    expected_output="Technical briefing detailing target Customer ID, contract metrics, and root-cause churn triggers.",
    agent=investigador
)

tarefa_estrategia = Task(
    description="Based on the Investigator's forensic briefing, design a structured retention strategy detailing value add-ons, plan restructuring, or commercial concessions.",
    expected_output="Comprehensive retention proposal with economic justification and customer lifetime value safeguards.",
    agent=arquiteto
)

tarefa_email = Task(
    description="Using the approved retention strategy, compose the official executive outreach email for immediate dispatch. Tone must be empathetic, highly polished, and focused on strategic partnership.",
    expected_output="Complete email body formatted in Markdown with subject line and executive signature.",
    agent=comunicador
)

print("✅ Task execution chain registered.")
```

### Step 6: Triggering the Autonomous Crew
Instantiate the Crew in sequential execution mode:

```python
squad_retencao = Crew(
    agents=[investigador, arquiteto, comunicador],
    tasks=[tarefa_analise, tarefa_estrategia, tarefa_email],
    process=Process.sequential,
    verbose=True
)

print("\n🚀 SQUAD ENGAGED! Monitoring Multi-Agent Chain-of-Thought...\n")
resultado_final = await squad_retencao.kickoff_async()

print("\n" + "="*80)
print("📄 OPERATION CHURN ZERO EXECUTIVE REPORT")
print("="*80 + "\n")
print(resultado_final)
```

---

## 🧪 Validation & Acceptance Criteria

- [ ] Telemetry displays sequential multi-agent reasoning traces (`Thought: ...`, `Action: ...`) in console logs.
- [ ] Contextual handoff verified: the Architect's proposal explicitly addresses the root causes flagged by the Investigator.
- [ ] Outreach email synthesized by the Communicator is complete, polished, and free of unfilled placeholders (`[Client Name]`).
- [ ] Confirmed standard execution targeting **`gemini/gemini-3.8-flash`**.

---

## 🧹 Cleanup

To terminate memory allocations and shut down the Colab runtime:
1. Select **Runtime** -> **Interrupt execution** if tasks are ongoing.
2. Select **Runtime** -> **Restart session** to flush RAM and environment states.

---

## 💡 Strategic Extension Challenges

* **Hierarchical Process Orchestration:** Change `process=Process.sequential` to `process=Process.hierarchical` and configure `manager_llm=llm_config`. Observe how an autonomous manager agent coordinates task delegations dynamically.
* **Automated Dispatch Integration:** Add a code cell that persists the finalized outreach email into an external file `retention_proposal_vip.md` for automated CRM webhook consumption.

