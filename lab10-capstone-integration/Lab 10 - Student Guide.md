# Lab 10 - The Grand Integration: Structured Agentic Dashboard (UI + JSON Schema)

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Google Colab (Cloud Runtime) + Ngrok Tunneling  
* **Language / Stack:** Python 3.10+, Streamlit, CrewAI, Gemini 3.8 Flash (`gemini-3.8-flash`), PyNgrok  
* **Estimated Duration:** 25 to 30 minutes  

---

## 🎯 Lab Objectives

The objective of this capstone laboratory is to execute the "Final Technical Surgery" of the curriculum: connecting the cognitive intelligence core with deterministic structured JSON Schema output (Lab 08) directly into the reactive web presentation layer in Streamlit (Lab 09). Students deliver a full-fledged enterprise product of **Insight-as-a-Service**. The system ingests raw tabular datasets, triggers an autonomous AI Squad running **Gemini 3.8 Flash**, performs resilient regex parsing, and displays boardroom-ready metric cards with complete session state retention.

Upon completing this laboratory, you will be able to:
1. Integrate an end-to-end cognitive AI analytics product (Ingestion $\rightarrow$ Multi-Agent Deliberation $\rightarrow$ JSON Schema $\rightarrow$ Front-End UI).
2. Configure the CrewAI squad to standardize on **Gemini 3.8 Flash** (`gemini-3.8-flash`) via LiteLLM routing.
3. Implement fault-tolerant parsing by extracting JSON blocks via regular expressions (`re.search`), immune to residual markdown fences.
4. Maintain application state across user clicks and parameter updates using `st.session_state`.
5. Present C-Suite analytical intelligence partitioned across visual metric cards (`st.metric`), actionable warnings (`st.warning`), and technical IT audit trees (`st.json`).

---

## 📋 Prerequisites & Materials

1. **Google Colab:** Active session at [colab.new](https://colab.new).
2. **Free Ngrok Account:** Authtoken configured at [ngrok.com](https://ngrok.com).
3. **Google AI Studio API Key:** Generated at [aistudio.google.com](https://aistudio.google.com).
4. **Validation Dataset:** `PesquisaClientes.csv` (auto-downloaded via the self-healing routine in the notebook).
5. **Starter Notebook:** [`Lab 10 - Colab Notebook.ipynb`](./Lab%2010%20-%20Colab%20Notebook.ipynb).

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Install Dependencies & Self-Healing Ingestion in Google Colab
Open the notebook in Google Colab and execute Cell 1 to install the multi-agent, front-end, and tunneling packages and auto-download `PesquisaClientes.csv`:

```python
!pip install crewai streamlit pyngrok pandas -q
import urllib.request, os
if not os.path.exists('PesquisaClientes.csv'):
    urllib.request.urlretrieve('https://raw.githubusercontent.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/main/lab01-cx-analytics/Lab%2001%20-%20PesquisaClientes.csv', 'PesquisaClientes.csv')
    print('✅ Demo dataset downloaded successfully! / Dataset de demonstração baixado com sucesso!')
```

### Step 2: Compile the Integrated Dashboard (`app.py`)
Execute Cell 2 of the notebook, which writes the unified web application using `%%writefile` (including the sidebar language selector and automatic fallback to `PesquisaClientes.csv`):

```python
%%writefile app.py
import streamlit as st
import pandas as pd
import os
import json
import re
from crewai import Agent, Task, Crew, Process

# 1. Page Configuration & Executive Branding
st.set_page_config(page_title="BI Pro Dashboard", layout="wide")
st.title("🏢 Agentic Analytics: Insight-as-a-Service")

with st.sidebar:
    st.header("⚙️ Control Panel / Painel de Controle")
    api_key = st.text_input("🔑 Gemini API Key:", type="password")
    output_lang = st.selectbox("🌐 Output Language / Idioma:", ["Portuguese (Brazil)", "English"])
    st.divider()
    st.info("MBA BI & Analytics - FIAP")

# 2. Data Ingestion & Squad Trigger (Upload or Self-Healing Default)
uploaded_file = st.file_uploader("📂 Upload CSV dataset (or leave empty to use default PesquisaClientes.csv):", type="csv")
data_source = uploaded_file if uploaded_file is not None else ('PesquisaClientes.csv' if os.path.exists('PesquisaClientes.csv') else None)

if data_source and api_key:
    df = pd.read_csv(data_source)
    if uploaded_file is None:
        st.caption("ℹ️ Using pre-loaded Colab dataset: `PesquisaClientes.csv` / Usando base padrão do Colab.")
    st.dataframe(df.head(10), use_container_width=True)
    
    if st.button("🚀 Generate Structured Analysis / Gerar Análise Estruturada"):
        os.environ["GEMINI_API_KEY"] = api_key
        os.environ["GOOGLE_API_KEY"] = api_key
        
        with st.spinner("🧠 Squad processing telemetry via JSON Schema... / Processando dados... Aguarde."):
            instrucoes_sistemicas = (
                f"You are a Senior Strategy Consultant from FIAP's MBA in BI & Analytics. "
                f"Your tone is pragmatic, ROI-driven, and strictly grounded in the provided data. "
                f"Write all string values in {output_lang}."
            )
            
            analyst = Agent(
                role="FIAP Strategic BI Consultant",
                goal=f"Synthesize actionable, schema-compliant strategic insights in {output_lang}",
                backstory=instrucoes_sistemicas,
                llm="gemini/gemini-3.8-flash"
            )
            
            # Controlled sampling to prevent token limits and rate limit exhaustion
            dataset_contexto = df.head(15).to_csv(index=False)
            
            task = Task(
                description=f"""Analyze the telemetry sample below and synthesize a structured strategic insight (values in {output_lang}):

{dataset_contexto}

Return MANDATORILY a JSON object matching this exact schema:
{{
  "titulo": "string",
  "metrica_critica": "string",
  "recomendacao": "string",
  "impacto_estimado": "Baixo" | "Médio" | "Alto"
}}

Return ONLY raw JSON, with no extra markdown or commentary.""",
                expected_output="A valid JSON object with keys: titulo, metrica_critica, recomendacao, impacto_estimado.",
                agent=analyst
            )
            
            crew = Crew(agents=[analyst], tasks=[task], process=Process.sequential)
            raw_result = crew.kickoff()
            
            # 3. Resilient Parsing via Regex and State Hydration
            try:
                raw_text = str(raw_result.raw if hasattr(raw_result, "raw") else raw_result)
                json_match = re.search(r'\{.*\}', raw_text, re.DOTALL)
                
                if json_match:
                    st.session_state["analise_data"] = json.loads(json_match.group(0))
                    st.session_state["raw_output"] = raw_text
                else:
                    raise ValueError("JSON object not found in agent output / JSON não identificado.")
                    
            except Exception as e:
                st.error(f"❌ Error parsing JSON / Erro ao processar JSON: {e}")
                st.write("**Raw Output / Retorno Bruto:**", raw_result)

    # 4. Boardroom Executive Presentation
    if "analise_data" in st.session_state:
        data = st.session_state["analise_data"]
        st.success("✅ Decision Intelligence Generated! / Inteligência Gerada com Sucesso!")
        st.divider()
        
        col1, col2 = st.columns(2)
        with col1:
            st.metric(label="Insight Title / Título do Insight", value=data.get("titulo", "N/A"))
            st.write(f"**Critical Metric / Métrica Crítica:** {data.get('metrica_critica', 'N/A')}")
        
        with col2:
            st.metric(label="Estimated Impact / Impacto Estimado", value=data.get("impacto_estimado", "Médio"))
            st.warning(f"**Recommendation / Recomendação:** {data.get('recomendacao', 'N/A')}")
        
        st.divider()
        st.write("### 🛠️ Raw JSON Contract (IT Audit / Auditoria IT)")
        st.json(data)

elif not api_key:
    st.warning("⚠️ Enter your Gemini API Key in the sidebar to activate the Agent Squad. / Insira sua Gemini API Key no menu lateral.")
```

### Step 3: Launch Ngrok Tunnel & Start Streamlit Server
In Cell 3, paste your Ngrok authtoken and run:

```python
NGROK_TOKEN = "PASTE_YOUR_NGROK_TOKEN_HERE"

from pyngrok import ngrok
import os

if NGROK_TOKEN:
    ngrok.kill()
    ngrok.set_auth_token(NGROK_TOKEN)
    public_url = ngrok.connect(8501).public_url
    print(f"🔗 EXECUTIVE DASHBOARD ACTIVE AT: {public_url}")
    print("--- ")
    !streamlit run app.py --server.port 8501
else:
    print("❌ ERROR: You must provide NGROK_TOKEN to generate public access.")
```

### Step 4: Operating the Executive Dashboard
1. Open the public URL provided by Ngrok.
2. Enter your **Gemini API Key** in the sidebar.
3. Upload `PesquisaClientes.csv`.
4. Click **"🚀 Generate Structured Analysis"** and watch the squad synthesize the findings.
5. Verify the simultaneous rendering of boardroom cards (`st.metric`) and technical telemetry (`st.json`).

---

## 🧪 Validation & Acceptance Criteria

- [ ] Web application boots on port 8501 and establishes an active public Ngrok tunnel.
- [ ] Agent squad targets the standardized **`gemini/gemini-3.8-flash`** model.
- [ ] Regular expression parsing isolates the JSON payload enclosed within `{...}` without parsing crashes.
- [ ] Session state preserves outputs across clicks without view resets.
- [ ] The raw JSON reflects the 4 declared data contract keys.

---

## 🧹 Cleanup

To close the tunnel and free the socket:
```python
from pyngrok import ngrok
ngrok.kill()
print("🛑 Ngrok tunnel and processes cleanly terminated.")
```

---

## 💡 Strategic Extension Challenges

* **Contract Expansion:** Add mitigation milestones (`"acoes_prioritarias": ["string"]`) to the schema and render them as a bulleted checklist (`st.markdown`) below the recommendation.
* **Telemetry Export:** Use `st.download_button` to export the sanitized JSON payload as a `.json` file for downstream data pipeline consumption.

