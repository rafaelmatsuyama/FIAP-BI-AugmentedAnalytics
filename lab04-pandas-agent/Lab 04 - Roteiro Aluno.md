# Lab 04 - O Analista de Dados Nativo com Pandas e Gemini API

**Curso / Disciplina:** MBA em Business Intelligence and Analytics (BI) — Augmented Analytics & AI-Driven Insights (AA)  
**Ambiente:** Google Colab ([colab.research.google.com](https://colab.research.google.com/))  
**Linguagem / Stack:** Python 3.10+ / Google GenAI SDK (`google-genai`) / Pandas / Seaborn / Gemini Flash 3.8 (`gemini-3.8-flash`)  
**Duração Estimada:** 25 a 30 minutos  

---

## 🎯 Objetivo do Lab

O objetivo deste laboratório é construir e operar um agente analista nativo capaz de interpretar perguntas de negócio em linguagem natural, traduzi-las dinamicamente em código Python/Pandas executável e retornar respostas quantitativas e gráficos estatísticos sem necessidade de consultas manuais ou pipelines legados de SQL.

Ao final deste laboratório, você será capaz de:
1. Configurar um agente cognitivo de análise de dados integrado ao ambiente Google Colab com o modelo **Gemini Flash 3.8** (`gemini-3.8-flash`).
2. Implementar um módulo de saneamento autônomo (`sanitizar_dados()`) para correção de inconsistências de formatação e remoção de espaços em branco em dados textuais.
3. Formular consultas analíticas avançadas em linguagem natural cobrindo segmentação demográfica, dispersão de renda e correlações socioeconômicas.
4. Auditar a camada de código intermediária gerada pela IA, identificando potenciais falhas de interpretação estatística ou filtros indevidos.
5. Produzir visualizações gráficas sob demanda (dispersão, barras e distribuições) alimentadas por código Python gerado em tempo de execução.

---

## 📋 Pré-requisitos & Materiais

* Acesso ao [Google Colab](https://colab.research.google.com/).
* Chave de autenticação gerada no [Google AI Studio](https://aistudio.google.com/).
* Arquivos fornecidos neste laboratório (localizados na pasta `lab04-pandas-agent` do repositório):
  * [`Lab 04 - Colab Notebook.ipynb`](./Lab%2004%20-%20Colab%20Notebook.ipynb): Notebook estruturado contendo as funções do analista nativo.
  * [`Lab 04 - adult.csv`](./Lab%2004%20-%20adult.csv): Dataset do Censo Socioeconômico dos EUA (~32.500 registros) contendo dados de idade, ocupação, escolaridade, horas trabalhadas e renda (`ABOVE50K`).

---

## 🚀 Passo a Passo Guiado

### Passo 1: Abertura do Notebook no Google Colab
1. Acesse o [Google Colab](https://colab.research.google.com/).
2. Na janela de abertura, selecione a aba **"GitHub"** (ou faça o upload manual do arquivo local).
3. Informe o repositório institucional: `rafaelmatsuyama/FIAP-BI-AugmentedAnalytics`.
4. Abra o notebook: `lab04-pandas-agent/Lab 04 - Colab Notebook.ipynb`.

### Passo 2: Carga do Dataset `adult.csv`
1. No menu lateral esquerdo do Colab, clique no ícone de pasta (**Arquivos**).
2. Faça o upload do arquivo [`Lab 04 - adult.csv`](./Lab%2004%20-%20adult.csv) para o diretório raiz da sessão (`/content/`).
3. Execute a célula de instalação de pacotes (`google-genai tabulate`).

### Passo 3: Configuração da Credencial
Na célula de configuração, insira sua chave de API do Google AI Studio:
```python
from google import genai
from google.genai import types

API_KEY = 'SUA_CHAVE_AQUI'
client = genai.Client(api_key=API_KEY)
```
Execute a célula para autenticar a conexão com a API do Gemini via novo SDK oficial.

### Passo 4: Saneamento Automatizado da Base (Data Prep)
O dataset original possui espaços em branco residuais em colunas categóricas (ex: `' Female'` em vez de `'Female'`). Execute a função de saneamento:
```python
sanitizar_dados()
```
* **O que acontece:** O modelo `gemini-3.8-flash` recebe o schema das colunas, gera o código Python de strip/limpeza e aplica as alterações diretamente no DataFrame `df` global.

### Passo 5: Consultas Analíticas em Linguagem Natural
Utilize a função `chat_com_dados()` para provocar o agente com perguntas estratégicas:

1. **Segmentação de Gênero e Renda:**
   ```python
   chat_com_dados("Qual a principal ocupação das mulheres que ganham acima de 50k?")
   ```
2. **Jornada de Trabalho e Comportamento:**
   ```python
   chat_com_dados("Pessoas casadas trabalham em média mais horas por semana que solteiros?")
   ```
3. **Retorno sobre Escolaridade:**
   ```python
   chat_com_dados("Quem tem mestrado (Masters) ganha proporcionalmente mais do que quem tem graduação (Bachelors)?")
   ```

### Passo 6: Geração Dinâmica de Visualizações Gráficas
Solicite a plotagem de distribuições estatísticas complexas:
```python
chat_com_dados("Gere um gráfico de dispersão de Idade vs Horas por Semana colorindo por Sexo.")
```
* **Inspeção de Auditoria:** Observe que a IA importa `matplotlib.pyplot` ou `seaborn`, mapeia as variáveis corretas e exibe o gráfico na tela sem que você precise escrever sintaxe gráfica.

---

## 🧪 Validação & Critérios de Aceite

Certifique-se de que todas as etapas foram concluídas com sucesso:
- [ ] O dataset [`Lab 04 - adult.csv`](./Lab%2004%20-%20adult.csv) foi carregado sem erros de `FileNotFoundError`.
- [ ] A rotina `sanitizar_dados()` executou com sucesso, eliminando espaços extras das colunas de texto.
- [ ] O modelo invocado no código foi o **`gemini-3.8-flash`**.
- [ ] O agente respondeu com precisão analítica às perguntas de segmentação e retorno de escolaridade.
- [ ] Pelo menos uma visualização gráfica foi renderizada na tela via `chat_com_dados()`.
- [ ] O código gerado internamente foi inspecionado, validando ausência de filtros ou exclusões indevidas.

---

## 🧹 Cleanup

Após a conclusão das consultas:
1. No menu superior do Google Colab, clique em **Ambiente de execução** -> **Desconectar e excluir ambiente de execução** para liberar os recursos computacionais alocados.
2. Certifique-se de que sua API Key não ficou exposta publicamente caso venha a salvar uma cópia do notebook no GitHub pessoal.

---

## 💡 Desafios Complementares (Para Alunos Avançados)

1. **Análise de Disparidade Salarial:** Formule um prompt solicitando a probabilidade condicional de ganhar acima de 50k segmentada por raça e gênero simultaneamente.
2. **Clusterização Informal:** Solicite ao agente que identifique os 3 perfis demográficos com maior concentração de renda alta na base.
3. **Auditoria de Robustez:** Teste uma pergunta indutiva sobre uma coluna que não existe na base (ex: `"Qual o estado de nascimento das pessoas acima de 50k?"`) e verifique se o agente lida com a ausência da variável ou gera uma exceção de forma transparente.
