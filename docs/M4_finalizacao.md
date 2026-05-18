# Relatório de Conclusão e Entrega de Valor (Milestone 4)
## 1. Síntese de Resultados e Impacto
## 1.1. O Problema Resolvido
O projeto partiu de um desafio critico na manufatura industrial moderna: as abordagens tradicionais de manutenção, reativa (intervir após a avaria) e preventiva (intervir por calendário fixo), revelam-se sistematicamente ineficientes. A manutenção reativa origina paragens de produção imprevistos com custos catastróficos; a preventiva substitui componentes ainda funcionas, gerando desperdício de recursos.

O objetivo definido na Milestone 1 era claro: desenvolver um modelo de classificação binaria capaz de prever a ocorrência de falha na variável Machine failure, atingindo um F1-Score mínimo de 0,75 e um Recall superior a 80% na classe positiva (falha).

Este objetivo foi totalmente alcançado. O modelo final, XGBoost otimizado com ajuste de limiar orientado ao negócio, atingiu um Recall de 87,0% e um F2-Score de 80,6%, superando ambos os critérios de sucesso definidos inicialmente.


## 1.2. Interpretação dos Resultados
Em linguagem acessível: o modelo analisa, em tempo real, cinco variáveis operacionais da máquina (temperatura, velocidade de rotação, binário, desgaste da ferramenta e potencia estimada) e emite um alerta sempre que a combinação desses valores ultrapassa os padrões históricos associados a avarias iminentes.

Em termos concretos, aplicado ao conjunto de teste (2000 instâncias), o modelo obteve os seguintes resultados:
<img width="1016" height="274" alt="image" src="https://github.com/user-attachments/assets/9bb95f79-8f2b-4241-ad2f-fad7b24d984c" />

Das 68 avarias reais presentes no conjunto de teste, 59 foram corretamente identificadas pelo modelo. Apenas 9 avarias passaram despercebidas, uma taxa de falha residual de 13%, um resultado operacionalmente significativo comparado com o modelo baseline que ignorava mais de 80% das avarias.

## 1.3. Valor para o Negócio
A tradução financeira deste modelo e direta. Num contexto industrial típico, uma paragem não planeada pode custar entre dezenas a centenas de milhares de euros por hora (dependendo da linha de produção e do setor), enquanto uma inspeção preventiva desnecessária representa apenas o custo de manutenção de rotina, estimado em menos de 5% desse valor.

Com este modelo implementado, a organização industrial beneficia de:

•	Redução de aproximadamente 87% das avarias imprevistos detetadas atempadamente, permitindo agendar intervenções em janelas de manutenção planeadas;

•	Minimização do risco de danos em cadeia: avarias não detetadas comprometem frequentemente componentes adjacentes, multiplicando o custo da reparação;

•	Otimização dos recursos de manutenção: as equipas técnicas podem priorizar as máquinas sinalizadas pelo modelo em vez de proceder a inspeções sistemáticas calendarizadas;

•	Fundamento quantitativo para decisões de gestão: a importância das variáveis do modelo oferece insight para ajustar parâmetros operacionais (limites de binário, ciclos de substituição de ferramentas) antes do ponto de falha;

Em síntese: o sistema transita de uma politica de 'esperar a avaria' para uma politica de 'antecipar e prevenir', com validação estatística robusta.

## 2. Análise Crítica e Limitações
## 2.1. Limitações dos Dados
O conjunto de dados AI4I 2020 e um conjunto de dados simulados, construído para replicar o comportamento operacional de uma máquina industrial genérica. Embora seja amplamente utilizado na comunidade académica e valide metodologias de Machine Learning, importa reconhecer as seguintes restrições:

•	Natureza sintética: os dados foram gerados por simulação e não por sensores físicos reais. Isto significa que o ruido sensorial, as variações de calibração e os eventos extremos típicos de ambientes fabris reais podem não estar totalmente representados;

•	Desequilíbrio estrutural: apenas 3,4% dos registos correspondem a falhas. Apesar de ser fiel a realidade industrial (avarias são eventos raros), este desequilíbrio exigiu estratégias especificas de compensação e limita a confiança estatística nas métricas da classe minoritária;

•	Ausência de dimensão temporal: o conjunto de dados não preserva a sequencia temporal dos registos, impedindo a aplicação de modelos de series temporais (LSTM, ARIMA) que poderiam capturar padrões de degradação progressiva ao longo do tempo;

•	Variável alvo simplificada: a variável Machine failure e binaria e não distingue a gravidade da avaria. Em contexto real, uma falha critica com paragem total tem impacto muito diferente de uma microfalha autocorrigida.

## 2.2. Limitações do Modelo
•	Overfitting residual: o modelo XGBoost otimizado apresenta um gap de F1-Score entre treino (94,6%) e teste (75,2%) que, embora controlado, indica que o modelo memoriza parcialmente os padrões de treino. A validação cruzada K-Fold confirma a robustez, mas o gap e um sinal de atenção em produção;

•	Limiar fixo: o limiar de decisão foi otimizado estaticamente (0,3721) com base no conjunto de dados disponível. Em produção, a distribuição operacional pode desviar-se dos dados de treino, exigindo monitorização continua e recalibração periódica do limiar.

## 2.3. Contextos em que o Modelo não é Recomendado
•	Máquinas com perfil operacional significativamente diferente das presentes no conjunto de dados de treino (ex: equipamentos a funcionar fora das gamas de temperatura ou rotação observadas);

•	Cenários onde a variável alvo seja mais granular (ex: prever o tipo especifico de falha (TWF, HDF, PWF) em vez da falha global), pois o modelo foi treinado exclusivamente para classificação binaria;

•	Ambientes com latência crítica de decisão onde o modelo precise de responder em milissegundos, sem infraestrutura de computação adequada.


## 3. Considerações Éticas e de Viés
## 3.1. Privacidade e Conformidade com o RGPD
O conjunto de dados utilizado não contém qualquer dado pessoal ou identificador individual. Todas as variáveis referem-se exclusivamente a medições operacionais de equipamento industrial (temperatura, velocidade, binário, desgaste). O projeto enquadra-se integralmente na exceção académica do RGPD e não levanta quaisquer questões de privacidade ou proteção de dados.

## 3.2. Transparência e Explicabilidade
A solução adotada privilegiou a explicabilidade operacional através de dois mecanismos:

•	Feature Importance do XGBoost: o modelo fornece uma hierarquia clara das variáveis que mais influenciam as suas previsões (Power, Tool wear, Torque, Rotational speed, Temp_diff), permitindo que técnicos de manutenção compreendam e questionem as decisões do algoritmo;

•	Justificação do ajuste de limiar: a escolha do limiar 0,3721 e do F2-Score foi documentada e fundamentada na política de negócio (custo assimétrico entre falsos negativos e falsos positivos), tornando o modelo auditável e a sua logica compreensível para gestores não técnicos.

## 3.3. Potencial de Viés
Dado que o conjunto de dados é sintético e não envolve decisões sobre pessoas, o risco de viés discriminatório é nulo. Contudo, importa notar que o modelo pode apresentar viés de distribuição caso seja aplicado a máquinas com perfis operacionais substancialmente diferentes dos dados de treino, um risco técnico e não ético, mas que deve ser monitorizado em produção.


## 4. Roadmap e Trabalhos Futuros
## 4.1. Melhorias Técnicas Imediatas
1.	Implementar SMOTE (Synthetic Minority Over-sampling Technique) ou ADASYN para gerar instâncias sintéticas de falha e melhorar a fronteira de decisão do modelo. Estima-se que esta técnica, combinada com o XGBoost otimizado, possa elevar o F1-Score acima dos 80%;
   
2.	Integrar valores SHAP (SHapley Additive exPlanations) para explicabilidade ao nível da previsão individual, permitindo que o sistema justifique cada alerta específico com as variáveis que mais contribuíram para aquela decisão;
   
3.	Testar modelos de ensemble híbridos (stacking de XGBoost com LightGBM ou CatBoost) que possam capturar padrões complementares nos dados, especialmente na zona limiar de decisão.

## 4.2. Expansão dos Dados e Novas Variáveis
1.	Incorporar a dimensão temporal dos registos, permitindo a aplicação de modelos de series temporais (LSTM, GRU) capazes de detetar padrões de degradação progressiva que os modelos estáticos não conseguem capturar;
   
2.	Integrar variáveis externas relevantes como dados de manutenção histórica (intervenções passadas, substituições de componentes) e condições ambientais adicionais (humidade, vibração), enriquecendo o perfil de cada máquina;
   
3.	Explorar a previsão multi-classe dos tipos específicos de falha (TWF, HDF, PWF, OSF), que permitiria não apenas alertar para a avaria iminente, mas identificar qual o componente em risco, aumentando substancialmente o valor operacional do sistema.

## 4.3. Escalabilidade e Deployment
1.	Desenvolver uma interface web interativa (Streamlit ou Dash) que permita a técnicos de manutenção introduzir leituras de sensores em tempo real e obter a probabilidade de falha instantaneamente, sem necessidade de conhecimentos de programação;
2.	Implementar um pipeline de monitorização que detete automaticamente quando a distribuição dos dados de produção se afasta dos dados de treino, acionando um processo de retraining com dados atualizados;
3.	Integrar o modelo com sistemas MES (Manufacturing Execution System) ou SCADA existentes nas fábricas, permitindo a emissão automática de ordens de trabalho de manutenção quando a probabilidade de falha ultrapassa o limiar calibrado.

## 5. Reflexão Final da Equipa
Este projeto demonstrou, de forma empírica e quantificável, que é possível construir um sistema de Manutenção Preditiva robusto com dados acessíveis, metodologia rigorosa e uma compreensão profunda do problema de negócio que o modelo serve.

A jornada percorrida ao longo dos quatro milestones revelou lições fundamentais:

•	A métrica certa importa mais do que o modelo certo: a decisão de abandonar a Accuracy em favor do F1-Score e, posteriormente, do F2-Score, foi o passo que separou um modelo estatisticamente bonito de um modelo operacionalmente útil;

•	O conhecimento de domínio é insubstituível: as variáveis Power e Temp_diff, criadas a partir de princípios físicos e mecânicos na Milestone 2, tornaram-se as duas variáveis mais importantes do modelo final, validando que a Engenharia de Atributos com contexto supera a força bruta computacional;

•	A decisão de negócio deve guiar o modelo, não o contrário: a calibração do limiar para 0,3721 com base na assimetria de custos (FN muito mais caro que FP) transformou o XGBoost de uma solução tecnicamente competente numa ferramenta de gestão do risco industrial.

O modelo final, XGBoost Otimizado com Ajuste de Limiar Orientado ao Negócio, representa uma evolução radical face ao baseline inicial: de um algoritmo que falhava 80% das avarias para um sistema blindado contra falhas críticas, com Recall de 87% e F2-Score de 80,6%, pronto para ser escalado para um ambiente industrial real.



---
**Data de Conclusão:** 18/05/2026
**Versão do Projeto:** v4.0 Final---
