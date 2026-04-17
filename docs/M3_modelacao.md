# Milestone 3: Modelação e Avaliação
## 1. Estratégia de Modelação

* **Divisão do dataset:**: Para preparar os dados para os algoritmos de Machine Learning, utilizámos uma divisão de 80% dos dados para treino (8000 instâncias) e 20% para teste (2000 instâncias). Para garantir a total reprodutibilidade dos resultados em execuções futuras, fixámos a semente aleatória (random_state=42).
O passo técnico mais crítico nesta divisão foi a utilização do parâmetro de estratificação (stratify=y). Dada a natureza altamente desbalanceada da nossa variável alvo (apenas ~3,4% de avarias reais), a estratificação garantiu que esta proporção minoritária se mantivesse rigorosamente idêntica tanto no conjunto de treino como no conjunto de teste. Sem este passo, corríamos o risco estatístico de gerar um conjunto de teste sem falhas suficientes para uma avaliação fiável do modelo.
* **Métrica de Sucesso:**
A métrica principal eleita para avaliar o sucesso e a performance dos modelos foi o **F1-Score**, complementado pela métrica de **Recall**  e pela área sob a curva **ROC-AUC**. 
A escolha destas métricas justifica-se pelo desequilíbrio das classes e foi imediatamente comprovada pela execução do nosso Modelo Baseline (Regressão Logística). O baseline obteve uma Exatidão (*Accuracy*) de 96,75%, o que, numa análise superficial, pareceria um excelente resultado. No entanto, o seu F1-Score foi de apenas 28,5% e o Recall de uns meros 19,1%. Isto significa que o modelo, apesar de acertar quase sempre, está apenas a prever a classe maioritária ("não falha") e a falhar redondamente na deteção das verdadeiras avarias (falsos negativos). 
Num contexto de manutenção preditiva industrial, o custo de não prever uma falha (falso negativo, que resulta em paragem de produção e quebra da máquina) é muito superior ao custo de uma inspeção desnecessária (falso positivo). Por isso, o F1-Score (que equilibra *Precision* e *Recall*) é a única métrica que reflete a verdadeira viabilidade do nosso modelo para a resolução do problema de negócio.
## 2. Experiências Realizadas
### 2.1. Modelo Baseline
*O ponto de partida simples.*
* **Algoritmo:** (p/ex.: Regressão Logística)
* **Resultado:** (p/ex.: Accuracy: 0.72)
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
