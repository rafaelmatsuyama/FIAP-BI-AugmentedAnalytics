# Lab 06 - Visual Multi-Agent Workflow Orchestration with Dify.ai

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Dify.ai Cloud ([dify.ai](https://dify.ai/))  
* **Language / Stack:** No-Code / Visual Workflow / Google Gemini Flash 3.8 (`gemini-3.8-flash`) / Markdown  
* **Estimated Duration:** 25 to 30 minutes  

---

## 🎯 Lab Objectives

The objective of this laboratory is to design and operate a visual multi-agent workflow based on Directed Acyclic Graphs (DAGs) in Dify.ai. By coordinating specialized AI agents with complementary responsibilities (Market Intelligence Researcher → Chief Strategy Officer), organizations produce comprehensive competitive intelligence dossiers in record time ("Operation Flash Insight"), overcoming the cognitive fatigue and context dilution typical of monolithic chat interfaces.

Upon completing this laboratory, you will be able to:
1. Transition from monolithic chat prompts to visually orchestrated cognitive assembly lines.
2. Integrate the Google AI Studio provider in Dify.ai Cloud using the canonical **Gemini Flash 3.8** (`gemini-3.8-flash`) model.
3. Design and configure input triggers (*Start Node*), cognitive reasoning nodes (*LLM Nodes*), and delivery endpoints (*End Node*).
4. Parametrize semantic context passing and variable references (`{{#node_id.text#}}`) across sequential agent steps.
5. Generate an executive competitive intelligence memorandum featuring business model forensics, risk matrices, and 30-day counteroffensive roadmaps.

---

## 📋 Prerequisites & Materials

* Free account on [Dify.ai Cloud](https://dify.ai/) (authenticate via Google or GitHub).
* Active API Key generated in [Google AI Studio](https://aistudio.google.com/apikey).
* Modern Web Browser (Google Chrome, Microsoft Edge, or Firefox).
* Reference DSL Template: [`Sinalizador_Concorrencia_MBA.yml`](./Sinalizador_Concorrencia_MBA.yml) (available in this directory for optional instant inspection).
* Zero local installation required (100% managed cloud environment).

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Access Dify Cloud & Initialize Workflow
1. Navigate to [dify.ai](https://dify.ai/) and log in to the Cloud console.
2. On the dashboard, click **"Create from Blank"** (or use **"Import DSL File"** with `Sinalizador_Concorrencia_MBA.yml` for instant reference).
3. Select application type: **"Workflow"**.
4. Configure metadata:
   * **App Name:** `Sinalizador_Concorrencia_MBA`
   * **Description:** `Visual multi-agent workflow for competitive intelligence and strategic counteroffensive.`
5. Click **"Create"** to open the visual orchestration canvas.

### Step 2: Configure Gemini Model Provider
1. In the upper-right corner (profile menu), navigate to **Settings**.
2. Select the **Model Provider** tab.
3. Locate the **Google / Gemini** card and click **Setup** (or **Add Key**).
4. Paste your **Google AI Studio API Key**.
5. Save and verify that the green *Active/Configured* indicator appears.
6. Return to the Workflow canvas.

### Step 3: Configure the Trigger (Start Node)
1. Click the **Start** node on the canvas.
2. In the right panel under **User Input Field**, click **"+"** to add a parameter:
   * **Type:** `Short Text`
   * **Variable Name:** `nome_concorrente`
   * **Label:** `Competitor Name or Market Event`
   * **Mandatory:** Enabled

### Step 4: Add the Intelligence Researcher (LLM Node 1)
1. Drag or connect the output connector **"+"** of the *Start* node to an **LLM** block.
2. Rename the node: `Agente_Pesquisador`.
3. Configure parameters in the right panel:
   * **Model:** **Google / Gemini** → **`gemini-3.8-flash`**.
   * **System Prompt:**
     ```text
     You are a Senior Market Intelligence and Competitive Strategy Specialist.
     Your mission is to perform detailed business model forensics on the indicated competitor, isolating unique value propositions, revenue engines, and target audiences.
     ```
   * **User Prompt:** Click **"+ Add Message"** and input:
     ```text
     Analyze the business model and strategic positioning of: {{#start.nome_concorrente#}}

     Structure your diagnosis with:
     1. Core Value Proposition
     2. Monetization Architecture
     3. Primary Strategic Strengths & Asymmetric Threats
     ```
   * **Thinking Level:** Set to **`Low`** (or **`Medium`**) for rapid, low-latency market synthesis.

### Step 5: Add the Chief Strategy Officer (LLM Node 2)
1. Connect the output of `Agente_Pesquisador` to a second **LLM** block.
2. Rename the node: `Agente_Estrategia`.
3. Configure parameters:
   * **Model:** **Google / Gemini** → **`gemini-3.8-flash`**.
   * **System Prompt:**
     ```text
     You are the Chief Strategy Officer (CSO) of our enterprise.
     You are pragmatic, highly analytical, and focused on margin defense and client retention.
     Ingesting market intelligence, your role is to formulate an executive counteroffensive action plan for the Board of Directors.
     ```
   * **User Prompt:** Click **"+ Add Message"** and pass the upstream agent output:
     ```text
     Based on the competitive intelligence dossier prepared by our research team below:
     
     ---
     {{#Agente_Pesquisador.text#}}
     ---
     
     Draft a BOARDROOM DEFENSE MEMORANDUM detailing:
     1. Top 3 Immediate Vulnerabilities for our enterprise customer base
     2. 30-Day Counteroffensive Action Roadmap (Tactical Milestones)
     3. Primary Differentiation Lever we must communicate immediately
     
     Format the output in executive Markdown using tables and punchy single-line bullets.
     ```
   * **Thinking Level:** Set to **`High`** to unlock deep reasoning capacity for risk isolation and strategic roadmapping.

### Step 6: Connect Delivery Endpoint (End Node)
1. Connect the output of `Agente_Estrategia` to the **End** block.
2. In the *End* node panel, configure the output mapping:
   * **Variable:** `report`
   * **Value:** Bind to `{{#Agente_Estrategia.text#}}`

### Step 7: Execution & Verification
1. Click **Run** (or **Preview**) in the upper right toolbar.
2. In the input dialog, submit a test scenario:
   * **nome_concorrente:** `OpenAI (Deploying low-cost native multimodal agent APIs for enterprise fintechs)`
3. Observe the sequential graph execution: the Start node triggers the Researcher agent, which streams context directly into the Strategy Director, delivering the finalized memorandum in the End node.

---

## 🧪 Validation & Acceptance Criteria

- [ ] Both LLM nodes standardize on **`gemini-3.8-flash`**.
- [ ] The `nome_concorrente` input passes deterministically into the Researcher agent.
- [ ] Context passing from Node 1 to Node 2 functions via `{{#Agente_Pesquisador.text#}}`.
- [ ] The End node outputs an executive Markdown memorandum detailing risk vectors and 30-day tactical countermeasures.
- [ ] Total end-to-end multi-agent graph execution terminates within 30 seconds.

---

## 🧹 Cleanup

1. If operating on shared or institutional hardware, navigate to **Settings** → **Model Provider** in Dify to remove or deactivate your API key.
2. Workflows remain preserved in your cloud workspace for production reuse.

---

## 💡 Strategic Extension Challenges

* **Live Web Grounding:** Insert a **Tool Node** (Tavily or Google Search) upstream of the Researcher to inject fresh news telemetry published within the past 24 hours.
* **Structured Risk Parsing (Code Node):** Add a lightweight Python code node between the LLM steps to extract numerical risk scores into a JSON schema before board delivery.
* **REST API Deployment:** In the **Publish** tab of Dify, inspect the auto-generated REST API endpoint (`/v1/workflows/run`) for headless consumption in Streamlit dashboards.

