# API

> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].

O API pode ser pensado como um conjunto de protocolos que possibilitam a comunicação 
entre aplicações de software, podendo transferir dados, atributos e funcionalidades. Por isso
é essencial que o dev moderno tenha domínio sobre o uso de APIs, pois são como a espinha dorsal 
da programação contemporânea, estando em todos os lugares, quando se utiliza 
uma conta google para logar em um site, quando um aplicativo sobre climático oferece previsões, 
todos estão utilizando API.

A partir desse entendimento, evidenciaremos a sua [arquitetura](#arquitetura) e 
[documentação](#documentação)

## Arquitetura

Cada API é distinta uma da outra, como são essencialmente protocolos, cada um varia 
de acordo com seu contexto, desenvolvedor, métodos, objetivos e [etc]. Entretanto observamos 
tendências em sua construção, tipicamente são feitas para o [HTTP], pois é uma interface
amigável e comum para desenvolvedores, além de compátivel em aplicações escritas em 
Java, Ruby, Python, entre outras linguagens.

Com o tempo, foram se criando métodos, línguas, estilos e padronizações no uso das APIs.
Esses modelos fornecem aos usuários estruturas e regras definidas para a utilização
do API, os mais relevantes para o nosso trabalho seriam o [Rest](./api/rest.md) e [GraphQL](./api/graphql.md).

Para compreender melhor as APIs, devemos decompô-la a suas estruturas fundamentais:
[Endpoints](#endpoints), [recursos](#recursos) e [métodos](#métodos).

### Endpoints

Pense em endpoints como as portas para a API, cada um é uma URL especial que acessa as
diferentes partes da API, podem representar funções e recursos oferecidos pelo 
protocolo. Por exemplo uma aplicação metereológica pode conter endpoints que informam
sobre as correntes de ar, do histórico climático, previsões entre outros.

O endpoint é crucial no design da API pois define a estrutura e como os clientes vão
interagir com ela. Tipicamente, o endpoint segue um padrão consistindo em utilizar a
url da API como base, seguido por um path que identifica o recurso e ação desejado.

- **URL base**: https://api.exemplo.com
- **endpoint 1**: https://api.exemplo.com/clima/previsao
- **endpoint 2**: https://api.exemplo.com/clima/corrente-de-ar

Estes endpoints permitem que o cliente acesse diferentes aspectos sobre dados
climáticos, com cada um sendo associado a um recurso e método diferente.

### Recursos

No contexto de APIs, recursos são os dados ou objetos que a API pode prover ou 
manipular. Exemplos de recursos em API incluem perfis, produtos ou até mesmo 
conceitos abstratos como um token de autênticação. Os [endpoints](#endpoints) são os
responsáveis por definir como estes recursos são acessados ou modificados.

Considere o exemplo:
Uma API hipotética lida com [e-commerce], essa API deve oferecer recursos como
produtos, compras, usuários, carrinho de compras e etc.

Cada um destes recursos representa um conjunto de dados e funções. Os produtos por 
exemplo devem conter informações como os preços, nomes, entre outras 
características. Os endpoints da API irão permitir que clientes manipulem estes
recursos de acordo com o contexto desejado.


### Métodos
<!-- Esta classe métodos ficou vaga e superficial, difícil aprofundar sem falar diretamente 
do REST. Candidata a ser retirada inteiramente -->
Descrevem as ações que podem ser feitas com os recursos da API através dos endpoints,
pense como as coisas que um controle remoto pode fazer, o método é o que irá informar 
ao servidor o que espera ser feito com as informações presentes nas requests.

Existem diversos protocolos para os métodos de uma API, o mais relevante para nosso 
trabalho seriam as APIs [REST](./api/rest.md), que resumidamente, são APIs criadas 
no intuito de serem utilizadas em uma aplicação.

<!-- Mudada a estrutura, cortando a seção de peculiaridades e mantendo somente Documentação e Arquitetura -->
## Documentação
APIs são essenciais na organização e integração de sistemas de software na modernidade, porém
apenas ter a API não é suficiente, é igualmente importante prover instruções e guias claros
em como usá-las, para isso é necessário a documentação. 

No contexto da API documentação se torna incrivelmente importante, porque cada API
é diferente, a habilidade de um dev de manipular uma não necessariamente se transfere
para outra, por isso um manual com instruções e guias claros para cada API é de suma 
importância. Algumas das perguntas que um desenvolvedor deve se perguntar, tanto ao criar 
quanto para analizar uma API são: É necessário autênticação? nesse manual é possível testar realizando 
[chamadas](#swagger)? Quais chamadas devo fazer? quais as URLs dos endpoints? Que tipos de dados estou 
procurando? De modo geral existem três tipos acesso a documentação de uma API, privada, parceira e pública. 
Privada sendo apenas para uso interno, parceiro sendo para uso e compartilhamento apenas com seletos 
parceiros e pública sendo livre para qualquer desenvolvedor. 



### Tipos de documentação 
Como cada API é diferente, algumas possuem métodos de documentação diferentes para
atender as diversas necessidades do desenvolvedor, a seguir listamos as formas mais
relevantes de documentação de API que irá encontrar:

- **Documentação por referência**: Procede informações detalhadas sobre a API, seus
endpoints, métodos, parâmetros e formatos de resposta. Serve como um manual técnico que 
desenvolvedores podem se referir para detalhes específicos ou qualquer dúvida ao usar a API.

- **Tutoriais e guias**: Mias voltado para ações específicas, oferece instruções passo-a-passo,
tutoriais provêm exemplos práticos e usos comuns, guiam o desenvolvedor de forma mais direta.
Podem também incluir dicas para melhores práticas do uso da API.

- **Documentação conceitual**: Explica o propósito da API, conceitos chave e como ela encaixa
no sistema. Esse tipo de documentação ajuda o desenvolvedor a entender o contexto e racionalização
por trás da API, facilitando o entendimento de funcionalidades e casos de uso.

- **Logs de mudança e notas de lançamento**: Apresentam relatórios acerca de atualizações, novas
ferramentas, correções e outras modificações na API, ajuda o desenvolvedor a entender como as 
mudanças podem afetar suas aplicações.

### Swagger
<!-- as informações a seguir foram tiradas de https://swagger.io/tools/open-source/ -->
[Swagger](https://swagger.io/docs/specification/v2_0/basic-structure/) é um framework 
open-source de documentação para APIs [REST](./api/rest.md), ou seja o Swagger
facilita tanto o desenvolvimento da documentação como também o entendimento, o dev
tanto que cria a documentação quanto o que a utiliza para entender a API são beneficiados.

Essencialmente é um arquivo [YAML]/[JSON] que descreve toda a API (endpoints, métodos, etc.), 
importante ressaltar que devido a sua padronização é legível tanto por humanos quanto por 
máquinas. A interface do swagger é interativa, logo o usuário pode rapidamente navegar
pelos endpoints, ver exemplos e notavelmente executar chamadas diretamente no navegador, 
entenda chamadas como uma requisição [http] feito a um endpoint específico.

Exemplo de chamada:
```python
GET /usuarios/10
```

Desta forma o desenvolvedor se familiariza com o funcionamento da API sem ter que criar 
nenhum script além do que já existe na documentação swagger.

As ferramentas do ecossistema Swagger incluem:
- **Swagger UI**: Cria a interface interativa no navegador, permite executar chamadas.

- **Swagger Editor**: Editor para escrever e validar a especificação em YAML/JSON.

- **Swagger Codegen**: Gera código cliente/servidor a partir da especificação.




[e-commerce]: https://www.ibm.com/br-pt/think/topics/ecommerce
[JSON]: https://www.mongodb.com/resources/languages/what-is-json
[YAML]: https://www.ibm.com/think/topics/yaml
[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados
[etc]: https://aws.amazon.com/what-is/api/
[http]: https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/