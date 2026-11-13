# Lab 10 - A Grande Integração: Dashboard Agentic Estruturado (UI + JSON Schema)

* **Curso:** MBA em Business Intelligence & Analytics
* **Disciplina:** Augmented Analytics & AI-Driven Insights
* **Ambiente / Plataforma:** Google Colab (Cloud Runtime) + Túnel Ngrok
* **Linguagem / Stack:** Python 3.10+, Streamlit, CrewAI, Gemini 3.8 Flash (`gemini-3.8-flash`), PyNgrok
* **Duração Prevista:** 25 a 30 minutos

---

## 🎯 Objetivo do Lab
Capacitar o aluno a realizar a "Cirurgia Técnica Final" da disciplina: conectar o cérebro cognitivo refinado com saída estruturada via JSON Schema (Lab 08) à camada de apresentação interativa web em Streamlit (Lab 09), construindo um produto analítico completo de **Insight-as-a-Service**. O sistema recebe dados brutos tabulares, dispara uma Squad de agentes autônomos baseada no motor **Gemini 3.8 Flash**, executa parsing determinístico via regex resiliente e renderiza cards executivos de diretoria com retenção de sessão.

Ao concluir este laboratório, você será capaz de:
1. Integrar um pipeline analítico de IA de ponta a ponta (Ingestão $\rightarrow$ Agentes $\rightarrow$ JSON Schema $\rightarrow$ Front-end Web).
2. Forçar a Squad CrewAI a operar com o modelo oficial **Gemini 3.8 Flash** (`gemini-3.8-flash`) via LiteLLM.
3. Implementar parsing tolerante a falhas extraindo payloads JSON via expressões regulares (`re.search`), imune a marcadores Markdown residuais.
4. Preservar o estado da aplicação entre interações do usuário utilizando `st.session_state`.
5. Apresentar dados executivos divididos entre cards de métricas (`st.metric`), alertas de recomendação (`st.warning`) e painel de auditoria técnica (`st.json`).

---

## 📋 Pré-requisitos & Materiais
1. **Google Colab:** Sessão ativa em [colab.new](https://colab.new).
2. **Conta Gratuita Ngrok:** Authtoken configurado em [ngrok.com](https://ngrok.com).
3. **API Key do Google AI Studio:** Chave gerada em [aistudio.google.com](https://aistudio.google.com).
4. **Dataset de Teste:** Arquivo `PesquisaClientes.csv` (com auto-download *Self-Healing* integrado no notebook).
5. **Notebook do Laboratório:** Arquivo [`Lab 10 - Colab Notebook.ipynb`](./Lab%2010%20-%20Colab%20Notebook.ipynb).

---

## 🚀 Passo a Passo Guiado

### Passo 1: Instalação de Dependências e Carga Self-Healing no Colab
Abra o notebook no Google Colab e execute a Célula 1 para instalar os componentes de agentes, front-end, tunelamento e baixar automaticamente o dataset de demonstração:

```python
!pip install crewai streamlit pyngrok pandas -q
import urllib.request, os
if not os.path.exists('PesquisaClientes.csv'):
    urllib.request.urlretrieve('https://raw.githubusercontent.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/main/lab01-cx-analytics/Lab%2001%20-%20PesquisaClientes.csv', 'PesquisaClientes.csv')
    print('✅ Demo dataset downloaded successfully! / Dataset de demonstração baixado com sucesso!')
```
* **O que acontece:** O ambiente prepara o runtime com `streamlit` para interface, `crewai` para orquestração de agentes, `pyngrok` para exposição pública da porta 8501 e provisiona `PesquisaClientes.csv`.

### Passo 2: Compilação do Dashboard Integrado (`app.py`)
Execute a Célula 2 do notebook, que grava a aplicação web completa utilizando a diretiva `%%writefile` (com seletor de idioma e fallback automático para `PesquisaClientes.csv`):

```python
%%writefile app.py
import streamlit as st
import pandas as pd
import os
import json
import re
from crewai import Agent, Task, Crew, Process

# 1. Configuração da Página e Títulos
st.set_page_config(page_title="BI Pro Dashboard", layout="wide")
st.title("🏢 Agentic Analytics: Insight-as-a-Service")

with st.sidebar:
    st.header("⚙️ Control Panel / Painel de Controle")
    api_key = st.text_input("🔑 Gemini API Key:", type="password")
    output_lang = st.selectbox("🌐 Output Language / Idioma:", ["Portuguese (Brazil)", "English"])
    st.divider()
    st.info("MBA BI & Analytics - FIAP")

# 2. Ingestão de Dados (Upload ou Fallback Automático Self-Healing)
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
            
            # Amostragem representativa controlada contra estouro de quota
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
            
            # 3. Extração Resiliente via Regex e Persistência de Sessão
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

    # 4. Exibição Executiva do Dashboard
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

### Passo 3: Ativação do Túnel Ngrok e Inicialização do Servidor
Insira seu Authtoken pessoal do Ngrok na Célula 3 e execute:

```python
NGROK_TOKEN = "COLE_SEU_NGROK_TOKEN_AQUI"

from pyngrok import ngrok
import os

if NGROK_TOKEN:
    ngrok.kill()
    ngrok.set_auth_token(NGROK_TOKEN)
    public_url = ngrok.connect(8501).public_url
    print(f"🔗 DASHBOARD PROFISSIONAL ATIVO EM: {public_url}")
    print("--- ")
    !streamlit run app.py --server.port 8501
else:
    print("❌ ERRO: Você precisa inserir o NGROK_TOKEN para gerar o link de acesso.")
```

### Passo 4: Operando o Dashboard no Navegador
1. Clique no link gerado pelo Ngrok (`https://...ngrok-free.app`).
2. Insira sua **Gemini API Key** no menu lateral esquerdo.
3. Suba o dataset `PesquisaClientes.csv`.
4. Clique em **"🚀 Gerar Análise Estruturada"** e aguarde a Squad sintetizar o insight.
5. Verifique a renderização simultânea dos cards executivos (`st.metric`) e a árvore de validação (`st.json`).

---

## 🧪 Validação & Critérios de Aceite
- [ ] A aplicação sobe na porta 8501 e é exposta publicamente pelo Ngrok.
- [ ] A chamada da Squad instancia o modelo canônico **`gemini/gemini-3.8-flash`**.
- [ ] O parser de regex isola com sucesso o JSON delimitado entre chaves (`{...}`), prevenindo quebras de tipagem.
- [ ] Os dados permanecem renderizados na interface via `st.session_state` mesmo após interações na página.
- [ ] O bloco `Raw JSON` reflete exatamente os 4 campos do contrato de dados homologado no Lab 08.

---

## 🧹 Cleanup
Para liberar os recursos de porta e encerrar o túnel:
```python
from pyngrok import ngrok
ngrok.kill()
print("🛑 Túnel Ngrok e processos finalizados com sucesso.")
```

---

## 💡 Desafios Complementares
* **Desafio 1 (Contrato Expandido):** Adicione uma lista de ações mitigadoras ao JSON Schema (`"acoes_prioritarias": ["string"]`) e renderize-as como uma lista com marcadores (`st.markdown`) abaixo da recomendação.
* **Desafio 2 (Exportação de Dossiê):** Utilize `st.download_button` para exportar o JSON sanitizado como arquivo `.json` para consumo por serviços downstream.

