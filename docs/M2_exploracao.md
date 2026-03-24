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

* **Nova Variável Temp_diff:** Criámos esta variável calculando a diferença exata entre a temperatura do processo e a temperatura ambiente (Process temperature [K] - Air temperature [K]). O objetivo desta métrica é capturar o esforço térmico da máquina e a sua eficiência na dissipação de calor, o que ajuda o algoritmo a identificar limiares críticos que despoletam avarias por sobreaquecimento.

* **Nova Variável Power:** Criámos esta métrica através do produto entre o binário e a velocidade de rotação (Torque [Nm] * Rotational speed [rpm]). Esta variável representa a potência mecânica global exigida ao equipamento num dado instante. Ao fundir estas duas grandezas numa só, facilitamos ao modelo a identificação de picos de esforço ou quebras de energia que estão diretamente na origem das falhas de potência.

Validação de Relevância:
Após a criação destes novos atributos, executámos o método .corr() para verificar a correlação linear entre a Temp_diff, a Power e a variável alvo Machine failure. Esta validação confirmou que estas novas features físicas possuem poder preditivo e fornecem sinais úteis para o modelo de Machine Learning.
### 3.3. Seleção de Atributos (Feature Selection)

De modo a garantir a robustez do modelo preditivo e a qualidade do processo de aprendizagem do algoritmo, efetuou-se uma seleção rigorosa dos atributos a manter no conjunto de dados, o que resultou na eliminação de duas categorias de variáveis:

* Remoção de Variáveis de Identificação: Numa primeira fase da preparação dos dados, procedeu-se à eliminação das colunas UDI e Product ID. Uma vez que estas variáveis funcionam exclusivamente como identificadores únicos de cada registo e números de série das ferramentas, não possuem qualquer relação de causalidade ou valor preditivo face à ocorrência de avarias. A sua manutenção no conjunto de dados introduziria apenas ruído matemático desnecessário no treino do modelo.

* Remoção de Variáveis com Fuga de Dados (Data Leakage): Numa etapa subsequente, foram eliminadas as colunas correspondentes aos modos de falha determinísticos específicos (TWF, HDF, PWF, OSF e RNF). Sendo a variável alvo a classificação binária geral Machine failure, a presença destas subcategorias no conjunto de treino provocaria um fenómeno severo de data leakage (fuga de dados). O modelo aprenderia a mapear a avaria apoiando-se exclusivamente na leitura destes rótulos diretos, perdendo a necessidade e a capacidade de extrair os verdadeiros padrões de correlação subjacentes às grandezas físicas e operacionais (temperaturas, velocidade, binário e desgaste).

## 4. Dicionário de Dados Final (Pós-Processamento)
*Listagem final das variáveis que serão entregues ao modelo na Fase 3.*
## Dicionário das variáveis

## 4. Dicionário de Dados Final (Pós-Processamento)
*Listagem final das variáveis que serão entregues ao modelo na Fase 3, após limpeza, transformação e engenharia de atributos.*

| Variável | Tipo Estatístico | Domínio | Classes / Escala Semântica | Definição Operacional | Papel Analítico |
|----------|----------------|--------|----------------------------|----------------------|-----------------|
| Air temperature [K] | Numérica contínua | Normalizado (~[-3, 3]) | Média = 0, DP = 1 | Temperatura ambiente da máquina (escalonada) | Sensor térmico |
| Process temperature [K] | Numérica contínua | Normalizado (~[-3, 3]) | Média = 0, DP = 1 | Temperatura do processo industrial (escalonada) | Sensor térmico |
| Rotational speed [rpm] | Numérica contínua | Normalizado (~[-3, 3]) | Média = 0, DP = 1 | Velocidade de rotação da máquina (escalonada) | Sensor mecânico |
| Torque [Nm] | Numérica contínua | Normalizado (~[-3, 3]) | Média = 0, DP = 1 | Binário aplicado durante o funcionamento (escalonado) | Sensor mecânico |
| Tool wear [min] | Numérica contínua | Normalizado (~[-3, 3]) | Média = 0, DP = 1 | Tempo de desgaste acumulado da ferramenta (escalonado) | Estado do equipamento |
| Machine failure | Categórica binária | {0, 1} | 0 = normal, 1 = falha | Indica se ocorreu falha geral na máquina | Variável alvo |
| Type_Encoded | Numérica discreta | {0, 1, 2} | 0 = L, 1 = M, 2 = H | Tipo de produto (nível de qualidade) após Ordinal Encoding | Operacional |
| Temp_diff | Numérica contínua | ~[8, 12] | Escala original (Kelvin) | Diferença entre a temperatura do processo e a temperatura ambiente | Variável derivada (Feature Engineering) |
| Power | Numérica contínua | ~[3000, 240000] | Escala original (W) | Potência estimada da máquina (Torque × Rotational speed) | Variável derivada (Feature Engineering) |




## 5. Conclusões da Fase de Exploração

### 5.1. O que aprendemos sobre o dataset face à Milestone 1?

Durante a Milestone 1, a compreensão do conjunto de dados era predominantemente teórica e estrutural. A Análise Exploratória de Dados (EDA) conduzida nesta segunda fase permitiu extrair conhecimento empírico e físico fundamental sobre a dinâmica operacional do equipamento:

* **O Desafio da Avaliação (Desequilíbrio de Classes):** A quantificação exata da variável alvo revelou um desequilíbrio acentuado, com as falhas a representarem apenas 3,39% dos registos. Esta constatação evidenciou que métricas tradicionais, como a Exatidão (Accuracy), serão estatisticamente ilusórias na fase de modelação. Estabeleceu-se assim a necessidade de focar a avaliação em métricas como o F1-Score e o Recall para aferir a real capacidade preditiva sobre a classe minoritária.
* **Dinâmica Termo-Mecânica e Variáveis Derivadas:** Ao contrário da observação isolada das variáveis na fase inicial, compreendemos a interação física entre elas. O cálculo da variável `Temp_diff` provou que a máquina opera consistentemente com uma temperatura de processo superior à temperatura ambiente (num intervalo médio de 8 a 12 Kelvin). Simultaneamente, confirmou-se que o binário (Torque) atua como o elo central de esforço, apresentando uma forte correlação negativa com a rotação, o que motivou a criação da métrica de potência global (`Power`).
* **O Valor Preditivo dos Outliers:** Concluiu-se que os valores extremos nas distribuições (como os picos de rotação na ordem das 2886 rpm) não constituem ruído informacional ou erros de calibração dos sensores. Representam assinaturas mecânicas autênticas de picos de stress que antecedem avarias por sobrecarga e potência, o que justificou plenamente a sua manutenção no dataset.
* **O Risco de Fuga de Dados (Data Leakage):** Percebemos que a presença das variáveis binárias de modos de falha específicos (TWF, HDF, PWF, OSF e RNF) inviabilizaria o treino correto do algoritmo para a variável alvo (Machine failure), provocando fuga de dados. A sua remoção tornou-se um passo metodológico crítico e imprevisto na Milestone 1.

### 5.2. Os dados são suficientes para avançar para a modelação?

Sim, os dados demonstraram ser robustos e encontram-se rigorosamente preparados para avançar para a fase de modelação (Fase 3). O processo de preparação permitiu transitar de um registo operacional bruto para uma matriz analítica otimizada. 

Para atingir este estado de prontidão, garantiu-se a inexistência de valores nulos e executaram-se as seguintes transformações:
1. **Redução de Ruído:** Eliminação de identificadores desprovidos de valor preditivo (UDI e Product ID).
2. **Prevenção de Fuga de Dados:** Eliminação das subcategorias determinísticas de falha.
3. **Conversão Numérica Integral:** Transformação da variante categórica de qualidade do produto através da aplicação de Ordinal Encoding (Type_Encoded).
4. **Padronização Matemática:** Aplicação do StandardScaler às variáveis contínuas, mitigando a discrepância de escalas (milhares nas rotações face a dezenas no binário) e prevenindo o enviesamento de algoritmos baseados em distância.
5. **Enriquecimento do Modelo:** Injeção de conhecimento de domínio através das novas métricas físicas (Temp_diff e Power).

Em síntese, o conjunto de dados processado reúne todas as condições técnicas, matemáticas e físicas para o treino de algoritmos preditivos. O principal desafio analítico reservado para a próxima etapa consistirá na implementação de técnicas de reamostragem ou ajuste de pesos (class weights) para mitigar o severo desequilíbrio da variável alvo.

*Data de última atualização: [24/03/2026]* 
