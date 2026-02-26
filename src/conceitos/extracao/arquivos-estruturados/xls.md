# XLS ou XLSX

> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].

O XLS é um dos formatos mais comuns e antigos de planilha, pois é o padrão dos 
arquivos feitos no Excel, XLS significa 'planilha Excel'. É importante entender 
o funcionamento e os seus contrastes com o XLSX, uma versão mais nova e diferente.
Neste artigo discutiremos diversos elementos como suas [estruturas](#estruturas) e [peculiaridades](#peculiaridades).

De forma geral, os arquivos XLSX/XLS são [menores] que outras extensões de planilha,
isso ocorre pois o XLSX é um formato de arquivo comprensado (zipado) com vários 
[XMLs] dentro, diferente do CSV, cujo arquivo é somente a planilha, sem utilzar 
métodos de compressão.

## Estruturas

O XLSX/XLS é um formato de arquivo definido por uma coleção de estruturas que formam um 
conteúdo [workbook], diferente do csv, que seria uma única planilha, esta extensão
pode incluir múltiplas tabelas e planilhas de conteúdo estruturado ou semi-estruturado.

### Planinlhas e abas

- **Workbook**: Seria a estrutura dos arquivos XLSX/XLS, esse pode ser entendido como 
um documento de múltiplas páginas onde cada aba é um [worksheet], desta forma pode 
se afirmar que o workbook é o conjunto e os sheets são as páginas.

- **Sheets**: Seriam as páginas que formam um workbook, podem conter diversos tipos de informação 
como tabelas, textos, gráficos, fórmulas, etc. Cada sheet é formado por um [grid], assim
todas as abas são como matrizes.

- **Células**: A menor unidade presente numa aba XLSX, aqui é armazenada uma unidade de
qualquer tipo de informação, em um CSV seria como qualquer dado entre dois separadores. 


### Diferenças entre XLS e XLSX

O XLS é o formato original de planilha desenvolvido pela Microsoft no final dos anos 80 e 
foi então incorporado pelo Excel, a extensão é usada para todas versões do Excel pré 2007. 
Seus dados são codificados de tal maneira que podem haver incompatibilidades com softwares 
mais modernos. O XLSX é uma adaptação moderna do XLS, baseada no formato XML, com mais 
capacidade e melhor compressão de dados. 

- **XLS**: Capacidade de 65.536 linhas e 256 colunas.

- **XLSX**: Capacidade de 1.048.576 linhas e 16.384 colunas.

De modo geral, o XLSX é menos provável de corromper, também pode ter arquivos menores, 
mas caso esses benefícios não sejam necessários e você já possui o arquivo em 
XLS, não há necessidade de converter para o XLSX.


## Peculiaridades

Existem diversas formas que alteram toda a estrutura de um XLSX/XLS em contraste com um CSV, 
como seria o [VBA](#vba), que altera toda a maneira em que o coletor de dados deve interagir com 
o arquivo, ou também algumas peculiaridades inerentes ao formato que limitam o uso de 
[chunking/streaming](#chunking-e-streaming).

### Chunking e streaming

Quando se tem um arquivo muito grande para ser carregado diretamente na memória, é preciso
utilizar a técnica de chunking/streaming, onde somente "pedaços" do arquivo são carregados
assim aliviando a CPU, a memória e facilitando o trabalho a ser feito no arquivo, porém em
contraste com o [CSV](./csv.md), essa técnica é muito mais difícil de ser implementada no XLSX/XLS. 

O formato XLSX/XLS é essencialmente vários XMLs zipados (sheet1.xml, styles.xml...) e por 
isso é muito difícil aplicar técnicas de chunking e streaming, porque é preciso descompactar e 
[parsear] o XML sequencialmente, ao invés de ler linha por linha. Essa decompressão é então 
armazenada em memória e a lógica escrita é processada. Assim, é possível que haja  uma sobrecarga 
de memória.

### VBA

"Virtual Basic for Applications" basicamente é um recurso que permite ao usuário aplicar 
funções de programação básicas dentro do Excel, realizar cálculos, automatizar processos
de edição e [etc]. 

Para obter os dados presentes num arquivo que utiliza o VBA, vai muito de caso a caso, é 
necessário entender que caso queira salvar os dados do arquivo em um dataframe pandas
usando o python, a biblioteca só é capaz de ler o estado atual do arquivo, ou seja caso
os dados só se materializem em caso de interação com o VBA, o programa deve levar isso
em conta. 



[workbook]:https://learn.microsoft.com/en-us/openspecs/office_file_formats/ms-xls/24fed501-1f2e-435b-a371-ad29a57dba58#gt_343c4660-90e1-4d86-b9cc-5007075d9dfe
[worksheet]:https://learn.microsoft.com/en-us/office/open-xml/spreadsheet/overview 
[grid]: https://www.w3schools.com/excel/excel_format_grids.php
[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados
[etc]:https://www.techtudo.com.br/dicas-e-tutoriais/2022/12/o-que-e-vba-no-excel-e-como-usar-veja-exemplos-comandos-e-tutorial.ghtml
[parsear]:https://netenrich.com/fundamentals/parsing
[menores]: https://medium.com/@ArgyleStreetProgramming/why-storing-data-in-excel-is-better-than-in-csv-4dcf6a8d2819
[XMLs]: https://www.freecodecamp.org/portuguese/news/o-que-e-um-arquivo-xml-como-abrir-arquivos-xml-e-quais-sao-os-melhores-editores-de-xml/