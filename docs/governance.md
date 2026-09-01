# Governança de Dados — Projeto Pulso

Este documento registra as decisões, limitações e responsabilidades relacionadas aos dados utilizados no projeto Pulso. Ele não é um anexo burocrático: é parte central do projeto, porque acreditamos que sistemas de IA responsáveis começam pela transparência sobre os dados que os alimentam.

---

## 1. Princípios que orientam este projeto

O Pulso foi construído com a premissa de que dados cardiológicos carregam consequências reais para pessoas reais. Por isso, cada decisão sobre coleta, uso e limitações dos dados foi documentada com o mesmo cuidado dedicado ao código.

Três perguntas guiam este documento:

- De onde vêm esses dados?
- O que eles representam — e o que não representam?
- Quais riscos existem se um modelo treinado nesses dados for mal interpretado?

---

## 2. Dataset numérico — UCI Heart Disease (Cleveland)

### Origem

- Fonte: UCI Machine Learning Repository
- URL: https://archive.ics.uci.edu/dataset/45/heart+disease
- Subconjunto utilizado: processed.cleveland.data
- Coletado originalmente por: Robert Detrano, V.A. Medical Center, Long Beach e Cleveland Clinic Foundation
- Ano de coleta: 1988
- Licença: Creative Commons Attribution 4.0 International (CC BY 4.0)

### Descrição

O dataset contém 303 registros de pacientes submetidos a angiografia coronariana. Cada registro representa um paciente e inclui 13 variáveis clínicas e fisiológicas, além de um diagnóstico de presença ou ausência de doença cardíaca.

### Variáveis

| Variável | Descrição | Tipo |
|---|---|---|
| age | Idade do paciente (anos) | Numérica |
| sex | Sexo biológico (1 = masculino, 0 = feminino) | Categórica binária |
| cp | Tipo de dor no peito (1 = angina típica, 2 = angina atípica, 3 = dor não anginosa, 4 = assintomático) | Categórica ordinal |
| trestbps | Pressão arterial em repouso na admissão hospitalar (mmHg) | Numérica |
| chol | Colesterol sérico (mg/dl) | Numérica |
| fbs | Glicemia em jejum > 120 mg/dl (1 = sim, 0 = não) | Categórica binária |
| restecg | Resultado do eletrocardiograma em repouso (0 = normal, 1 = anormalidade ST-T, 2 = hipertrofia ventricular esquerda) | Categórica ordinal |
| thalach | Frequência cardíaca máxima atingida durante teste de esforço (bpm) | Numérica |
| exang | Angina induzida por exercício (1 = sim, 0 = não) | Categórica binária |
| oldpeak | Depressão do segmento ST induzida por exercício em relação ao repouso | Numérica |
| slope | Inclinação do segmento ST no pico do exercício (1 = ascendente, 2 = plano, 3 = descendente) | Categórica ordinal |
| ca | Número de vasos principais coloridos por fluoroscopia (0–3) | Numérica discreta |
| thal | Resultado do teste de talassemia (3 = normal, 6 = defeito fixo, 7 = defeito reversível) | Categórica |
| target | Diagnóstico de doença cardíaca (0 = ausente, 1–4 = graus crescentes de presença) | Variável-alvo |

### Qualidade dos dados

- Registros totais: 303
- Valores ausentes: presentes nas colunas `ca` e `thal`, representados pelo caractere `?`
- Esses valores ausentes precisarão ser tratados nas fases seguintes antes do treinamento de qualquer modelo

### Distribuição por sexo

| Sexo | Registros | Percentual |
|---|---|---|
| Masculino (sex = 1) | 206 | 68% |
| Feminino (sex = 0) | 97 | 32% |

Esta distribuição é um dado de governança crítico e está documentada intencionalmente como ponto central do projeto.

---

## 3. Viés identificado — representação de gênero

Esta é a questão de governança mais relevante do dataset e o eixo temático central do projeto Pulso.

### O problema clínico

A doença arterial coronariana em mulheres apresenta sintomas frequentemente distintos dos sintomas masculinos clássicos. Enquanto homens tendem a apresentar dor torácica típica (angina), mulheres frequentemente relatam fadiga, náusea, dor nas costas ou no maxilar — sintomas que historicamente foram subvalorizados em triagens clínicas e em estudos epidemiológicos.

Essa diferença não é apenas clínica: ela se reflete nos dados. Estudos publicados nos Arquivos Brasileiros de Cardiologia e em periódicos internacionais documentam que mulheres com doença cardíaca são diagnosticadas mais tarde, encaminhadas com menos frequência para exames invasivos e sub-representadas em ensaios clínicos.

### O problema nos dados

O UCI Cleveland dataset reflete esse contexto histórico. Com apenas 32% de registros femininos, qualquer modelo de Machine Learning treinado diretamente nesse dataset terá menos exemplos para aprender padrões de risco em mulheres. Isso pode resultar em:

- Menor acurácia para pacientes do sexo feminino
- Maior taxa de falsos negativos em mulheres (doença presente, modelo não detecta)
- Perpetuação de um viés clínico histórico por meio de um sistema automatizado

### O que o projeto Pulso faz com isso

O Pulso não ignora esse viés — ele o nomeia, documenta e usa como motivação para decisões técnicas nas fases seguintes:

- Na Fase 2 (Machine Learning), a avaliação do modelo será segmentada por sexo, não apenas pelo desempenho geral
- Métricas como recall por subgrupo serão priorizadas sobre acurácia global
- Estratégias de balanceamento de classes serão avaliadas com atenção ao impacto diferencial por gênero

---

## 4. Dataset textual

### Texto 1

- Arquivo: `assets/texts/genero_doenca_cardiovascular.txt`
- Tema: diferenças de gênero no diagnóstico e tratamento de doenças cardiovasculares
- Fonte: literatura científica brasileira (SciELO / Arquivos Brasileiros de Cardiologia)
- Uso pretendido: análise de sentimentos, extração de entidades clínicas, classificação de tópicos em NLP (Fase 5)

### Texto 2

- Arquivo: `assets/texts/monitoramento_remoto_cardiaco.txt`
- Tema: telemonitoramento cardíaco e uso de dispositivos remotos no acompanhamento de pacientes
- Fonte: literatura científica brasileira (SciELO / BVS)
- Uso pretendido: extração de informações sobre protocolos de monitoramento, base para o módulo de IoT (Fase 3)

---

## 5. Dataset visual — imagens de ECG

- Fonte: Kaggle (dataset público de imagens de ECG)
- Link para acesso: https://drive.google.com/drive/folders/1pWUyu3WJ6LYGySPHHTI8K3kaqvChJpKN?usp=share_link
- Formato: imagens .jpg e .png de traçados eletrocardiográficos
- Quantidade: mínimo 100 imagens
- Uso pretendido: detecção de padrões visuais, identificação de anomalias, classificação de ritmos cardíacos por Visão Computacional (Fase 4)

### Limitações das imagens

- As imagens representam traçados de ECG em papel ou tela, não sinais brutos
- A qualidade e resolução variam entre as imagens do dataset
- A classificação das imagens por condição clínica deve ser tratada como referência acadêmica, não como diagnóstico validado

---

## 6. Uso pretendido e uso não pretendido

### Uso pretendido

O Pulso é um projeto acadêmico desenvolvido no contexto da Graduação Tecnológica em Inteligência Artificial da FIAP. Seus dados, modelos e análises têm finalidade exclusivamente educacional e de pesquisa aplicada.

O objetivo é demonstrar como dados cardiológicos podem ser coletados, organizados, analisados e utilizados para treinar modelos de IA em um ambiente controlado e supervisionado.

### Uso não pretendido

Este projeto não deve ser utilizado para:

- Diagnóstico clínico real de pacientes
- Triagem médica em ambientes hospitalares ou de atenção primária
- Tomada de decisão clínica sem supervisão de profissional de saúde habilitado
- Qualquer aplicação que substitua avaliação médica humana

Os modelos desenvolvidos neste projeto não foram validados clinicamente e não atendem aos requisitos regulatórios de dispositivos médicos (ANVISA, CE Mark, FDA).

---

## 7. Supervisão humana

Em todas as fases do projeto, os resultados produzidos pelos modelos de IA serão tratados como suporte à decisão, não como decisão final. A supervisão humana é um princípio não negociável do Pulso.

Isso significa que, mesmo nas simulações acadêmicas, os outputs dos modelos serão sempre apresentados com:

- Nível de confiança
- Limitações conhecidas
- Recomendação de validação por especialista

---

## 8. Privacidade

O dataset UCI Cleveland é composto por dados anonimizados coletados em ambiente hospitalar controlado. Não há informações de identificação pessoal (nome, CPF, endereço, contato) nos registros.

Os textos utilizados são de domínio público ou acesso aberto.

As imagens de ECG utilizadas são provenientes de datasets públicos sem identificação de pacientes.

---

## 9. Versionamento

| Versão | Data | Descrição |
|---|---|---|
| 1.0 | Agosto 2026 | Versão inicial — Fase 1 do projeto Pulso |

---

## 10. Responsabilidade

Este documento foi elaborado por Richard Schmitz no contexto da Fase 1 do projeto Pulso, disciplina de Inteligência Artificial da FIAP, turma 2025/2.

Dúvidas, correções ou sugestões podem ser encaminhadas pelo repositório GitHub do projeto.
