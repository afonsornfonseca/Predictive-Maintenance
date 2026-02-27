# Milestone 2: Análise Exploratória e Engenharia de Atributos
## 1. Análise Exploratória de Dados (EDA)
### 1.1. Distribuição da Variável Alvo
A variável alvo do nosso projeto é “Machine failure”, que assume dois valores: 0 quando a máquina funciona normalmente e 1 quando ocorre uma falha. Para percebermos como esta variável se distribui no dataset, calculámos as frequências e representámos os resultados num gráfico circular no Kaggle.
O que observámos foi um forte desequilíbrio entre classes: 96,61% dos registos correspondem a funcionamento normal, enquanto apenas 3,39% representam falhas reais. Isto significa que estamos perante um problema típico de manutenção preditiva: as falhas são eventos raros quando comparadas com períodos de operação normal.
Este ponto é importante porque influencia diretamente a forma como iremos avaliar o modelo mais à frente. Se utilizássemos apenas a métrica de accuracy, poderíamos ser facilmente enganados. Por exemplo, um modelo que previsse sempre “não falha” teria automaticamente cerca de 96% de acerto, mas não estaria efetivamente a cumprir o objetivo do projeto, que é identificar falhas.
Assim, esta análise inicial permite-nos antecipar duas decisões importantes para a fase de modelação:
Primeiro, teremos de utilizar métricas mais adequadas para classes desbalanceadas, como recall da classe de falha, F1-score ou curvas Precision-Recall, que avaliam melhor a capacidade do modelo em detetar os casos raros.
Segundo, poderá ser necessário aplicar estratégias específicas para lidar com o desbalanceamento, como o uso de pesos diferenciados para as classes ou técnicas de reamostragem. Ainda não aplicámos nenhuma dessas técnicas nesta fase, mas já sabemos que este será um aspeto crítico no desenvolvimento do modelo.
Em resumo, a análise da variável alvo mostrou-nos que o dataset está bem estruturado, mas apresenta um desafio claro: as falhas são poucas. Isso não é um problema é uma característica realista do cenário industrial mas obriga-nos a ter cuidado na forma como vamos treinar e avaliar o modelo.
### 1.2. Correlações Relevantes
Após a construção do mapa de correlação das variáveis numéricas, foi possível identificar relações fortes e moderadas entre alguns atributos do dataset.
A primeira relação que se destaca é entre Rotational speed [rpm] e Torque [Nm], com uma correlação negativa muito forte (≈ -0.88). O gráfico de dispersão confirma visualmente esta relação: à medida que a velocidade de rotação aumenta, o torque tende a diminuir de forma consistente. Esta relação é coerente do ponto de vista físico e indica dependência direta entre estas duas variáveis operacionais.
Outra relação forte observada foi entre Air temperature [K] e Process temperature [K], com correlação positiva elevada (≈ 0.88). O scatter plot mostra praticamente uma relação linear, indicando que a temperatura do processo acompanha de perto a temperatura ambiente. Isto sugere que estas variáveis carregam informação muito semelhante.
Relativamente à variável alvo Machine failure, verificou-se que as maiores correlações ocorrem com as variáveis que representam tipos específicos de falha (HDF, OSF, PWF e TWF), com valores entre 0.36 e 0.58. No entanto, estas variáveis estão diretamente relacionadas com a definição da falha global, pelo que a sua elevada correlação era expectável.
Quando analisamos apenas variáveis de sensores independentes, observa-se que:
Torque [Nm] apresenta a maior correlação positiva com a falha (≈ 0.19);
Tool wear [min] apresenta também correlação positiva (≈ 0.10);
As restantes variáveis apresentam correlação muito reduzida.
Os boxplots reforçam esta análise: máquinas que registaram falha apresentam, em média, valores de torque e desgaste da ferramenta superiores às que não falharam.
De forma geral, conclui-se que o torque e o desgaste da ferramenta parecem ser os sensores com maior relevância preditiva entre as variáveis operacionais analisadas.
## 2. Qualidade dos Dados e Limpeza
### 2.1. Tratamento de Dados em Falta (Missing Data)
Após a verificação inicial do dataset (através do método isnull().sum()), confirmou-se que o conjunto de dados está perfeitamente preenchido. Não existem valores nulos ou em falta em nenhuma das 14 colunas ao longo das 10.000 instâncias. Por conseguinte, não foi necessário aplicar qualquer técnica de imputação (como substituição por média/mediana) ou eliminação de registos, mantendo-se a integridade total dos dados originais para a fase de modelação.
### 2.2. Outliers e Inconsistências

Após a análise descritiva das variáveis, não foram encontrados valores fisicamente impossíveis, erros de digitação ou inconsistências lógicas no dataset. Todas as variáveis operacionais respeitam os seus limites naturais (por exemplo, o desgaste da ferramenta (Tool wear [min]) e o binário (Torque [Nm]) não apresentam valores negativos, com mínimos de 0 e 3.8 respetivamente).

Tratamento de Outliers (Valores Extremos):
A análise visual através de boxplots revelou a presença de outliers estatísticos, particularmente nas variáveis Rotational speed [rpm] (com valores extremos a atingir as 2886 rpm) e Torque [Nm].
A nossa estratégia foi a manutenção integral de todos os outliers, não aplicando qualquer técnica de remoção ou limitação (como Winsorization). No contexto específico da manutenção preditiva industrial, estes valores extremos não representam "ruído" ou erros de leitura dos sensores, mas sim picos de stress mecânico ou anomalias operacionais reais. Como são frequentemente estes desvios abruptos que causam as falhas de sobrecarga (OSF) ou falhas de potência (PWF), a sua remoção destruiria o poder preditivo do nosso modelo perante a variável alvo.

## 3. Engenharia de Atributos (Feature Engineering)
### 3.1. Transformações Realizadas
* **Encoding:** (Ex: "Convertemos a variável 'Género' em numérica usando One-Hot Encoding.")
* **Escalonamento:** (Ex: "Aplicámos o StandardScaler nas variáveis numéricas para que todas
fiquem na mesma escala.")
### 3.2. Criação de Novos Atributos
*Descrevam as variáveis que criaram para ajudar o modelo.*
* **Nova Variável [Nome]:** (Ex: "Criámos a 'Tenure_Per_Year' que divide o tempo de contrato
pela idade do cliente.")
## 4. Dicionário de Dados Final (Pós-Processamento)
*Listagem final das variáveis que serão entregues ao modelo na Fase 3.*
| Atributo | Tipo | Descrição |
| :--- | :--- | :--- |
| `cliente_id` | ID | Removido (não preditivo) |
| `idade_norm` | Float | Idade após normalização |
| `is_premium` | Binary | 1 para clientes com plano superior |
## 5. Conclusões da Fase de Exploração
*O que aprenderam sobre o dataset que não sabiam no final do Milestone 1? Os dados são suficientes
para avançar para a modelação?*
---
*Data de última atualização: [DD/MM/AAAA]* 
