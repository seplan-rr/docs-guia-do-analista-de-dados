# XML
> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].

A Extensible Markup Language (XML) é uma linguagem de [marcação] tal qual o [HTML] fora
criada para armazenar e transportar dados de forma estruturada e legível, tanto para as 
pessoas quanto para máquinas. Diferentemente de linguagens de programação, essas que 
executam comandos, o objetivo do XML é ordenar e descrever informações, utilizando tags 
(etiquetas) customizadas que definem o que cada dado representa. Investigaremos suas 
particularidades na [estrutura](#estrutura) e [consumo de documentos XML](#consumir-xmls).

## Estrutura
A propriedade chave que define o XML é que dados nele se "auto-definem" através de etiquetas. 
Significando que a estrutura e contexto dos dados são atrelados dentro do dado em 
si. Por exemplo ao invés de simplesmente armazenar "Daniel" como texto simples, então o 
XML armazena como `<to>Daniel<to>`, imediatamente informando o dev que "Daniel" é um 
receptor de algo.   

Essa estrutura auto-definida combate um problema crítico no desenvolvimento de software:
o compartilhamento de dados entre aplicações diferentes. Sem o uso de formatos de dados 
uniformes como XML, desenvolvedores teriam que criar códigos personalizados para cada 
aplicação e cada dataset que desejam compartilhar.

Exemplo de XML
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Daniel</to>
  <from>Jaime</from>
  <heading>Lembrete</heading>
  <body>Não esqueça de comparecer 10h amanhã.</body>
</note>
```
- declaração de *[encoding](csv.md#codificação-encoding)*, definir erroneamente quebra o *[parsing]* utilizado no XML.
- `<note>` é o elemento raíz, determina o tipo de dado sendo escrito, neste caso uma 
notação/mensagem.
- `<to>` indica o destinatário da nota.
- `<from>` define o remetente.
- `<heading>` indica o assunto ou título da nota, resume o conteúdo no corpo do texto.
- `<body>` corpo do texto, seria o conteúdo da nota

### Schemas 
<!--https://www.w3schools.com/xml/xml_schema.asp -->
Schema descreve a estrutura de um documento XML, declara quais elementos existem, quais
atributos, ordem, tipo de dado e outras restrições. Serve para validar documentos XML e 
garantir qualidade e consistência de dados. Em outras palavras, o schema é um documento
separado que estabelece as regras a serem seguidas pelo XML. 

Digamos que um XML busca definir atributos para diversas pessoa, para isso devem ser inclusos
dados como nome, genero, etc. O schema é o encarregado de definir quais dados exatamente 
são obrigatórios, por exemplo deve conter um dado `<nome>`, `<genero>` deve conter somente
masculino/feminino/outro, entre outras especificações. 

Exemplo de um Schema:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

  <xs:element name="pessoa">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="genero">
          <xs:simpleType>
            <xs:restriction base="xs:string">
              <xs:enumeration value="masculino"/>
              <xs:enumeration value="feminino"/>
              <xs:enumeration value="outro"/>
            </xs:restriction>
          </xs:simpleType>
        </xs:element>

        <xs:element name="nome" type="xs:string"/>
        <xs:element name="sobrenome" type="xs:string"/>

      </xs:sequence>
    </xs:complexType>
  </xs:element>

</xs:schema>
```

### Atributos e *Child Element*
<!--fonte: https://www.w3schools.com/xml/xml_dtd_el_vs_attr.asp 
e https://structuredauthoring.net/xml/readings/2_Elements_and_Attributes.html-->
Digamos que um dev deseja criar um elemento, para esse elemento devem ser atribuidas 
diferentes características e dados, isso pode ser feito de modos diferentes no XML, 
podendo armazenar as características em atributos ou *child elements* (CE) contidos em outros 
elementos. Observe o exemplo a seguir:

```xml
<nome>Daniel</nome>
```
Exemplo de elemento sem nenhum atributo ou *CE*. Vamos atrelar alguns dados
a este elemento, podemos fazer das seguintes formas:

```xml
<pessoa>
  <genero>masculino</genero>
  <nome>Daniel</nome>
  <sobrenome>Ismael</sobrenome>
</pessoa>
```
No XML acima `<genero>`, `<nome>` e `<sobrenome>` são *child elements* relacionados ao
elemento principal `<pessoa>`. 

```xml
<pessoa genero="masculino">
  <nome>Daniel</nome>
  <sobrenome>Ismael</sobrenome>
</pessoa>
```
Neste exemplo, `genero` é um atributo a `<pessoa>` e não um *CE*. 

Não existem regras que afirmem quando usar os atributos ou os *child element*, fica a 
critério do desenvolvedor, porém de modo geral os atributos são mais utilizados para 
metadados curtos como ID, lang e type, pois são mais convenientes para esses valores 
curtos e rápidos. Atributos possuem algumas limitações no XML que o diferem do *CE*,
por exemplo não podem conter múltiplos valores, não são facilmente expandidos, não 
descrevem precisamente a estrutura do elemento e são mais difíceis de manipular através
de código.

*Child elements* apresentam mais flexibilidade para o desenvolvedor, devido ao suporte
de conteúdo complexo como listas e objetos aninhados, permitem repetição e mapeia bem
para [JSON](json.md) pois cada elemento se torna um objeto. Todavia o *CE* acaba deixando o 
arquivo XML mais verboso e extenso, dificultando um pouco a leitura para pessoas, além
de que se usado de forma exagerada, cria um documento mais pesado.


## Consumir XMLs
<!-- https://www.ibm.com/docs/en/streamsets-legacy-chcloud?topic=formats-reading-processing-xml-data -->
Existem diversas formas de consumir arquivos XML, o maior fator do método utilizado seria 
o quão bem formado é o arquivo, isso é, respeita as regras estruturais do XML, parsers padrão 
podem ser utilizados para interpretar a hierarquia e transformar os dados em estruturas 
manipuláveis. Entretanto, na prática é comum lidar com XMLs malformados ou datados, oriundos
de sistemas legado por exemplo. Nestes casos, é necessário tratar o conteúdo como texto, aplicando
delimitadores ou outros processos de limpeza.







[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados
[parsing]: https://developer.mozilla.org/en-US/docs/Glossary/Parse
[marcação]: https://www.lenovo.com/us/en/glossary/markup-language/?orgRef=https%253A%252F%252Fwww.google.com%252F&srsltid=AfmBOoqRnFvvxE9g28ZML5iWcitHUCUX42QBg0KXCxGfy_fAB1yr50xY
[HTML]: https://developer.mozilla.org/en-US/docs/Web/HTML