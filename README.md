# Projeto de Previsão de Evasão em EdTech

---

## 🚀 Visão Geral

Este projeto aborda o desafio da **alta taxa de evasão de alunos nos primeiros 30 dias** em uma EdTech que oferece cursos online. Nosso objetivo é desenvolver uma solução de Ciência de Dados e Machine Learning capaz de **prever quais alunos têm maior risco de abandono precoce**, fornecendo insights acionáveis para implementar estratégias de retenção eficazes.

O trabalho está estruturado em quatro fases principais:

1.  **Entendimento e Exploração dos Dados (EDA):** Análise detalhada do dataset para identificar padrões e comportamentos.
2.  **Construção do Modelo de Machine Learning:** Desenvolvimento, treinamento e comparação de algoritmos de classificação.
3.  **Interpretação e Aplicação no Negócio:** Tradução dos resultados técnicos em recomendações estratégicas para as equipes da EdTech.
4.  **Desafio Prático de Análise:** Resolução de um problema de dados com artefatos propositais e entrega rápida (simulação de cenário real).

---

## 📊 Dados

O dataset utilizado contém informações de **500 alunos**, abrangendo diversas variáveis:

* **Dados demográficos:** idade, gênero, nível educacional.
* **Informações do curso:** categoria do curso, tipo de assinatura.
* **Métricas de engajamento na plataforma:** dias ativo na primeira semana, minutos por sessão, total de sessões, posts em fórum, mensagens para tutores.
* **Desempenho inicial:** conclusão do primeiro quiz.
* **Tipo de dispositivo utilizado.**
* **Variável alvo:** `evadiu_30_dias` (indicando se o aluno abandonou o curso nos primeiros 30 dias).

---

## 📂 Estrutura do Repositório

O repositório está organizado da seguinte forma:
├── 01_Analise_Exploratoria_Evasao.ipynb

├── 02_Modelagem_Evasao.ipynb

├── Relatorio_Evasao_EdTech.pdf 

├── edtech_dropout_dataset_tratado

└── databases/

└── edtech_dropout_dataset.csv  

---

## 🔬 Parte 1: Entendimento e Exploração dos Dados (EDA)

O notebook `01_Analise_Exploratoria_Evasao.ipynb` contém a análise exploratória detalhada.

### Principais Insights da EDA:

* **Desbalanceamento da Classe Alvo:** A variável `evadiu_30_dias` é significativamente desbalanceada, com aproximadamente **84% de alunos que não evadiram** e **16% que evadiram**. Isso foi crucial para as escolhas na fase de modelagem.
* **Tratamento de Dados:**
    * A coluna `education_level` (nível educacional) foi a única com dados ausentes e foi tratada com a criação de uma nova categoria "Não Informado".
    * Outliers em variáveis numéricas de engajamento foram capeados no percentil 95.
    * Todas as colunas e valores categóricos foram traduzidos para o português para maior clareza.
* **Padrões de Risco Preliminares:**
    * **Baixo número de `total_sessoes`** e a **não conclusão do `primeiro_quiz`** são fortes indicadores de risco.
    * Alunos que utilizam **"Tablet" ou "Desktop"** como dispositivo e os inscritos na **categoria de curso "Design"** mostraram maior propensão à evasão.
    * Assinantes **"Grátis"** e com **nível educacional "Não Informado" ou "Ensino Médio"** também apresentaram taxas de evasão mais elevadas.

---

## 🤖 Parte 2: Construção do Modelo de Machine Learning

O notebook `02_Modelagem_Evasao.ipynb` documenta o processo de desenvolvimento e avaliação dos modelos de Machine Learning.

### Pipeline de Modelagem:

1.  **Pré-processamento:** As features categóricas foram transformadas usando One-Hot Encoding, e as numéricas escalonadas com `StandardScaler`.
2.  **Tratamento de Desbalanceamento:** Foi aplicada a técnica **SMOTE (Synthetic Minority Over-sampling Technique)** no conjunto de treino para balancear as classes e melhorar o desempenho na identificação da classe minoritária.
3.  **Algoritmos Comparados:** Foram avaliados diversos algoritmos de classificação, incluindo **Regressão Logística, Árvore de Decisão, LightGBM** (em diferentes configurações) e **KNN (K-Nearest Neighbors)**.
4.  **Métricas de Avaliação:** A avaliação focou em **Recall, Precisão e F1-Score** para a classe "Evadiu", além de AUC-ROC e Acurácia Geral, visando balancear a identificação correta de evasões com a minimização de falsos positivos.

### Melhor Modelo Selecionado:

O modelo **LightGBM Ajustado (com SMOTE e threshold otimizado em 0.4192)** foi selecionado por apresentar o melhor equilíbrio de desempenho para a classe "Evadiu".

| Métrica                 | Valor    | Descrição                                                                         |
| :---------------------- | :------- | :-------------------------------------------------------------------------------- |
| **Recall (Evadiu)** | **0.25** | O modelo identifica 25% dos alunos que realmente evadirão.                          |
| **Precisão (Evadiu)** | **0.67** | Quando o modelo prevê que um aluno vai evadir, ele está correto em 67% das vezes. |
| **F1-Score (Evadiu)** | **0.36** | Média harmônica entre Recall e Precisão para a classe "Evadiu".                     |
| **AUC-ROC** | **0.5990** | Mede a capacidade de discriminação do modelo entre as classes.                      |
| Acurácia Geral          | 0.86     | A proporção total de previsões corretas (para ambas as classes).                   |

### Interpretabilidade do Modelo:

As análises de importância de features (LightGBM) e SHAP (SHapley Additive exPlanations) indicaram que as variáveis mais influentes para a previsão de evasão são `media_minutos_por_sessao`, `total_sessoes` e `idade`. Valores mais baixos de sessões e minutos tendem a aumentar a probabilidade de evasão.

---

## 📈 Parte 3: Interpretação e Aplicação no Negócio

Este relatório (`Relatorio_Evasao_EdTech.pdf`) traduz os achados técnicos em **recomendações estratégicas e acionáveis** para as equipes pedagógica e de produto da EdTech.

### Pontos Chave Abordados no Relatório:

* **Perfis de Alunos em Risco:** Detalhamento dos perfis com maior propensão à evasão.
* **Ações de Retenção Sugeridas:** Propostas de intervenções proativas, como notificações inteligentes, contato de tutores e otimização da experiência, baseadas nos perfis de risco.
* **Variáveis para Monitoramento:** Quais dados devem ser priorizados na coleta e monitoramento contínuo para refinar as estratégias.
* **Potencial e Limitações do Modelo:** Explica como o modelo pode ser integrado para disparar alertas e acionar tutores, destacando as limitações importantes como **Falsos Negativos (75% das evasões não detectadas)** e **Falsos Positivos (1 em cada 3 previsões de evasão é incorreta)**, bem como a necessidade de validação contínua.

---

## 🔧 Parte 4: Desafio Prático de Análise

Esta seção do projeto (a ser desenvolvida em um notebook futuro) abordará um cenário prático com um novo conjunto de dados contendo problemas propositais. O objetivo é demonstrar a capacidade de:

* Identificar e tratar rapidamente inconsistências nos dados (ex: nomes de variáveis diferentes, dados futuros no passado).
* Treinar um modelo base de forma ágil.
* Escolher um threshold de classificação ideal e justificar essa decisão com base nas métricas de negócio.
* Comunicar as decisões de forma clara e concisa.

---

## ⚙️ Como Rodar o Projeto (Instruções Locais)

Para replicar esta análise e modelos em sua máquina local, siga os passos abaixo:

1.  **Clone o Repositório:**
    ```bash
    git clone [https://github.com/SeuUsuario/edtech-evasao-retencao.git](https://github.com/SeuUsuario/edtech-evasao-retencao.git)
    cd edtech-evasao-retencao
    ```
    *(Lembre-se de substituir `SeuUsuario` pelo seu nome de usuário no GitHub no comando acima e também na seção de Contato abaixo).*
2.  **Crie e Ative um Ambiente Virtual (Recomendado):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # No Windows: .\venv\Scripts\activate
    ```
3.  **Instale as Dependências:**
    ```bash
    pip install -r requirements.txt
    ```
    *(Crie um arquivo `requirements.txt` na raiz do seu projeto contendo todas as bibliotecas usadas, por exemplo: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `lightgbm`, `imbalanced-learn`, `shap`)*
4.  **Execute os Notebooks Jupyter:**
    ```bash
    jupyter notebook
    ```
    Abra os notebooks `01_Analise_Exploratoria_Evasao.ipynb` e `02_Modelagem_Evasao.ipynb` e execute as células sequencialmente para reproduzir a análise e os resultados.

---

