# Extração
 
## Formas de consumir arquivos CSV

Arquivos CSV são compatíveis com diversos software de leitura de planilha, o Excel sendo o mais comum. Porém quando trabalhamos com automação, muitas vezes é mais interessante manipular o CSV através do pandas, já aplicando certos filtros. 

Algumas sintaxes e parâmetros para manipulação de CSV a seguir:

Abrir e ler o arquivo em um dataframe, mostrando só as primeiras linhas

```python
import pandas as pd

df=pd.read_csv('arquivo.csv')

#.head mostra só as primeiras linhas
print(df.head())
```

Filtrar colunas específicas do dataframe

```python
#define as colunas para ler
usecols= ['id','nome','host_id','bairro','tipo_de_quarto','preco','estadia_minima']

hotel_data = pd.read_csv('data/listing.csv', index_col='id', usecols=usecols)

print(hotel_data.head())
```

Outros parâmetros

```python
#Declara o separador usado para ler os dados do csv, o padrão é a vírgula, mas você encontrará arquivos que utilizam outros
pd.read_csv('data.csv', sep=';')

#Declara a linha que será utilizada como header da planilha, o padrão é 0
pd.read_csv('data.csv', header=3)

#Define o datatype para os dados ou colunas
pd.read_csv('data.csv', dtype={'id':int, 'price':float})
```