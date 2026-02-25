# Milestone 2: Análise Exploratória e Engenharia de Atributos
## 1. Análise Exploratória de Dados (EDA)
### 1.1. Distribuição da Variável Alvo
A nossa variável alvo 'Machine failure' está severamente desequilibrada, com 96,61% das ocorrências a representarem um funcionamento normal e apenas 3,39% a corresponderem a falhas reais.
### 1.2. Correlações Relevantes
*Quais as variáveis que têm maior relação com o problema? Incluam referências a gráficos que
geraram no Kaggle.*
* **Atributo A vs. Alvo:** (Ex: "Notámos que quanto maior a idade, menor a probabilidade de
cancelamento.")
* **Atributo B vs. Alvo:** (Ex: "O tipo de contrato mensal está fortemente ligado à saída de
clientes.")
## 2. Qualidade dos Dados e Limpeza
### 2.1. Tratamento de Dados em Falta (Missing Data)
Após a verificação inicial do dataset (através do método isnull().sum()), confirmou-se que o conjunto de dados está perfeitamente preenchido. Não existem valores nulos ou em falta em nenhuma das 14 colunas ao longo das 10.000 instâncias. Por conseguinte, não foi necessário aplicar qualquer técnica de imputação (como substituição por média/mediana) ou eliminação de registos, mantendo-se a integridade total dos dados originais para a fase de modelação.
### 2.2. Outliers e Inconsistências
2.2. Outliers e Inconsistências
Inconsistências:
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
