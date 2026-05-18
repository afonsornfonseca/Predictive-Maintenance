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
* **`docs/`**: Documentação técnica detalhada dividida por *Milestones* (M1, M2 e M3).
* **`notebooks/`**: Jupyter Notebooks para experimentação, limpeza e modelação.
* **`src/`**: Código-fonte modular (scripts `.py`) para funções reutilizáveis.
* **`reports/`**: Relatórios finais, apresentações e exportação de figuras (`figures/`).
* **`requirements.txt`**: Ficheiro de configuração com as bibliotecas necessárias.
## 1. Iniciação (*Milestone 1*)
### Contexto e Problema de Negócio
O presente projeto insere-se no contexto da indústria, onde a operação contínua e eficiente dos equipamentos é fundamental para garantir produtividade e controlo de custos.
Falhas inesperadas em máquinas industriais podem originar paragens não planeadas, aumento de custos de manutenção e perdas significativas de produção.
Neste contexto, o principal desafio consiste em antecipar a ocorrência de falhas com base em dados operacionais, permitindo uma abordagem de manutenção mais eficiente e preventiva.
Assim, este projeto pretende explorar dados de funcionamento de máquinas industriais com o objetivo de prever a ocorrência de falhas e apoiar a tomada de decisão.
### Objetivos do Projeto

- **Objetivo 1:** Desenvolver um modelo de classificação para prever falhas em máquinas industriais, atingindo um F1-Score mínimo de 0.85 até ao *Milestone 3*.

- **Objetivo 2:** Criar novas variáveis relevantes (diferença térmica e potência) e integrá-las na análise exploratória até ao *Milestone 2*.

- **Objetivo 3:** Desenvolver um modelo capaz de identificar o tipo de falha, com uma accuracy superior a 80% até ao *Milestone 4*.

## Perguntas de Investigação

- De que forma o tipo de produto (*L, M, H*) influencia o desgaste da ferramenta e a ocorrência de falhas?

- Qual a relação entre velocidade de rotação e torque na ocorrência de falhas associadas à potência?

- Como influencia a diferença entre temperatura do processo e ambiente na ocorrência de falhas térmicas?

- Existem padrões de funcionamento que se aproximam de situações de falha sem efetivamente causar avaria?

### Fonte de Dados
* **Dataset:** (https://www.kaggle.com/datasets/afonsornfonseca/ai4i-2020-predictive-maintenance)
* **Dimensão:** 10.000 Instâncias, 14 Colunas
## 2. Exploração (*Milestone 2*)
### Limpeza e Preparação

- Remoção de variáveis irrelevantes (*UID e Product ID*).
- Tratamento e validação dos tipos de dados.
- Criação de novas variáveis derivadas:
  - **Temp_diff**: diferença entre temperatura do processo e temperatura do ar;
  - **Power**: produto entre torque e velocidade de rotação.
- Remoção de variáveis associadas a tipos de falha (*TWF, HDF, PWF, OSF, RNF*) para evitar *data leakage*.

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
## 3. Modelação (*Milestone 3*)
### Abordagem Técnica
* **Modelos:** Foram avaliados diferentes modelos de classificação supervisionada. Inicialmente, foram implementados modelos baseline (Regressão Logística e Árvore de Decisão) para estabelecer um ponto de referência. Posteriormente, foram testados modelos mais complexos, nomeadamente *Random Forest* e *XGBoost*, tendo este último sido selecionado como modelo final após otimização de hiperparâmetros via GridSearchCV (*5-fold* estratificado), devido ao seu desempenho superior.

* **Métrica Principal:** A métrica de otimização evoluiu ao longo do projeto. O *F1-Score* guiou a fase de *GridSearchCV*, atingindo 75,2% no conjunto de teste. Posteriormente, adotou-se o *F2-Score* (beta=2) como métrica de negócio, que atribui o dobro do peso ao *Recall* face à *Precision*, refletindo a assimetria de custos real: uma avaria não detetada tem impacto incomparavelmente superior ao de uma inspeção desnecessária.
  
* **Ajuste de Limiar:** O limiar de decisão padrão (0,50) foi substituído pelo limiar ótimo 0,3721, identificado pela curva *Precision-Recall* com critério *F2-Score*. Esta calibração elevou o *Recall* de ~63% para 87,0%, reduzindo os falsos negativos para apenas 9 em 2000 instâncias de teste, ao custo controlado de 35 falsos positivos.

## 4. Finalização (*Milestone 4*)
### Resposta ao Problema

O projeto desenvolveu um sistema de Manutenção Preditiva Industrial baseado em *XGBoost*, capaz de prever falhas de equipamento com um Recall de 87% e *F2-Score* de 80,6%, superando os objetivos definidos inicialmente. O modelo analísa cinco variáveis operacionais em tempo real (potência estimada, desgaste da ferramenta, binário, velocidade de rotação e diferença térmica) e emite alertas antes da ocorrência de avarias, permitindo à organização substituir uma política reativa por uma abordagem preditiva e orientada ao risco. Das 68 avarias reais no conjunto de teste, 59 foram detetadas atempadamente, reduzindo os custos de paragem não planeada a uma fração dos valores típicos de manutenção corretiva.

### Recomendações de Inovação
1. Implementar *SMOTE* para geração de instâncias sintéticas de falha e potencialmente elevar o *F1-Score* acima dos 80%;

2. Integrar valores *SHAP* para explicabilidade individual de cada alerta emitido pelo modelo;

3. Incorporar a dimensão temporal dos registos e testar modelos de séries temporais (*LSTM*) para capturar padrões de degradação progressiva;

4. Expandir o modelo para classificação multi-classe dos tipos de falha (*TWF, HDF, PWF, OSF*), identificando o componente em risco e não apenas a falha global;

5. Desenvolver uma interface Streamlit para uso em tempo real por técnicos de manutenção sem conhecimentos de programação.

## Como Reproduzir este Projeto
1. Clone o repositório: `git clone [url-do-repo]`
2. Instale as dependências: `pip install -r requirements.txt`
3. Execute os notebooks na pasta `notebooks/` seguindo a ordem numérica.
**Instituição:** Coimbra Business School | ISCAC
**Curso:** Licenciatura em Ciência de Dados para a Gestão
**Unidade Curricular:** Projeto em Ciência de Dados
**Professor Responsável:** Dora Melo (dmelo@iscac.pt)
