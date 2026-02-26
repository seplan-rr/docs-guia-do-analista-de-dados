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

## APIs Públicas

APIs REST são compatíveis com diversas linguagens de programação e também frameworks
interativos como o [swagger](../api.md#swagger), no caso da prensença destes frameworks
de documentação é muito facilitado o trabalho para a compreensão do funcionamento da API.
Porém de modo geral se utiliza de código e bibliotecas como `pandas` para armazenar os 
dados requisitados. 

A seguir alguns exemplos iniciais para auxiliar no consumo da API:

#### Abrir o endpoint e criar um dataframe
<!--Explicar melhor a função de cada um-->
```python
import requests
import pandas as pd
from pandas import json_normalize

url = "https://api.exemplo.gov.br/v1/recursos"
params = {"ano": 2023, "uf": "SP", "limit": 100}  # filtros no servidor
headers = {"Accept": "application/json", "Authorization": "Bearer " + TOKEN}

resp = requests.get(url, params=params, headers=headers, timeout=30)
resp.raise_for_status()
data = resp.json()  # se for JSON

# Se a resposta for uma lista de objetos planos:
df = pd.DataFrame(data)
print(df.head())

# Se a resposta for objeto com campo "items" que contém a lista:
df = pd.DataFrame(data["items"])
print(df.head())

# Se houver estrutura aninhada, use json_normalize:
df = json_normalize(data, record_path="items", meta=["ano", "uf"], errors="ignore")
print(df.head())

```
#### Lidando com paginação
[Paginação] é o mecanismo pelo qual uma API divide grandes conjuntos de dados em partes
(páginas) menores. Existe para facilitar cache, melhorar latência, proteger o servidor
e permitir controle de carga. Processar incorretamente a paginação de uma API REST
devolve o dataset incompleto, comprometendo sua análise.
<!--Definir isso aqui-->
```python
import time

def fetch_allpagelimit(base_url,params,headers,page_param="page",limit_param="limit"):
    page = 1
    all_items = []
    while True:
        params.update({page_param: page, limit_param: 500})
        resp = requests.get(base_url, params=params, headers=headers, timeout=30)
        if resp.status_code == 429:
            retry = int(resp.headers.get("Retry-After", 5))
            time.sleep(retry)
            continue
        resp.raise_for_status()
        payload = resp.json()
        items = payload.get("items") or payload  # adapta conforme API
        if not items:
            break
        all_items.extend(items)
        # condição de parada: quando menos do que o limite ou campo next absent
        if len(items) < params[limit_param]:
            break
        page += 1
        time.sleep(0.1)  # comportamento educado: evita bursts
    return pd.DataFrame(all_items)

df = fetch_all_page_limit(url, params={"ano":2023}, headers=headers)

```

#### Lendo um endpoint que já retorna um CSV
```python
from io import StringIO

csv_url = "https://api.exemplo.gov.br/v1/export?formato=csv&ano=2023"
resp = requests.get(csv_url, headers=headers, timeout=60)
resp.raise_for_status()
df_csv = pd.read_csv(StringIO(resp.text), sep=",") 
print(df_csv.head())

```


[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados
[Paginação]: https://thiagolima.blog.br/parte-5-pagina%C3%A7%C3%A3o-ordena%C3%A7%C3%A3o-e-filtros-em-apis-restful-3045d88b4114