# Milestone 1: Iniciação e Definição do Projeto
## 1. Descrição Detalhada do Problema
O setor da manufatura industrial encontra-se numa fase de transição acelerada impulsionada pela Indústria 4.0, onde a digitalização e a recolha contínua de dados operacionais através de sensores industriais (IoT) assumem um papel central. Neste cenário de alta competitividade, a eficiência operacional e a minimização de paragens não planeadas na linha de produção são fatores críticos para a sustentabilidade, cumprimento de prazos e rentabilidade das organizações.

Tradicionalmente, a gestão da fiabilidade de equipamentos baseia-se em abordagens subotimizadas: a manutenção reativa (intervenção corretiva pós-falha), que resulta em paragens inesperadas e custos de reparação severos; ou a manutenção preventiva (intervenção calendarizada), que gera desperdício financeiro e de materiais ao descartar componentes que ainda possuem um ciclo de vida útil considerável. A transição para a Manutenção Preditiva, suportada por algoritmos de Machine Learning e pela análise multivariável de dados reais (como temperatura, binário e rotação), surge assim como a solução tecnológica imperativa para antecipar estados de pré-falha e otimizar a intervenção nos equipamentos.
## 2. Objetivos SMART
Para antecipar avarias industriais, o projeto visa tornar transparentes para o modelo de Machine Learning as causas subjacentes à variável alvo (Machine failure), modelando matematicamente cinco modos de falha independentes. A abordagem analítica rege-se pelos seguintes objetivos:

**Objetivo 1:** Desenvolver um modelo de classificação binária para prever a ocorrência da variável geral de falha (Machine failure), atingindo um F1-Score mínimo de 0.85 na identificação de avarias, de modo a minimizar paragens operacionais não planeadas, até à entrega do Milestone 3.

**Objetivo 2:** Desenvolver uma pipeline de Engenharia de Variáveis que extraia três novas métricas físicas baseadas nas regras de operação do equipamento: diferença térmica (para previsão de HDF), potência calculada em rad/s (para PWF) e esforço de binário face ao desgaste (para OSF), integrando-as na Análise Exploratória (EDA) a entregar no Milestone 2.

**Objetivo 3:** Treinar um algoritmo de classificação multiclasse capaz de distinguir e diagnosticar a causa raiz da avaria entre os modos específicos (TWF, HDF, PWF, OSF e RNF) com uma Exatidão Global (Accuracy) superior a 80%, a apresentar no relatório final do Milestone 4.

## Perguntas de Investigação

A fase de exploração de dados (EDA) procurará ainda responder às seguintes questões de investigação, que apresentam um impacto direto na compreensão da operação mecânica e na identificação das causas de avaria:

De que forma a variante de qualidade do produto (L, M, H) influencia a taxa de adição de desgaste à ferramenta (Tool wear) e altera os limites críticos de falha por sobrecarga (OSF)?

Qual a correlação entre a velocidade de rotação e o binário (Torque) nos cenários em que a potência resultante ultrapassa os limites de operação segura (< 3500 W ou > 9000 W), causando uma falha de potência (PWF)?

Como interage a diferença entre a temperatura do processo e a temperatura ambiente com a velocidade de rotação na previsão de falhas por dissipação de calor (HDF)?

Existem padrões de funcionamento operacional (combinações de desgaste, temperatura e binário) que operem muito perto dos limiares críticos de avaria sem chegarem a registar falha, constituindo potenciais falsos alarmes teóricos na operação da máquina?

## 3. Metodologia de Gestão (PBL)
* **Divisão de Tarefas:**
* **Membro Afonso Fonseca:** README.MD, Kaggle Notebook, M1_indicacao.md.
* **Membro Artur Yakovenko:** .gitignore, M1_indicacao.md.
* **Membro Bernardo Vieira:** requirements.txt, docs, M1_indicacao.md.
* **Ferramentas de Colaboração:** GitHub, Reuniões Semanais via Discord
  
## 4. Análise de Viabilidade dos Dados

Disponibilidade:

A fase inicial do projeto começou com a identificação e obtenção do conjunto de dados a utilizar. O dataset selecionado foi descarregado a partir da plataforma Kaggle, através do seguinte endereço: https://www.kaggle.com/datasets/afonsornfonseca/ai4i-2020-predictive-maintenance
Após o download, o ficheiro do dataset foi armazenado no espaço de trabalho do grupo e, de seguida, carregado novamente para o Kaggle com o objetivo de criar um Notebook associado ao dataset. Esta abordagem permitiu iniciar rapidamente o desenvolvimento num ambiente já configurado para análise de dados e execução de código, facilitando a colaboração e a reprodutibilidade do trabalho.
Neste momento, os dados não se encontram numa base de dados relacional (por exemplo, SQL Server, MySQL ou PostgreSQL). O dataset é utilizado em formato CSV, o que é adequado à sua dimensão (10.000 registos e 14 variáveis) e ao âmbito do projeto, permitindo leitura e manipulação direta com ferramentas como Pandas/NumPy dentro do notebook.

Qualidade Inicial:

Numa inspeção inicial ao ficheiro CSV, verificou-se que o dataset apresenta uma estrutura tabular estável e consistente: 10.000 linhas e 14 colunas, incluindo variáveis numéricas (temperaturas, velocidade rotacional, torque e desgaste da ferramenta), variáveis categóricas (tipo de produto) e variáveis binárias de falha.
Do ponto de vista de integridade, os dados revelam-se, à partida, de boa qualidade para análise:
Não foram detetados valores em falta (missing values) nas colunas do dataset.
Não foram identificadas linhas duplicadas.
As tipologias das variáveis encontram-se coerentes com o esperado (numéricas para medições e binárias para indicadores de falha).
No entanto, identificam-se desde já aspetos importantes que terão impacto direto na fase de exploração e modelação (M2/M3):
Desbalanceamento da variável alvo (“Machine failure”)
A ocorrência de falha é relativamente rara: existem 339 casos de falha em 10.000 registos, correspondendo a cerca de 3,39% do total. Este desbalanceamento é comum em cenários reais de manutenção preditiva, mas exige cuidados técnicos (métricas adequadas, estratégias de validação e, eventualmente, técnicas de reamostragem/ponderação).
Relação entre “Machine failure” e os tipos de falha específicos
O dataset inclui variáveis binárias para tipos específicos de falha (TWF, HDF, PWF, OSF, RNF). Numa verificação preliminar, observou-se que a correspondência entre a variável “Machine failure” e a presença/ausência destes indicadores não é perfeita em todos os registos (existem alguns casos em que aparece um tipo específico marcado sem “Machine failure”, e casos inversos).
Este ponto será analisado detalhadamente na M2 para compreender se se trata de uma regra definida pelo dataset (por exemplo, tratamento especial da falha RNF) ou de inconsistências que exijam decisões de pré-processamento.
Variáveis de identificação
As colunas UDI e Product ID funcionam como identificadores e, por regra, não acrescentam valor preditivo direto; ainda assim, serão avaliadas na M2 para confirmar se devem ser removidas do treino do modelo (evitando ruído ou enviesamento).
Em síntese, o dataset apresenta boa qualidade estrutural, mas levanta desafios relevantes e interessantes (sobretudo desbalanceamento e coerência entre variáveis de falha), que serão tratados formalmente nas fases seguintes.

Ética:

Do ponto de vista ético e de conformidade com privacidade, o dataset não contém dados pessoais. As variáveis existentes referem-se exclusivamente a medições operacionais de máquinas e indicadores técnicos de falhas, não incluindo nomes, moradas, contactos, localizações pessoais ou qualquer identificador humano.
Adicionalmente, trata-se de um dataset de natureza pública e amplamente utilizado para fins académicos e de investigação, o que reduz riscos associados a utilização indevida de informação sensível. Assim, não existem implicações relevantes ao nível do RGPD, uma vez que:
não há dados pessoais ou sensíveis;
os identificadores presentes (por exemplo, Product ID) referem-se a produtos/máquinas e não a indivíduos;
a finalidade do uso é académica e enquadrada no contexto de análise e modelação.
Desta forma, conclui-se que o dataset é eticamente apropriado para o desenvolvimento do projeto e não requer procedimentos adicionais de anonimização.


## 5. Cronograma Interno
| Fase | Data Limite | Entregável Esperado |
| :--- | :--- | :--- |
| M1: Iniciação |24/02/2026| Repositório estruturado e Plano de Projeto. |
| M2: Exploração | [Data] | Notebook de EDA e Dados Processados. |
| M3: Modelação | [Data] | Comparação de algoritmos e métricas. |
| M4: Finalização| [Data] | Pitch e Relatório Final. |
---
*Data de última atualização: [20/02/2026]*
