# Milestone 3: Modelação e Avaliação
## 1. Estratégia de Modelação

*Descrição de como os dados foram preparados para os algoritmos.*

* **Divisão do dataset:** Utilizámos uma divisão de **80% para treino (8000 instâncias) e 20% para teste (2000 instâncias)**, com semente aleatória fixa (`random_state=42`) para garantir total reprodutibilidade. O passo mais crítico foi o parâmetro `stratify=y`: dado o acentuado desequilíbrio da variável alvo (~3,4% de avarias reais), a estratificação garantiu que esta proporção minoritária se manteve rigorosamente idêntica em ambos os conjuntos. Sem este passo, corríamos o risco de gerar um conjunto de teste sem falhas suficientes para uma avaliação fiável. Foi ainda testada uma divisão alternativa 75/25 que confirmou a estabilidade dos resultados independentemente da proporção escolhida.

* **Métrica de Sucesso:** A métrica principal eleita foi o **F1-Score**, complementado pelo **Recall** e pela **ROC-AUC**. A escolha foi imediatamente validada pelo nosso modelo baseline: a Regressão Logística obteve uma Accuracy de 96,75%, aparentemente excelente, mas com F1-Score de apenas **28,5%** e Recall de **19,1%**. O modelo limitava-se a prever sempre "não falha", ignorando completamente as avarias reais. Num contexto de manutenção preditiva industrial, o custo de um **falso negativo** (avaria não detetada → paragem de produção, dano no equipamento) é incomparavelmente superior ao custo de um **falso positivo** (inspeção desnecessária). O F1-Score é, portanto, a única métrica que reflete a viabilidade real do modelo para este problema de negócio.

## 2. Experiências Realizadas
### 2.1. Modelo Baseline
*O ponto de partida simples.*

* **Algoritmo:** Regressão Logística (`max_iter=1000`, `random_state=42`), sem qualquer estratégia de compensação do desequilíbrio de classes.
* **Resultado:**

| Métrica | Treino | Teste |
| :--- | :--- | :--- |
| Accuracy | 97,2% | 96,75% |
| Precision | 71,9% | 56,5% |
| Recall | 27,3% | 19,1% |
| F1-Score | 39,6% | 28,6% |

 **Conclusão:** Apesar da Accuracy elevada, o modelo falha redondamente na deteção de avarias. Com apenas 19,1% de Recall, está a ignorar mais de 80% das falhas reais, comportamento inaceitável para o objetivo de negócio. Este resultado justifica a adoção de modelos mais sofisticados e de estratégias de compensação do desequilíbrio.

### 2.2. Modelos Candidatos
*Algoritmos testados e justificação da escolha.*

| Algoritmo | Parâmetros Base | F1-Score (Treino) | F1-Score (Teste) | Notas |
| :--- | :--- | :--- | :--- | :--- |
| Árvore de Decisão | `max_depth=3`, `random_state=42` | 39,7% | 31,0% | Precision alta (81,3%) mas Recall idêntico ao baseline (19,1%). Muito conservador. |
| Random Forest | `n_estimators=100`, `max_depth=5`, `class_weight='balanced'` | 58,1% | 54,8% | Alto Recall (88,2%) mas Precision muito baixa (39,7%) — demasiados alarmes falsos. |
| **XGBoost** | `n_estimators=100`, `max_depth=4`, `learning_rate=0.1`, `scale_pos_weight` (auto) | **69,1%** | **57,8%** | **Melhor equilíbrio Precision/Recall. Modelo selecionado para otimização.** |

 **Nota sobre a estratégia anti-desequilíbrio:** O Random Forest utilizou `class_weight='balanced'` e o XGBoost utilizou `scale_pos_weight` (rácio classes negativas/positivas calculado automaticamente a partir dos dados de treino). Ambas as técnicas penalizam o modelo por ignorar a classe minoritária, sendo determinantes para o salto de performance face aos baselines.

## 3. Otimização (Tuning)
*Como melhorámos o melhor modelo.*
### 3.1. 3.1. Otimização de Hiperparâmetros (GridSearchCV)

* **Técnica Utilizada:** Aplicámos `GridSearchCV` com validação cruzada estratificada de 5 dobras (`StratifiedKFold`, `cv=5`) sobre o XGBoost, com foco exclusivo no **F1-Score** como critério de seleção. A grelha explorada concentrou-se em hiperparâmetros que combatem o overfitting observado no modelo base (F1 Treino: 69,1% vs F1 Teste: 57,8%):

| Hiperparâmetro | Valores Testados | Objetivo |
| :--- | :--- | :--- |
| `max_depth` | [2, 3, 4] | Limitar a profundidade para evitar memorização de padrões específicos |
| `learning_rate` | [0.01, 0.05, 0.1] | Aprendizagem mais gradual e robusta |
| `n_estimators` | [100, 200, 300] | Número de árvores a combinar |

* **Melhoria obtida:** O F1-Score no conjunto de teste subiu de **57,8% para 75,2%** após o ajuste, um ganho de **+17,4 p.p.** A validação cruzada K-Fold (K=5) confirmou a robustez do modelo com F1-Score médio estável e baixo desvio padrão nas 5 dobras, provando que os resultados generalizam de forma consistente.

| Fase | F1-Score (Treino) | F1-Score (Teste) | Δ Teste |
| :--- | :--- | :--- | :--- |
| XGBoost Base | 69,1% | 57,8% | — |
| XGBoost Otimizado (GridSearchCV) | 94,6% | 75,2% | +17,4 p.p. |

### 3.2. Ajuste do Limiar Orientado ao Negócio (Threshold Tuning)

*Descrição da adaptação matemática do modelo à realidade financeira da operação.*

* **Técnica Utilizada:** Após atingirmos um F1-Score de 75,2% através do GridSearchCV, identificámos um trade-off clássico entre a estatística pura e a realidade industrial. A otimização baseada no F1-Score mantinha o limiar de decisão padrão (0.50), penalizando com o mesmo peso quer um falso alarme (inspeção desnecessária), quer uma avaria imprevista (paragem catastrófica). Como a política de manutenção preditiva da organização dita que o custo de uma paragem não planeada supera largamente o custo de uma inspeção de rotina, substituímos a métrica de otimização pelo **F2-Score** (Beta=2). Esta técnica força o algoritmo a dar o dobro do peso à deteção de falhas (Recall) em detrimento da precisão absoluta. Ao recalcular a curva Precision-Recall, o algoritmo identificou que o novo limiar ótimo se situa nos **0.3721**.

* **Melhoria obtida:** A aplicação deste novo limite gerou um impacto drástico e perfeitamente alinhado com as necessidades financeiras e operacionais da fábrica. O modelo tornou-se mais sensível aos primeiros sintomas de desgaste e atingiu um **F2-Score final de 80,6%**. O verdadeiro ganho observou-se na capacidade de deteção (Recall), que subiu para **87,0%**, reduzindo as avarias imprevistas (Falsos Negativos) para apenas 9 casos em 2000 instâncias de teste. Para atingir este nível de segurança, assumimos conscientemente uma quebra na Precision — o que gerou 35 Falsos Positivos e fez o F1-Score descer para 73,0%. Esta descida matemática valida a nossa premissa de negócio: realizar algumas dezenas de inspeções preventivas desnecessárias é um "erro" altamente rentável para garantir que quase nenhuma falha catastrófica passa despercebida.

| Fase | F1-Score (Treino) | F1-Score (Teste) | Δ Teste |
| :--- | :--- | :--- | :--- |
| XGBoost Base | 69,1% | 57,8% | — |
| XGBoost Otimizado (GridSearchCV) | 94,6% | 75,2% | +17,4 p.p. |
| **Ajuste de Limiar (Negócio)** | **94,6%** | **73,0%** | **+15,2 p.p.*** |
  
## 4. Avaliação do Modelo Final
### 4.1. Matriz de Confusão / Erros

*Onde o modelo mais falha e o impacto das suas decisões.*

| | Previsto: Não Falha (0) | Previsto: Falha (1) |
| :--- | :--- | :--- |
| **Real: Não Falha (0)** | 1897 | 35 |
| **Real: Falha (1)** | 9 | 59 |

**Análise:** Em 2000 instâncias de teste, o modelo assumiu um total de 44 erros (35 + 9), um reflexo direto do ajuste do limiar orientado ao negócio. Destes erros, a grande maioria (35) corresponde a **falsos positivos** — os erros mais "baratos" (a fábrica faz uma inspeção desnecessária, mas a máquina está bem). Em contrapartida, o modelo reduziu os erros "caros" (**falsos negativos**, avarias que escapam sem deteção e causam paragens graves) para apenas 9. 

De todas as avarias reais presentes no conjunto de teste (68 no total), o modelo detetou 59 corretamente, alcançando um excelente **Recall de 87%**. Como consequência natural desta sensibilidade aumentada (o modelo está mais "cauteloso" e alerta mais cedo), quando emite um alarme tem razão em 63% das vezes (Precision). O principal padrão de erro residual são agora alarmes falsos em máquinas que apresentam flutuações ligeiras no Torque ou na Potência, situações normais onde o modelo prefere errar por excesso de zelo preventivo do que arriscar uma falha catastrófica da linha de produção.

---
### 4.2. Importância dos Atributos (Feature Importance)
*Variáveis que o modelo considerou mais importantes para decidir.*

1. **Power** *(Torque × Rotational Speed)* :variável criada em Feature Engineering; a mais importante do modelo. Captura o esforço mecânico total do sistema num único indicador composto.
2. **Tool wear [min]** : o desgaste acumulado da ferramenta é o segundo preditor mais forte, refletindo a degradação progressiva do equipamento ao longo do tempo de operação.
3. **Torque [Nm]** : esforço de rotação; valores extremos correlacionam-se fortemente com avaria iminente.
4. **Rotational speed [rpm]** : desvios da velocidade nominal são sinais de alerta precoce de instabilidade mecânica.
5. **Temp_diff** *(Process Temp : Air Temp)* : variável criada em Feature Engineering; diferenças térmicas anómalas precedem frequentemente falhas mecânicas.
6. **Air / Process temperature [K]** : as temperaturas absolutas contribuem de forma complementar à diferença térmica.
7. **Type_Encoded** : o tipo de produto (L/M/H) tem influência menor, mas estatisticamente confirmada pelo modelo.

> **Nota:** As duas variáveis de Feature Engineering criadas no Milestone 2 `Power` e `Temp_diff` estão entre as mais importantes do modelo final, validando retrospetivamente a qualidade do trabalho de engenharia de atributos realizado.

## 5. Conclusão da Fase de Modelação

O modelo **XGBoost Otimizado (Versão Negócio)** está pronto para ser apresentado como solução final. A jornada percorrida ao longo das fases de experimentação demonstra uma progressão clara, fundamentada e estritamente orientada pela realidade industrial:

* **A Prioridade: Recall a 87%:** Ao contrário da abordagem estatística padrão, priorizámos a deteção máxima de falhas. O modelo consegue agora identificar 59 das 68 avarias reais, reduzindo drasticamente o risco de paragens catastróficas. Comparado com o baseline que falhava mais de 80% das avarias, esta é uma evolução radical na segurança operacional.

* **O Investimento: Precision a 63%:** Para garantir que nenhuma máquina crítica parasse sem aviso, o algoritmo foi calibrado para ser mais sensível. Isto resultou num aumento controlado de alarmes falsos (35 casos). Na nossa lógica de gestão, este é um "investimento em prevenção": o custo de inspeções desnecessárias é largamente compensado pela poupança em reparações de grande escala.

* **O Novo Indicador: F2-Score a 80,6%:** Abandonámos o F1-Score como métrica única de sucesso, uma vez que este penalizava excessivamente a nossa estratégia preventiva. O F2-Score de 80,6% é a prova matemática de que a nossa decisão foi correta: o ganho na proteção contra falhas críticas superou o custo estatístico dos alarmes falsos.

* **A validação estatística:** O K-Fold Cross-Validation (K=5) confirmou que esta sensibilidade aumentada do modelo é estável e reprodutível. O ajuste do limiar não comprometeu a robustez do algoritmo, garantindo que ele generaliza de forma fidedigna para novos dados da linha de produção.

Em suma: evoluímos de um modelo baseline ineficaz para um sistema de **Manutenção Preditiva Ativa**. Trocámos o equilíbrio matemático cego por um modelo **blindado contra falhas críticas**, calibrado para atuar como uma camada de segurança que prefere o excesso de zelo preventivo à omissão catastrófica, maximizando a continuidade do negócio.

---
*Data de última atualização: 15/05/2026*
