# Lab 05 - AutoML Nativo com Scikit-Learn e Gemini API

**Curso / Disciplina:** MBA em Business Intelligence and Analytics (BI) — Augmented Analytics & AI-Driven Insights (AA)  
**Ambiente:** Google Colab ([colab.research.google.com](https://colab.research.google.com/))  
**Linguagem / Stack:** Python 3.10+ / Google GenAI SDK (`google-genai`) / Scikit-Learn / Pandas / Gemini Flash 3.8 (`gemini-3.8-flash`)  
**Duração Estimada:** 25 a 30 minutos  

---

## 🎯 Objetivo do Lab

O objetivo deste laboratório é construir e operar um agente autônomo de Machine Learning capaz de interpretar demandas de negócio, projetar e executar dinamicamente um pipeline preditivo completo (*White-Box AutoML*) no Scikit-Learn, eliminando a dependência de plataformas proprietárias de caixa-preta (*Vendor Lock-in*) e entregando métricas executivas de explicabilidade (*XAI*) para estancar a evasão de clientes (*Churn*).

Ao final deste laboratório, você será capaz de:
1. Configurar um agente cognitivo de AutoML integrado ao ambiente Google Colab com o modelo **Gemini Flash 3.8** (`gemini-3.8-flash`) via novo SDK oficial `google-genai`.
2. Delegar à IA a automação das etapas de Engenharia de Recursos (*Feature Engineering*), codificação de variáveis categóricas e divisão estratificada de treino e teste (80/20).
3. Treinar e validar modelos preditivos de classificação baseados em comitês de árvores de decisão (*Random Forest Classifier*).
4. Interpretar a Matriz de Confusão sob a ótica de risco corporativo, balanceando custos de Falsos Positivos e Falsos Negativos.
5. Extrair e apresentar gráficos de Importância das Variáveis (*Feature Importance*) para subsidiar decisões estratégicas da Diretoria.

---

## 📋 Pré-requisitos & Materiais

* Acesso ao [Google Colab](https://colab.research.google.com/).
* Chave de autenticação gerada no [Google AI Studio](https://aistudio.google.com/).
* Arquivos fornecidos neste laboratório (localizados na pasta `lab05-automl-scikit` do repositório):
  * [`Lab 05 - Colab Notebook.ipynb`](./Lab%2005%20-%20Colab%20Notebook.ipynb): Notebook estruturado contendo a função do agente de AutoML.
  * [`Lab 05 - EvasaoClientesClassificados.xlsx`](./Lab%2005%20-%20EvasaoClientesClassificados.xlsx): Base de clientes de seguros contendo dados cadastrais, nível de cobertura, atividade e a coluna alvo `Perdido?` (0 = Retido, 1 = Cancelado).

---

## 🚀 Passo a Passo Guiado

### Passo 1: Abertura do Notebook no Google Colab
1. Acesse o [Google Colab](https://colab.research.google.com/).
2. Na janela de abertura, selecione a aba **"GitHub"**.
3. Informe o repositório institucional: `rafaelmatsuyama/FIAP-BI-AugmentedAnalytics`.
4. Abra o notebook: `lab05-automl-scikit/Lab 05 - Colab Notebook.ipynb`.

### Passo 2: Carga do Dataset de Evasão
1. No menu lateral esquerdo do Colab, clique no ícone de pasta (**Arquivos**).
2. Faça o upload do arquivo [`Lab 05 - EvasaoClientesClassificados.xlsx`](./Lab%2005%20-%20EvasaoClientesClassificados.xlsx) para o diretório raiz da sessão (`/content/`).
3. Execute a primeira célula para instalar o novo SDK oficial e utilitários:
   ```bash
   !pip install -q -U google-genai openpyxl tabulate
   ```

### Passo 3: Configuração da Credencial
Na célula de configuração, insira sua chave de API do Google AI Studio:
```python
from google import genai
from google.genai import types

API_KEY = 'SUA_CHAVE_AQUI'
client = genai.Client(api_key=API_KEY)
```
Execute a célula para autenticar a conexão com a API do Gemini.

### Passo 4: Carga e Inspeção da Base
Execute a célula de leitura de dados para instanciar o DataFrame `df` e inspecionar os primeiros registros:
```python
df = pd.read_excel('Lab 05 - EvasaoClientesClassificados.xlsx')
```
* Observe que a coluna `Perdido?` representa a evasão histórica dos clientes.

### Passo 5: Treinamento Automatizado do Pipeline de AutoML
Execute a função `executar_automl_nativo()` passando a pergunta estratégica de negócio:
```python
executar_automl_nativo("Identifique as 3 variáveis que mais influenciam a saída do cliente e avalie a qualidade do modelo.")
```
* **O que acontece:** O modelo **Gemini 3.8 Flash** recebe as colunas e a amostra de dados, escreve o script completo em Scikit-Learn (removendo `ID Cliente`, aplicando `LabelEncoder`, dividindo em 80/20, treinando `RandomForestClassifier`), executa o código no ambiente e exibe na tela a Acurácia, o Relatório de Classificação e os gráficos gerados.

### Passo 6: Interpretação Executiva da Matriz de Confusão e XAI
Inspecione as saídas gráficas geradas pela IA:
1. **Matriz de Confusão:**
   * *Falso Positivo:* Prever que o cliente vai sair quando ele ficaria (risco: concessão de desconto desnecessário).
   * *Falso Negativo:* Prever que o cliente fica quando ele vai cancelar (risco crítico: perda irreversível de receita recorrente).
2. **Feature Importance (Importância das Variáveis):**
   * Identifique quais colunas aparecem no topo do gráfico (geralmente **Nível de Cobertura** e **Idade**).
   * Conclua a tese de negócio: planos com cobertura deficiente geram insatisfação e aceleram o cancelamento.

---

## 🧪 Validação & Critérios de Aceite

Certifique-se de que todas as etapas foram concluídas com sucesso:
- [ ] O arquivo [`Lab 05 - EvasaoClientesClassificados.xlsx`](./Lab%2005%20-%20EvasaoClientesClassificados.xlsx) foi carregado sem erros de importação do `openpyxl`.
- [ ] A chamada de IA utilizou a biblioteca oficial `google-genai` e o modelo **`gemini-3.8-flash`**.
- [ ] O código Python Scikit-Learn foi gerado, extraído e executado dinamicamente em memória.
- [ ] O modelo treinado atingiu acurácia entre **78% e 88%** no conjunto de teste.
- [ ] O gráfico de *Feature Importance* foi renderizado na tela identificando os maiores drivers de evasão.
- [ ] Você identificou as implicações financeiras dos Falsos Negativos em relação aos Falsos Positivos.

---

## 🧹 Cleanup

Após a conclusão do treinamento:
1. No menu superior do Google Colab, clique em **Ambiente de execução** -> **Desconectar e excluir ambiente de execução** para encerrar a sessão e liberar memória RAM.
2. Certifique-se de que sua API Key não ficou salva em versões públicas de notebooks.

---

## 💡 Desafios Complementares (Para Alunos Avançados)

1. **Comparação de Modelos (Benchmark):** Solicite ao agente uma comparação entre algoritmos:
   ```python
   executar_automl_nativo("Compare um modelo Random Forest com uma Regressão Logística. Qual obteve melhor Recall para a classe de cancelamento?")
   ```
2. **Simulação de Impacto Financeiro (ROI):** Assuma que cada cliente perdido representa R$ 6.000 de perda anual e cada ação de retenção custa R$ 500. Calcule a economia líquida gerada pelo modelo ao estancar 70% dos cancelamentos previstos.
3. **Ponte para Sistemas Multi-Agentes (Aula 03):** Se você já sabe exatamente quais clientes possuem maior probabilidade de cancelamento, como automatizar a abordagem comercial? Este é o ponto de partida do Lab 07 com CrewAI.
