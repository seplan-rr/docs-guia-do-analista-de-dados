# Extração

> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].
 
## Consumo de Arquivos CSV

Arquivos CSV são compatíveis com diversos *softwares* de leitura de planilha,
como Microsoft Excel e LibreOffice Calc. Porém, quando trabalhamos com
automação, temos que manipular os arquivos através de código, usando bibliotecas
como `pandas` e `polars`. Assim, temos alguns comandos básicos abaixo:

### Abrir o Arquivo CSV como DataFrame, Mostrando Suas Primeiras Linhas

```python
import pandas as pd

df = pd.read_csv("arquivo.csv")
print(df.head())
```

### Filtrar Colunas Específicas de um DataFrame

```python
cols = [
  "id",
  "nome",
  "host_id",
  "bairro",
  "tipo_de_quarto",
  "preco",
  "estadia_minima",
]

hotel_data = pd.read_csv(
  "hotel_data.csv",
  usecols=cols,
)
```

### Outras Parâmetros de Leitura do Arquivo

```python
# Declara o separador usado para ler os dados do CSV (O separador padrão é a
# vírgula)
pd.read_csv("data.csv", sep=";")

# Declara a linha que será utilizada como cabeçalho do DataFrame (A linha padrão
# é a primeira)
pd.read_csv("data.csv", header=3)

# Declara o tipo dos dados nas colunas do DataFrame. Não é aconselhável utilizar
# os tipos padrão do Python, como int ou float, mas sim os tipos anuláveis
# (nullable) indicados na documentação do pandas, como "Int64" e "Float64"
pd.read_csv("data.csv", dtype={
  "id": "Int64",
  "price": "Float64",
})
```

[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados
