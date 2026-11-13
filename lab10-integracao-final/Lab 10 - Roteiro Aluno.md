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
4. **Dataset de Teste:** Arquivo `PesquisaClientes.csv` (ou base tabular similar).
5. **Notebook do Laboratório:** Arquivo `Lab 10 - Integração Final e Dashboard Estruturado.ipynb`.

---

## 🚀 Passo a Passo Guiado

### Passo 1: Instalação de Dependências no Ambiente Colab
Abra o notebook no Google Colab e execute a Célula 1 para instalar os componentes de agentes, front-end e tunelamento:

```python
!pip install crewai streamlit pyngrok pandas -q
print("✅ Bibliotecas instaladas com sucesso!")
```
* **O que acontece:** O ambiente prepara o runtime com `streamlit` para interface, `crewai` para orquestração de agentes e `pyngrok` para exposição pública da porta 8501.

### Passo 2: Compilação do Dashboard Integrado (`app.py`)
Execute a Célula 2 do notebook, que grava a aplicação web completa utilizando a diretiva `%%writefile`:

```python
%%writefile app.py
import streamlit as st
import pandas as pd
import os
import json
import re
from crewai import Agent, Task, Crew, Process

# 1. Configuração da Página e Títulos
st.set_page_config(page_title="BI Pro Dashboard", layout="wide", page_icon="🏢")
st.title("🏢 Agentic Analytics: Insight-as-a-Service")
st.caption("MBA em Business Intelligence & Analytics — FIAP")

with st.sidebar:
    st.header("⚙️ Painel de Controle")
    api_key = st.text_input("Gemini API Key:", type="password")
    st.divider()
    st.info("Padrão Homologado: Gemini 3.8 Flash")

# 2. Ingestão de Dados e Disparo da Squad
uploaded_file = st.file_uploader("Suba o dataset para análise estratégica (CSV):", type="csv")

if uploaded_file and api_key:
    df = pd.read_csv(uploaded_file)
    st.dataframe(df.head(10), use_container_width=True)
    
    if st.button("🚀 Gerar Análise Estruturada"):
        os.environ["GEMINI_API_KEY"] = api_key
        os.environ["GOOGLE_API_KEY"] = api_key
        
        with st.spinner("🧠 Squad processando dados via JSON Schema... Aguarde."):
            instrucoes_sistemicas = (
                "Você é um Consultor Estratégico do MBA em BI & Analytics da FIAP. "
                "Seu tom é pragmático, focado em ROI e métricas de negócio. "
                "Sempre baseie suas conclusões nos dados fornecidos."
            )
            
            analyst = Agent(
                role="Consultor Estratégico FIAP",
                goal="Gerar insights acionáveis e estruturados",
                backstory=instrucoes_sistemicas,
                llm="gemini/gemini-3.8-flash"
            )
            
            # Amostragem representativa controlada contra estouro de quota
            dataset_contexto = df.head(15).to_csv(index=False)
            
            task = Task(
                description=f"""Analise a amostra de dados abaixo e gere um insight estratégico estruturado:

{dataset_contexto}

Retorne OBRIGATORIAMENTE um objeto JSON com o formato:
{{
  "titulo": "string",
  "metrica_critica": "string",
  "recomendacao": "string",
  "impacto_estimado": "Baixo" | "Médio" | "Alto"
}}

Retorne APENAS o JSON puro, sem blocos explicativos ou markdown adicionais.""",
                expected_output="Um objeto JSON com as chaves: titulo, metrica_critica, recomendacao, impacto_estimado.",
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
                    raise ValueError("JSON não identificado no retorno do agente.")
                    
            except Exception as e:
                st.error(f"❌ Erro ao processar JSON: {e}")
                st.write("**Retorno Bruto:**", raw_result)

    # 4. Exibição Executiva do Dashboard
    if "analise_data" in st.session_state:
        data = st.session_state["analise_data"]
        st.success("✅ Inteligência Gerada com Sucesso!")
        st.divider()
        
        col1, col2 = st.columns(2)
        with col1:
            st.metric(label="Título do Insight", value=data.get("titulo", "N/A"))
            st.write(f"**Métrica Crítica:** {data.get('metrica_critica', 'N/A')}")
        
        with col2:
            st.metric(label="Impacto Estimado", value=data.get("impacto_estimado", "Médio"))
            st.warning(f"**Recomendação:** {data.get('recomendacao', 'N/A')}")
        
        st.divider()
        st.write("### 🛠️ Raw JSON (Para Auditoria IT)")
        st.json(data)

elif not api_key:
    st.warning("⚠️ Insira sua Gemini API Key no menu lateral para ativar a Squad de Agentes.")
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

