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
* **Objetivo 1:** - Prever falhas em máquinas industriais.
* **Objetivo 2:** - Identificar os principais fatores de influência.
### Fonte de Dados
* **Dataset:** (https://www.kaggle.com/datasets/afonsornfonseca/ai4i-2020-predictive-maintenance)
* **Dimensão:** 10.000 Instâncias, 14 Colunas
## 2. Exploração (Milestone 2)
### Limpeza e Preparação
* ### Limpeza e Preparação

- Remoção de variáveis irrelevantes (UID e Product ID).
- Tratamento e validação dos tipos de dados.
- Criação de novas variáveis derivadas:
  - **Temp_diff**: diferença entre temperatura do processo e temperatura do ar;
  - **Power**: produto entre torque e velocidade de rotação.
- Remoção de variáveis associadas a tipos de falha (TWF, HDF, PWF, OSF, RNF) para evitar data leakage.

*Detalhes completos disponíveis em* `docs/M2_exploracao.md`.
### Principais Conclusões (EDA)
> *Dica: Insere aqui o gráfico mais importante do projeto.*
* **Ponto-chave:** [Ex: Identificámos que o fator X influencia em 40% o resultado Y, por aplicação
do método ganho de informação]
## 3. Modelação (Milestone 3)
### Abordagem Técnica
* **Modelos:** [Ex: Random Forest e XGBoost]
* **Métrica Principal:** [Ex: F1-Score ou RMSE]
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
