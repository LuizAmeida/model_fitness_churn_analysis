# 🏋️‍♂️ Model Fitness — Análise Preditiva e Estratégias de Retenção de Churn

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit_Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

> **Sprint 13 — Projeto TripleTen**  
> **Autor:** Luiz Almeida  
> **LinkedIn:** [linkedin.com/in/luizmarques84](https://linkedin.com/in/luizmarques84) | **GitHub:** [github.com/LuizAlmeida](https://github.com/LuizAlmeida)

---

## 📌 Visão Geral do Projeto

A rede de academias **Model Fitness** está desenvolvendo uma estratégia baseada em dados para combater o **churn** (rotatividade de clientes). O principal desafio de negócio consiste em identificar precocemente os frequentadores com alta probabilidade de cancelamento e traçar perfis comportamentais claros para orientar estratégias proativas de retenção.

Neste projeto, realizamos uma **Análise Exploratória de Dados (EDA)** detalhada, construímos modelos de **Aprendizado de Máquina Supervisionado** (Regressão Logística e Random Forest) para prever a evasão e aplicamos algoritmos de **Clusterização Não Supervisionada (K-Means)** para segmentar a base de clientes.

---

## 📊 Estrutura e Dicionário de Dados

A base de dados conta com **4.000 registros** e **14 variáveis**, divididas em dados cadastrais e comportamento de uso:

| Variável | Descrição |
| :--- | :--- |
| `gender` | Gênero do cliente (1 = Masculino, 0 = Feminino) |
| `Near_Location` | Se o cliente mora ou trabalha perto da academia (1 = Sim, 0 = Não) |
| `Partner` | Se o cliente é funcionário de uma empresa parceira (1 = Sim, 0 = Não) |
| `Promo_friends` | Se o cliente entrou por indicação de um amigo (1 = Sim, 0 = Não) |
| `Phone` | Se o cliente forneceu número de telefone residencial/celular |
| `Contract_period` | Duração do contrato atual em meses (1, 6 ou 12 meses) |
| `Group_visits` | Se o cliente participa de aulas em grupo (1 = Sim, 0 = Não) |
| `Age` | Idade do cliente |
| `Avg_additional_charges_total` | Gasto total médio em serviços adicionais (cafeteria, massagem, etc.) |
| `Month_to_end_contract` | Meses restantes até o fim do contrato atual |
| `Lifetime` | Tempo de vida do cliente na academia em meses |
| `Avg_class_frequency_total` | Frequência média semanal de visitas no histórico total |
| `Avg_class_frequency_current_month` | Frequência média semanal de visitas no mês atual |
| `Churn` | **Variável Alvo:** Cancelou a matrícula no mês atual (1 = Sim, 0 = Não) |

---

## 🔍 Principais Descobertas da Análise Exploratória (EDA)

A taxa média geral de churn na base analisada é de **26,5%**. Na comparação entre clientes ativos (`Churn = 0`) e clientes que evadiram (`Churn = 1`), observam-se os seguintes padrões:

* ⏳ **Tempo de Contrato:** Clientes que evadem possuem contratos significativamente mais curtos (média de **1,7 meses** vs. **5,7 meses** dos ativos).
* 🏃 **Frequência de Visitas:** A queda na frequência no mês atual é o indicador mais forte de churn. Clientes ativos mantêm média de **2,0 visitas/semana**, enquanto os churners caem para **1,0 visita/semana**.
* 🤝 **Parcerias e Indicações:** Clientes vindos de convênios empresa (`Partner`) ou indicações de amigos (`Promo_friends`) apresentam maior lealdade e menor taxa de cancelamento.
* 💳 **Gastos Adicionais:** Clientes engajados consomem mais serviços extras na academia ($\approx R\$ 158$ vs. $R\$ 115$ dos churners).

---

## 🤖 Modelagem Preditiva (Machine Learning)

Dividimos a base de dados em conjuntos de **Treino (80%)** e **Teste (20%)** com `random_state=42`. Avaliamos dois modelos de classificação:

1. **Regressão Logística** (com padronização prévia via `StandardScaler`)
2. **Random Forest Classifier**

### 📈 Comparativo de Desempenho

| Modelo | Acurácia (Accuracy) | Precisão (Precision) | Sensibilidade (Recall) | F1-Score |
| :--- | :---: | :---: | :---: | :---: |
| **Regressão Logística** | **91,63%** | **87,29%** | **78,22%** | **82,51%** |
| **Random Forest** | 91,00% | 84,95% | 78,22% | 81,44% |

> **Conclusão da Modelagem:** A **Regressão Logística** obteve o melhor desempenho geral, destacando-se na métrica de **Precision (87,29%)** e **F1-Score (82,51%)**, sendo o modelo recomendado para implantação em produção.

---

## 🎯 Segmentação de Clientes (K-Means Clustering)

Após a padronização dos dados com `StandardScaler`, executamos o algoritmo **K-Means** configurado com **5 clusters**:

* 🔴 **Cluster de Alto Risco (Churn ~58%):** Contratos mensais curtos, baixíssima frequência semanal no mês atual e menor engajamento com serviços extras.
* 🟢 **Clusters de Fidelidade (Churn < 5%):** Clientes com tempo de vida elevado (`Lifetime`), planos semestrais/anuais e alta participação em aulas em grupo.

---

## 💡 Recomendações Práticas de Negócio

1. **Protocolo do Primeiro Mês (Onboarding):** A maior parte dos cancelamentos ocorre no 1º mês (`Lifetime` médio = 1). Desenvolver um acompanhamento individualizado nos primeiros 30 dias para garantir que o aluno atinja ao menos 2 treinos por semana.
2. **Incentivo à Conversão de Contratos:** Criar campanhas promocionais que facilitem a transição do plano mensal para planos de 6 ou 12 meses.
3. **Alerta de Frequência em Tempo Real:** Configurar disparos automáticos de e-mail/WhatsApp assim que a frequência semanal do aluno cair abaixo de 1,5 visitas no mês corrente.
4. **Expansão de Convênios Empresariais:** Intensificar parcerias corporativas (`Partner`) e programas do tipo *"Traga um Amigo"* (`Promo_friends`), pois trazem perfis organicamente mais retidos.

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas

* **Linguagem:** Python 3.9+
* **Análise de Dados:** Pandas, NumPy
* **Visualização de Dados:** Matplotlib, Seaborn
* **Machine Learning:** Scikit-Learn (`LogisticRegression`, `RandomForestClassifier`, `KMeans`, `StandardScaler`)
* **Desenvolvimento:** Jupyter Notebook / Google Colab

---

## 🚀 Como Executar o Projeto

1. **Clone este repositório:**
   ````
   git clone [https://github.com/LuizAlmeida/model-fitness-churn-analysis.git](https://github.com/LuizAlmeida/model-fitness-churn-analysis.git)
   cd model-fitness-churn-analysis
   ````

**Autor**: Luiz Almeida
- Projeto Sprint 13 do Bootcamp TripleTen de Análise de Dados
