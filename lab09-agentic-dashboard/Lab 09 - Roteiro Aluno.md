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
4. **Dataset de Demonstração:** Arquivo `PesquisaClientes.csv` (ou base tabular similar).
5. **Notebook do Laboratório:** Arquivo `Lab 09 - Dashboard de Analytics.ipynb`.

---

## 🚀 Passo a Passo Guiado

### Passo 1: Instalação das Dependências de Front-End e Túnel
Abra o notebook no Google Colab e execute a célula de instalação dos pacotes necessários:

```python
!pip install -q crewai streamlit pyngrok pandas tabulate
print("✅ Bibliotecas instaladas com sucesso!")
```
* **O que acontece:** O ambiente prepara o servidor web do Streamlit, os componentes de agentes e o cliente do Ngrok.

### Passo 2: Geração do Aplicativo Web (`app.py`)
Execute a célula que compila o código do front-end diretamente no disco do Colab usando a diretiva `%%writefile`:

```python
%%writefile app.py
import streamlit as st
import pandas as pd
import os
from crewai import Agent, Task, Crew, Process

# 1. Configuração da Página e Títulos Executivos
st.set_page_config(page_title="BI Agentic Dashboard", layout="wide", page_icon="🤖")

st.title("🤖 Agentic Analytics Dashboard")
st.subheader("Insight-as-a-Service: Transformando Dados em Decisões com IA")

# 2. Painel Lateral de Controle (Sidebar)
with st.sidebar:
    st.header("⚙️ Configurações da Sessão")
    api_key = st.text_input("Insira sua Gemini API Key:", type="password")
    model_choice = st.selectbox("Selecione o Modelo:", ["gemini/gemini-3.8-flash"])
    st.divider()
    st.caption("MBA em Business Intelligence & Analytics — FIAP")

# 3. Ingestão de Dados e Disparo do Agente
uploaded_file = st.file_uploader("Faça o upload do seu dataset de negócio (CSV):", type="csv")

if uploaded_file and api_key:
    df = pd.read_csv(uploaded_file)
    
    # Cards Executivos de BI
    col1, col2, col3 = st.columns(3)
    col1.metric("📊 Total de Registros", len(df))
    col2.metric("📋 Total de Colunas", len(df.columns))
    col3.metric("🔢 Colunas Numéricas", len(df.select_dtypes(include='number').columns))
    
    st.write("### 🔍 Amostra dos Dados Carregados", df.head(5))
    
    # Disparo com persistência em session_state
    if st.button("🚀 Disparar Análise com Agente de IA"):
        os.environ["GEMINI_API_KEY"] = api_key
        os.environ["GOOGLE_API_KEY"] = api_key
        
        with st.spinner("🤖 Agente analista processando métricas e elaborando recomendações..."):
            analyst = Agent(
                role='Analista Sênior de Estratégia de BI',
                goal='Identificar anomalias e gerar 3 recomendações de alto impacto',
                backstory='Consultor sênior de BI focado em ROI e melhoria operacional.',
                llm=model_choice
            )
            
            task = Task(
                description=f'Analise as variáveis {df.columns.tolist()} na amostra a seguir e apresente um sumário executivo estruturado: {df.head(10).to_markdown()}',
                expected_output='Dossiê executivo em Markdown com: 1. Diagnóstico Principal, 2. Métrica Crítica, 3. Recomendações Táticas.',
                agent=analyst
            )
            
            crew = Crew(agents=[analyst], tasks=[task], process=Process.sequential)
            raw_result = crew.kickoff()
            st.session_state["relatorio_analise"] = str(raw_result)

    # Exibição persistente e exportação
    if "relatorio_analise" in st.session_state:
        st.success("✅ Diagnóstico Concluído com Sucesso!")
        st.markdown("---")
        st.markdown(st.session_state["relatorio_analise"])
        
        st.download_button(
            label="📥 Baixar Dossiê Executivo (.md)",
            data=st.session_state["relatorio_analise"],
            file_name="dossie_executivo_bi.md",
            mime="text/markdown"
        )

elif not api_key:
    st.info("👉 Insira sua API Key do Google AI Studio na barra lateral para habilitar os recursos analíticos.")
```

### Passo 3: Ativação do Túnel Ngrok e Inicialização do Servidor
Na célula seguinte, configure seu authtoken pessoal e inicie o serviço:

```python
from pyngrok import ngrok
import subprocess
import time

# Insira o authtoken copiado do painel do Ngrok
NGROK_TOKEN = "COLE_SEU_NGROK_TOKEN_AQUI"
ngrok.set_auth_token(NGROK_TOKEN)

# Inicia o servidor Streamlit em segundo plano na porta 8501
processo = subprocess.Popen(["streamlit", "run", "app.py", "--server.port", "8501", "--server.headless", "true"])
time.sleep(3)

# Abre o túnel público apontando para a porta 8501
public_url = ngrok.connect(8501)
print(f"🚀 Dashboard disponível publicamente em: {public_url}")
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

