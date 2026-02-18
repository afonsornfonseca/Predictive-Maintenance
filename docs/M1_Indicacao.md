# Milestone 1: Iniciação e Definição do Projeto
## 1. Descrição Detalhada do Problema
[Expandir a descrição do README. Explicar o contexto do setor (ex: retalho, banca, saúde) e
porque é que este problema é relevante no momento atual.]
## 2. Objetivos SMART
*Defina os objetivos do projeto seguindo a lógica SMART (Específico, Mensurável, Atingível,
Relevante e Temporal):*
1. **Objetivo 1:** [Ex: Reduzir o erro de previsão de stock em 15% até ao final do semestre.]
2. **Objetivo 2:** [Ex: Identificar os 5 principais perfis de consumo através de técnicas de
clustering.]
## 3. Metodologia de Gestão (PBL)
* **Divisão de Tarefas:**
* **Membro A:** Responsável pela Engenharia de Dados.
* **Membro B:** Responsável pela Modelação Estatística.
* **Membro C:** Responsável pela Visualização e Documentação.
* **Ferramentas de Colaboração:** [Ex: GitHub Projects para Kanban, reuniões semanais via
Teams/Discord].
## 4. Análise de Viabilidade dos Dados
* **Disponibilidade:** [Os dados já foram descarregados? Estão em base de dados?]
* **Qualidade Inicial:** [Ex: Notámos que faltam dados de datas em algumas colunas, precisaremos
de tratar isso na M2.]
* **Ética:** [Os dados cumprem o RGPD? Estão anonimizados?]
* **1. Visão Geral e Estrutura dos Dados**
O dataset é composto por 10.000 instâncias (linhas) e 14 variáveis (colunas), apresentando uma integridade de dados impecável, uma vez que não foram detetados valores nulos em nenhuma das categorias. A estrutura combina dados identificativos (como o UDI e o Product ID) com medições físicas cruciais para a manutenção preditiva, tais como temperatura ambiente, temperatura do processo, velocidade de rotação, binário (torque) e o desgaste da ferramenta. Do ponto de vista técnico, os dados estão corretamente tipificados: as variáveis físicas são representadas por números reais (float64) e inteiros (int64), enquanto as categorias qualitativas, como o tipo de produto (Type), são tratadas como objetos.

* **2. Análise Estatística e Operacional**
Os indicadores estatísticos revelam máquinas a operar, em média, com uma temperatura ambiente de aproximadamente 300 K e uma velocidade de rotação média de 1538 rpm. É observável uma dispersão considerável na velocidade de rotação, que atinge um máximo de 2886 rpm, e no torque, que varia entre os 3.8 Nm e os 76.6 Nm. Estas variações são fundamentais, pois o desgaste da ferramenta (que apresenta uma média de 107 minutos) é um dos principais indicadores de stress mecânico acumulado nos equipamentos.

* **3. Diagnóstico de Falhas e Alvos de Previsão**
A variável alvo principal, Machine failure, indica que a taxa de falha no conjunto de dados é de aproximadamente 3.39%. Este valor demonstra um cenário de dados desbalanceados, onde as situações de avaria são raras face ao funcionamento normal. O dataset é particularmente rico por detalhar os modos de falha específicos que o modelo de Machine Learning deverá identificar:

HDF (Dissipação de Calor): A causa mais frequente entre as falhas listadas (1.15%).

OSF (Excesso de Esforço): Representa 0.98% das ocorrências.

PWF (Falha de Potência): Registada em 0.95% dos casos.

TWF (Desgaste da Ferramenta): Incidência de 0.46%.

RNF (Falhas Aleatórias): Apenas 0.19%, representando eventos imprevisíveis.
## 5. Cronograma Interno
| Fase | Data Limite | Entregável Esperado |
| :--- | :--- | :--- |
| M1: Iniciação | [Data] | Repositório estruturado e Plano de Projeto. |
| M2: Exploração | [Data] | Notebook de EDA e Dados Processados. |
| M3: Modelação | [Data] | Comparação de algoritmos e métricas. |
| M4: Finalização| [Data] | Pitch e Relatório Final. |
---
*Data de última atualização: [18/02/2026]*
