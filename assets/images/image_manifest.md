# Manifesto de Imagens — Projeto Pulso

## Fonte

- Dataset: ECG Images dataset of Cardiac and COVID-19 Patients
- Autor: Sajid Hussain
- URL: https://www.kaggle.com/datasets/sajid576/ecg-image-dataset
- Licença: CC BY-NC-SA 4.0 (Creative Commons Attribution-NonCommercial-ShareAlike 4.0)
- Acesso: público, requer conta Kaggle

## Armazenamento externo

As imagens estão hospedadas no Google Drive devido ao volume de arquivos:

- Link: https://drive.google.com/drive/folders/1pWUyu3WJ6LYGySPHHTI8K3kaqvChJpKN?usp=share_link
- Acesso: público (qualquer pessoa com o link pode visualizar e baixar)

## Composição do conjunto

| Classe | Descrição clínica | Quantidade |
|---|---|---|
| Normal | Traçado de ECG sem alterações patológicas | ~25 imagens |
| Abnormal | Traçado com alterações diversas (arritmias, isquemia, bloqueios) | ~75 imagens |
| **Total** | | **100+ imagens** |

## Formato

- Extensão: .jpg e .png
- Tipo: imagens de traçados eletrocardiográficos digitalizados
- Resolução: variável entre as imagens do dataset

## Limitações e considerações de governança

- As imagens representam traçados de ECG em papel ou tela digitalizada, não sinais brutos em formato de série temporal
- A qualidade e resolução variam entre as imagens
- As classes (Normal / Abnormal) são rótulos do dataset original e devem ser tratadas como referência acadêmica, não como diagnóstico validado
- Não foi verificado se diferentes imagens pertencem ao mesmo paciente ou exame. Essa verificação será necessária antes de realizar divisões de treino e teste na Fase 4
- Não existe vínculo direto entre os pacientes da base Cleveland (dados numéricos) e os ECGs deste conjunto visual. A comparação entre modelos numérico e visual será feita em nível de padrão clínico e categoria diagnóstica, não de paciente individual

## Uso pretendido

As imagens serão utilizadas na Fase 4 do projeto para:

- Pré-processamento: conversão para escala de cinza, normalização e remoção de ruído
- Detecção de padrões: identificação de ondas P, QRS e T
- Classificação: uso de redes neurais convolucionais (CNN) para categorizar traçados por condição
