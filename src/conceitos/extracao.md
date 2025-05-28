# Extração

> Esta página ainda está sendo construída. Se quiser contribuir com o guia, [acesse o nosso repositório](https://github.com/seplan-rr/guia-do-analista-de-dados).

A etapa de extração de dados envolve buscar os dados brutos diretamente de suas fontes, podendo agregá-los em uma área intermediária para a etapa de transformação. Ela pode ser realizada de diversas formas diferentes, mas nós lidamos primariamente com *[web scraping]()* e [arquivos estruturados]() compartilhados dentro da organização.

Note que, quando falamos de área intermediária, se refere a qualquer lugar onde o dado é armazenado temporariamente para que possa ser transformado. Alguns exemplos são: em memória, banco de dados (local ou remoto) e sistema de arquivos.

A extração pode ser categorizada como:

- [Notificação](#notificação)
- [Gradual](#gradual)
- [Completa](#completa)

## Notificação

A extração visando a notificação é a mais simples forma de extração de dados. Ela apenas verifica se houve uma atualização dos dados brutos na fonte e envia uma alerta para um sistema de notificações. Também é possível configurar algumas fontes para que elas enviem essa notificação de forma automática.

Esse tipo de extração é interessante por consumir bem menos recursos, já que geralmente não é necessário interagir com a área intermediária nela.

**Exemplos**

- Conexão via *[webhook](https://rockcontent.com/br/blog/o-que-e-um-webhook/)* à fonte para receber alertas automaticamente
- Assinatura em uma *[newsletter](https://en.wikipedia.org/wiki/Newsletter)* que anuncie quando houverem atualizações dos dados
- Leitura periódica à fonte de dados para verificar se houveram alterações (geralmente via *[crawler](https://www.cloudflare.com/pt-br/learning/bots/what-is-a-web-crawler/)*)

## Gradual

A extração gradual é aquela que visa obter apenas os dados que sofreram alterações em um determinado período. Fica subentendido que foi executada uma [extração completa](#completa) para construir a base de dados em um primeiro momento, e as extrações graduais apenas vão atualizando os dados conforme necessário.

Essa forma de extrair dados tem que interagir com a área intermediária, acrescentando os dados coletados nela. Ela é mais eficiente do que executar [extrações completas](#completa) quando houver atualização de dados, não precisando sobrescrever dados já coletados que não sofreram alterações.

**Exemplos**

- Consulta de novos relatórios do [SICONFI](https://siconfi.tesouro.gov.br/siconfi/index.jsf) a cada bimestre/quadrimestre

## Completa

A extração completa coleta todos os dados, independente de terem sido alterados ou não, a fim de carregá-los na área intermediária. Essa operação costuma ser mais custosa conforme o volume de dados aumenta.

**Exemplos**

- Coleta do arquivo CSV das emendas parlamentares individuais e de bancada do [Tesouro Nacional](https://www.tesourotransparente.gov.br/ckan/dataset/emendas-parlamentares-individuais-e-de-bancada), onde os dados são acrescentados ao final do arquivo periodicamente
