# Lab 08 - Refinamento e JSON Schema (Google AI Studio)

* **Curso:** MBA em Business Intelligence & Analytics
* **Disciplina:** Augmented Analytics & AI-Driven Insights
* **Ambiente / Plataforma:** Google AI Studio (Web Console)
* **Linguagem / Stack:** Gemini 3.8 Flash (`gemini-3.8-flash`), Structured Outputs, JSON Schema, Enums
* **Duração Prevista:** 20 a 25 minutos

---

## 🎯 Objetivo do Lab
Capacitar o aluno a estabelecer governança rígida e contratos de dados determinísticos para modelos de linguagem no Google AI Studio, eliminando a imprevisibilidade conversacional e forçando a IA a entregar exclusivamente saídas estruturadas em JSON Schema com tipagem e campos controlados por Enum.

Ao concluir este laboratório, você será capaz de:
1. Redigir *System Instructions* executivas que eliminem opiniões vagas e estabeleçam a voz da marca.
2. Ativar e configurar o recurso nativo de *Structured Output* no Google AI Studio.
3. Declarar contratos de dados usando tipos primitivos (`string`, `number`, `boolean`) e listas.
4. Travar níveis de criticidade e risco com tipos enumerados (*Enums*) para prevenir alucinação categórica.
5. Executar testes de estresse contra entradas ambíguas para auditar a resiliência e invariância do schema JSON.

---

## 📋 Pré-requisitos & Materiais
1. **Google AI Studio:** Acesso ao console oficial em [aistudio.google.com](https://aistudio.google.com).
2. **Conta Google:** Login ativo com conta Google habitual.
3. **Dataset de Validação:** Arquivo `PesquisaClientes.csv` (fornecido na disciplina).

---

## 🚀 Passo a Passo Guiado

### Passo 1: Acesso ao Console e Inicialização do Prompt
1. Acesse [aistudio.google.com](https://aistudio.google.com).
2. No menu lateral ou tela inicial, clique em **"Create New Prompt"** $\rightarrow$ selecione **"Chat prompt"** (ou **"Freeform prompt"**).
3. Selecione o modelo oficial da disciplina: **Gemini 3.8 Flash** (`gemini-3.8-flash`).

### Passo 2: Estabelecendo a "Constituição" (System Instructions)
No painel de configuração lateral ou campo superior **System Instructions**, insira as diretrizes de governança executiva:

```text
Você é um Consultor Estratégico Sênior do MBA em Business Intelligence da FIAP.
Seu tom é estritamente analítico, pragmático e focado em ROI e métricas quantificáveis.

Diretrizes Inegociáveis:
1. Jamais inicie respostas com expressões subjetivas como "Eu acho" ou "Em minha opinião".
2. Toda asserção deve se apoiar explicitamente nas evidências dos dados fornecidos.
3. Cada oportunidade deve conter estimativa de impacto categorizada em Baixo, Médio ou Alto.
4. Responda estritamente no schema de dados definido, sem texto introdutório ou saudações.
```
* **O que acontece:** O modelo internaliza o papel consultivo e suprime maneirismos conversacionais prolixos.

### Passo 3: Definição do Contrato de Dados (Structured Output)
1. No menu de parâmetros à direita, localize o bloco **"Structured Output"** (ou **"Response Format"**).
2. Ative a chave de seleção (**Toggle ON**) e clique em **"Edit Schema"** (ou JSON Schema).
3. Configure as propriedades do objeto raiz:
   * `titulo_insight`: Tipo `STRING` (Descrição: "Título executivo curto do achado").
   * `metrica_chave`: Tipo `STRING` (Descrição: "Métrica quantitativa ou indicador crítico").
   * `impacto_estimado`: Tipo `STRING` com restrição de **ENUM**: `["Baixo", "Médio", "Alto"]`.
   * `recomendacao_estrategica`: Tipo `STRING` (Descrição: "Ação de negócio recomendada").
   * `acoes_imediatas`: Tipo `ARRAY` de `STRING` (Descrição: "Passos táticos para implementação").

* **Por que isso é crítico:** O uso do Enum impede que a IA responda variações como "Muito Alto", "Moderado" ou "Crítico", garantindo que dashboards e APIs consumam os dados sem erros de parse.

### Passo 4: Teste de Ingestão e Carga com Dados Reais
1. Abra o arquivo `PesquisaClientes.csv` no seu computador e copie uma amostra de 5 a 10 linhas.
2. Cole a amostra no campo de mensagem do usuário com o comando:
   ```text
   Analise os registros de atendimento a seguir e consolide o principal diagnóstico operacional:
   [COLE_AQUI_AS_LINHAS_DO_CSV]
   ```
3. Execute o disparo e observe a saída gerada.

* **Saída Esperada:** O modelo responde exclusivamente um bloco JSON perfeitamente válido, sem qualquer caractere extra antes de `{` ou após `}`.

### Passo 5: Teste de Stress de Resiliência Estrutural
1. No campo de mensagem do usuário, envie uma pergunta intencionalmente informal e ambígua, sem qualquer dado estruturado:
   ```text
   Acho que os clientes estão insatisfeitos com a velocidade de resposta, o que podemos fazer a respeito?
   ```
2. Execute o disparo e observe a saída gerada.
3. Observe que, mesmo diante de uma entrada puramente qualitativa e desestruturada, o modelo é obrigado pelo Structured Output a preencher todos os campos do schema JSON (`titulo_insight`, `metrica_chave`, `impacto_estimado`, `recomendacao_estrategica`), selecionando obrigatoriamente um dos valores válidos do Enum e garantindo que o dashboard corporativo nunca receba payloads quebrados.

---

## 🧪 Validação & Critérios de Aceite
1. **Conformidade de Schema:** O payload retornado deve passar em validadores JSON padrão (`json.loads`) sem necessidade de tratamento de string.
2. **Respeito ao Enum:** O campo `impacto_estimado` deve conter rigorosamente um dos três valores autorizados (`Baixo`, `Médio` ou `Alto`).
3. **Ausência de Conversação:** A resposta não deve conter frases como "Aqui está o seu JSON" ou "Espero ter ajudado".

---

## 🧹 Cleanup
1. Caso não pretenda reutilizar o prompt, feche a aba ou clique em **Clear Chat**.
2. Para salvar seu trabalho como referência, clique em **Save** no canto superior direito e nomeie como `FIAP-MBA-Contract-Gov`.

---

## 💡 Desafios Complementares
* **Desafio 1 (Schema Aninhado):** Adicione um sub-objeto `kpis_afetados` contendo `nome_indicador` (string) e `variacao_percentual` (number).
* **Desafio 2 (Exportação para Código):** Clique no botão **"Get Code"** no topo do AI Studio, selecione a aba **Python** e observe como o schema JSON é injetado diretamente como argumento no SDK oficial `google-genai`.

