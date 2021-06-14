---
layout: post
title:  "Tratamento dos Dados"
date:   2020-04-04 12:00:00 -0300
categories: jekyll update
published: false
---

### Importando bibliotecas


```python
import numpy as np
import pandas as pd
import unicodedata
```

### Definindo função para carregar os dados


```python
def load_dataframe(year, month):
    filepath = 'data/DadosBO_{0}_{1}(ROUBO DE CELULAR).csv'.format(year, month)
    cols = ['DATAOCORRENCIA', 'PERIDOOCORRENCIA','LATITUDE','LONGITUDE','DESCRICAOLOCAL']
    return pd.read_csv(filepath, encoding='utf-16', delimiter='\t', decimal=',', dayfirst=True, usecols=cols)
```


```python
load_dataframe(2019, 7)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DATAOCORRENCIA</th>
      <th>PERIDOOCORRENCIA</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>DESCRICAOLOCAL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>30/06/2019</td>
      <td>DE MADRUGADA</td>
      <td>-23.739677</td>
      <td>-46.568831</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30/06/2019</td>
      <td>A NOITE</td>
      <td>-23.549807</td>
      <td>-46.818855</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30/06/2019</td>
      <td>PELA MANHÃ</td>
      <td>-23.517874</td>
      <td>-46.404587</td>
      <td>Comércio e serviços</td>
    </tr>
    <tr>
      <th>3</th>
      <td>29/06/2019</td>
      <td>A NOITE</td>
      <td>-23.683388</td>
      <td>-46.765615</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>4</th>
      <td>28/06/2019</td>
      <td>A NOITE</td>
      <td>-23.510888</td>
      <td>-46.406821</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>22997</th>
      <td>31/07/2019</td>
      <td>A NOITE</td>
      <td>-23.666017</td>
      <td>-46.715504</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>22998</th>
      <td>30/07/2019</td>
      <td>A NOITE</td>
      <td>-23.663700</td>
      <td>-46.542615</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>22999</th>
      <td>31/07/2019</td>
      <td>PELA MANHÃ</td>
      <td>-23.662083</td>
      <td>-46.669987</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>23000</th>
      <td>31/07/2019</td>
      <td>A NOITE</td>
      <td>-23.571818</td>
      <td>-46.503985</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>23001</th>
      <td>19/07/2019</td>
      <td>PELA MANHÃ</td>
      <td>-23.543473</td>
      <td>-46.616914</td>
      <td>Via Pública</td>
    </tr>
  </tbody>
</table>
<p>23002 rows × 5 columns</p>
</div>



<hr>

Erros comuns incluem valores ausentes, formatos mistos e inconsistentes.

### Dados ausentes
Tomemos como exemplo os dados de Abril de 2017. O método `info()` exibe as informações do *DataFrame*, incluindo o número de entradas, o número de valores não-ausentes, tipo dos objetos das colunas e a quantidade de memória utilizada:


```python
load_dataframe(2017, 2).info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 28924 entries, 0 to 28923
    Data columns (total 5 columns):
    DATAOCORRENCIA      28924 non-null object
    PERIDOOCORRENCIA    28924 non-null object
    LATITUDE            26322 non-null float64
    LONGITUDE           26322 non-null float64
    DESCRICAOLOCAL      28924 non-null object
    dtypes: float64(2), object(3)
    memory usage: 1.1+ MB


Observando as informações sobre este conjunto de dados, vemos que ela possui um total de 29841 amostras, e as colunas 'DATAOCORRENCIA', 'PERIDOOCORRENCIA' e 'DESCRICAOLOCAL' estão com os dados completos. Já as colunas 'LATITUDE' e 'LONGITUDE' possuem valores ausentes. Para eliminar as amostras que possuem valores ausentes, utilizamos o método `dropna()` do *DataFrame*:


```python
load_dataframe(2017, 4).dropna().info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 27228 entries, 0 to 29840
    Data columns (total 5 columns):
    DATAOCORRENCIA      27228 non-null object
    PERIDOOCORRENCIA    27228 non-null object
    LATITUDE            27228 non-null object
    LONGITUDE           27228 non-null float64
    DESCRICAOLOCAL      27228 non-null object
    dtypes: float64(1), object(4)
    memory usage: 1.2+ MB


    /opt/conda/lib/python3.7/site-packages/IPython/core/interactiveshell.py:3242: DtypeWarning: Columns (18) have mixed types. Specify dtype option on import or set low_memory=False.
      if (await self.run_code(code, result,  async_=asy)):


### Dados mistos
Utilizando o mesmo conjunto de dados, é possível verificar que os dados da coluna 'LATITUDE' é do tipo 'object', e não 'float64'. Investigando os dados entre as entradas 4115 e 4119, é percebe-se que a entrada 4117 possui dados inconsistentes.


```python
load_dataframe(2017, 4)[4115:4120]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DATAOCORRENCIA</th>
      <th>PERIDOOCORRENCIA</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>DESCRICAOLOCAL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4115</th>
      <td>04/04/2017</td>
      <td>A NOITE</td>
      <td>-23,593893571</td>
      <td>-46.501047</td>
      <td>Outros</td>
    </tr>
    <tr>
      <th>4116</th>
      <td>04/04/2017</td>
      <td>PELA MANHÃ</td>
      <td>-23,5796936808241</td>
      <td>-46.645494</td>
      <td>Outros</td>
    </tr>
    <tr>
      <th>4117</th>
      <td>04/04/2017</td>
      <td>DE MADRUGADA</td>
      <td>SP</td>
      <td>-23.652119</td>
      <td>-46,7359990728559</td>
    </tr>
    <tr>
      <th>4118</th>
      <td>05/04/2017</td>
      <td>DE MADRUGADA</td>
      <td>-23,4880860908695</td>
      <td>-46.571162</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>4119</th>
      <td>04/04/2017</td>
      <td>A NOITE</td>
      <td>-23,560560133</td>
      <td>-46.408481</td>
      <td>Via pública</td>
    </tr>
  </tbody>
</table>
</div>



Para eliminar este tipo de entrada, tentamos converter os dados de toda a coluna para o formato numérico com a função `to_numeric` da biblioteca Pandas. No caso de erro, o dado será definido como não-numérico e o eliminamos com o método `dropna()` do *DataFrame*.


```python
df = load_dataframe(2017, 4)[4115:4120]
df['LATITUDE'] = df['LATITUDE'].replace(',','.', regex=True)
df['LATITUDE'] = pd.to_numeric(df['LATITUDE'], errors='coerce')
df.dropna()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DATAOCORRENCIA</th>
      <th>PERIDOOCORRENCIA</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>DESCRICAOLOCAL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4115</th>
      <td>04/04/2017</td>
      <td>A NOITE</td>
      <td>-23.593894</td>
      <td>-46.501047</td>
      <td>Outros</td>
    </tr>
    <tr>
      <th>4116</th>
      <td>04/04/2017</td>
      <td>PELA MANHÃ</td>
      <td>-23.579694</td>
      <td>-46.645494</td>
      <td>Outros</td>
    </tr>
    <tr>
      <th>4118</th>
      <td>05/04/2017</td>
      <td>DE MADRUGADA</td>
      <td>-23.488086</td>
      <td>-46.571162</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>4119</th>
      <td>04/04/2017</td>
      <td>A NOITE</td>
      <td>-23.560560</td>
      <td>-46.408481</td>
      <td>Via pública</td>
    </tr>
  </tbody>
</table>
</div>



### Dados anormais
A entrada 8347 do conjunto de dados de Novembro de 2018 possui o ano anormal na coluna 'DATAOCORRENCIA':


```python
load_dataframe(2018, 9)[8345:8350]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DATAOCORRENCIA</th>
      <th>PERIDOOCORRENCIA</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>DESCRICAOLOCAL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8345</th>
      <td>12/09/2018</td>
      <td>DE MADRUGADA</td>
      <td>-23.930601</td>
      <td>-46.348533</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>8346</th>
      <td>29/08/2018</td>
      <td>A NOITE</td>
      <td>-23.469388</td>
      <td>-46.686008</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>8347</th>
      <td>29/08/1028</td>
      <td>A NOITE</td>
      <td>-23.448095</td>
      <td>-46.504321</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>8348</th>
      <td>12/09/2018</td>
      <td>PELA MANHÃ</td>
      <td>-23.704311</td>
      <td>-46.591965</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>8349</th>
      <td>11/09/2018</td>
      <td>A NOITE</td>
      <td>-23.730512</td>
      <td>-46.696965</td>
      <td>Via Pública</td>
    </tr>
  </tbody>
</table>
</div>



Para eliminar este tipo de entrada, tentamos converter os dados de toda a coluna para o formato *Datetime* com a função `to_datetime` da biblioteca Pandas. No caso de erro, o dado será definido como não-temporal e o eliminamos com o método `dropna()` do *DataFrame*.


```python
df = load_dataframe(2018, 9)[8345:8350]
df['DATAOCORRENCIA'] = pd.to_datetime(df['DATAOCORRENCIA'], dayfirst=True, errors='coerce')
df.dropna(inplace=True)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DATAOCORRENCIA</th>
      <th>PERIDOOCORRENCIA</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>DESCRICAOLOCAL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8345</th>
      <td>2018-09-12</td>
      <td>DE MADRUGADA</td>
      <td>-23.930601</td>
      <td>-46.348533</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>8346</th>
      <td>2018-08-29</td>
      <td>A NOITE</td>
      <td>-23.469388</td>
      <td>-46.686008</td>
      <td>Via pública</td>
    </tr>
    <tr>
      <th>8348</th>
      <td>2018-09-12</td>
      <td>PELA MANHÃ</td>
      <td>-23.704311</td>
      <td>-46.591965</td>
      <td>Via Pública</td>
    </tr>
    <tr>
      <th>8349</th>
      <td>2018-09-11</td>
      <td>A NOITE</td>
      <td>-23.730512</td>
      <td>-46.696965</td>
      <td>Via Pública</td>
    </tr>
  </tbody>
</table>
</div>



### Dados inconsistentes
Utilizando o mesmo conjunto de dados, é possível verificar inconsistências quanto ao uso de letras maiúsculas nos textos da coluna 'DESCRICAOLOCAL'. O método `value_counts()` da série retorna a frequência de valores únicos.


```python
df['DESCRICAOLOCAL'].value_counts()
```




    Via Pública    2
    Via pública    2
    Name: DESCRICAOLOCAL, dtype: int64



### Definindo função para remoção dos acentos

Para resolver esta inconsistência, definimos uma função para remover acentos e depois convertemos os caracteres em minúsculas.


```python
def remove_accent(text):
    return ''.join((c for c in unicodedata.normalize('NFD', text) if unicodedata.category(c) != 'Mn'))
```


```python
df['DESCRICAOLOCAL'].apply(remove_accent).str.lower().value_counts()
```




    via publica    4
    Name: DESCRICAOLOCAL, dtype: int64



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


```python
df_raw = load_dataframe(2017, 4)
df_clear = clear_dataframe(df_raw)
df_clear.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 20329 entries, 0 to 29840
    Data columns (total 5 columns):
    DATAOCORRENCIA      20329 non-null datetime64[ns]
    PERIDOOCORRENCIA    20329 non-null object
    LATITUDE            20329 non-null float64
    LONGITUDE           20329 non-null float64
    DESCRICAOLOCAL      20329 non-null object
    dtypes: datetime64[ns](1), float64(2), object(2)
    memory usage: 952.9+ KB

