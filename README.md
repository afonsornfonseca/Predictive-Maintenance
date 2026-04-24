# Previsão de Falhas em Máquinas Industriais com Recurso a Machine Learning 
## Identificação da Equipa
* **Grupo nº:** 3
* **Membros:**
* Bernardo Vieira - 2021124221
* Artur Yakovenko - 2023138730
* Afonso Fonseca - 2023141637
## Organização do Repositório
A estrutura deste projeto segue as boas práticas de Ciência de Dados e Engenharia de Software:
* **`data/`**: Armazenamento de dados (dados brutos em `raw/` e processados em `processed/`).
* **`docs/`**: Documentação técnica detalhada dividida por Milestones (M1, M2 e M3).
* **`notebooks/`**: Jupyter Notebooks para experimentação, limpeza e modelação.
* **`src/`**: Código-fonte modular (scripts `.py`) para funções reutilizáveis.
* **`reports/`**: Relatórios finais, apresentações e exportação de figuras (`figures/`).
* **`requirements.txt`**: Ficheiro de configuração com as bibliotecas necessárias.
## 1. Iniciação (Milestone 1)
### Contexto e Problema de Negócio
O presente projeto insere-se no contexto da indústria, onde a operação contínua e eficiente dos equipamentos é fundamental para garantir produtividade e controlo de custos.
Falhas inesperadas em máquinas industriais podem originar paragens não planeadas, aumento de custos de manutenção e perdas significativas de produção.
Neste contexto, o principal desafio consiste em antecipar a ocorrência de falhas com base em dados operacionais, permitindo uma abordagem de manutenção mais eficiente e preventiva.
Assim, este projeto pretende explorar dados de funcionamento de máquinas industriais com o objetivo de prever a ocorrência de falhas e apoiar a tomada de decisão.
### Objetivos do Projeto

- **Objetivo 1:** Desenvolver um modelo de classificação para prever falhas em máquinas industriais, atingindo um F1-Score mínimo de 0.85 até ao Milestone 3.

- **Objetivo 2:** Criar novas variáveis relevantes (diferença térmica e potência) e integrá-las na análise exploratória até ao Milestone 2.

- **Objetivo 3:** Desenvolver um modelo capaz de identificar o tipo de falha, com uma accuracy superior a 80% até ao Milestone 4.

## Perguntas de Investigação

- De que forma o tipo de produto (L, M, H) influencia o desgaste da ferramenta e a ocorrência de falhas?

- Qual a relação entre velocidade de rotação e torque na ocorrência de falhas associadas à potência?

- Como influencia a diferença entre temperatura do processo e ambiente na ocorrência de falhas térmicas?

- Existem padrões de funcionamento que se aproximam de situações de falha sem efetivamente causar avaria?

### Fonte de Dados
* **Dataset:** (https://www.kaggle.com/datasets/afonsornfonseca/ai4i-2020-predictive-maintenance)
* **Dimensão:** 10.000 Instâncias, 14 Colunas
## 2. Exploração (Milestone 2)
### Limpeza e Preparação

- Remoção de variáveis irrelevantes (UID e Product ID).
- Tratamento e validação dos tipos de dados.
- Criação de novas variáveis derivadas:
  - **Temp_diff**: diferença entre temperatura do processo e temperatura do ar;
  - **Power**: produto entre torque e velocidade de rotação.
- Remoção de variáveis associadas a tipos de falha (TWF, HDF, PWF, OSF, RNF) para evitar data leakage.

*Detalhes completos disponíveis em* `docs/M2_exploracao.md`.
### Principais Conclusões (EDA)
<img width="1238" height="974" alt="image" src="https://github.com/user-attachments/assets/7366431c-e5a6-4c3f-9b59-f21f661d850e" />

* **Ponto-chave:** Principais Conclusões (EDA)

- O dataset apresenta um forte desbalanceamento, com predominância da classe "não falha".
- A variável **Torque** apresenta uma relação positiva com a ocorrência de falhas.
- A variável **Power** revelou uma correlação negativa relevante com a variável alvo.
- As variáveis de temperatura apresentam baixa influência direta na ocorrência de falhas.
- Foi identificada uma forte correlação entre **Rotational speed** e **Torque**, indicando possível redundância.

> Os resultados obtidos permitiram compreender melhor os fatores que influenciam a ocorrência de falhas e orientar as decisões de modelação.
## 3. Modelação (Milestone 3)
### Abordagem Técnica
* **Modelos:** Foram avaliados diferentes modelos de classificação supervisionada. Inicialmente, foram implementados modelos baseline (Regressão Logística e Árvore de Decisão) para estabelecer um ponto de referência. Posteriormente, foram testados modelos mais complexos, nomeadamente Random Forest e XGBoost, tendo este último sido selecionado como modelo final após otimização de hiperparâmetros, devido ao seu desempenho superior.
* **Métrica Principal:** A métrica principal utilizada foi o F1-Score, uma vez que permite avaliar o equilíbrio entre *precision* e *recall*. Esta escolha justifica-se pelo desbalanceamento da variável alvo e pela necessidade de minimizar falsos negativos, dado o impacto crítico de falhas não detetadas no contexto de manutenção preditiva.
## 4. Finalização (Milestone 4)
### Resposta ao Problema
[Resumo da solução e como ela gera valor para o negócio.]
### Recomendações de Inovação
1. [Sugestão prática baseada nos resultados]
## Como Reproduzir este Projeto
1. Clone o repositório: `git clone [url-do-repo]`
2. Instale as dependências: `pip install -r requirements.txt`
3. Execute os notebooks na pasta `notebooks/` seguindo a ordem numérica.
**Instituição:** Coimbra Business School | ISCAC
**Curso:** Licenciatura em Ciência de Dados para a Gestão
**Unidade Curricular:** Projeto em Ciência de Dados
**Professor Responsável:** Dora Melo (dmelo@iscac.pt)
