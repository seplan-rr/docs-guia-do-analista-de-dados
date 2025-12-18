# Transformação

> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].

## Métodos e Atributos da Estrutura de DataFrame

Um DataFrame é uma estrutura de dados que organiza dados em uma tabela
bidimensional de linhas e colunas, similar a uma planilha. Essa é a estrutura de
dados mais comum usada na manipulação dos dados, geralmente utilizando `pandas`.

Alguns métodos descritivos sobre os dados de um DataFrame `pandas` são
apresentados abaixo:

```python
import pandas as pd

df = pd.read_csv("dados.csv")

# Mostrar colunas disponíveis no DataFrame
print(df.columns)

# Mostrar resumo dos dados, como número de colunas, tipos de dados, etc. no DataFrame
print(data.info())

# Mostrar uma descrição estatística dos dados no DataFrame
print(data.describe())
```

## Como Lidar com Datasets Grandes

Como os computadores atuais têm um limite de memória muito baixo, entre 8 GB e
32 GB, geralmente. Quando lidamos com arquivos de dezenas de GB ou até TB, é
inviável simplesmente carregá-los na memória, tanto pelo consumo de recursos,
quanto pelo tempo necessário para realizar as operações.

Dessa forma, é importante aplicar os conceitos de *streaming* e *sharding* para
ou consumir os dados aos poucos ou filtrá-los o máximo possível antes de
carregá-los na memória (ainda assim, sem garantias de que os dados filtrados não
terão um volume absurdo).

A biblioteca `pandas` fornece algumas maneiras de lidar com esse problema, por
meio do parâmetro `chunksize`, que lê $n$ linhas por vez de um arquivo. Isso
permite trabalhar com múltiplos DataFrames, cada um trazendo uma parcela dos
dados.

### Importando um Dataset Grande

```python
import pandas as pd

for chunk in pd.read_csv(
    "dados.csv",
    chunksize=1000,  # Representa a quantidade de linhas por chunk
):
    # Realize suas manipulações aqui, considerando o chunk como um DataFrame com
    # a mesma estrutura do arquivo "dados.csv"
```

[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados
