# JSON
> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].

<!--https://aws.amazon.com/pt/documentdb/what-is-json/-->
JavaScript Object Notation, o JSON, é um formato de armazenamento e transporte de dados.
JSON é legível para humanos e rápido de processar para máquinas, este é independente de 
qualquer linguagem de programação e é um output comum para APIs e diversas outras aplicações.

Na prática, o JSON serve principalmente para transportar dados de um sistema ao outro, por
exemplo um servidor enviando dados para um site, um aplicativo consultando uma API, o JSON
costuma ser o formato escolhido. JSON é leve, não exige processamento demasiado e não tem
regras tão rigídas como o [XML](.xml.md). Para o analista de dados, é relevante o estudo do
JSON pois grande parte das fontes modernas de dados não utiliza a forma tabular clássica. 
APIs REST, serviços governamentais, plataformas digitais, dentre outros, todos usualmente
entregam seus dados em JSON.

Exploraremos todas as parculiaridades do JSON que possam ser relevantes ao dia a dia do
analista, em particular sua [estrutura](#estrutura).

## Estrutura
<!--https://www.codecademy.com/article/what-is-json-->
O JSON é oriundo da linguagem Javascript (JS), ou seja sua aparência é similar a objetos JS. 
Abaixo observamos um exemplo de objeto em JSON:

```JSON
{
    "estudante":{
        "nome": "Raissa Medeiros",
        "idade": 30,
        "integral": true,
        "languages": [ "JavaScript", "HTML", "CSS" ],
        "media": 6.9,
        "materiaPredileta": null
    }
}
```
<!--https://medium.com/swlh/json-structure-and-schema-986f6ff64b94-->
Analisando o código, podemos destrinchar as regras estruturais do JSON como:

- Objetos são contidos dentro de um par de chaves `{}`.

- Colchetes `[]` contém Arrays. 

- Dados são armazenados numa estrutura par de *[key:value]* (chave e valor/atributo).

- Todo par *key:value* é separado de um por outro por uma vírgula `,` tal qual se separa
um array. Diferentemente do Javascript, vírgulas finais, aquelas que ficam ao final de
uma lista, são proibidas.

- Nomes de propriedades devem ser indicados entre aspas duplas `""`.

No exemplo que observamos constam todos os *Data type* compativeís com o JSON, esses
sendo: `string`, `numero`, `objeto`, `array`, `boolean`, `null`.

<!-- quase tudo das subceções abaixo foram retiradas de https://realpython.com/python-json/-->
### Erros de sintaxe comuns no JSON
O padrão do JSON não permite diversos elementos que podem estar presentes por exemplo no 
JavaScript, linguagem que originou o formato. Portanto é normal que desenvolvedores que
estejam familiarizados somente com o JS cometam erros sem perceber, no código a seguir 
destacamos alguns erros recorrentes que podem aparecer em JSON:

```JSON
   {
       "nome": 'Frida',
       "endereco": {
       "trabalho": null, ///não paga aluguel também
       "casa": "cruviana",
        },
    "amigos": [
         {
            "nome": "Coutinho",
            "hobbies": ["futebol", "leitura", "filmes",]
         }
     ]
  }
```
- 'Frida' está contido em aspas simples, não permitido no JSON, sempre
deve ser usado as aspas duplas `""`.

- Observe presença de um comentário, comentários não são permitidos pelo JSON.

- Presente no documento temos uma vírgula final, ou seja que existe depois do último 
elemento de uma lista, proibido no JSON. O mesmo problema ocorre novamente no código, 
onde a vírgula final existe no final de um array.

Existem websites voltados unicamente para a validação sintática e na depuração do 
JSON enquanto o dev escreve, o [JsonLint] por exemplo faz isso, garante que o modelo
esteja formalmente correto. É preciso destacar porém, que estes sites corrigem apenas
erros avulsos de sintaxe por exemplo, e não a "lógica" do modelo, para isso existem
os Schemas.

### Schema JSON
<!--https://json-schema.org/overview/what-is-jsonschema-->
JSON Schema é um padrão para definir a estrutura e regras de dados JSON. A validação 
JSON checa se o arquivo está conforme o esperado pelo SCHEMA, assim mantendo o documento
organizado de maneira previsível. Por isso, entenda o schema como um guia estrutural para 
o JSON. 

JSON Schema usa um documento JSON separado que provêm a planta para os dados JSON, o que
significa que o schema por si só é legível por máquinas e humanos, abaixo um exemplo de 
JSON Schema:

```JSON
{

    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "title": "Example user object",
    "description": "This is an example",
    "type": "object",
    "properties": {
        "id": {
            "type": "string",
            "description": "This is the ID of the user",
            "format": "UUID"
        },
        "name": {
            "type": "string",
            "description": "This is the full name of the user",
        },
        "age": {
            "type": "number",
            "description": "This is the age of the user. We only allow adult users.",
            "minimum": 18,
        },
       "address": {
            "type": "object",
            "properties": {
                "streetAddress": { "type": "string" },
                "city": { "type": "string" },
                "state": { "type": "string" },
                "zipcode": { "type": "string" },
                "country": { "type": "string" }
            },
            "required": ["streetAddress", "city", "state", "zipcode", "country"]
            },
        "interests": {
            "type": "array",
            "items": {
                "type": "string",
                "enum": ["sports", "music", "movies", "books"]
            }
        },
        "createdAt": {
            "type": "string",
            "format": "date-time"
        },
    },
    "required": ["id", "name", "address"]
}
```

Como podemos observar, o schema acima fornece muito contexto ao nosso JSON. Definindo
claramente o *type* e limites de cada campo, fornecendo descrições onde necessário. Por
exemplo, se possuissemos dados contendo um user de 16 anos, seria invalidado pois o 
schema especifica que users devem ter pelo menos 18 anos. 

Existem diversas situações para usar de um JSON Schema, como os *adresses*, *blog posts*, 
calendário, entre muitos outros. O [link] mostra mais exemplos para se aprofundar.


### Caso de uso: JSON em API com python

<!--https://stackoverflow.blog/2022/06/02/a-beginners-guide-to-json-the-data-format-for-the-internet/-->
Um dos usos mais comums do JSON é na utilização de [APIs](/src/conceitos/extracao/api/api.md), tanto em *requests* quanto em 
respostas. O formato é muito mais compactos que outros padrões e permite fácil consumo,
por exemplo, no python tipicamente se *parse* uma string JSON para tipos nativos usando
o `json.loads()`, assim obtendo uma lista pronta para o uso. 

Dependendo da estrutura da resposta da API, o analista pode realizar algo simples como: 
`data = json.loads(response.text)` assim obtendo um `dict` python que poderá ser iterado, 
indexado e transformado. A operação oposta também é possível, que seria transformar um objeto
python em uma JSON string para realizar uma chamada, utilizando `json.dumps(obj)`. Caso
o analista deseje o payload ele codificará: `requests.post(url, json=my_dict)`.









[JsonLint]: https://jsonlint.com/
[link]: https://json-schema.org/learn/json-schema-examples
[key:value]: https://hazelcast.com/foundations/data-and-middleware-technologies/key-value-store/
[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados