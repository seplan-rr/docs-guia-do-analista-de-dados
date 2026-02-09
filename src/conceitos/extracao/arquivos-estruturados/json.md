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
analista, em particular sua [estrutura](#estrutura) e [como consumir arquivos JSON](#como-consumir-json).

## Estrutura
<!--https://www.codecademy.com/article/what-is-json-->
O JSON é oriundo da linguagem Javascript (JS), ou seja sua aparência é similar a objetos JS. 
Abaixo observamos um exemplo de objeto em JSON:

<span id="codigo_exemplo"></span>

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
1   {
2       "nome": 'Frida',
3       "endereco": {
4       "trabalho": null, ///não paga aluguel também
5       "casa": "cruviana",
6        },
7    "amigos": [
8         {
9            "nome": "Coutinho",
10           "hobbies": ["futebol", "leitura", "filmes",]
11        }
12    ]
13 }
```
- Na linha 2 'Frida' está contido em aspas simples, não permitido no JSON, sempre
deve ser usado as aspas duplas `""`.

- Linha 4 tem a presença de um comentário, comentários não são permitidos pelo JSON.

- Presente na linha 5 temos uma vírgula final, ou seja que existe depois do último 
elemento de uma lista, proibido no JSON. O mesmo problema é presente na linha 10, onde
a vírgula final existe no final de um array.

### Escrevendo JSON com python

Python suporta o formato JSON nativamente através do módulo `json`, que foi criado
especificamente para ler e escrever strings formatadas como JSON. Portanto, é muito
conveniente converter *data types* python para dados JSON e vice-versa.

O formato JSON pode ser útil quando se quer salvar dados fora do seu programa python,
ao invés de criar toda uma database, pode ser utilizado um arquivo JSON para armazenar 
esses dados. Para escrever dados python a um arquivo JSON se utilza o `json.dump()`.
Mantenha a atenção, pois diferentes *data types* python como listas e tuples são convertidas
para o mesmo tipo de JSON array, o que pode causar problemas caso se deseje transformar 
os dados de volta ao python. 

<!--Escolhi não incluir um código exemplo, para manter o foco teórico, sem transformar
essa seção em um how-to ou guia prático-->


## Como consumir JSON

Ao consumir um JSON, é boa prática começar validando sua estrutura básica e entendendo 
qual é o elemento raíz com `json.loads`, evite assumir que esses dados já estão no formato 
esperado. Sempre trate campos opcionais e valores `null`, pois os JSONs reais raramente 
são perfeitamente consistentes. Também é recomendável evitar acessar chaves diretamente 
sem verificação, use métodos que lidem melhor com ausências como `dict.get()` e a 
normalização de estruturas aninhadas antes de qualquer análise mais profunda. 

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











[key:value]: https://hazelcast.com/foundations/data-and-middleware-technologies/key-value-store/
[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados