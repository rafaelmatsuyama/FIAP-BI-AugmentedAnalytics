# Lab 06 - Orquestração Visual de Fluxos Multi-Agentes com Dify.ai

**Curso / Disciplina:** MBA em Business Intelligence and Analytics (BI) — Augmented Analytics & AI-Driven Insights (AA)  
**Ambiente:** Dify.ai Cloud ([dify.ai](https://dify.ai/))  
**Linguagem / Stack:** No-Code / Visual Workflow / Google Gemini Flash 3.8 (`gemini-3.8-flash`) / Markdown  
**Duração Estimada:** 25 a 30 minutos  

---

## 🎯 Objetivo do Lab

O objetivo deste laboratório é construir e operar um sistema multi-agente visual orientado a grafos direcionados (*DAG - Directed Acyclic Graphs*) no Dify.ai, orquestrando agentes de IA com papéis complementares (Pesquisador de Mercado → Diretor de Estratégia) para produzir análises competitivas em tempo recorde ("Operação Flash Insight"), superando a sobrecarga cognitiva e a saturação de contexto típicas do chat monolítico.

Ao final deste laboratório, você será capaz de:
1. Compreender a transição de prompts isolados para linhas de montagem intelectuais coordenadas visualmente.
2. Configurar o provedor do Google AI Studio no Dify.ai Cloud com o modelo canônico **Gemini Flash 3.8** (`gemini-3.8-flash`).
3. Projetar nós de entrada (*Start Node*), nós cognitivos de LLM (*LLM Nodes*) e nós de entrega (*End Node*).
4. Parametrizar a transferência semântica de variáveis e referências de contexto (`{{#node_id.text#}}`) entre agentes encadeados.
5. Gerar um relatório executivo de inteligência competitiva com diagnóstico de modelo de negócio, mapa de riscos e plano de contraofensiva em Markdown.

---

## 📋 Pré-requisitos & Materiais

* Acesso ao [Dify.ai Cloud](https://dify.ai/) (cadastro gratuito com conta Google ou GitHub).
* Chave de autenticação ativa gerada no [Google AI Studio](https://aistudio.google.com/apikey).
* Navegador Web moderno (Google Chrome, Microsoft Edge ou Firefox).
* Arquivo DSL de referência (opcional): [`Sinalizador_Concorrencia_MBA.yml`](file:///C:/Dropbox/PC/Documents/GitHub/FIAP-BI-AugmentedAnalytics/lab06-dify-workflow/Sinalizador_Concorrencia_MBA.yml) (disponível nesta pasta para importação direta de conferência).
* Não é necessária instalação de Python local ou execução de terminal (ambiente 100% gerenciado em nuvem).

---

## 🚀 Passo a Passo Guiado

### Passo 1: Acesso ao Dify Cloud e Criação do Workflow
1. Acesse [dify.ai](https://dify.ai/) e faça o login no ambiente Cloud.
2. Na tela inicial (Dashboard), clique no botão **"Create from Blank"** (Criar do Zero) — *ou clique na seta ao lado e selecione **"Import DSL File"** enviando o arquivo `Sinalizador_Concorrencia_MBA.yml` caso queira inspecionar a arquitetura final diretamente*.
3. Caso crie do zero, selecione o tipo de aplicação: **"Workflow"** (Fluxo de Trabalho).
4. Defina o nome do projeto:
   * **App Name:** `Sinalizador_Concorrencia_MBA`
   * **Description:** `Fluxo visual multi-agente para monitoramento e contraofensiva competitiva.`
5. Clique em **"Create"** para abrir o canvas visual de orquestração.

### Passo 2: Configuração do Modelo no Provedor Gemini
1. No canto superior direito da tela do Dify (ou no menu de perfil), clique no ícone do usuário e acesse **Settings** (Configurações).
2. Selecione a aba **Model Provider** (Provedor de Modelos).
3. Localize o card **Google / Gemini** e clique em **Setup** (ou **Add Key**).
4. Cole sua **API Key** gerada no Google AI Studio.
5. Confirme e verifique se o indicador verde (*Active/Configured*) aparece ao lado do Gemini.
6. Feche a janela de configurações e retorne ao canvas do Workflow.

### Passo 3: Parametrizar o Nó de Início (Start Node)
1. No canvas, clique sobre o bloco **Start**.
2. Na barra lateral direita, na seção **User Input Field** (Campos de Entrada), clique no ícone **"+"** para adicionar um campo.
3. Configure o campo:
   * **Type:** `Short Text` (Texto Curto)
   * **Variable Name:** `nome_concorrente`
   * **Label:** `Nome do Concorrente ou Notícia`
   * **Mandatory:** Ativado (Obrigatório)

### Passo 4: Adicionar o Agente Pesquisador (LLM Node 1)
1. Clique no conector de saída **"+"** do nó *Start* e selecione o bloco **LLM**.
2. Renomeie o nó no topo para: `Agente_Pesquisador`.
3. Configure o bloco na barra lateral:
   * **Model:** Selecione **Google / Gemini** → **`gemini-3.8-flash`**.
   * **System Prompt:**
     ```text
     Você é um Especialista Sênior em Inteligência de Mercado e Estratégia Competitiva.
     Sua missão é realizar uma autópsia detalhada do modelo de negócio do concorrente informado, destacando diferenciais, fontes de receita e públicos-alvo.
     ```
   * **User Prompt:** Clique em **"+ Add Message"** e insira:
     ```text
     Analise profundamente o modelo de negócio da seguinte empresa/notícia: {{#start.nome_concorrente#}}

     Estruture sua resposta com:
     1. Proposta de Valor Central
     2. Mecanismo de Monetização
     3. Principais Pontos Fortes e Ameaças
     ```
   * **Thinking Level:** Defina em **`Low`** (ou **`Medium`**) para síntese ágil do modelo de negócio sem latência desnecessária.

### Passo 5: Adicionar o Agente de Estratégia (LLM Node 2)
1. Clique no conector de saída **"+"** do nó *Agente_Pesquisador* e adicione um segundo bloco **LLM**.
2. Renomeie o nó para: `Agente_Estrategia`.
3. Configure o bloco:
   * **Model:** Selecione **Google / Gemini** → **`gemini-3.8-flash`**.
   * **System Prompt:**
     ```text
     Você é o Chief Strategy Officer (CSO) da nossa organização.
     Você é crítico, pragmático e orientado a mitigação de riscos e defesa de margem.
     Recebendo o diagnóstico de mercado, você deve formular um plano de contraofensiva executiva para a Diretoria.
     ```
   * **User Prompt:** Clique em **"+ Add Message"** e insira o prompt que consome a saída do primeiro agente:
     ```text
     Com base no dossiê de mercado gerado pelo nosso time de pesquisa abaixo:
     
     ---
     {{#Agente_Pesquisador.text#}}
     ---
     
     Gere um MEMORANDO ESTRATÉGICO DE DEFESA para o Board contendo:
     1. Os 3 Maiores Riscos Imediatos para a nossa carteira de clientes
     2. Plano de Contraofensiva em 30 Dias (Ações Práticas)
     3. Alavanca de Diferenciação que devemos enfatizar
     
     Exiba o relatório em Markdown corporativo, com tabelas e bullets objetivos.
     ```
   * **Thinking Level:** Defina em **`High`** para maximizar a profundidade analítica na identificação de riscos e formulação da contraofensiva executiva.

### Passo 6: Conectar o Nó de Fechamento (End Node)
1. Conecte a saída do *Agente_Estrategia* ao bloco **End** (Final).
2. No painel direito do bloco *End*, configure a variável de entrega:
   * **Variable:** `report`
   * **Value:** Selecione `{{#Agente_Estrategia.text#}}`

### Passo 7: Teste Executivo e Validação
1. No canto superior direito, clique em **Run** (ou **Preview**).
2. Na caixa de diálogo de entrada, informe o cenário de teste:
   * **nome_concorrente:** `OpenAI (Lançamento de modelos multimodais de baixo custo para o setor financeiro)`
3. Acompanhe a execução do grafo: o nó Start ativa o Agente Pesquisador, cujo texto é transmitido instantaneamente para o Agente de Estratégia, culminando na entrega do relatório no nó End.

---

## 🧪 Validação & Critérios de Aceite

Certifique-se de que todas as etapas foram concluídas com sucesso:
- [ ] O modelo configurado em ambos os nós de LLM foi o **`gemini-3.8-flash`**.
- [ ] A variável `nome_concorrente` foi passada corretamente do nó Start para o Agente Pesquisador.
- [ ] A saída do Agente Pesquisador foi injetada no Agente de Estratégia através da sintaxe `{{#Agente_Pesquisador.text#}}`.
- [ ] O nó End retornou o relatório completo formatado em Markdown com análise de riscos e plano de contraofensiva.
- [ ] O tempo total de execução do grafo multi-agente foi inferior a 30 segundos.

---

## 🧹 Cleanup

Após o término da prática:
1. Caso tenha utilizado computadores institucionais ou compartilhados, acesse **Settings** → **Model Provider** no Dify e remova ou desative sua API Key do Gemini.
2. O workflow permanece salvo na sua conta do Dify Cloud para reutilização em projetos de consultoria ou no trabalho de conclusão da disciplina.

---

## 💡 Desafios Complementares (Para Alunos Avançados)

1. **Inclusão de Nó de Pesquisa na Web:** Adicione um nó de ferramenta (*Tool Node*) com o plugin **Google Search** ou **Tavily Search** antes do Agente Pesquisador para alimentar a análise com notícias publicadas nas últimas 24 horas.
2. **Nó de Extração Estruturada (Code Node):** Adicione um bloco de código Python intermediário entre os agentes para extrair métricas de risco em JSON e validar a severidade antes de despachar para o Diretor de Estratégia.
3. **Publicação via API REST:** Na aba **Publish** do Dify, explore como transformar esse fluxo visual em uma API web (`/v1/workflows/run`) consumível por sistemas legados de BI ou dashboards do Streamlit.
