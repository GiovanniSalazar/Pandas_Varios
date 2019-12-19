# Pandas | Varios 

Para este ejemplo utilizaremos un dataset sobre la venta de videojuegos. Link del dataset : https://www.kaggle.com/gregorut/videogamesales/download

Archivo vgsales.csv del presente repositorio
Rank,Name,Platform,Year,Genre,Publisher,NA_Sales,EU_Sales,JP_Sales,Other_Sales,Global_Sales

Primero instalar Pandas con :

```sh
pip install pandas
```

### Ejemplos :

* Leer un archivo csv delimitado por comas.

```sh
import pandas as pd

#Leer un archivo csv
df=pd.read_csv('vgsales.csv',delimiter=',')
print(df.head())
```
* Leer un archivo csv delimitado por comas , pero indicandole solo las columnas que requieren.

```sh
import pandas as pd

df=pd.read_csv('vgsales.csv',delimiter=',',usecols=["Name", "Platform", "Year"])
print(df.head())
```
* Ventas globales por genero

```sh
import pandas as pd

df=pd.read_csv('vgsales.csv',delimiter=',',usecols=["Genre", "Global_Sales"])
print(df.groupby(['Genre']).sum())
```
* Crear csv partiendo desde un string

```sh
import pandas as pd
from io import StringIO

data = """
PDB,CHAIN,SP_PRIMARY
5d8b,N,P60490
5d8b,,P80377
5d8b,O,P60491
"""

df = pd.read_csv(StringIO(data), sep=',')
print(df)
```

* Dividir un dataframe dependiendo del valor de una columna

```sh
import pandas as pd
from io import StringIO

data = """
A,B,C
5d8a,N,1
5d8b,A,1
5d8c,B,2
5d8b,C,2
5d8f,Y,3
5d8b,X,3
"""

df = pd.read_csv(StringIO(data), sep=',')
for x, y in df.groupby('C'):
    print(y)
```

* Comparar valores de columnas usando pandas y numpy

```sh
import numpy as np
import pandas as pd
from io import StringIO

data = """
Col1,Adj,MA3
A,1,2
B,8,5
C,7,7
"""

df = pd.read_csv(StringIO(data),sep=',')
# > <  =
df['Adj Close > MA3'] =np.where(df['Adj']>df['MA3'],'1', np.where(df['Adj']<df['MA3'],'0', 'equals'))
print(df)
```
* Dividir una columnas en base a un caracter 

```sh
import pandas as pd
from pandas import Series
import numpy as np
dfm = pd.DataFrame({'id': np.arange(5), 'words': ['apple;pear;orange', 'apple', 'pear;grape', 'orange', 'orange;pear']})
print(dfm.words.str.split(';').explode().value_counts())
```
* Convertir columnas en json 

```sh
import pandas as pd
from io import StringIO

data = """
NETWORK_USERID,TALK_STATUS,TALK_STATUS_EXPIRY,CATEGORY,CATEGORY_EXPIRY
f40daf16-f069-4c1d-ac2a-d1504f0fc147,Talker,15/12/2020,MN_FFBQ,23/12/2019
4d3e9e50-f88b-4c0b-a700-881474f992ab,Lurker,15/12/2020,MN_FFBQ,23/12/2019
c2e2fa63-efad-4b7d-b11e-77d9c8692677,Lurker,15/12/2020,MN_FFBQ,23/12/2019
c46a2af4-0c20-486e-9ae0-6323269f252d,Lurker,15/12/2020,MN_FFBQ,23/12/2019
f6f88be2-dca6-4129-93ed-2b32a633e1ec,Talker,15/12/2020,MN_FFBQ,23/12/2019
"""
df = pd.read_csv(StringIO(data),sep=',')

v = df.values.tolist()
lst = []
col1=['value','expiry']

for row in v:
    lst.append([row[0],'['+str(dict(zip(col1, [row[3],row[4]])))+']'])

df1 = pd.DataFrame(lst,columns=['NETWORK_USERID','CATEGORY'])
print(df1)
```
* Contar palabras de una columna 

```sh
from collections import Counter
from io import StringIO
import pandas as pd

data = """
Expression,Description
Sad,"people are sad because they got no money."
Happy,"people are happy because ..really."
Sad,"people are miserable because they broke up"
Happy,"They got good money"
"""
#read csv
df = pd.read_csv(StringIO(data),sep=',')
#Only select result where Expression = 'Sad'
dfToList=df[df['Expression']=='Sad']['Description'].tolist()
# All dict 
print(dict(Counter(" ".join(dfToList).split(" ")).items()))

words=dict(Counter(" ".join(dfToList).split(" ")).items())

for key in words:
  # Here your conditions what you want
  print(key, '->', words[key])
```

* Pandas leer tabla html

```sh
from bs4 import BeautifulSoup
import pandas as pd
import re

soup = BeautifulSoup(open("table.html"), "lxml")
df_table = pd.read_html(s)
df_table = df_table[0]
```
* Realizar un pivot de un dataframe

```sh
import pandas as pd

columns = ['country','rate','year','source']
df = pd.DataFrame(columns=columns)
df.loc[0] = ['US',25.2,2003,'international']
df.loc[1] = ['US',30,2003,'goverment']
df.loc[2] = ['US',35,2005,'goverment']
df.loc[3] = ['China',12.5,2004,'goverment']
df.loc[4] = ['China',15.6,2004,'international']
df1=df.groupby(['country', 'year'], as_index=False)['rate'].mean()
print(df1)
print('-----')
df2 = df1.pivot_table(index=['country'], columns='year', values='rate').reset_index().fillna('')
print(df2)
```
