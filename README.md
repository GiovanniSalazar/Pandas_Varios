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
