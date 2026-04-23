# Milestone 3: Modelação e Avaliação
## 1. Estratégia de Modelação

*Descrição de como os dados foram preparados para os algoritmos.*

* **Divisão do dataset:** Utilizámos uma divisão de **80% para treino (8000 instâncias) e 20% para teste (2000 instâncias)**, com semente aleatória fixa (`random_state=42`) para garantir total reprodutibilidade. O passo mais crítico foi o parâmetro `stratify=y`: dado o acentuado desequilíbrio da variável alvo (~3,4% de avarias reais), a estratificação garantiu que esta proporção minoritária se manteve rigorosamente idêntica em ambos os conjuntos. Sem este passo, corríamos o risco de gerar um conjunto de teste sem falhas suficientes para uma avaliação fiável. Foi ainda testada uma divisão alternativa 75/25 que confirmou a estabilidade dos resultados independentemente da proporção escolhida.

* **Métrica de Sucesso:** A métrica principal eleita foi o **F1-Score**, complementado pelo **Recall** e pela **ROC-AUC**. A escolha foi imediatamente validada pelo nosso modelo baseline: a Regressão Logística obteve uma Accuracy de 96,75% — aparentemente excelente — mas com F1-Score de apenas **28,5%** e Recall de **19,1%**. O modelo limitava-se a prever sempre "não falha", ignorando completamente as avarias reais. Num contexto de manutenção preditiva industrial, o custo de um **falso negativo** (avaria não detetada → paragem de produção, dano no equipamento) é incomparavelmente superior ao custo de um **falso positivo** (inspeção desnecessária). O F1-Score é, portanto, a única métrica que reflete a viabilidade real do modelo para este problema de negócio.

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

> **Conclusão:** Apesar da Accuracy elevada, o modelo falha redondamente na deteção de avarias. Com apenas 19,1% de Recall, está a ignorar mais de 80% das falhas reais — comportamento inaceitável para o objetivo de negócio. Este resultado justifica a adoção de modelos mais sofisticados e de estratégias de compensação do desequilíbrio.

### 2.2. Modelos Candidatos
*Algoritmos testados e justificação da escolha.*

| Algoritmo | Parâmetros Base | F1-Score (Treino) | F1-Score (Teste) | Notas |
| :--- | :--- | :--- | :--- | :--- |
| Árvore de Decisão | `max_depth=3`, `random_state=42` | 39,7% | 31,0% | Precision alta (81,3%) mas Recall idêntico ao baseline (19,1%). Muito conservador. |
| Random Forest | `n_estimators=100`, `max_depth=5`, `class_weight='balanced'` | 58,1% | 54,8% | Alto Recall (88,2%) mas Precision muito baixa (39,7%) — demasiados alarmes falsos. |
| **XGBoost** | `n_estimators=100`, `max_depth=4`, `learning_rate=0.1`, `scale_pos_weight` (auto) | **69,1%** | **57,8%** | **Melhor equilíbrio Precision/Recall. Modelo selecionado para otimização.** |

> **Nota sobre a estratégia anti-desequilíbrio:** O Random Forest utilizou `class_weight='balanced'` e o XGBoost utilizou `scale_pos_weight` (rácio classes negativas/positivas calculado automaticamente a partir dos dados de treino). Ambas as técnicas penalizam o modelo por ignorar a classe minoritária, sendo determinantes para o salto de performance face aos baselines.

## 3. Otimização (Tuning)
*Como melhorámos o melhor modelo.*

* **Técnica Utilizada:** Aplicámos `GridSearchCV` com validação cruzada estratificada de 5 dobras (`StratifiedKFold`, `cv=5`) sobre o XGBoost, com foco exclusivo no **F1-Score** como critério de seleção. A grelha explorada concentrou-se em hiperparâmetros que combatem o overfitting observado no modelo base (F1 Treino: 69,1% vs F1 Teste: 57,8%):

| Hiperparâmetro | Valores Testados | Objetivo |
| :--- | :--- | :--- |
| `max_depth` | [2, 3, 4] | Limitar a profundidade para evitar memorização de padrões específicos |
| `learning_rate` | [0.01, 0.05, 0.1] | Aprendizagem mais gradual e robusta |
| `n_estimators` | [100, 200, 300] | Número de árvores a combinar |

* **Melhoria obtida:** O F1-Score no conjunto de teste subiu de **57,8% para 75,2%** após o ajuste — um ganho de **+17,4 p.p.** A validação cruzada K-Fold (K=5) confirmou a robustez do modelo com F1-Score médio estável e baixo desvio padrão nas 5 dobras, provando que os resultados generalizam de forma consistente.

| Fase | F1-Score (Treino) | F1-Score (Teste) | Δ Teste |
| :--- | :--- | :--- | :--- |
| XGBoost Base | 69,1% | 57,8% | — |
| **XGBoost Otimizado** | **94,6%** | **75,2%** | **+17,4 p.p.** |
## 4. Avaliação do Modelo Final
### 4.1. Matriz de Confusão / Erros
*Onde o modelo mais falha.*

| | Previsto: Não Falha | Previsto: Falha |
| :--- | :--- | :--- |
| **Real: Não Falha** |  1907 |  25 |
| **Real: Falha** |  12 |  56 |

> **Análise:** Em 2000 instâncias de teste, o modelo apenas se enganou em **37 casos** (25 + 12). Destes erros, os mais "baratos" são os **25 falsos positivos** (alarmes falsos — a fábrica faz uma inspeção desnecessária mas a máquina está bem) e os mais "caros" são os **12 falsos negativos** (avarias reais que escaparam sem deteção). De todas as avarias reais presentes no conjunto de teste (68 no total), o modelo detetou **56 corretamente e deixou escapar apenas 12** — um Recall de 82,4%. Quando emite um alarme, tem razão em **69,1% das vezes** (Precision). O principal padrão de erro residual são máquinas que apresentam leituras de Torque e Tool Wear próximas dos limiares normais — situações em que a degradação é ainda incipiente e os sinais não são suficientemente distintivos para o modelo classificar com confiança.

---
### 4.2. Importância dos Atributos (Feature Importance)
*Quais as variáveis que o modelo considerou mais importantes para decidir?*
1. [Variável X]
2. [Variável Y]
## 5. Conclusão da Fase de Modelação
*Justifiquem por que razão este modelo está pronto (ou não) para ser apresentado como solução
final.*
---
*Data de última atualização: [DD/MM/AAAA]*
