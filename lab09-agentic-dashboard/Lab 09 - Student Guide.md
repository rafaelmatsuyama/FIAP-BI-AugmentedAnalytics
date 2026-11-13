# Lab 09 - Agentic Analytics Dashboard with Streamlit UI & Ngrok

* **Program / Course:** MBA in Business Intelligence & Analytics — Augmented Analytics & AI-Driven Insights  
* **Environment:** Google Colab (Cloud Runtime) + Ngrok Tunneling  
* **Language / Stack:** Python 3.10+, Streamlit, CrewAI, Gemini 3.8 Flash (`gemini-3.8-flash`), PyNgrok  
* **Estimated Duration:** 25 to 30 minutes  

---

## 🎯 Lab Objectives

The objective of this laboratory is to transform AI analytical pipelines into interactive, reactive applications accessible to business decision-makers. Students build a comprehensive executive web application in pure Python (Streamlit) that ingests corporate datasets, triggers autonomous agent squads, and renders actionable *Insight-as-a-Service* reports.

Upon completing this laboratory, you will be able to:
1. Develop an executive web interface (Agentic UI) in pure Python without writing HTML, CSS, or JavaScript.
2. Build interactive forms with file selectors (`file_uploader`), masked credential inputs, and reactive execution triggers.
3. Expose ephemeral Google Colab local runtimes to the public internet securely using Ngrok tunneling.
4. Connect visual front-end presentation layers to multi-agent orchestration backends in real time.
5. Diagnose and resolve front-end lifecycle and session retention challenges (*Session State*) common in data-intensive applications.

---

## 📋 Prerequisites & Materials

1. **Google Colab:** Active session at [colab.new](https://colab.new).
2. **Free Ngrok Account:** Account at [ngrok.com](https://ngrok.com) with *Authtoken* copied from the dashboard.
3. **Google AI Studio API Key:** Generated at [aistudio.google.com](https://aistudio.google.com).
4. **Demonstration Dataset:** `PesquisaClientes.csv` (auto-downloaded via the self-healing routine in the notebook).
5. **Starter Notebook:** [`Lab 09 - Colab Notebook.ipynb`](./Lab%2009%20-%20Colab%20Notebook.ipynb).

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Install Dependencies & Self-Healing Ingestion
Open the notebook in Google Colab and run the installation and auto-download cell:

```python
!pip install crewai streamlit pyngrok pandas -q
import urllib.request, os
if not os.path.exists('PesquisaClientes.csv'):
    urllib.request.urlretrieve('https://raw.githubusercontent.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/main/lab01-cx-analytics/Lab%2001%20-%20PesquisaClientes.csv', 'PesquisaClientes.csv')
    print('✅ Demo dataset downloaded successfully! / Dataset de demonstração baixado com sucesso!')
```

### Step 2: Compile the Streamlit Web Application (`app.py`)
Execute the cell compiling the front-end application to disk using `%%writefile` (including the sidebar language selector and automatic fallback to `PesquisaClientes.csv` when no file is uploaded):

```python
%%writefile app.py
import streamlit as st
import pandas as pd
import os
from crewai import Agent, Task, Crew, Process

# 1. Page Configuration & Executive Branding
st.set_page_config(page_title="BI Agentic Dashboard", layout="wide")

st.title("🤖 Agentic Analytics Dashboard")
st.subheader("Transforming Telemetry into Executive Decisions with AI Squads")

# 2. Control Sidebar
with st.sidebar:
    st.header("⚙️ Control Panel / Configurações")
    api_key = st.text_input("🔑 Gemini API Key:", type="password")
    model_choice = st.selectbox("🧠 Model / Modelo:", ["gemini/gemini-3.8-flash"])
    output_lang = st.selectbox("🌐 Output Language / Idioma:", ["Portuguese (Brazil)", "English"])
    st.divider()
    st.info("MBA in BI & Analytics - FIAP")

# 3. Data Ingestion (Upload or Self-Healing Default)
uploaded_file = st.file_uploader("📂 Upload CSV dataset (or leave empty to use default PesquisaClientes.csv):", type="csv")
data_source = uploaded_file if uploaded_file is not None else ('PesquisaClientes.csv' if os.path.exists('PesquisaClientes.csv') else None)

if data_source and api_key:
    df = pd.read_csv(data_source)
    if uploaded_file is None:
        st.caption("ℹ️ Using pre-loaded Colab dataset: `PesquisaClientes.csv` / Usando base padrão do Colab.")
    
    col1, col2, col3 = st.columns(3)
    col1.metric("📊 Total Records / Registros", len(df))
    col2.metric("📋 Total Columns / Colunas", len(df.columns))
    col3.metric("🔢 Numeric Columns / Numéricas", len(df.select_dtypes(include='number').columns))
    
    st.write("### 🔍 Telemetry Sample / Amostra dos Dados Carregados", df.head(5))
    
    if st.button("🚀 Launch Agent Squad / Disparar Squad de Agentes"):
        os.environ["GEMINI_API_KEY"] = api_key
        os.environ["GOOGLE_API_KEY"] = api_key
        
        with st.spinner("🤖 Agents analyzing telemetry... / Agentes analisando dados... Aguarde."):
            analyst = Agent(
                role='Senior Strategic Data Analyst',
                goal=f'Identify critical business patterns in the provided telemetry (respond in {output_lang})',
                backstory='Expert in statistical modeling, customer behavior, and executive boardroom synthesis.',
                llm=model_choice
            )
            
            task = Task(
                description=f'Analyze the variables {df.columns.tolist()} in the following sample and synthesize an executive brief in {output_lang}: {df.head(10).to_markdown()}',
                expected_output=f'A Markdown executive report in {output_lang} containing 3 actionable strategic insights.',
                agent=analyst
            )
            
            crew = Crew(agents=[analyst], tasks=[task], process=Process.sequential)
            raw_result = crew.kickoff()
            st.session_state["relatorio_analise"] = str(raw_result)

    if "relatorio_analise" in st.session_state:
        st.success("✅ Analysis Completed! / Análise Concluída!")
        st.markdown("--- ")
        st.markdown(st.session_state["relatorio_analise"])
        
        st.download_button(
            label="📥 Download Executive Dossier (.md) / Baixar Dossiê Executivo",
            data=st.session_state["relatorio_analise"],
            file_name="dossie_executivo_bi.md",
            mime="text/markdown"
        )

elif not api_key:
    st.warning("⚠️ Please enter your Gemini API Key in the sidebar to begin. / Insira sua API Key no menu lateral para começar.")
```

### Step 3: Activate Ngrok Tunnel & Start Streamlit Server
Configure your personal authtoken and launch the service:

```python
NGROK_TOKEN = "PASTE_YOUR_NGROK_TOKEN_HERE"

from pyngrok import ngrok
import os

if NGROK_TOKEN:
    ngrok.kill()
    ngrok.set_auth_token(NGROK_TOKEN)
    public_url = ngrok.connect(8501).public_url
    print(f"🔗 LIVE DASHBOARD URL / DASHBOARD ATIVO EM: {public_url}")
    !streamlit run app.py --server.port 8501
```

### Step 4: Operating the Web Dashboard
1. Click the public URL generated by Ngrok (`https://xxxx-xx.ngrok-free.app`).
2. If the Ngrok security confirmation interstitial appears, click **"Visit Site"**.
3. In the left sidebar, paste your **Gemini API Key**.
4. Upload `PesquisaClientes.csv`.
5. Click **"🚀 Trigger AI Agent Analysis"** and inspect the executive briefing.

---

## 🧪 Validation & Acceptance Criteria

- [ ] Public URL opens the Streamlit front-end without gateway timeout errors.
- [ ] API Key input masks characters (`type="password"`).
- [ ] Uploading the CSV and clicking the trigger renders the Markdown analysis report.
- [ ] Session state retains report outputs without wiping the view on clicks.

---

## 🧹 Cleanup

To terminate processes and destroy the tunnel:
```python
from pyngrok import ngrok
ngrok.kill()
processo.terminate()
print("🛑 Web server and Ngrok tunnel cleanly terminated.")
```

---

## 💡 Strategic Extension Challenges

* **Executive Metric Cards:** Add `st.metric(label="Sample Records", value=len(df))` cards directly above the report.
* **Export Utilities:** Add options to download the analysis as structured PDF or HTML summaries.

