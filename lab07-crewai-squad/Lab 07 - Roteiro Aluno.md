# Lab 07 - Operação Churn Zero (Squads com CrewAI)

* **Curso:** MBA em Business Intelligence & Analytics
* **Disciplina:** Augmented Analytics & AI-Driven Insights
* **Ambiente / Plataforma:** Google Colab (Cloud Runtime)
* **Linguagem / Stack:** Python 3.10+, CrewAI, Gemini 3.8 Flash (`gemini-3.8-flash`)
* **Duração Prevista:** 25 a 30 minutos

---

## 🎯 Objetivo do Lab
Capacitar o aluno a estruturar e orquestrar uma Squad autônoma de inteligência artificial em código Python via CrewAI, superando o modelo de agente generalista ("faz-tudo") e automatizando a triagem, estratégia e comunicação executiva para reverter cancelamentos de clientes em tempo real.

Ao concluir este laboratório, você será capaz de:
1. Configurar o ecossistema CrewAI conectado à API do Google Gemini via chaves de ambiente seguras.
2. Definir agentes especialistas com papéis corporativos delimitados (`Role`, `Goal` e `Backstory`).
3. Desenhar tarefas sequenciais com contratos claros de saída (`Expected Output`) e encadeamento de contexto.
4. Auditar a cadeia de raciocínio (*Chain of Thought - CoT*) da IA através dos logs detalhados de execução.
5. Mensurar o ganho de produtividade corporativa (*ROI*) ao substituir processos manuais por squads autônomas.

---

## 📋 Pré-requisitos & Materiais
1. **Google Colab:** Acesso a uma sessão ativa em [colab.new](https://colab.new).
2. **API Key do Google AI Studio:** Chave gratuita gerada em [aistudio.google.com](https://aistudio.google.com).
3. **Dataset da Operação:** Arquivo `Lab 05 - EvasaoClientesClassificados.xlsx` (com auto-download resiliente *Self-Healing* integrado no notebook).
4. **Notebook Base:** Arquivo [`Lab 07 - Colab Notebook.ipynb`](./Lab%2007%20-%20Colab%20Notebook.ipynb).

---

## 🚀 Passo a Passo Guiado

### Passo 1: Preparação do Ambiente e Dependências
No seu notebook do Google Colab, execute a instalação do framework CrewAI:

```python
# Instalação silenciosa do CrewAI e utilitários
!pip install -q crewai openpyxl tabulate
print("✅ Ambiente configurado com sucesso!")
```
* **O que acontece:** O Colab baixa o motor de orquestração do CrewAI e as bibliotecas de integração com provedores de LLM.

### Passo 2: Configuração de Credenciais, Idioma & Provedor de LLM
Configure as variáveis de ambiente necessárias para comunicação com a API do Gemini e o idioma de saída:

```python
import os
import pandas as pd
from crewai import Agent, Task, Crew, Process

# Insira sua chave obtida no Google AI Studio
API_KEY = "COLE_SUA_API_KEY_AQUI"
OUTPUT_LANGUAGE = "Portuguese (Brazil)" # @param ["Portuguese (Brazil)", "English"]
os.environ["GOOGLE_API_KEY"] = API_KEY
os.environ["GEMINI_API_KEY"] = API_KEY

# Configuração do modelo oficial da disciplina no padrão LiteLLM/CrewAI
llm_config = "gemini/gemini-3.8-flash"
print(f"✅ Motor de IA configurado: {llm_config} | Output: {OUTPUT_LANGUAGE}")
```
* **Por que fazer isso:** O CrewAI utiliza o LiteLLM para roteamento de modelos. Passar o identificador como string no formato `"gemini/..."` garante compatibilidade total com o validador Pydantic sem conflitos de esquema.

### Passo 3: Ingestão dos Dados de Churn (Self-Healing Runtime)
Carregue a planilha de clientes classificados com risco de evasão (o bloco baixa automaticamente a planilha do GitHub caso não esteja em `/content/`) e prepare o extrato das contas mais críticas:

```python
import os
import urllib.request

DATASET_NAME = 'Lab 05 - EvasaoClientesClassificados.xlsx'
RAW_URL = 'https://raw.githubusercontent.com/rafaelmatsuyama/FIAP-BI-AugmentedAnalytics/main/lab05-automl-scikit/Lab%2005%20-%20EvasaoClientesClassificados.xlsx'

if not os.path.exists(DATASET_NAME):
    urllib.request.urlretrieve(RAW_URL, DATASET_NAME)

df = pd.read_excel(DATASET_NAME)
amostra_clientes = df.head(5).to_markdown()
print("✅ Inteligência de dados carregada!")
```
* **Saída Esperada:** Uma tabela formatada em Markdown contendo as métricas de tempo de contrato, cobertura e score de cancelamento dos 5 clientes da amostra.

### Passo 4: Convocando a Squad Especialista
Crie os três agentes corporativos, atribuindo responsabilidades exclusivas:

```python
investigador = Agent(
    role='Analista de Risco e Dados (The Investigator)',
    goal='Identificar padrões de insatisfação nos clientes fornecidos e eleger o caso mais crítico.',
    backstory='Você é um detetive de dados sênior especializado em Churn Analytics. Sua missão é descobrir o motivo exato pelo qual o cliente VIP deseja cancelar.',
    allow_delegation=False,
    llm=llm_config
)

arquiteto = Agent(
    role='Estrategista Comercial de Retenção (The Architect)',
    goal='Desenhar uma oferta personalizada de retenção baseada no valor e na dor do cliente crítico.',
    backstory='Você é um mestre em negociação comercial de apólices e planos enterprise. Você cria propostas financeiramente sustentáveis e irrecusáveis.',
    allow_delegation=False,
    llm=llm_config
)

comunicador = Agent(
    role='Redator Executivo de Sucesso do Cliente (The Communicator)',
    goal='Redigir uma mensagem de abordagem personalizada em Português do Brasil com tom premium e acolhedor.',
    backstory='Você é especialista em comunicação corporativa C-Level. Transforma dados frios e descontos em empatia, respeito e valorização da parceria.',
    allow_delegation=False,
    llm=llm_config
)

print("✅ Squad convocada: Investigador, Arquiteto e Comunicador postos.")
```

### Passo 5: Traçando o Fluxo de Tarefas Sequenciais
Conecte os entregáveis de cada agente para formar uma linha de produção lógica:

```python
tarefa_analise = Task(
    description=f"Analise detalhadamente a lista de clientes a seguir e aponte quem apresenta maior urgência de intervenção e por quê: {amostra_clientes}",
    expected_output="Dossiê técnico com o ID do cliente mais crítico, histórico contratual e justificativa da iminência de cancelamento.",
    agent=investigador
)

tarefa_estrategia = Task(
    description="Com base no diagnóstico do Investigador, crie uma estratégia de retenção estruturada com contrapartida de valor, benefícios adicionais ou repactuação comercial.",
    expected_output="Plano de retenção estruturado com proposta de valor, termos econômicos e salvaguardas financeiras.",
    agent=arquiteto
)

tarefa_email = Task(
    description="Utilizando a estratégia aprovada, redija a minuta oficial de e-mail personalizada para envio imediato ao tomador de decisão do cliente. O tom deve ser solícito, elegante e focado em parceria de longo prazo.",
    expected_output="Texto integral do e-mail em Markdown pronto para envio institucional, com assunto e assinatura executiva.",
    agent=comunicador
)

print("✅ Encadeamento de tarefas registrado.")
```

### Passo 6: Disparo e Auditoria da Operação
Instancie a Crew com processo sequencial e execute a operação:

```python
squad_retencao = Crew(
    agents=[investigador, arquiteto, comunicador],
    tasks=[tarefa_analise, tarefa_estrategia, tarefa_email],
    process=Process.sequential,
    verbose=True
)

print("\n🚀 SQUAD EM AÇÃO! Acompanhe o Chain of Thought da IA...\n")
resultado_final = await squad_retencao.kickoff_async()

print("\n" + "="*80)
print("📄 RELATÓRIO EXECUTIVO DA OPERAÇÃO CHURN ZERO")
print("="*80 + "\n")
print(resultado_final)
```

---

## 🧪 Validação & Critérios de Aceite
Para validar a conclusão bem-sucedida do laboratório, verifique:
1. **Auditoria de CoT nos Logs:** O console deve exibir as mensagens de raciocínio intermediário (`Thought: ...`, `Action: ...`) de cada agente em sequência.
2. **Transferência Contextual:** A proposta criada pelo Arquiteto deve citar explicitamente os atributos de risco do cliente destacado pelo Investigador.
3. **Consistência do E-mail:** A minuta redigida pelo Comunicador deve estar em Português do Brasil, sem marcadores residuais de template (`[Nome do Cliente]`) e pronta para disparo comercial.

---

## 🧹 Cleanup
Para liberar os recursos de memória da sua sessão no Google Colab:
1. Interrompa a execução de células ativas no menu **Ambiente de Execução $\rightarrow$ Interromper execução**.
2. Caso deseje reiniciar o estado de dependências, utilize **Ambiente de Execução $\rightarrow$ Reiniciar sessão**.

---

## 💡 Desafios Complementares
* **Desafio 1 (Hierarchical Crew):** Modifique o parâmetro `process=Process.sequential` para `process=Process.hierarchical` adicionando um `manager_llm=llm_config`. Observe como um agente gerente passa a coordenar a delegação de tarefas em vez da linha de produção rígida.
* **Desafio 2 (Exportação Automática):** Adicione um bloco de código que persista o resultado do relatório final em um arquivo `Plano_Retencao_VIP.md` no disco do Colab para download direto.

