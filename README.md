**(EN)**

# 🕵️‍♂️ Risk Analyst Case – Suspicious Behavior Detection and Anti-fraud Solution

## 📌 Objective
This project aims to analyze a **hypothetical transaction** database in a **card-not-present (CNP)** environment, identify **suspicious behaviors** and propose **anti-fraud solutions**.

---

## 📁 Project Structure
├── data/
│ ├── raw/
| │ ├── transactional-sample.csv # Provided database
│ ├── processed/
| │ ├── device_worst.csv # Device blacklist
| │ ├── merchant_worst.csv # Merchant blacklist
| │ ├── user_worst.csv # User blacklist
| │ ├── train_processed.csv # Processed training base
| │ ├── train_processed.csv # Processed training base
├── notebooks/
│ ├── eda_feature_engineering.ipynb # Exploration and feature engineering notebook
│ ├── train_model.ipynb # Notebook for ML model training and evaluation
├── README.md # This document

---

## 🧠 Challenge Description
The challenge consists of:
1. Analyzing the dataset to identify **fraud patterns**.
2. Presenting **insights and hypotheses** about suspicious behaviors.
3. Suggesting **additional data** useful for strengthening fraud detection.
4. Proposing **preventive measures** and a **conceptual anti-fraud system**.
5. Explaining **flows and roles** in the payments industry.

---

## 📊 1. Exploratory Data Analysis (EDA)

### 📌 Main variables
- `transaction_id`: transaction identifier
- `card_number`: card number
- `transaction_date`: transaction timestamp
- `user_id`: cardholder identifier
- `device_id`: device identifier
- `merchant_id`: merchant identifier
- `transaction_amount`: transaction amount
- `has_cbk`: indicates if there was a chargeback (1 = fraud)

### 🔍 Questions investigated
- Do frauds occur with **higher average values**?
  A: Yes, it can be confirmed that the average value of fraudulent transactions is more than double that of normal transactions

- Are there **merchants**, **users** or **devices** with high fraud concentration?
  A: Yes, there is a small audience of each of these ids that concentrates more than 70% of frauds

- Are there **temporal patterns** (day of the week, time of day)?
  A: A pattern of frauds concentrated at night and on Fridays is noted.

- Is transaction behavior **consistent with user history**?
  A: There is clearly a deviation from the user's standard behavior for fraudulent transactions.

- Is **transaction volume** correlated with fraud percentage?
  A: Apparently there is no relationship between transaction volume and fraud percentage, but it would be more assertive to define by analyzing a larger dataset.

---

### ⚙️ Feature Engineering

#### 🧮 Created variables
| Feature | Description |
|----------|------------|
| `transaction_hour` | Transaction hour |
| `transaction_day` | Transaction day of the week |
| `transaction_period` | Morning, Afternoon, Night, Dawn |
| `mean_amount_user` | User average |
| `median_amount_user` | User median |
| `ratio_to_user_mean` | Ratio between transaction and user average |
| `diff_from_user_mean` | Difference from user average X transaction value|
| `mean_amount_day` | Global average by day of the week |
| `median_amount_day` | Global median by day of the week |
| `diff_from_mean_day` | Difference from day average X transaction value|
| `ratio_to_mean_day` | Ratio to day average |
| `mean_amount_period` | Global average by period |

These variables help capture **behavioral deviations**, fundamental in fraud detection.

---

## 💰 2. Data Enrichment

Aiming to improve fraud detection, I suggest enriching the database with the following variables:

- IP address (geolocation, multiple IPs per user)
- Card BIN (brand, country)
- User history (account age, usage frequency)

---

## 🤖 3. Modeling

I used **Machine Learning** methods to capture fraudulent operation patterns within the available data.
Three ML algorithms were tested:

- Isolation Forest
- XGBoost
- Random Forest

I only used tree models as they are the most used in the financial market and have better performance with data with high collinearity.

The algorithm that obtained the best performance was **XGBoost** with the following metrics (test base):

### Basic metrics:

| Metric | Value |
| --------|---------- |
| `Accuracy` | 0.9313 |
| `Precision` | 0.9048 |
| `Recall` | 0.4872 |
| `F1-score` | 0.6333 |
| `AUC` | 0.9121 |
| `KS` | 0.8781 |

### Confusion Matrix

![alt text](image.png)

### ROC Curve

![alt text](image-1.png)

---

## 🧱 4. Anti-fraud Recommendations

### 🧭 Short Term (Rules)
- Block transactions with `transaction_amount` much above user average.
- Review merchants with **high chargeback rates** (blacklist).
- Block or review **repeat devices** (blacklist).

### 🤖 Medium Term (Modeling)
- Train **supervised model** (Isolation Forest / XGBoost) with created features.
- Use **risk score system** with thresholds.

### 🧠 Long Term (Architecture)
- **Real-time pipeline** with:
  1. Transaction ingestion
  2. Enrichment (IP, geolocation, history, blacklists)
  3. Score calculation
  4. Automatic decision (approve / review / block)
- **Feedback loop** with chargebacks to retrain models.

The use case is located in the **train_model.csv** notebook with cut-point analysis and actual values

---

## 🏦 5. Payment Industry Context

### 💰 Financial Flow
1. **Customer** makes the purchase
2. **Gateway** sends data to **(sub-)acquirer**
3. **Acquirer** sends to **brand (Visa, Master)**
4. **Brand** consults the **issuer** (customer's bank)
5. Response: **authorization or denial**
6. Settlement → value to **merchant**

### 📡 Information Flow
- Transactions traffic through multiple players
- Logs and metadata feed anti-fraud systems

### 🧱 Differences between Players
| Player | Function | Risk |
|--------|--------|-------|
| **Acquirer** | Processes and settles transactions | Assumes risk |
| **Sub-acquirer** | Intermediates merchants | Partial risk |
| **Gateway** | Only routes requests | Does not assume risk |

### ⚠️ Chargebacks
- **Chargeback**: forced return of value to customer (dispute or fraud)
- **Cancellation**: voluntary reversal
- High chargeback rate → indicator of **fraud** or **operational inefficiency**

### 🧠 Anti-fraud
- System that analyzes transactions in real time
- Calculates score based on rules and models
- Triggers manual review, blocking or approval

---

## 📚 Technologies Used
- **Python 3.11**
- **Pandas / NumPy** (data analysis)
- **Matplotlib / Seaborn** (visualization)
- **Scikit-learn** (ML, preprocessing)

---

## 🧾 Conclusion
The analysis demonstrated that **simple statistical patterns**, combined with **machine learning models** and **business rules**, are capable of identifying **potentially fraudulent behaviors**.
The next step is **integrating these insights** into an **operational anti-fraud pipeline**, with **supervised models** and **continuous review based on chargebacks**.

---

## 👤 Author
**João Victor Cardoso**
📧 [joaovictorcs.20@gmail.com]
💼 [[LinkedIn](https://www.linkedin.com/in/jo%C3%A3o-victor-cardoso/) / [GitHub](https://github.com/joaovcardosodev) ]

--- 

**PT**

# 🕵️‍♂️ Risk Analyst Case – Detecção de Comportamentos Suspeitos e Solução Antifraude

## 📌 Objetivo
Este projeto tem como objetivo analisar uma base de **transações hipotéticas** em ambiente **card-not-present (CNP)**, identificar **comportamentos suspeitos** e propor **soluções antifraude**.

---

## 📁 Estrutura do Projeto
├── data/
│ ├── raw/
| │ ├── transactional-sample.csv # Base de dados fornecida
│ ├── processed/
| │ ├── device_worst.csv # Blacklist de dispositivos
| │ ├── merchant_worst.csv # Blacklist de comerciantes
| │ ├── user_worst.csv # Blacklist de usuários
| │ ├── train_processed.csv # Base de treino processada
| │ ├── train_processed.csv # Base de treino processada
├── notebooks/
│ ├── eda_feature_engineering.ipynb # Notebook de exploração de feature engineering
│ ├── train_model.ipynb # Notebook para treinamento e avaliação de modelos ML  
├── README.md # Este documento

---

## 🧠 Descrição do Desafio
O desafio consiste em:
1. Analisar o dataset para identificar **padrões de fraude**.
2. Apresentar **insights e hipóteses** sobre comportamentos suspeitos.
3. Sugerir **dados adicionais** úteis para fortalecer a detecção de fraude.
4. Propor **medidas preventivas** e um **sistema antifraude conceitual**.
5. Explicar **fluxos e papéis** na indústria de pagamentos.

---

## 📊 1. Análise Exploratória (EDA)

### 📌 Variáveis principais
- `transaction_id`: identificador da transação
- `card_number`: número do cartão
- `transaction_date`: timestamp da transação  
- `user_id`: identificador do portador do cartão  
- `device_id`: identificador do dispositivo  
- `merchant_id`: identificador do comerciante  
- `transaction_amount`: valor da transação  
- `has_cbk`: indica se houve chargeback (1 = fraude)  

### 🔍 Perguntas investigadas
- Fraudes ocorrem com **valores médios mais altos**?
  R: Sim, pode-se constatar que a média dos valores das transações fraudulentas é mais que o dobro do que transações normais

- Existem **merchants**, **users** ou **devices** com alta concentração de fraudes?
  R: Sim, existe um público pequeno de cada um desses ids que concentra mais que 70% das fraudes

- Há **padrões temporais** (dia da semana, período do dia)?  
  R: Nota-se um padrão de fraudes concentradas a noite e nas sextas-feiras.

- Comportamento da transação é **coerente com o histórico do usuário**?
  R: Existe claramente um desvio do comportamento padrão do usuário para transações fraudulentas.

- O **volume de transações** está correlacionado com o percentual de fraudes?
  R: Aparentemente não existe relação entre o volume de transações e o percentual de fraudes, mas ficaria mais assertivo definir analisando um dataset maior.

---

### ⚙️ Engenharia de Features

#### 🧮 Variáveis criadas
| Feature | Descrição |
|----------|------------|
| `transaction_hour` | Hora da transação |
| `transaction_day` | Dia da semana da transação |
| `transaction_period` | Manhã, Tarde, Noite, Madrugada |
| `mean_amount_user` | Média do usuário |
| `median_amount_user` | Mediana do usuário |
| `ratio_to_user_mean` | Relação entre transação e média do usuário |
| `diff_from_user_mean` | Diferença da média do usuário X valor da transação|
| `mean_amount_day` | Média global por dia da semana |
| `median_amount_day` | Mediana global por dia da semana |
| `diff_from_mean_day` | Diferença da média do dia X valor da transação|
| `ratio_to_mean_day` | Relação com média do dia |
| `mean_amount_period` | Média global por período |

Essas variáveis ajudam a capturar **desvios de comportamento**, fundamentais na detecção de fraudes.

---

## 💰 2. Enriquecimento de Dados

Visando melhorar a detecção de fraudes sugiro o enriquecimento da base de dados com as seguintes variáveis:

- IP address (geolocalização, múltiplos IPs por usuário)
- BIN do cartão (bandeira, país)
- Histórico do usuário (idade da conta, frequência de uso)

---

## 🤖 3. Modelagem

Utilizei métodos de **Machine Learning** para capturar padrões de operações fraudulentas dentro dos dados disponíveis.
Foram testados três algoritmos de ML sendo eles:

- Isolation Forest
- XGBoost
- Random Forest

Usei apenas modelos de árvores já que são os mais utilizados dentro do mercado financeiro e têm melhor desempenho com dados com alta colinearidade.

O algortimo que obteve o melhor desempenho foi o **XGBoost** com as seguintes métricas (base de teste):

### Métricas basicas:

| Métrica | Valor |
| --------|---------- |
| `Accuracy` | 0.9313 |
| `Precision` | 0.9048 |
| `Recall` | 0.4872 |
| `F1-score` | 0.6333 |
| `AUC` | 0.9121 |
| `KS` | 0.8781 |

### Matriz de Confusão

![alt text](image.png)

### ROC Curve

![alt text](image-1.png)

---

## 🧱 4. Recomendações Antifraude

### 🧭 Curto Prazo (Regras)
- Bloquear transações com `transaction_amount` muito acima da média do usuário.
- Revisar merchants com **alta taxa de chargebacks** (blacklist).
- Bloquear ou revisar **devices** reincidentes (blacklist).

### 🤖 Médio Prazo (Modelagem)
- Treinar **modelo supervisionado** (Isolation Forest / XGBoost) com as features criadas.
- Utilizar **sistema de score de risco** com thresholds.

### 🧠 Longo Prazo (Arquitetura)
- **Pipeline real-time** com:
  1. Ingestão da transação
  2. Enriquecimento (IP, geolocalização, histórico, blacklists)
  3. Cálculo de score
  4. Decisão automática (aprovar / revisar / bloquear)
- **Feedback loop** com chargebacks para re-treinar modelos.

O caso de uso está localizado no notebook **train_model.csv** com analise de ponto de corte e valores reais

---

## 🏦 5. Contexto da Indústria de Pagamentos

### 💰 Fluxo Financeiro
1. **Cliente** realiza a compra  
2. **Gateway** envia dados para **(sub-)acquirer**  
3. **Acquirer** envia para **bandeira (Visa, Master)**  
4. **Bandeira** consulta o **emissor** (banco do cliente)  
5. Resposta: **autorização ou negação**  
6. Liquidação → valor ao **merchant**

### 📡 Fluxo de Informação
- Transações trafegam por múltiplos players
- Logs e metadados alimentam sistemas antifraude

### 🧱 Diferenças entre Players
| Player | Função | Risco |
|--------|--------|-------|
| **Acquirer** | Processa e liquida transações | Assume risco |
| **Sub-acquirer** | Intermedia merchants | Risco parcial |
| **Gateway** | Só roteia requisições | Não assume risco |

### ⚠️ Chargebacks
- **Chargeback**: devolução forçada do valor ao cliente (disputa ou fraude)
- **Cancelamento**: reversão voluntária
- Alta taxa de chargebacks → indicador de **fraude** ou **ineficiência operacional**

### 🧠 Antifraude
- Sistema que analisa transações em tempo real
- Calcula score com base em regras e modelos
- Acionamento de revisão manual, bloqueio ou aprovação

---

## 📚 Tecnologias Utilizadas
- **Python 3.11**
- **Pandas / NumPy** (análise de dados)
- **Matplotlib / Seaborn** (visualização)
- **Scikit-learn** (ML, pré-processamento)

---

## 🧾 Conclusão
A análise demonstrou que **padrões estatísticos simples**, combinados com **modelos de machine learning** e **regras de negócio**, são capazes de identificar **comportamentos potencialmente fraudulentos**.  
O próximo passo é a **integração desses insights** em um **pipeline antifraude operacional**, com **modelos supervisionados** e **revisão contínua baseada em chargebacks**.

---

## 👤 Autor
**João Victor Cardoso**  
📧 [joaovictorcs.20@gmail.com]  
💼 [[LinkedIn](https://www.linkedin.com/in/jo%C3%A3o-victor-cardoso/) / [GitHub](https://github.com/joaovcardosodev) ]