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
| Accuracy | ~96,8% | 96,75% |
| Precision | — | 56,5% |
| Recall | — | 19,1% |
| F1-Score | 39,6% | 28,6% |

> **Conclusão:** Apesar da Accuracy elevada, o modelo falha redondamente na deteção de avarias. Com apenas 19,1% de Recall, está a ignorar mais de 80% das falhas reais — comportamento inaceitável para o objetivo de negócio. Este resultado justifica a adoção de modelos mais sofisticados e de estratégias de compensação do desequilíbrio.

### 2.2. Modelos Candidatos
*Listagem dos algoritmos testados e a justificação da escolha.*
| Algoritmo | Parâmetros Base | Métrica (Treino) | Métrica (Teste) | Notas |
| :--- | :--- | :--- | :--- | :--- |
| Random Forest | n_estimators=100 | 0.95 | 0.82 | Sinais de overfitting |
| XGBoost | default | 0.88 | 0.85 | Melhor generalização |
| SVM | kernel='rbf' | 0.80 | 0.79 | Lento no treino |
## 3. Otimização (Tuning)
*Descrevam como melhoraram o melhor modelo.*
* **Técnica Utilizada:** (p/ex.: "Utilizámos GridSearchCV para ajustar os hiperparâmetros
`max_depth` e `learning_rate`.")
* **Melhoria obtida:** (p/ex.: "O F1-Score subiu de 0.85 para 0.88 após o ajuste.")
## 4. Avaliação do Modelo Final
### 4.1. Matriz de Confusão / Erros
*Analisem onde o modelo mais falha.*
> **Análise:** (p/ex.: "O modelo ainda confunde a Classe A com a Classe B em 10% dos casos devido
à semelhança nos atributos X e Y.")
### 4.2. Importância dos Atributos (Feature Importance)
*Quais as variáveis que o modelo considerou mais importantes para decidir?*
1. [Variável X]
2. [Variável Y]
## 5. Conclusão da Fase de Modelação
*Justifiquem por que razão este modelo está pronto (ou não) para ser apresentado como solução
final.*
---
*Data de última atualização: [DD/MM/AAAA]*
