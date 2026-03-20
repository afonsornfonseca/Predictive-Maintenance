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

No gráfico que relaciona Rotational speed [rpm] com Torque [Nm], observa-se um padrão bem definido e coerente com o valor de correlação negativa forte identificado anteriormente. À medida que a velocidade de rotação aumenta, o torque diminui de forma consistente, formando uma estrutura quase curva e bem delimitada. Esta organização visual confirma que existe uma dependência operacional clara entre estas duas variáveis, não sendo uma relação aleatória. Do ponto de vista físico, esta dinâmica é expectável em sistemas mecânicos rotativos, onde velocidade e binário se ajustam para manter determinados níveis de potência. Ao analisar a distribuição da variável Machine failure neste gráfico, verifica-se que as falhas surgem com maior frequência em zonas de torque mais elevado, independentemente do regime de rotação. Isto reforça a ideia de que o torque representa diretamente o nível de esforço mecânico aplicado à máquina e, consequentemente, o risco associado ao seu funcionamento. Assim, a inclusão destas duas variáveis justifica-se não apenas pela correlação estatística, mas também pela sua relevância operacional e pela interpretação física consistente que apresentam.

No gráfico Tool wear [min] vs Torque [Nm], apesar de existir maior dispersão dos pontos, é possível identificar uma tendência relevante: as falhas concentram-se com maior incidência em regiões onde o desgaste da ferramenta e o torque assumem valores mais elevados. Este comportamento sugere uma interação entre degradação progressiva e esforço mecânico. À medida que a ferramenta se desgasta, o sistema necessita de aplicar maior força para manter o desempenho do processo produtivo, o que conduz ao aumento do torque. Esse aumento representa maior stress mecânico e eleva a probabilidade de ocorrência de falha. Este gráfico acrescenta uma dimensão temporal à análise, uma vez que o desgaste é um fenómeno acumulativo, ao contrário da velocidade e do torque, que refletem condições instantâneas de funcionamento. Assim, a variável Tool wear complementa o torque ao permitir capturar falhas que resultam de degradação gradual do equipamento e não apenas de picos súbitos de esforço.

Relativamente ao gráfico Air temperature [K] vs Process temperature [K], observa-se uma relação linear muito forte, praticamente sobrepondo os pontos ao longo de uma reta crescente. Esta evidência visual confirma a elevada correlação positiva identificada no mapa de correlação. A temperatura do processo acompanha de perto a temperatura ambiente, indicando que o contexto externo influencia diretamente as condições internas da máquina. Embora as falhas não apresentem neste gráfico um padrão de separação tão evidente como nos gráficos relacionados com torque e desgaste, estas variáveis são importantes para caracterizar o ambiente térmico de operação. O contexto térmico pode influenciar o comportamento dos componentes mecânicos e elétricos, especialmente em situações de esforço elevado. Quando relacionado com os outros gráficos, é plausível que níveis elevados de torque, associados a desgaste significativo, possam contribuir para aumento de temperatura no processo, reforçando a importância de considerar simultaneamente variáveis mecânicas e térmicas.

Em conjunto, estes três gráficos demonstram que as variáveis selecionadas representam diferentes dimensões do funcionamento da máquina. A velocidade de rotação e o torque caracterizam o regime mecânico instantâneo, o desgaste da ferramenta introduz a componente acumulativa de degradação, e as temperaturas descrevem o contexto ambiental e térmico da operação. A análise visual confirma que o torque surge como variável central, funcionando como elo de ligação entre esforço mecânico, desgaste progressivo e possíveis impactos térmicos. Desta forma, os gráficos não apenas validam os resultados obtidos no mapa de correlação, como também reforçam a relevância preditiva das variáveis escolhidas para a fase seguinte de modelação.

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
* **Encoding:** Convertemos a variável categórica de texto Type em formato numérico utilizando Ordinal/Label Encoding. Uma vez que esta variável representa uma hierarquia lógica de qualidade das ferramentas, mapeámos as categorias com pesos crescentes: L (Low) = 0, M (Medium) = 1 e H (High) = 2. A coluna original em formato texto foi posteriormente removida, resultando na nova variável Type_Encoded.
* **Escalonamento:** Aplicámos o método StandardScaler às cinco variáveis preditoras contínuas do dataset (Air temperature [K], Process temperature [K], Rotational speed [rpm], Torque [Nm] e Tool wear [min]). Esta transformação reajustou os dados para apresentarem uma média de 0 e um desvio padrão de 1. Esta etapa foi fundamental para colocar todas as métricas na mesma escala de grandeza, garantindo que os futuros algoritmos de Machine Learning não atribuam um peso desproporcional à "Velocidade de Rotação" (cujos valores absolutos chegam aos 2800) em detrimento do "Binário" (cujos valores rondam os 40). As variáveis binárias relativas às falhas e a recém-criada Type_Encoded foram intencionalmente excluídas deste escalonamento para preservarem a sua correta interpretação matemática.
### 3.2. Criação de Novos Atributos
Com base no conhecimento do domínio (manutenção preditiva e física mecânica), criámos duas novas variáveis a partir dos dados originais para ajudar o modelo a capturar padrões mais complexos de desgaste e esforço do equipamento:

Nova Variável Temp_diff: Criámos esta variável calculando a diferença exata entre a temperatura do processo e a temperatura ambiente (Process temperature [K] - Air temperature [K]). O objetivo desta métrica é capturar o esforço térmico da máquina e a sua eficiência na dissipação de calor, o que ajuda o algoritmo a identificar limiares críticos que despoletam avarias por sobreaquecimento.

Nova Variável Power: Criámos esta métrica através do produto entre o binário e a velocidade de rotação (Torque [Nm] * Rotational speed [rpm]). Esta variável representa a potência mecânica global exigida ao equipamento num dado instante. Ao fundir estas duas grandezas numa só, facilitamos ao modelo a identificação de picos de esforço ou quebras de energia que estão diretamente na origem das falhas de potência.

Validação de Relevância:
Após a criação destes novos atributos, executámos o método .corr() para verificar a correlação linear entre a Temp_diff, a Power e a variável alvo Machine failure. Esta validação confirmou que estas novas features físicas possuem poder preditivo e fornecem sinais úteis para o modelo de Machine Learning.

## 4. Dicionário de Dados Final (Pós-Processamento)
*Listagem final das variáveis que serão entregues ao modelo na Fase 3.*
## Dicionário das variáveis

| Variável | Tipo Estatístico | Domínio | Classes / Escala Semântica | Definição Operacional | Papel Analítico |
|----------|----------------|--------|----------------------------|----------------------|-----------------|
| UID | Numérica discreta | [1, 10000] | — | Identificador único de cada registo | Identificador (não preditivo) |
| Product ID | Categórica nominal | — | L / M / H + número de série | Identificação do produto com variante de qualidade | Identificador (não preditivo) |
| Type | Categórica nominal | {L, M, H} | L = Low, M = Medium, H = High | Tipo de produto (nível de qualidade) | Operacional |
| Air temperature [K] | Numérica contínua | ~[295, 305] | — | Temperatura ambiente da máquina | Sensor térmico |
| Process temperature [K] | Numérica contínua | ~[305, 315] | — | Temperatura do processo industrial | Sensor térmico |
| Rotational speed [rpm] | Numérica discreta | ~[1200, 3000] | — | Velocidade de rotação da máquina | Sensor mecânico |
| Torque [Nm] | Numérica contínua | ~[3, 80] | — | Binário aplicado durante o funcionamento | Sensor mecânico |
| Tool wear [min] | Numérica discreta | [0, 250] | — | Tempo de desgaste acumulado da ferramenta | Estado do equipamento |
| Machine failure | Categórica binária | {0,1} | 0 = normal, 1 = falha | Indica se ocorreu falha na máquina | Variável alvo |
| TWF | Categórica binária | {0,1} | Tool Wear Failure | Falha devido a desgaste excessivo da ferramenta | Tipo de falha |
| HDF | Categórica binária | {0,1} | Heat Dissipation Failure | Falha por dissipação de calor insuficiente | Tipo de falha |
| PWF | Categórica binária | {0,1} | Power Failure | Falha devido a potência inadequada | Tipo de falha |
| OSF | Categórica binária | {0,1} | Overstrain Failure | Falha por esforço excessivo | Tipo de falha |
| RNF | Categórica binária | {0,1} | Random Failure | Falha aleatória sem padrão definido | Tipo de falha |

## 5. Conclusões da Fase de Exploração
*O que aprenderam sobre o dataset que não sabiam no final do Milestone 1? Os dados são suficientes
para avançar para a modelação?*
---
*Data de última atualização: [DD/MM/AAAA]* 
