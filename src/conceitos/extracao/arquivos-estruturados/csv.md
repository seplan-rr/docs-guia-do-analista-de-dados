# CSV

> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].

O CSV (*Comma-Separated Values*) é um dos formatos mais simples para representar
dados estruturados, por isso é uma extensão de arquivo comum para planilhas.
É importante entender a extensão de forma compreensiva, pois ela é amplamente
utilizada no contexto de dados. Discutiremos então elementos como sua
[estrutura](#estrutura) e suas [peculiaridades](#peculiaridades).

## Estrutura

Um arquivo CSV qualquer geralmente tem esse formato:

```plaintext
Nome,Idade,CorPreferida
João,20,Verde
Maria,21,Amarelo
```

Nesse exemplo, podemos observar alguns elementos fundamentais:

- **Cabeçalho**: `Nome`, `Idade` e `CorPreferida` são o cabeçalho (*header*) desse
arquivo CSV. É importante notar que nem sempre o cabeçalho vai estar presente,
podendo estar em um arquivo separado chamado **Dicionário de Dados**;
- **Valores**: os valores são aqueles separados por um separador, nesse caso a
vírgula, como o próprio nome CSV indica. Eles podem ou não estar encapsulados,
geralmente por aspas ('simples' ou "duplas");

Assim, cada linha em um arquivo CSV trata de um conjunto de valores separados
por um separador.

### Separadores

Separadores ou Delimitadores são caracteres ou padrões usados no arquivo CSV
para separar os valores de uma linha. O separador padrão é a `,` (vírgula),
mas outros comumente encontrados são a `|` (barra vertical) e o `;` (ponto
e vírgula).

> Mas o que acontece quando um valor contém o separador?

Se você tem uma coluna `Descrição`, por exemplo, com um valor de `mesmo assim,
seguimos`, você não pode simplesmente usá-lo no arquivo CSV com o separador
`,`, senão a linha vai resultar em um erro na hora da leitura. Para evitar isso,
temos duas opções:

- usar um separador diferente e incomum para separar os valores, como o `|`;
- encapsular os valores que contém o separador em aspas simples ou duplas;

O mais recomendado é encapsular o valor em aspas, caso necessário, porque mesmo
separadores incomuns ainda podem resultar em erros.

### Codificação (*Encoding*)

O *Encoding* trata da representação em *bytes* dos caracteres. Isso influencia
como o arquivo deve ser lido pelas aplicações. Para maiores informações,
recomendamos a leitura [desse artigo][absolute-minimum-unicode-character-sets].
O *encoding* mais comum para arquivos CSV é o `UTF-8`, que suporta caracteres da
gigantesca maioria das línguas e símbolos (como emojis).

É importante sempre estar ciente do *encoding* utilizado pelo arquivo que você
queira manipular, pois utilizar o errado é como tentar destrancar uma porta com
a chave de outra. O pior mesmo é quando achamos que o *encoding* é o correto até
descobrirmos uma série de ??? ou ���.

Para checar o *encoding* de um arquivo podemos usar o seguinte comando no
Windows, Linux ou MacOS:

```bash
file ARQUIVO
```

A saída esperada terá esse formato:

```bash
ARQUIVO: encoding
```

## Peculiaridades

Como arquivos CSV são muito flexíveis em suas estruturas, há muitas formas
diferentes de representá-los. Algumas dessas formas são inerentes a programas
específicos, como é o caso do [*Byte Order Mark* (BOM)](#byte-order-mark-bom). Em
geral, é recomendado sempre tentar adotar normas como a [RFC 4180][rfc-4180]. 

### Byte order mark (BOM)

Em arquivos CSV gerados pelo Microsoft Excel, é comum o uso de uma pequena
sequência de *bytes* no início do arquivo, indicando o seu *encoding*. O
nome dessa sequência é BOM (*Byte Order Mark*). Se você utiliza o Excel para
manipular arquivos CSV e pretende compartilhar eles com alguém, faça um favor a
todos nós e opte por não usar o BOM ao salvar o arquivo.

Caso você queira saber se um arquivo CSV contém BOM, o comando mostrado na seção
de [Codificação](#codificação-encoding) indica a sua presença por meio do texto
`(with BOM)`.

### Arquivos `.csv.zip`

Arquivos CSV comprimidos são um indicador de que ele será um arquivo volumoso
(1+ GB). São necessárias técnicas específicas para lidar com esse tipo de
arquivo, como *streaming*, principalmente porque a memória do seu computador
é limitada. Em ambientes Linux, recomendamos usar as técnicas [descritas
aqui][filtragem-de-arquivos-csv-compactados].

É possível também utilizar uma abordagem mais universal por meio da biblioteca
padrão do Python, como mostrado abaixo:

```python
import io
import pandas as pd
from zipfile import ZipFile

zip_path = 'NOME_DO_ARQUIVO.csv.zip'
member_name = None

with ZipFile(zip_path) as z:
    member_name = z.namelist()[0]
    with z.open(member_name, 'r') as fbytes:
        # Alguns detalhes sobre o TextIOWrapper:
        # - Utilize encoding='utf-8-sig' caso o arquivo CSV contenha um BOM
        # - Utilize errors='replace' se você não se importa com dados
        # potencialmente corrompidos
        text_stream = io.TextIOWrapper(fbytes, encoding='utf-8', errors='replace')

        # Abaixo, temos a leitura do arquivo em partes (chunks), por meio do
        # parâmetro chunksize. Esse parâmetro determina quantas linhas são lidas
        # de cada vez, e deve ser ajustado considerando a memória disponível e o
        # volume esperado de dados por linha. Um bom indicador do volume de
        # dados de uma linha é o número de colunas do CSV. Via de regra, quanto
        # mais colunas, menor deve ser o chunksize para ambientes de baixa
        # memória.
        chunks = pd.read_csv(
            text_stream,
            sep=",",         # Indica qual separador o CSV utiliza
            chunksize=1000,  # Número de linhas lidas por iteração
        )

        for i, chunk in enumerate(chunks):
            # Realize as manipulações dos dados aqui
```

[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados
[rfc-4180]: https://datatracker.ietf.org/doc/html/rfc4180
[absolute-minimum-unicode-character-sets]: https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/
[filtragem-de-arquivos-csv-compactados]: https://www.linkedin.com/pulse/como-reduzi-em-mais-de-200x-o-consumo-ram-da-extra%25C3%25A7%25C3%25A3o-henrique-fjoef/
