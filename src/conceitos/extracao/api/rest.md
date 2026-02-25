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

**Header** é talvez o componente mais importante para o analista poder compreender a API 
que está trabalhando, pois neles estão presentes muitas informações sobre o conteúdo da 
resposta e o acesso, a seguir algumas informações que se encontra nos headers de uma 
API:

- **Autenticação**: Carregam credenciais como API *Keys*, *tokens*, dentre outros. Servem
para verificar a identidade do usuário e filtrar o acesso a certos recursos.

- **Formatação de dados**: `Content-type` informa o servidor sobre o formato de dado sendo
enviado ou o tipo que o cliente aceita receber. Garante que as informações possam ser corretamente
processadas e interpretadas.

- **Caching**: Provêm diretivas sobre mecanismos de cache, ajuda a reduzir carga no servidor.

- **Encoding**: Informa quais algoritmos de criptografia são utilizados na requisição e
resposta

- **User-Agent**: Observa qual cliente utilizado na requisição, como celular, navegador e
etc.


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


### Exemplos de clientes para consumir APIs Rest

Qualquer demanda que exija dados retirados de planilhas governamentais, disponíveis no 
[gov.br], precisará na maioria dos casos utilizar as APIs REST disponíveis na plataforma para 
obter as informações desejadas. O [SICONFI] por exemplo, utiliza de APIs em suas requisições
de dados.



[SICONFI]: https://www.gov.br/conecta/catalogo/apis/siconfi-extratos-das-declaracoes-contabeis
[gov.br]: https://www.gov.br/conecta/catalogo/
[cliente-servidor]:https://medium.com/@brijesh.sriv.misc/the-client-server-model-backbone-of-modern-networking-318f46310a35
[URI]: https://developer.mozilla.org/en-US/docs/Web/URI
[HTTP]: https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/
[cache]: https://www.lenovo.com/br/pt/glossary/what-is-cache-memory/?orgRef=https%253A%252F%252Fwww.google.com%252F&srsltid=AfmBOoqPBNpe9tsnGF2sZuc3Gt0micERzPM4akHl_SuZhj2Ev16h_oFa