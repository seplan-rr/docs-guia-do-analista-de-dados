# REST e RESTFUL

Representational State Transfer, o REST é um estilo de arquitetura de API, se trata de um 
dos mais comuns e relevantes no dia a dia de um desenvolvedor, pois pode ser desenvolvida
por virtualmente qualquer linguagem de programação, diferente de alternativas como SOAP,
que apresentam frameworks mais restritos. API RESTFUL é o termo utilizado para qualquer 
API que siga rigorosamente o modelo de arquitetura REST.

Investigaremos eos elementos como a [estrutura](#estrutura) do REST e como uma API pode ser
RESTFUL, além de como melhor [consumir](#como-consumir-apis-rest) APIs REST.
<!-- Fonte: https://www.ibm.com/think/topics/rest-apis -->

<!-- Usar serviços do governo de exemplo -->

## Estrutura REST
A arquitetura REST é altamente difundida devido sua flexibilidade, no sentido que pode ser 
desenvolvida uma API REST utilizando diversas linguagens de programação diferentes e 
suporta uma grande variedade de formatos de dados distintos. A única requisição é que a API seja 
RESTFUL, ou seja, siga os princípios da arquitetura REST diligentemente. Os princípios do REST e
a estrutura das requisições REST API serão enunciados a seguir:

### REST e HTTP
<!--https://medium.com/@bvsahane89/mastering-rest-apis-http-methods-in-rest-653ab25140e7-->
Os métodos que APIs REST utilizam para se comunicar são os protocolos padrão do [HTTP], que
segue um modelo [cliente-servidor] com um servidor <em>[Stateless](#statelessness)</em>. O cliente realiza uma
requisição para uma [URI] específica utilizando um método HTTP, e então o servidor responde 
através uma representação do [recurso](../api.md#recursos). Estes métodos são utilizados para realizar
operações [CRUD](https://medium.com/geekculture/crud-operations-explained-2a44096e9c88) 
(Create, read, update, delete), alguns destes métodos incluem:

- **GET**: Obtém um recurso.
- **POST**: Cria um novo recurso.
- **PUT**: Atualiza um recurso existente.
- **DELETE**: Remove um recurso.
- **PATCH**: Atualiza parcialmente um recurso.
- **OPTIONS**: Descreve as opções de comunicação com o recurso desejado.
- **HEAD**: Devolve os headers de um recurso sem o body.
<!--Adicionar fonte definindo headers e body? Entendo como desnecessário-->

### Princípios da arquitetura REST

#### Interface uniforme
Todas as requisições da API para um mesmo recurso devem ser iguais, ou seja eles devem usar 
o mesmo [Endpoint](../api.md#endpoints), método HTTP e o mesmo formato de resposta, independente do cliente. 
```python
GET /users/123
```
Exemplo de requisição correta.

A API REST deve assegurar que o mesmo dado, como nome ou email pertençam ao mesmo 
URI, isso evita duplicidade lógica e organiza informações em um único recurso.
Finalmente, recursos não devem ser grandes demais mas é fundamental que incluam todas
informações que o cliente talvez precise. Desta forma o recurso fica coeso e completo.
```json
{
  "id": 123,
  "name": "Ana",
  "email": "ana@email.com",
  "status": "active",
  "createdAt": "2023-05-10"
}
```
Exemplo de saída completa após uma requisição.

#### Separação de cliente e servidor
No design de uma REST API, aplicações cliente e servidor devem ser independentes uma 
da outra. A única informação que a aplicação do cliente deve saber é o URI do recurso
requisitado, paralelarmente, o servidor não pode alterar em nada a aplicação do cliente
além de meramente repassar os dados requisitados via HTTP. 

#### <em>Statelessness </em>
<em>Stateless</em> é quando o servidor não mantém o estado da aplicação entre requisições, ou seja,
cada requisição é independente e não depende de memória de requisições anteriores, logo 
o servidor não pode assumir que uma requisição é uma continuação da anterior por exemplo. 
Devido a tal limitação, requisições devem conter toda informação necessária para ser 
processada, ou seja dentre outras informações, requisições devem informar quem é o cliente,
o que ele quer fazer e os dados necessários (body, headers, etc.)

```json
GET /orders/987
Authorization: Bearer eyJhbGciOi...
``` 
Exemplo de requisição correta, <em>stateless</em> então não importa se for a primeira ou 
milésima requisição, funciona sozinha.

#### Cache 
Quando possível, recursos devem ser armazenados em memória [cache] tanto para o servidor quanto 
para o cliente. As respostas do servidor também devem conter informação se o recurso requisitado
pode ser armazenado em cache. O objetivo seria melhorar performance para o cliente e aprimorar
escalabilidade para o servidor.

#### Arquitetura em camadas 
Em REST APIs as chamadas e respostas passam por diferentes camadas, de modo geral o 
cliente e a aplicação do servidor não se conectam diretamente. Dependendo da API, pode 
existir qualquer quantidade de intermediários no processo de comunicação.


## Como consumir APIs REST
<!--correção de concordância-->
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
### Clientes para consumir APIs Rest


### Exemplos
Qualquer demanda que exija dados retirados de planilhas governamentais,
disponíveis no [gov.br], precisará na maioria dos casos utilizar as APIs 
REST disponíveis na plataforma para obter as informações desejadas.



[gov.br]: https://www.gov.br/conecta/catalogo/
[Paginação]: https://thiagolima.blog.br/parte-5-pagina%C3%A7%C3%A3o-ordena%C3%A7%C3%A3o-e-filtros-em-apis-restful-3045d88b4114
[cliente-servidor]:https://medium.com/@brijesh.sriv.misc/the-client-server-model-backbone-of-modern-networking-318f46310a35
[URI]: https://developer.mozilla.org/en-US/docs/Web/URI
[HTTP]: https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/
[cache]: https://www.lenovo.com/br/pt/glossary/what-is-cache-memory/?orgRef=https%253A%252F%252Fwww.google.com%252F&srsltid=AfmBOoqPBNpe9tsnGF2sZuc3Gt0micERzPM4akHl_SuZhj2Ev16h_oFa