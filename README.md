# CreditRiskClassification

## Passo a Passo do Projeto: Tarefa 02 Módulo 05

### Leitura das Bases de Dados:
Importe a biblioteca pandas: import pandas as pd.
Leia as bases de dados application_record.csv e pagamentos_largo.csv usando pd.read_csv.
Armazene as bases de dados nas variáveis propostas e pagamentos.

```python

import pandas as pd

propostas = pd.read_csv('application_record.csv')
pagamentos = pd.read_csv('pagamentos_largo.csv')
```
## Marcação de Default no Mês:

Utilize um loop para percorrer as colunas de pagamentos (de mes_00 a mes_24).
Para cada coluna, utilize o método isin(['2', '3', '4', '5']) para identificar os atrasos relevantes e converta para 0 ou 1 usando astype(int).

```python
for col in pagamentos.columns[1:]:
    pagamentos[col] = pagamentos[col].isin(['2', '3', '4', '5']).astype(int)
```
## Bons' e 'Maus' ao Longo de Todos os 24 Meses:

Crie uma nova coluna chamada 'mau_pagador' em pagamentos.
Utilize o método sum(axis=1) para calcular a soma de atrasos para cada cliente ao longo dos meses.
Marque como True se a soma for maior que 0 e False caso contrário.

```python
pagamentos['mau_pagador'] = pagamentos.iloc[:, 1:].sum(axis=1) > 0
```
## Marcando Proponentes Expostos ao Risco de Crédito:

Crie uma nova DataFrame chamada propostas_expostos filtrando as linhas em propostas cujos IDs estão presentes em pagamentos['ID'].

```python
propostas_expostos = propostas[propostas['ID'].isin(pagamentos['ID'])]
```

## Consolidando as Informações:

Faça uma junção (merge) entre propostas_expostos e as colunas relevantes de pagamentos ([['ID', 'mau_pagador']]) usando o ID como chave e preenchendo os valores ausentes com False.

```python
dados_finais = propostas_expostos.merge(pagamentos[['ID', 'mau_pagador']], on='ID', how='left').fillna(False)
```
## Exibindo os Resultados:

Imprima os resultados de cada etapa para verificar se as operações foram realizadas corretamente.

```python
print("Tarefa 1 - Marcar default no mês:\n", pagamentos.head())
print("\nTarefa 2 - 'Bons' e 'Maus' ao longo de todos os 24 meses:\n", pagamentos[['ID', 'mau_pagador']].head())
print("\nTarefa 3 - Marcando proponentes expostos ao risco de crédito:\n", propostas_expostos.head())
print("\nTarefa 4 - Consolidando as informações:\n", dados_finais.head())
print("\nTarefa 5 - Verificando:\n", contagem_default)
```
