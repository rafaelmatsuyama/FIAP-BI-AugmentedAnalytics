# Lab 02 - O Analista Programável com Python e Gemini API

**Curso / Disciplina:** MBA em Business Intelligence and Analytics (BI) — Augmented Analytics & AI-Driven Insights (AA)  
**Ambiente:** Google Colab ([colab.research.google.com](https://colab.research.google.com/))  
**Linguagem / Stack:** Python 3.10+ / Google GenAI SDK (`google-genai`) / Pandas / Gemini Flash 3.8 (`gemini-3.8-flash`)  
**Duração Estimada:** 25 a 30 minutos  

---

## 🎯 Objetivo do Lab

O objetivo deste laboratório é automatizar a análise financeira em escala de portfólios de investimento (Venture Capital), integrando a API do Gemini ao ambiente Python para transformar dados tabulares em relatórios de diretoria ("White-Box Analytics") de forma programática.

Ao final deste laboratório, você será capaz de:
1. Obter e configurar credenciais seguras de API no **Google AI Studio**.
2. Integrar a nova biblioteca oficial `google-genai` em um notebook Jupyter no **Google Colab**.
3. Estruturar representações tabulares eficientes (Markdown serialization) para consumo otimizado por LLMs.
4. Programar chamadas de inferência utilizando o modelo **Gemini Flash 3.8** (`gemini-3.8-flash`) com parametrização controlada de tokens (`max_output_tokens=10000`) e temperatura.
5. Interpretar relações não triviais de eficiência de capital (investimento em P&D vs. Lucro Líquido vs. Gastos Administrativos) em 50 startups de tecnologia.

---

## 📋 Pré-requisitos & Materiais

* Acesso ao [Google Colab](https://colab.research.google.com/).
* Chave de API gerada no [Google AI Studio](https://aistudio.google.com/) (Menu "Get API key").
* Arquivos fornecidos neste laboratório:
  * [`Lab 02 - Colab Notebook.ipynb`](./Lab%2002%20-%20Colab%20Notebook.ipynb): Notebook estruturado com o pipeline em Python.
  * [`Lab 02 - 50_Startups.csv`](./Lab%2002%20-%2050_Startups.csv): Base de dados financeiros contendo R&D Spend, Administration, Marketing Spend, State e Profit.

---

## 🚀 Passo a Passo Guiado

### Passo 1: Obtenção da API Key no Google AI Studio
1. Acesse [https://aistudio.google.com/](https://aistudio.google.com/).
2. No menu lateral esquerdo, clique em **"Get API key"**.
3. Selecione **"Create API key"** (em um projeto novo ou existente).
4. Copie a chave de autenticação alfanumérica gerada. Guarde-a de forma temporária.

### Passo 2: Abertura do Notebook no Google Colab
1. Acesse o [Google Colab](https://colab.research.google.com/).
2. Na tela inicial de seleção de notebook, escolha a aba **"GitHub"** (ou use a aba **"Upload"** caso tenha baixado o arquivo localmente).
3. Na busca do GitHub, informe o repositório da disciplina: `rafaelmatsuyama/FIAP-BI-AugmentedAnalytics`.
4. Selecione o arquivo: `lab02-financial-analytics/Lab 02 - Colab Notebook.ipynb`.

### Passo 3: Carga do Dataset
1. Na barra lateral esquerda do Colab, clique no ícone de pasta (**Arquivos**).
2. Faça o upload do arquivo [`Lab 02 - 50_Startups.csv`](./Lab%2002%20-%2050_Startups.csv) para a raiz da sessão `/content/`.

### Passo 4: Configuração da Credencial e Bibliotecas
Na Célula 1 do notebook, insira sua API Key obtida no Passo 1:
```python
API_KEY = 'SUA_CHAVE_AQUI'
client = genai.Client(api_key=API_KEY)
```
Execute a célula para instalar as dependências (`google-genai`, `tabulate`) e autenticar a sessão do cliente GenAI.

### Passo 5: Execução da Chamada de Inferência (Gemini Flash 3.8)
Prossiga executando as células seguintes:
1. **Carga e Visualização:** O Pandas carrega o dataset e exibe as 5 primeiras linhas.
2. **Serialização em Markdown:** O método `df.head(15).to_markdown()` formata a tabela preservando a semântica de colunas para o modelo.
3. **Invocação do Agente:** O modelo `gemini-3.8-flash` recebe o prompt executivo de Venture Capital via `client.models.generate_content` com `max_output_tokens=10000`, garantindo a geração integral do relatório sem cortes.
4. **Relatório Gerado:** Observe a saída formatada contendo a melhor alocação para investimento Série A.

---

## 🧪 Validação & Critérios de Aceite

- [ ] O notebook executou sem erros de autenticação (`403 Forbidden` ou `API Key Invalid`).
- [ ] O dataset `Lab 02 - 50_Startups.csv` foi carregado exibindo 50 linhas totais.
- [ ] O modelo invocado no código foi o `gemini-3.8-flash`.
- [ ] O relatório final apontou métricas financeiras precisas e justificou a startup recomendada com base na correlação entre investimento em Inovação (R&D) e Lucro (Profit).

---

## 💡 Desafios Complementares

* **Automação Batch em Lote:** Como você modificaria o código para iterar sobre blocos de 10 em 10 startups e salvar as análises individuais em um arquivo consolidado `relatorio_analise.md`?
* **Ajuste de Risco Operacional:** Altere o parâmetro `temperature` de `0.3` para `0.8` e observe se a recomendação de aporte se torna mais arrojada em startups de alto risco e alto retorno.
