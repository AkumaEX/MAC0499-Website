---
layout: post
title:  "Visualização dos Dados"
date:   2020-04-11 12:00:00 -0300
categories: jekyll update
published: false
---

### Importando Bibliotecas 


```python
import numpy as np
import pandas as pd
import unicodedata
import matplotlib.pyplot as plt
plt.rcParams['figure.figsize'] = [13,5]
```

### Definindo função para carregamento dos dados


```python
def load_dataframe(year, month):
    filepath = 'data/DadosBO_{0}_{1}(ROUBO DE CELULAR).csv'.format(year, month)
    cols = ['DATAOCORRENCIA', 'PERIDOOCORRENCIA','LATITUDE','LONGITUDE','DESCRICAOLOCAL']
    return pd.read_csv(filepath, encoding='utf-16', delimiter='\t', decimal=',', dayfirst=True, usecols=cols)
```

### Definindo função para remoção de acentos


```python
def remove_accent(text):
    return ''.join((c for c in unicodedata.normalize('NFD', text) if unicodedata.category(c) != 'Mn'))
```

### Definindo função para limpeza dos dados


```python
def clear_dataframe(df):
    df['LATITUDE'] = df['LATITUDE'].replace(',','.', regex=True)
    df['LONGITUDE'] = df['LONGITUDE'].replace(',','.', regex=True)
    df['LATITUDE'] = pd.to_numeric(df['LATITUDE'], errors='coerce')
    df['LONGITUDE'] = pd.to_numeric(df['LONGITUDE'], errors='coerce')
    df['DATAOCORRENCIA'] = pd.to_datetime(df['DATAOCORRENCIA'], dayfirst=True, errors='coerce')
    df['DESCRICAOLOCAL'] = df['DESCRICAOLOCAL'].apply(remove_accent).str.lower()
    df.dropna(inplace=True)
    return df[(df['DESCRICAOLOCAL'] != 'outros') & (df['PERIDOOCORRENCIA'] != 'EM HORA INCERTA')]
```

### Definindo função para obter um subconjunto de dados


```python
lat_min = -23.357177
lat_max = -23.817390
lon_min = -46.82527
lon_max = -46.365057

def slice_dataframe(df):
    return df[(df['LATITUDE'] < lat_min) & (df['LATITUDE'] > lat_max) & (df['LONGITUDE'] > lon_min) & (df['LONGITUDE'] < lon_max)]
```

### Definindo função para exibir o mapa da distribuição de crimes


```python
def print_map(df):
    map_sp = plt.imread('image/sao_paulo.png')
    groups = df.groupby('DESCRICAOLOCAL')
    fig, ax = plt.subplots(figsize=(13,13))
    for name, group in groups:
        ax.scatter(group['LONGITUDE'], group['LATITUDE'], label=name, s=5)
    ax.set_title('Distribuição de Roubos de Celulares em São Paulo')
    ax.set_xlabel('Longitude')
    ax.set_ylabel('latitude')
    ax.legend()
    ax.imshow(map_sp, extent=(lon_min, lon_max, lat_max, lat_min), alpha=0.5)
    plt.show()
```

<hr>

### Tratamento dos dados

Carregamos todos os dados de 2019 concatenando os resultados da função `load_dataframe()` e realizamos a limpeza com a função `clear_dataframe()`.


```python
df = pd.concat([load_dataframe(2019, month) for month in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]])
df = clear_dataframe(df)
```

### Selecionando um subconjunto

Extraímos uma parcela dos dados que pertencem à região da capital de São Paulo com a função `slice_dataframe()`, para facilitar a visualização:


```python
df = slice_dataframe(df)
```

### Exibindo a distribuição dos crimes em 4 tipos de locais

Selecionamos as entradas que percentem às 4 categorias da coluna 'DESCRICAOLOCAL': 'comercio e servicos', 'terminal/estacao', 'restaurante e afins' e 'rodovia/estrada', para facilitar a visualização:


```python
df4 = df[((df['DESCRICAOLOCAL'] == 'comercio e servicos') | (df['DESCRICAOLOCAL'] == 'terminal/estacao') | (df['DESCRICAOLOCAL'] == 'restaurante e afins') | (df['DESCRICAOLOCAL'] == 'rodovia/estrada')]
print_map(df4)
```


![png]({{ site.baseurl }}/assets/2020-04-11/output_19_0.png)


### Proporção de crimes em função do período do dia
A proporção de crimes em comércios e serviços diminui nos períodos 'A NOITE' e 'DE MADRUGADA', enquanto os crimes em restaurantes e afins aumentam nestes períodos.


```python
pd.crosstab(df4['PERIDOOCORRENCIA'], df4['DESCRICAOLOCAL'], normalize='index').plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7ff514078150>




![png]({{ site.baseurl}}/assets/2020-04-11/output_21_1.png)


### Proporção de crimes em função do dia da semana
A proporção de crimes em comércios e serviços diminui nos fins de semana. Nota-se também que a proporção de crimes em restaurantes e afins aumentam consideravelmente aos sábados.


```python
pd.crosstab(df4['DATAOCORRENCIA'].dt.day_name(), df4['DESCRICAOLOCAL'], normalize='index').plot.bar()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7ff5142a7b50>




![png]({{ site.baseurl}}/assets/2020-04-11/output_23_1.png)


### Distribuição dos crimes apenas em via pública

Total de crimes no ano de 2019 somente em via pública. É possível verifcar a gravidade do problema:


```python
dfp = df[df['DESCRICAOLOCAL'] == 'via publica']
print_map(dfp)
```


![png]({{ site.baseurl}}/assets/2020-04-11/output_25_0.png)
