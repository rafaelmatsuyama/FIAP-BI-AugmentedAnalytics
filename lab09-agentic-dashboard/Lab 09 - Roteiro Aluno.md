# Lab 09 - Dashboard de Analytics com Agentes Inteligentes (Streamlit UI)

* **Curso:** MBA em Business Intelligence & Analytics
* **Disciplina:** Augmented Analytics & AI-Driven Insights
* **Ambiente / Plataforma:** Google Colab (Cloud Runtime) + Túnel Ngrok
* **Linguagem / Stack:** Python 3.10+, Streamlit, CrewAI, Gemini 3.8 Flash (`gemini-3.8-flash`), PyNgrok
* **Duração Prevista:** 25 a 30 minutos

---

## 🎯 Objetivo do Lab
Capacitar o aluno a transformar pipelines analíticos baseados em IA em produtos interativos acessíveis para tomadores de decisão, construindo uma aplicação web executiva completa em Python puro (Streamlit) capaz de carregar datasets corporativos, disparar agentes autônomos e exibir relatórios acionáveis de *Insight-as-a-Service*.

Ao concluir este laboratório, você será capaz de:
1. Desenvolver uma interface web completa (Agentic UI) em Python sem escrever linhas de HTML, CSS ou JavaScript.
2. Criar formulários interativos com seletores de arquivos (`file_uploader`), campos seguros de credenciais e botões de gatilho.
3. Expor serviços locais do Google Colab para a internet pública utilizando tunelamento seguro via Ngrok.
4. Conectar a camada de apresentação visual ao motor de orquestração de agentes de IA em tempo real.
5. Diagnosticar e contornar os desafios de ciclo de vida e estado de sessão (*Session State*) típicos de aplicações analíticas.

---

## 📋 Pré-requisitos & Materiais
1. **Google Colab:** Sessão ativa em [colab.new](https://colab.new).
2. **Conta Gratuita Ngrok:** Cadastro em [ngrok.com](https://ngrok.com) com *Authtoken* copiado do painel.
3. **API Key do Google AI Studio:** Chave gerada em [aistudio.google.com](https://aistudio.google.com).
4. **Dataset de Demonstração:** Arquivo `PesquisaClientes.csv` (com auto-download *Self-Healing* integrado no notebook).
5. **Notebook do Laboratório:** Arquivo [`Lab 09 - Colab Notebook.ipynb`](./Lab%2009%20-%20Colab%20Notebook.ipynb).

---

## 🚀 Passo a Passo Guiado

### Passo 1: Instalação das Dependências e Carga Self-Healing
Abra o notebook no Google Colab e execute a Célula 1 de instalação dos pacotes e download automático do dataset:

```python
!pip install crewai streamlit pyngrok pandas -q
import urllib.request, os
if not os.path.exists('PesquisaClientes.csv'):
    urllib.request.urlretrieve('https://raw.githubusercontent.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/main/lab01-cx-analytics/Lab%2001%20-%20PesquisaClientes.csv', 'PesquisaClientes.csv')
    print('✅ Demo dataset downloaded successfully! / Dataset de demonstração baixado com sucesso!')
```
* **O que acontece:** O ambiente prepara o servidor web do Streamlit, os componentes de agentes, o cliente do Ngrok e garante que `PesquisaClientes.csv` esteja disponível na VM do Colab.

### Passo 2: Geração do Aplicativo Web (`app.py`)
Execute a célula que compila o código do front-end diretamente no disco do Colab usando a diretiva `%%writefile` (com seletor de idioma e fallback automático para `PesquisaClientes.csv` caso nenhum arquivo externo seja enviado):

```python
%%writefile app.py
import streamlit as st
import pandas as pd
import os
from crewai import Agent, Task, Crew, Process

# 1. Configuração da Página e Títulos Executivos
st.set_page_config(page_title="BI Agentic Dashboard", layout="wide")

st.title("🤖 Agentic Analytics Dashboard")
st.subheader("Transforming Telemetry into Executive Decisions with AI Squads")

# 2. Painel Lateral de Controle (Sidebar)
with st.sidebar:
    st.header("⚙️ Control Panel / Configurações")
    api_key = st.text_input("🔑 Gemini API Key:", type="password")
    model_choice = st.selectbox("🧠 Model / Modelo:", ["gemini/gemini-3.8-flash"])
    output_lang = st.selectbox("🌐 Output Language / Idioma:", ["Portuguese (Brazil)", "English"])
    st.divider()
    st.info("MBA in BI & Analytics - FIAP")

# 3. Ingestão de Dados (Upload ou Fallback Automático Self-Healing)
uploaded_file = st.file_uploader("📂 Upload CSV dataset (or leave empty to use default PesquisaClientes.csv):", type="csv")
data_source = uploaded_file if uploaded_file is not None else ('PesquisaClientes.csv' if os.path.exists('PesquisaClientes.csv') else None)

if data_source and api_key:
    df = pd.read_csv(data_source)
    if uploaded_file is None:
        st.caption("ℹ️ Using pre-loaded Colab dataset: `PesquisaClientes.csv` / Usando base padrão do Colab.")
    
    # Cards Executivos de BI
    col1, col2, col3 = st.columns(3)
    col1.metric("📊 Total Records / Registros", len(df))
    col2.metric("📋 Total Columns / Colunas", len(df.columns))
    col3.metric("🔢 Numeric Columns / Numéricas", len(df.select_dtypes(include='number').columns))
    
    st.write("### 🔍 Telemetry Sample / Amostra dos Dados Carregados", df.head(5))
    
    # Disparo com persistência em session_state
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

    # Exibição persistente e exportação
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

### Passo 3: Ativação do Túnel Ngrok e Inicialização do Servidor
Na célula seguinte, configure seu authtoken pessoal e inicie o serviço:

```python
NGROK_TOKEN = "COLE_SEU_NGROK_TOKEN_AQUI"

from pyngrok import ngrok
import os

if NGROK_TOKEN:
    ngrok.kill()
    ngrok.set_auth_token(NGROK_TOKEN)
    public_url = ngrok.connect(8501).public_url
    print(f"🔗 LIVE DASHBOARD URL / DASHBOARD ATIVO EM: {public_url}")
    !streamlit run app.py --server.port 8501
```

### Passo 4: Operando o Dashboard no Navegador
1. Clique na URL pública gerada pelo Ngrok (ex: `https://xxxx-xx.ngrok-free.app`).
2. Caso apareça a tela de confirmação do Ngrok, clique em **"Visit Site"**.
3. Na barra lateral esquerda, insira sua **Gemini API Key**.
4. Faça o upload do arquivo `PesquisaClientes.csv`.
5. Clique no botão **"🚀 Disparar Análise com Agente de IA"** e acompanhe a renderização do sumário executivo.

---

## 🧪 Validação & Critérios de Aceite
1. **Acesso Público:** A URL do Ngrok deve carregar a interface completa do Streamlit sem erros de timeout.
2. **Segurança de Credenciais:** O campo de API Key deve mascarar a digitação (`type="password"`).
3. **Execução Completa:** Após o upload e clique no botão, a interface deve exibir a mensagem de sucesso e o relatório analítico formatado em Markdown.

---

## 🧹 Cleanup
Para encerrar o servidor e fechar o túnel público:
```python
ngrok.kill()
processo.terminate()
print("🛑 Servidor web e túnel Ngrok finalizados com sucesso.")
```

---

## 💡 Desafios Complementares
* **Desafio 1 (Cards de Métricas):** Adicione componentes visuais `st.metric(label="Total de Clientes", value=len(df))` logo acima do relatório.
* **Desafio 2 (Download do Relatório):** Inclua um botão `st.download_button` permitindo ao usuário baixar o texto gerado pela IA em um arquivo `.txt` ou `.md`.

