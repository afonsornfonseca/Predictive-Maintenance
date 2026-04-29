# Milestone 1: Iniciação e Definição do Projeto

## 1. Descrição Detalhada do Problema

### Contexto e Relevância

A manufatura industrial atravessa uma transformação profunda impulsionada pela Indústria 4.0, onde a recolha contínua de dados operacionais através de sensores IoT (*Internet of Things*) permite uma visibilidade sem precedentes sobre o estado dos equipamentos em tempo real. Neste contexto altamente competitivo, a minimização de paragens não planeadas na linha de produção é um fator crítico para a sustentabilidade e rentabilidade das organizações industriais.

Tradicionalmente, a gestão da fiabilidade de equipamentos assenta em duas abordagens subótimas:

- **Manutenção reativa** (*corrective maintenance*): intervenção apenas após a ocorrência de falha, o que resulta em paragens inesperadas, perdas de produção e custos de reparação elevados.
- **Manutenção preventiva** (*preventive maintenance*): intervenção calendarizada com base em intervalos de tempo fixos, independentemente do estado real do equipamento, o que gera desperdício de recursos ao substituir componentes que ainda possuem vida útil considerável.

A transição para a **Manutenção Preditiva** (*Predictive Maintenance*), suportada por algoritmos de *Machine Learning* e pela análise multivariável de dados de sensores (temperatura, binário e velocidade de rotação), surge como a solução tecnológica capaz de antecipar estados de pré-falha e otimizar a intervenção nos equipamentos.

### O Dataset e a Variável Objetivo

O presente projeto utiliza o dataset **AI4I 2020 Predictive Maintenance Dataset**, disponível no *Kaggle*, que simula dados operacionais de uma máquina industrial com 10.000 registos e 14 variáveis. A variável objetivo central é `Machine failure`, uma variável binária que indica se ocorreu ou não uma falha, sendo complementada por cinco indicadores de tipos específicos de falha: desgaste da ferramenta (*TWF*), dissipação de calor (*HDF*), potência inadequada (*PWF*), esforço excessivo (*OSF*) e falha aleatória (*RNF*).

A abordagem é **supervisionada**, uma vez que a variável objetivo está definida no conjunto de dados, permitindo treinar e avaliar modelos de classificação com base em exemplos rotulados de falha e não-falha.

---

## 2. Objetivos SMART

Os objetivos do projeto seguem a lógica *SMART* (Específico, Mensurável, Atingível, Relevante e Temporal) e visam tornar transparente para o modelo de *Machine Learning* as causas subjacentes à variável `Machine failure`, modelando matematicamente os modos de falha independentes presentes no *dataset*.

**Objetivo 1:** Desenvolver um modelo de classificação binária que preveja a ocorrência de falha na variável `Machine failure`, atingindo um *F1-Score* mínimo de 0,85 na classe positiva (falha), com o objetivo de minimizar paragens operacionais não planeadas.

Este objetivo enquadra-se no problema de negócio central: antecipar a falha antes que esta ocorra. O *F1-Score* foi escolhido como métrica principal por penalizar tanto os falsos positivos como os falsos negativos, sendo adequado ao desequilíbrio de classes presente no *dataset* (~3,4% de falhas).

**Objetivo 2:** Construir uma *pipeline* de Engenharia de Variáveis que extraia, pelo menos, duas novas métricas físicas baseadas nas regras de operação do equipamento, nomeadamente a diferença térmica (relevante para a previsão de *HDF*) e a potência calculada em rad/s (relevante para *PWF*).

Este objetivo fundamenta-se nas regras físicas que determinam cada modo de falha no *dataset*. A criação explícita destas variáveis derivadas visa aumentar a capacidade preditiva dos modelos ao representar diretamente as relações causais entre os sensores e as falhas.

**Objetivo 3:** Desenvolver um modelo de classificação multiclasse capaz de distinguir e diagnosticar a causa raiz da falha entre os modos específicos (*TWF*, *HDF*, *PWF*, *OSF* e *RNF*), atingindo uma percentagem de Exatidão (*Accuracy*) superior a 80%.

Além de prever *se* ocorre uma falha (Objetivo 1), este objetivo visa identificar *qual* o tipo de falha, fornecendo informação acionável para a manutenção. Uma Exatidão superior a 80% foi definida como critério de sucesso realista, tendo em conta o nível de sobreposição entre algumas classes de falha e a reduzida dimensão de algumas delas.

---

## 3. Perguntas de Investigação

As perguntas de investigação foram formuladas em alinhamento com os Objetivos SMART, visando ser respondidas com base nos modelos desenvolvidos e na análise dos dados, e não apenas pela exploração estatística descritiva.

1. **De que forma a variante de qualidade da máquina (*L*, *M*, *H*) influencia a taxa de acumulação de desgaste da ferramenta (*Tool wear*) e altera os limiares críticos de falha por esforço excessivo (*OSF*)?**
   Espera-se que os modelos e a análise exploratória revelem diferenças significativas na distribuição do desgaste e nos padrões de *OSF* entre os três tipos de produto.

2. **Qual a combinação de velocidade de rotação e binário (*Torque*) que mais frequentemente resulta numa falha de potência (*PWF*), e essa relação é capturada pela variável de potência calculada (Objetivo 2)?**
   Esta pergunta valida diretamente a utilidade da *feature* derivada no Objetivo 2 e será respondida pela análise de importância de variáveis no modelo.

3. **De que forma a diferença entre a temperatura do processo e a temperatura ambiente interage com a velocidade de rotação na previsão de falhas por dissipação de calor (*HDF*)?**
   Esta pergunta explora a relação causal entre variáveis térmicas e mecânicas, e será respondida tanto pela análise exploratória como pela interpretação do modelo treinado.

4. **Existem padrões operacionais (combinações de desgaste, temperatura e binário) que operam próximo dos limiares críticos de falha sem a registar, constituindo potenciais falsos alarmes na operação da máquina?**
   Esta questão orienta a análise de fronteiras de decisão dos modelos e pode ter impacto direto na calibração dos alertas de manutenção preditiva.

---

## 4. Metodologia

O desenvolvimento do projeto segue a metodologia **CRISP-DM** (*Cross-Industry Standard Process for Data Mining*), uma abordagem estruturada e iterativa amplamente utilizada em projetos de Ciência de Dados, organizada nas seguintes fases:

| Fase CRISP-DM | Descrição | Milestone |
|:---|:---|:---|
| *Business Understanding* | Definição do problema, objetivos e critérios de sucesso | M1 |
| *Data Understanding* | Análise inicial da estrutura, qualidade e variáveis do *dataset* | M1/M2 |
| *Data Preparation* | Limpeza, transformação e criação de novas variáveis (*feature engineering*) | M2 |
| *Modeling* | Aplicação e comparação de algoritmos de *Machine Learning* | M3 |
| *Evaluation* | Análise do desempenho dos modelos com métricas adequadas | M3/M4 |

---

## 5. Metodologia de Gestão (PBL)

**Divisão de Tarefas:**

- **Afonso Fonseca:** `README.md`, *Kaggle Notebook*, `M1_iniciacao.md`
- **Artur Yakovenko:** `.gitignore`, `M1_iniciacao.md`
- **Bernardo Vieira:** `requirements.txt`, pasta `docs/`, `M1_iniciacao.md`

**Ferramentas de Colaboração:** *GitHub* (controlo de versões e revisão de código), *Kaggle* (desenvolvimento e execução dos *notebooks*), reuniões semanais via *Discord*.

---

## 6. Análise de Viabilidade dos Dados

### Disponibilidade

O *dataset* utilizado é o **AI4I 2020 Predictive Maintenance Dataset**, descarregado a partir da plataforma *Kaggle*:
> https://www.kaggle.com/datasets/afonsornfonseca/ai4i-2020-predictive-maintenance

Os dados encontram-se em formato CSV, o que é adequado à sua dimensão (10.000 registos e 14 variáveis) e ao âmbito do projeto, permitindo leitura e manipulação direta com *Pandas*/*NumPy* dentro do *notebook*. O *dataset* não está armazenado em base de dados relacional, não sendo necessário para o âmbito deste projeto.

### Qualidade Inicial dos Dados

Uma inspeção inicial ao ficheiro CSV revelou os seguintes aspetos:

**Pontos positivos:**
- Ausência total de valores em falta (*missing values*) em todas as colunas.
- Ausência de linhas duplicadas.
- Tipos de dados coerentes com o esperado: variáveis numéricas para medições de sensores, variáveis binárias para indicadores de falha.

**Desafios identificados:**

**Desequilíbrio da variável objetivo** (*class imbalance*): A variável `Machine failure` é altamente desequilibrada — existem apenas 339 casos de falha num total de 10.000 registos (≈3,4% da classe positiva). Num conjunto de dados desequilibrado, a maioria dos registos pertence a uma classe (sem falha), o que pode levar um modelo a simplesmente prever sempre "sem falha" e ainda assim obter uma *Accuracy* elevada. Por esse motivo, métricas como o *F1-Score* e a Matriz de Confusão são mais adequadas, e poderão ser exploradas técnicas de compensação como a ponderação de classes ou reamostragem (*SMOTE*), a avaliar na fase M2/M3.

**Inconsistências entre `Machine failure` e os tipos de falha específicos:** Numa verificação preliminar, observou-se que a correspondência entre a variável `Machine failure` e a presença dos indicadores *TWF*, *HDF*, *PWF*, *OSF* e *RNF* não é perfeita em todos os registos. Existem casos em que um tipo de falha específico está marcado sem que `Machine failure` esteja ativo, e vice-versa. Esta situação será analisada em detalhe na fase M2 para determinar se se trata de uma regra definida pelo *dataset* (por exemplo, no tratamento especial da falha *RNF*) ou de inconsistências que exijam decisões de pré-processamento.

**Variáveis de identificação:** As colunas `UDI` e `Product ID` funcionam como identificadores únicos e, por norma, não acrescentam valor preditivo. Serão avaliadas na fase M2 para confirmar se devem ser removidas do treino do modelo.

### Dicionário de Variáveis

| Variável | Tipo | Domínio | Definição Operacional | Papel Analítico |
|:---|:---|:---|:---|:---|
| `UID` | Numérica discreta | [1, 10000] | Identificador único de cada registo | Identificador (não preditivo) |
| `Product ID` | Categórica nominal | — | Identificação do produto com variante de qualidade (L/M/H + nº de série) | Identificador (não preditivo) |
| `Type` | Categórica nominal | {L, M, H} | Tipo de produto: *Low*, *Medium*, *High* | Variável operacional |
| `Air temperature [K]` | Numérica contínua | ~[295, 305] | Temperatura ambiente da máquina (Kelvin) | Sensor térmico |
| `Process temperature [K]` | Numérica contínua | ~[305, 315] | Temperatura do processo industrial (Kelvin) | Sensor térmico |
| `Rotational speed [rpm]` | Numérica discreta | ~[1200, 3000] | Velocidade de rotação da máquina | Sensor mecânico |
| `Torque [Nm]` | Numérica contínua | ~[3, 80] | Binário aplicado durante o funcionamento | Sensor mecânico |
| `Tool wear [min]` | Numérica discreta | [0, 250] | Tempo de desgaste acumulado da ferramenta | Estado do equipamento |
| `Machine failure` | Binária | {0, 1} | Indica se ocorreu falha na máquina (0 = normal, 1 = falha) | **Variável objetivo** |
| `TWF` | Binária | {0, 1} | *Tool Wear Failure* — Falha por desgaste excessivo da ferramenta | Tipo de falha |
| `HDF` | Binária | {0, 1} | *Heat Dissipation Failure* — Falha por dissipação de calor insuficiente | Tipo de falha |
| `PWF` | Binária | {0, 1} | *Power Failure* — Falha por potência inadequada | Tipo de falha |
| `OSF` | Binária | {0, 1} | *Overstrain Failure* — Falha por esforço excessivo | Tipo de falha |
| `RNF` | Binária | {0, 1} | *Random Failure* — Falha aleatória sem padrão definido | Tipo de falha |

### Ética e Conformidade

O *dataset* não contém dados pessoais. Todas as variáveis referem-se exclusivamente a medições operacionais de máquinas e indicadores técnicos de falha, sem qualquer identificador humano. Trata-se de um *dataset* de natureza pública, amplamente utilizado para fins académicos e de investigação, disponível abertamente no *Kaggle*. Desta forma, não se colocam questões ao nível do RGPD, uma vez que não existem dados pessoais ou sensíveis e a finalidade do uso é exclusivamente académica.

---

## 7. Referências

- **Dataset:** Afonso Fonseca (2024). *AI4I 2020 Predictive Maintenance Dataset*. Kaggle. https://www.kaggle.com/datasets/afonsornfonseca/ai4i-2020-predictive-maintenance
- Chapman, P. et al. (2000). *CRISP-DM 1.0: Step-by-step data mining guide*. SPSS Inc.
- James, G. et al. (2021). *An Introduction to Statistical Learning* (2nd ed.). Springer.
- Matzka, S. (2020). *Explainable Artificial Intelligence for Predictive Maintenance Applications*. IEEE.

---

## 8. Cronograma Interno

| Fase | Data Limite | Entregável Esperado |
|:---|:---|:---|
| M1: Iniciação | 24/02/2026 | Repositório estruturado e Plano de Projeto |
| M2: Exploração | 25/03/2026 | *Notebook* de EDA, *feature engineering* e dados processados |
| M3: Modelação | 23/04/2026 | Comparação de algoritmos e métricas de avaliação |
| M4: Finalização | [Data] | *Pitch* e Relatório Final |

---

*Data de última atualização: 23/04/2026*
