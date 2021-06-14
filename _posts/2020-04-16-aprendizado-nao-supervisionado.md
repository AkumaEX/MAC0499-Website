---
layout: post
title:  "Aprendizado não Supervisionado"
date:   2020-04-16 12:00:00 -0300
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

### Definindo função para exibir agrupamentos


```python
def print_clusters(df):
    map_sp = plt.imread('image/sao_paulo.png')
    groups = df.groupby('GRUPO')
    fig, ax = plt.subplots(figsize=(13,13))
    for name, group in groups:
        ax.scatter(group['LONGITUDE'], group['LATITUDE'], label=name, s=1)
    ax.set_title('Agrupamento de Roubos de Celulares em São Paulo')
    ax.set_xlabel('Longitude')
    ax.set_ylabel('Latitude')
    ax.imshow(map_sp, extent=(lon_min, lon_max, lat_max, lat_min), alpha=0.5)
    plt.show()
```

<hr>

### Carregando dados de Janeiro, Fevereiro e Março de 2019


```python
df = pd.concat([load_dataframe(2019, month) for month in [1, 2, 3]])
df = clear_dataframe(df)
df = slice_dataframe(df)
```

### Aprendizado não Supervisionado KMeans

Importamos a biblioteca `KMeans`, que realiza o agrupamento de instâncias similares. Para os dados, realizamos o agrupamento das entradas por localização.


```python
from sklearn.cluster import KMeans
```

### Resultado dos agrupamentos para 1 grupo


```python
kmeans = KMeans(n_clusters=1)
df['GRUPO'] = kmeans.fit_predict(df[['LATITUDE', 'LONGITUDE']])
print_clusters(df)
```


![png]({{ site.baseurl }}/assets/2020-04-16/output_19_0.png)


### Resultado do agrupamento para 5 grupos


```python
kmeans = KMeans(n_clusters=5)
df['GRUPO'] = kmeans.fit_predict(df[['LATITUDE', 'LONGITUDE']])
print_clusters(df)
```


![png]({{ site.baseurl }}/assets/2020-04-16/output_21_0.png)


### Resultado do agrupamento para 10 grupos


```python
kmeans = KMeans(n_clusters=10)
df['GRUPO'] = kmeans.fit_predict(df[['LATITUDE', 'LONGITUDE']])
print_clusters(df)
```


![png]({{ site.baseurl }}/assets/2020-04-16/output_23_0.png)


### Resultado do agrupamento para 2194 grupos

Para gerar grupos com média de 20 entradas, o número de grupos é definido dividindo o total de entradas por 20:


```python
n_rows, n_cols = df.shape
kmeans = KMeans(n_clusters=n_rows//20)
df['GRUPO'] = kmeans.fit_predict(df[['LATITUDE', 'LONGITUDE']])
print_clusters(df)
```


![png]({{ site.baseurl }}/assets/2020-04-16/output_25_0.png)


A proporção de tipos de local de crime por grupo é mais representativo do que levar em conta a cidade toda. Por exemplo, a proporção de roubos em estações/terminais deve ser maior em um grupo mais próximo a um terminal.
