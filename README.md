# Manutenção Preditiva em Indústria (IoT)

> **Previsão de falhas de máquinas através de dados de sensores para evitar paragens não planeadas.**

![Status do Projeto](https://img.shields.io/badge/Status-Em_Desenvolvimento-yellow)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)

## Sobre o Projeto

Na indústria 4.0, o tempo de inatividade não planeado custa milhões. A **Manutenção Preditiva** é a solução chave para este problema, permitindo que as empresas intervenham nas máquinas antes que estas avariem, baseando-se em dados reais e não em estimativas temporais.

Este projeto utiliza técnicas de **Machine Learning** e análise de dados de sensores (IoT) para prever a probabilidade de falha de equipamentos. O objetivo principal é criar um modelo capaz de distinguir entre o funcionamento normal e estados de pré-falha, analisando variáveis como temperatura, vibração (inferida), rotação e torque.

### Objetivos
* **O Problema:** Prever *quando* e *se* uma máquina vai avariar com base em leituras de sensores.
* **O Valor:** Redução de custos operacionais e prevenção de paragens críticas na linha de produção.
* **O Desafio Técnico:** Análise de correlações complexas entre múltiplos sensores e deteção de padrões em dados desbalanceados.

---

## O Conjunto de Dados (Dataset)

Utilizamos o **AI4I 2020 Predictive Maintenance Dataset**, disponibilizado pelo UCI Machine Learning Repository. Este é um dataset sintético que reflete dados reais de manutenção preditiva na indústria.

* **Fonte:** [UCI Machine Learning Repository - AI4I 2020](https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset)
* **Tamanho:** 10.000 instâncias (linhas).
* **Features Principais:**
    * `Air temperature [K]`: Temperatura ambiente.
    * `Process temperature [K]`: Temperatura do processo.
    * `Rotational speed [rpm]`: Rotação do fuso.
    * `Torque [Nm]`: Torque aplicado.
    * `Tool wear [min]`: Desgaste da ferramenta (em minutos).
* **Target (Alvo):** `Machine failure`.

---

## Tecnologias Utilizadas

* **Linguagem:** Python
* **Análise de Dados:** Pandas, NumPy
* **Visualização:** Matplotlib, Seaborn
* **Machine Learning:** Scikit-Learn (Random Forest, XGBoost, etc.)
* **Ambiente:** Jupyter Notebooks

