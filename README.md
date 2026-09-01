# Pulso — Inteligência Cardiológica com Consciência de Dados

Projeto acadêmico desenvolvido para a disciplina de Inteligência Artificial da FIAP (turma 2025/2), como parte do método PBL — Project Based Learning.

O Pulso simula o ecossistema de dados de uma plataforma de cardiologia inteligente. Ao longo de sete fases, o projeto integra Machine Learning, IoT, Visão Computacional, NLP e séries temporais aplicados à saúde cardiovascular.

Esta é a Fase 1: construção e documentação da base de dados.

---

## Por que "Pulso"?

O pulso é o sinal mais imediato da vida cardíaca. É também o que um médico busca primeiro ao avaliar um paciente. Neste projeto, o nome carrega dois significados: o sinal fisiológico que queremos analisar e o ritmo de um sistema de IA que precisa ser monitorado, calibrado e compreendido — não apenas executado.

---

## Eixo temático

O Pulso foi construído em torno de uma questão clínica e de governança documentada na literatura médica:

**doenças cardíacas em mulheres são sistematicamente subdiagnosticadas porque os dados históricos que treinaram sistemas de triagem refletem uma população majoritariamente masculina.**

Isso não é uma hipótese acadêmica. É um problema real, com consequências reais para pacientes reais. O projeto usa esse contexto como fio condutor para todas as decisões técnicas — desde a escolha do dataset até a forma como os modelos serão avaliados nas fases seguintes.

Além disso, o Pulso incorpora desde a Fase 1 uma perspectiva de monitoramento contínuo: os dados são estruturados para refletir variáveis que poderiam ser coletadas por dispositivos wearables, preparando o terreno para a integração com IoT na Fase 3.

---

## Estrutura do repositório

```
pulso/
│
├── README.md
├── LICENSE
│
├── data/
│   └── heart_disease.csv
│
├── assets/
│   └── texts/
│       ├── genero_doenca_cardiovascular.txt
│       └── monitoramento_remoto_cardiaco.txt
│
└── docs/
    └── governance.md
```

As imagens de ECG estão hospedadas externamente devido ao volume de arquivos. O link de acesso está na seção de dados visuais abaixo.

---

## Parte 1 — Dados Numéricos

### Dataset

- Nome: UCI Heart Disease Dataset (subconjunto Cleveland)
- Arquivo: `data/heart_disease.csv`
- Registros: 303 pacientes
- Variáveis: 13 variáveis clínicas + 1 variável-alvo
- Fonte original: https://archive.ics.uci.edu/dataset/45/heart+disease
- Licença: CC BY 4.0

### Origem dos dados

Os dados foram coletados originalmente em 1988 por Robert Detrano no V.A. Medical Center de Long Beach e na Cleveland Clinic Foundation. Cada registro representa um paciente submetido a angiografia coronariana, com variáveis clínicas e fisiológicas coletadas antes do procedimento.

Os dados são reais, anonimizados e amplamente utilizados como benchmark em pesquisas de Machine Learning aplicado à saúde.

O arquivo foi obtido diretamente do UCI Machine Learning Repository e recebeu apenas a adição do cabeçalho de colunas, sem qualquer modificação nos valores originais.

### Variáveis e relevância clínica

| Variável | Descrição | Relevância para IA |
|---|---|---|
| age | Idade do paciente | Fator de risco primário; correlação direta com incidência de DCV |
| sex | Sexo biológico (1 = masculino, 0 = feminino) | Variável central para análise de viés de gênero no projeto |
| cp | Tipo de dor no peito | Sintoma diferencial entre homens e mulheres; alta importância preditiva |
| trestbps | Pressão arterial em repouso (mmHg) | Indicador de hipertensão; fator de risco modificável |
| chol | Colesterol sérico (mg/dl) | Fator de risco lipídico; relevante para modelos de triagem |
| fbs | Glicemia em jejum > 120 mg/dl | Indicador de diabetes; comorbidade frequente em pacientes cardíacos |
| restecg | ECG em repouso | Detecta anormalidades elétricas antes do esforço |
| thalach | Frequência cardíaca máxima no esforço | Variável que pode ser monitorada por wearables; relevante para IoT |
| exang | Angina induzida por exercício | Sintoma diferencial com alta especificidade diagnóstica |
| oldpeak | Depressão do segmento ST | Indicador de isquemia miocárdica durante esforço |
| slope | Inclinação do segmento ST | Complementa oldpeak na avaliação de isquemia |
| ca | Vasos principais coloridos por fluoroscopia | Indicador direto de obstrução coronariana |
| thal | Resultado do teste de talassemia | Avalia perfusão miocárdica; alta relevância diagnóstica |
| target | Diagnóstico de doença cardíaca (0 = ausente, 1–4 = presente) | Variável-alvo para modelos de classificação |

### Viés identificado

O dataset apresenta desequilíbrio de representação por sexo: 68% dos registros são masculinos e 32% femininos. Esse viés é documentado e tratado como questão central de governança. Mais detalhes em `docs/governance.md`.

### Valores ausentes

As colunas `ca` e `thal` contêm valores ausentes representados pelo caractere `?`. O tratamento desses valores será realizado na Fase 2, durante o pré-processamento para treinamento dos modelos.

---

## Parte 2 — Dados Textuais

Os textos estão armazenados em `assets/texts/` e foram obtidos de fontes científicas abertas em português.

### Texto 1 — Gênero e doença cardiovascular

- Arquivo: `assets/texts/genero_doenca_cardiovascular.txt`
- Tema: diferenças clínicas e epidemiológicas entre homens e mulheres no contexto de doenças cardiovasculares
- Fonte: Arquivos Brasileiros de Cardiologia / SciELO Brasil

### Texto 2 — Monitoramento remoto cardíaco

- Arquivo: `assets/texts/monitoramento_remoto_cardiaco.txt`
- Tema: telemonitoramento cardíaco, dispositivos remotos e acompanhamento de pacientes fora do ambiente hospitalar
- Fonte: SciELO Brasil / BVS

### Como esses textos serão explorados por NLP

Os dois textos foram escolhidos com as fases seguintes em mente. Na Fase 5, quando o módulo de NLP for desenvolvido, eles poderão ser utilizados para:

- Extração de entidades clínicas: identificar automaticamente termos como sintomas, medicamentos, condições e procedimentos mencionados nos textos
- Classificação de tópicos: categorizar trechos por área temática (diagnóstico, tratamento, prevenção, monitoramento)
- Análise de contexto semântico: construir uma base de conhecimento cardiológico que possa alimentar um assistente virtual ou sistema de triagem textual
- Base para RAG (Retrieval-Augmented Generation): os textos podem funcionar como documentos de referência para um modelo de linguagem responder perguntas sobre cardiologia com embasamento em fontes confiáveis

A escolha por textos em português é intencional: o sistema simulado tem como contexto o sistema de saúde brasileiro, e modelos de NLP performam melhor quando treinados ou alimentados com textos no idioma do domínio de aplicação.

---

## Parte 3 — Dados Visuais

### Dataset de imagens de ECG

- Fonte: Kaggle (dataset público de imagens de ECG)
- Formato: imagens .jpg e .png de traçados eletrocardiográficos
- Quantidade: 100+ imagens
- Link de acesso: https://drive.google.com/drive/folders/1pWUyu3WJ6LYGySPHHTI8K3kaqvChJpKN?usp=share_link

O link acima está configurado para acesso público. Qualquer pessoa com o link pode visualizar e baixar as imagens.

### Por que ECG

O eletrocardiograma é o exame de referência para avaliação do ritmo cardíaco. Ele registra a atividade elétrica do coração ao longo do tempo e é capaz de identificar arritmias, isquemias, infartos e outras condições com alta especificidade quando interpretado corretamente.

A escolha por imagens de ECG — e não por raio-X ou ecocardiograma — é estratégica: o ECG é o exame mais diretamente relacionado às variáveis do dataset numérico (como `restecg`, `thalach` e `oldpeak`), criando coerência entre os três tipos de dados do projeto.

### Como essas imagens serão analisadas por Visão Computacional

Na Fase 4, as imagens de ECG serão processadas com técnicas de Visão Computacional para:

- Pré-processamento: conversão para escala de cinza, normalização de contraste e remoção de ruído com filtros como Gaussian Blur
- Detecção de bordas: identificação dos picos e vales do traçado (ondas P, QRS, T) usando algoritmos como Canny
- Extração de padrões: análise de morfologia das ondas para identificar padrões associados a condições específicas
- Classificação: uso de redes neurais convolucionais (CNN) para categorizar traçados por tipo de ritmo ou condição cardíaca

A conexão entre o sinal visual do ECG e as variáveis numéricas do dataset Cleveland é um dos diferenciais do projeto: nas fases avançadas, será possível comparar o que o modelo numérico e o modelo visual identificam sobre o mesmo paciente simulado.

---

## Governança de Dados

A documentação completa de governança está em `docs/governance.md`.

Ela cobre: origem e licença dos dados, viés identificado, qualidade dos dados, valores ausentes, uso pretendido, uso não pretendido, privacidade e supervisão humana.

O Pulso trata governança como parte do projeto, não como apêndice.

---

## Aviso importante

Este projeto é acadêmico e tem finalidade exclusivamente educacional. Os dados, modelos e análises produzidos não devem ser utilizados para diagnóstico clínico, triagem médica ou qualquer decisão relacionada à saúde de pacientes reais. Nenhum componente deste projeto foi validado clinicamente.

---

## Autor

Richard Schmitz
Graduação Tecnológica em Inteligência Artificial — FIAP
Turma 2025/2
GitHub: https://github.com/flango2023
