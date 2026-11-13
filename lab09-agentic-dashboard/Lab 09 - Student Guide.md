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
5. **Starter Notebook:** `Lab 09 - Dashboard de Analytics.ipynb`.

---

## 🚀 Guided Step-by-Step Walkthrough

### Step 1: Install Front-End and Tunneling Dependencies
Open the notebook in Google Colab and run the installation cell:

```python
!pip install -q crewai streamlit pyngrok pandas tabulate
print("✅ Web packages and tunneling libraries successfully installed!")
```

### Step 2: Compile the Streamlit Web Application (`app.py`)
Execute the cell compiling the front-end application to disk using `%%writefile`:

```python
%%writefile app.py
import streamlit as st
import pandas as pd
import os
from crewai import Agent, Task, Crew, Process

# 1. Page Configuration & Executive Branding
st.set_page_config(page_title="BI Agentic Dashboard", layout="wide", page_icon="🤖")

st.title("🤖 Agentic Analytics Dashboard")
st.subheader("Insight-as-a-Service: Converting Data into Decisions with AI")

# 2. Control Sidebar
with st.sidebar:
    st.header("⚙️ Session Settings")
    api_key = st.text_input("Enter your Gemini API Key:", type="password")
    model_choice = st.selectbox("Select Model:", ["gemini/gemini-3.8-flash"])
    st.divider()
    st.caption("MBA in Business Intelligence & Analytics — FIAP")

# 3. Data Ingestion & Execution Trigger
uploaded_file = st.file_uploader("Upload business dataset (CSV):", type="csv")

if uploaded_file and api_key:
    df = pd.read_csv(uploaded_file)
    st.dataframe(df.head(10), use_container_width=True)
    
    if st.button("🚀 Trigger AI Agent Analysis"):
        os.environ["GEMINI_API_KEY"] = api_key
        os.environ["GOOGLE_API_KEY"] = api_key
        
        with st.spinner("🧠 AI Agent analyzing strategic patterns... Please wait."):
            analyst = Agent(
                role="Strategic Business Analyst",
                goal="Uncover high-impact revenue and customer satisfaction insights",
                backstory="Senior management consultant specialized in converting data tables into executive action roadmaps.",
                llm=model_choice
            )
            
            task = Task(
                description=f"Analyze the following dataset sample and generate an executive strategic report:\n\n{df.head(20).to_markdown()}",
                expected_output="An executive briefing in Markdown detailing key anomalies, revenue levers, and 3 actionable recommendations.",
                agent=analyst
            )
            
            crew = Crew(agents=[analyst], tasks=[task], process=Process.sequential)
            resultado = crew.kickoff()
            
            st.session_state["relatorio_analise"] = str(resultado)
            st.success("✅ Strategic Analysis Successfully Generated!")

# 4. Executive Report Display
if "relatorio_analise" in st.session_state:
    st.divider()
    st.markdown(st.session_state["relatorio_analise"])
    
    st.download_button(
        label="📥 Download Executive Report (.md)",
        data=st.session_state["relatorio_analise"],
        file_name="executive_bi_dossier.md",
        mime="text/markdown"
    )

elif not api_key:
    st.info("👉 Enter your Google AI Studio API Key in the sidebar to enable analytics.")
```

### Step 3: Activate Ngrok Tunnel & Start Streamlit Server
Configure your personal authtoken and launch the service:

```python
from pyngrok import ngrok
import subprocess
import time

NGROK_TOKEN = "PASTE_YOUR_NGROK_TOKEN_HERE"
ngrok.set_auth_token(NGROK_TOKEN)

# Launch Streamlit server in the background on port 8501
processo = subprocess.Popen(["streamlit", "run", "app.py", "--server.port", "8501", "--server.headless", "true"])
time.sleep(3)

# Open secure tunnel forwarding port 8501
public_url = ngrok.connect(8501)
print(f"🚀 Dashboard publicly reachable at: {public_url}")
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

