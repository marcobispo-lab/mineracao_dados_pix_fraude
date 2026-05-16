# Detecção de Fraudes em Transações PIX

Projeto acadêmico e de portfólio desenvolvido no contexto da disciplina de **Data Mining**, da Pós-Graduação em Inteligência Artificial.

O objetivo do projeto é aplicar técnicas de **mineração de dados** e **Machine Learning** para identificar possíveis fraudes em transações PIX, seguindo a lógica do processo **CRISP-DM**.

---

## Visão Geral

O PIX se tornou um dos principais meios de pagamento no Brasil, sendo amplamente utilizado por pessoas físicas, empresas e instituições financeiras. Por ser um meio de pagamento instantâneo, disponível 24 horas por dia, ele também apresenta desafios relevantes para prevenção e detecção de fraudes.

Neste projeto, o problema foi tratado como uma tarefa de **classificação supervisionada**, na qual o objetivo é prever se uma transação é fraudulenta ou legítima.

A variável alvo do projeto é:

| Variável | Descrição |
|---|---|
| `is_fraud` | Indica se a transação é fraudulenta ou não |

As classes são:

| Classe | Significado |
|---|---|
| `0` | Transação legítima |
| `1` | Transação fraudulenta |

---

## Objetivo do Projeto

O principal objetivo foi desenvolver e avaliar modelos capazes de classificar transações PIX como fraudulentas ou não fraudulentas, considerando o forte desbalanceamento da base de dados.

Além da construção dos modelos, o projeto buscou demonstrar:

- entendimento do problema de negócio;
- análise exploratória dos dados;
- preparação e transformação das variáveis;
- tratamento de dados desbalanceados;
- comparação entre diferentes abordagens de modelagem;
- avaliação com métricas adequadas para fraude;
- interpretação dos resultados;
- análise dos trade-offs entre detecção de fraude e falsos positivos.

---

## Dataset

O dataset utilizado foi o **Pix Fraud Challenge v2**, disponível no Kaggle:

https://www.kaggle.com/datasets/raphaelnunes/pix-fraud-challenge-v2

O arquivo utilizado no projeto é:

```text
pix_fraud_v2.csv
```

Por questões de tamanho e organização do repositório, o arquivo CSV **não está incluído neste repositório**.

Para reproduzir o projeto, é necessário baixar o dataset diretamente do Kaggle.

---

## Atenção sobre o Caminho do Arquivo

Este notebook foi desenvolvido no **Google Colab** e está configurado para ler o arquivo CSV diretamente do **Google Drive**.

O caminho utilizado no notebook é:

```python
df = pd.read_csv("/content/drive/MyDrive/Colab Notebooks/Datasets/pix_fraud_v2.csv")
```

Portanto, para executar o projeto sem alterar o código, siga os passos abaixo:

1. Baixe o dataset no Kaggle.
2. Salve o arquivo com o nome:

```text
pix_fraud_v2.csv
```

3. No seu Google Drive, crie a pasta:

```text
MyDrive/Colab Notebooks/Datasets/
```

4. Coloque o arquivo `pix_fraud_v2.csv` dentro dessa pasta.
5. Abra o notebook no Google Colab.
6. Monte o Google Drive.
7. Execute as células em ordem.

Caso deseje salvar o arquivo em outro local, será necessário alterar manualmente o caminho na célula de leitura do CSV.

---

## Metodologia

O projeto foi estruturado seguindo as principais etapas do processo **CRISP-DM**:

### 1. Business Understanding

Nesta etapa, o problema foi analisado sob a ótica de negócio.

O objetivo de negócio é apoiar a identificação preventiva de transações PIX potencialmente fraudulentas, reduzindo prejuízos financeiros e apoiando processos antifraude.

O objetivo analítico foi traduzido para um problema de classificação binária:

> prever se uma transação é fraudulenta ou não fraudulenta.

---

### 2. Data Understanding

Nesta etapa, foi realizada a exploração inicial da base de dados, incluindo:

- verificação da estrutura da base;
- análise dos tipos de dados;
- análise da variável alvo;
- verificação de valores nulos;
- identificação de duplicidades;
- análise do desbalanceamento das classes;
- exploração inicial de padrões em variáveis numéricas e categóricas.

Um dos principais pontos identificados foi o forte desbalanceamento entre transações legítimas e fraudulentas.

Esse desbalanceamento impacta diretamente a escolha das métricas de avaliação, pois um modelo pode apresentar alta acurácia simplesmente prevendo a classe majoritária.

---

### 3. Data Preparation

A etapa de preparação dos dados incluiu tratamentos e transformações necessários para tornar a base adequada à modelagem.

Foram realizadas as seguintes operações:

- conversão da coluna de data/hora da transação;
- ordenação temporal da base;
- divisão temporal entre treino e teste;
- criação de variáveis temporais;
- tratamento de valores nulos;
- padronização de variáveis categóricas;
- tratamento de inconsistências em estados brasileiros;
- criação de flags para valores negativos;
- correção de valores inconsistentes;
- criação de novas variáveis;
- remoção de identificadores;
- codificação de variáveis categóricas com One-Hot Encoding.

Entre as variáveis criadas, destacam-se:

| Variável | Descrição |
|---|---|
| `transaction_hour` | Hora da transação |
| `transaction_day` | Dia da transação |
| `transaction_month` | Mês da transação |
| `transaction_year` | Ano da transação |
| `transaction_dayofweek` | Dia da semana da transação |
| `is_weekend` | Indica se a transação ocorreu em fim de semana |
| `is_night` | Indica se a transação ocorreu durante a madrugada |
| `amount_negative_flag` | Indica se o valor original da transação era negativo |
| `account_age_negative_flag` | Indica se a idade da conta apresentava valor negativo |
| `amount_vs_avg_ratio` | Relação entre valor da transação e valor médio histórico da conta |

---

### 4. Modeling

Foram avaliadas diferentes abordagens de classificação.

O primeiro modelo foi um baseline simples, utilizado como referência inicial. Em seguida, foram treinados modelos supervisionados com balanceamento de classes.

Os modelos avaliados foram:

| Modelo | Descrição |
|---|---|
| Baseline | Modelo que prevê sempre a classe majoritária |
| Regressão Logística | Modelo linear com `class_weight="balanced"` |
| Árvore de Decisão | Modelo baseado em regras, também com `class_weight="balanced"` |

A Regressão Logística foi utilizada por ser um modelo simples, interpretável e adequado como referência para problemas de classificação.

A Árvore de Decisão foi utilizada por permitir uma interpretação mais intuitiva das regras de separação entre as classes.

---

### 5. Evaluation

A avaliação dos modelos foi feita com métricas mais adequadas para problemas de fraude e dados desbalanceados.

As métricas utilizadas foram:

| Métrica | Interpretação |
|---|---|
| Accuracy | Proporção total de acertos |
| Precision | Entre as transações previstas como fraude, quantas realmente eram fraude |
| Recall | Entre as fraudes reais, quantas foram detectadas pelo modelo |
| F1-score | Média harmônica entre precision e recall |
| ROC-AUC | Capacidade geral de separação entre as classes |
| PR-AUC | Área sob a curva Precision-Recall, especialmente útil em dados desbalanceados |
| Matriz de confusão | Distribuição entre acertos, falsos positivos e falsos negativos |

Como a classe fraude é minoritária, a acurácia isolada não foi considerada suficiente para avaliar o desempenho dos modelos.

---

## Resultados

A tabela abaixo apresenta a comparação entre os modelos avaliados:

| Modelo | Accuracy | Precision | Recall | F1-score | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|---:|
| Baseline | 0.9705 | 0.0000 | 0.0000 | 0.0000 | - | - |
| Regressão Logística — threshold 0.50 | 0.8142 | 0.0960 | 0.6301 | 0.1667 | 0.7779 | 0.1118 |
| Regressão Logística — threshold ajustado | 0.8569 | 0.1123 | 0.5580 | 0.1869 | 0.7779 | 0.1118 |
| Árvore de Decisão — threshold padrão | 0.8015 | 0.0911 | 0.6385 | 0.1595 | 0.7731 | 0.1066 |
| Árvore de Decisão — threshold ajustado | 0.8479 | 0.1081 | 0.5736 | 0.1819 | 0.7731 | 0.1066 |

---

## Interpretação dos Resultados

O modelo baseline apresentou alta acurácia, aproximadamente 97%, mas não identificou nenhuma fraude.

Esse resultado evidencia um ponto importante: em bases desbalanceadas, a acurácia pode ser enganosa. Um modelo que sempre prevê a classe majoritária pode parecer eficiente, mas ser inútil para o objetivo principal do problema.

Os modelos supervisionados conseguiram detectar parte relevante das fraudes, mas apresentaram um trade-off entre:

- aumentar o recall, capturando mais fraudes;
- reduzir falsos positivos, evitando sinalizar muitas transações legítimas como suspeitas.

A Regressão Logística com threshold ajustado foi selecionada como o modelo mais equilibrado entre as abordagens testadas.

Essa escolha foi baseada em:

- maior F1-score;
- maior PR-AUC;
- melhor precisão;
- menor volume de falsos positivos em comparação com a árvore ajustada;
- desempenho geral semelhante ou superior nas principais métricas agregadas.

---

## Ajuste de Threshold

Além do threshold padrão de 0.50, foram testados diferentes limiares de classificação.

Esse ajuste é especialmente importante em problemas de fraude, pois a decisão final depende do equilíbrio entre dois tipos de erro:

| Tipo de erro | Descrição | Impacto |
|---|---|---|
| Falso positivo | Transação legítima classificada como fraude | Pode gerar bloqueio indevido ou revisão desnecessária |
| Falso negativo | Fraude classificada como legítima | Pode gerar prejuízo financeiro |

O threshold ajustado da Regressão Logística buscou um ponto de equilíbrio entre capturar fraudes e reduzir o volume de falsos positivos.

Em um cenário real, esse threshold poderia ser calibrado de acordo com:

- custo financeiro das fraudes;
- capacidade operacional da equipe antifraude;
- tolerância a bloqueios indevidos;
- criticidade da transação;
- políticas internas de risco.

---

## Interpretação das Variáveis

A análise dos coeficientes da Regressão Logística e da importância das variáveis na Árvore de Decisão indicou que algumas variáveis tiveram maior influência na classificação.

Entre as variáveis mais relevantes, destacaram-se:

- tipo de chave PIX;
- dispositivo utilizado;
- valor da transação;
- relação entre valor da transação e média histórica da conta;
- idade da conta;
- quantidade de transações nas últimas 24 horas;
- horário da transação;
- indicação de transação durante a madrugada.

A variável relacionada ao tipo de chave PIX aleatória apresentou forte associação com a classificação de fraude dentro da base analisada.

É importante destacar que essa interpretação indica associação estatística aprendida pelo modelo, e não causalidade.

Ou seja, o fato de determinada variável estar associada à fraude no modelo não significa que ela cause fraude, mas sim que apareceu como um padrão relevante nos dados utilizados.

---

## Análise de Erros

Após a escolha do modelo final, foi realizada uma análise dos erros de classificação.

Os resultados foram separados em quatro grupos:

| Grupo | Descrição |
|---|---|
| Verdadeiro Positivo | Fraude corretamente identificada |
| Verdadeiro Negativo | Transação legítima corretamente identificada |
| Falso Positivo | Transação legítima sinalizada como fraude |
| Falso Negativo | Fraude não detectada pelo modelo |

A análise mostrou que parte dos falsos positivos apresentava probabilidade estimada de fraude muito próxima à dos verdadeiros positivos.

Isso indica que algumas transações legítimas possuíam características semelhantes às fraudes reais, dificultando a separação entre as classes.

Também foi observado que os falsos negativos apresentaram, em média, menor probabilidade estimada de fraude, sugerindo que parte das fraudes não detectadas não apresentava sinais suficientemente fortes nas variáveis disponíveis.

Essa análise reforça que modelos antifraude devem ser avaliados não apenas pelas métricas agregadas, mas também pelo tipo de erro cometido e seu impacto operacional.

---

## Principais Aprendizados

Este projeto reforçou alguns pontos importantes em problemas de detecção de fraude:

1. **Acurácia pode ser enganosa em bases desbalanceadas**  
   O baseline teve alta acurácia, mas não detectou nenhuma fraude.

2. **Recall e precision devem ser analisados em conjunto**  
   Aumentar a detecção de fraudes pode gerar mais falsos positivos.

3. **O threshold é uma decisão de negócio**  
   O melhor limiar não depende apenas da métrica matemática, mas também do custo dos erros.

4. **Modelos simples podem ser bons pontos de partida**  
   Regressão Logística e Árvore de Decisão permitiram boa análise, interpretação e comparação.

5. **Explicabilidade é importante em problemas financeiros**  
   Em um cenário real, decisões automatizadas precisam ser interpretáveis, auditáveis e monitoradas.

---

## Tecnologias Utilizadas

As principais tecnologias e bibliotecas utilizadas foram:

- Python
- Google Colab
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook

---

## Como Executar o Projeto

### 1. Clone o repositório

```bash
git clone https://github.com/marcobispo-lab/mineracao_dados_pix_fraude.git
```

### 2. Acesse a pasta do projeto

```bash
cd mineracao_dados_pix_fraude
```

### 3. Instale as dependências

```bash
pip install -r requirements.txt
```

### 4. Baixe o dataset

Baixe o dataset no Kaggle:

```text
https://www.kaggle.com/datasets/raphaelnunes/pix-fraud-challenge-v2
```

### 5. Salve o arquivo no Google Drive

Coloque o arquivo `pix_fraud_v2.csv` no seguinte caminho:

```text
MyDrive/Colab Notebooks/Datasets/pix_fraud_v2.csv
```

### 6. Abra o notebook no Google Colab

Abra o notebook no Google Colab e monte o Google Drive.

### 7. Execute as células

Execute as células do notebook em ordem.

---

## Estrutura do Repositório

```text
.
├── README.md
├── requirements.txt
├── .gitignore
└── mineracao_dados_pix_fraude.ipynb
```

O arquivo `pix_fraud_v2.csv` não está incluído no repositório.

---

## Possíveis Melhorias Futuras

Algumas melhorias possíveis para versões futuras do projeto:

- testar modelos adicionais, como Random Forest, XGBoost ou LightGBM;
- aplicar validação temporal mais robusta;
- testar técnicas específicas para dados desbalanceados;
- realizar tuning de hiperparâmetros;
- aprofundar a análise de falsos positivos e falsos negativos;
- criar uma pipeline de pré-processamento mais modular;
- salvar o modelo treinado;
- simular cenários de custo financeiro para diferentes thresholds;
- desenvolver um dashboard de acompanhamento dos resultados;
- propor uma arquitetura conceitual de implantação em ambiente real.

---

## Limitações

Este projeto possui finalidade acadêmica e exploratória.

Os resultados obtidos não devem ser interpretados como uma solução pronta para produção. Em um ambiente real de prevenção a fraudes, seriam necessários controles adicionais, como:

- validação com dados reais atualizados;
- monitoramento contínuo de performance;
- avaliação de drift;
- governança do modelo;
- análise de impacto sobre usuários;
- revisão de aspectos regulatórios e de privacidade;
- integração com regras de negócio e análise humana.

---

## Autor

Desenvolvido por **Marco Antonio Bispo Silva**.

Projeto acadêmico de Data Mining aplicado à detecção de fraudes em transações PIX.