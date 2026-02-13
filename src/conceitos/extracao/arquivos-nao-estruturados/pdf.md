# PDF

> Esta página ainda está sendo construída. Se quiser contribuir com o guia,
> acesse o nosso [repositório].

Portable Document Format, o PDF, é um formato de visualização de dados pensado para a 
apresentação visual do leitor e não para a extração de dados. Ele descreve *como* algo deve 
ser desenhado na página (linhas, caixas, textos), e não *o que* aquele conteúdo representa 
semanticamente. Diferente do [JSON](../arquivos-estruturados/json.md), [CSV](../arquivos-estruturados/csv.md) ou [XML](../arquivos-estruturados/xml.md), o PDF não tem noção nativa de "tabela, 
"coluna" e etc. Para o analista isso é um problema, pois consumir o PDF se torna um exercício 
em extração e construção dos dados. Em outras palavras: é difícil.



O formato PDF é um dos mais comuns e mais utilizados no dia a dia, é utilizado para
compartilhar documentos em um formato paginado, voltado para a visualização humana, desta 
forma, pense no PDF como o papel eletrônico. Neste documento exploraremos a [estrutura](#estrutura-do-pdf) do formato e formas de [trabalhar](#pdf-na-análise-de-dados) como analista de dados utilizando o PDF. 

## Estrutura do PDF
<!--https://medium.com/@jberkenbilt/the-structure-of-a-pdf-file-6f08114a58f6-->
Fundamentalmente um arquivo PDF é uma coleção de objetos indexados, objeto no PDF que
seria qualquer *chunk* de dados estruturados. O arquivo consiste de um *header*, definições de 
objetos, uma tabela de referências (TR) e um *trailer*. O trailer é uma seção, como uma caixa 
ou carga, que funciona como um ponto de entrada da estrutura lógica do documento, a partir dele
que o leitor de pdf consegue interpretar e encontrar os objetos que compõem o arquivo.

Vamos analisar o arquivo PDF abaixo e entender sua composição.

```ASCII
%PDF-2.0
1 0 obj
<<
  /Pages 2 0 R
  /Type /Catalog
>>
endobj
2 0 obj
<<
  /Count 1
  /Kids [
    3 0 R
  ]
  /Type /Pages
>>
endobj
3 0 obj
<<
  /Contents 4 0 R
  /MediaBox [ 0 0 612 792 ]
  /Parent 2 0 R
  /Resources <<
    /Font << /F1 5 0 R >>
  >>
  /Type /Page
>>
endobj
4 0 obj
<<
  /Length 44
>>
stream
BT
  /F1 24 Tf
  72 720 Td
  (Batata) Tj
ET
endstream
endobj
5 0 obj
<<
  /BaseFont /Helvetica
  /Encoding /WinAnsiEncoding
  /Subtype /Type1
  /Type /Font
>>
endobj

xref
0 6
0000000000 65535 f 
0000000009 00000 n 
0000000062 00000 n 
0000000133 00000 n 
0000000277 00000 n 
0000000372 00000 n 
trailer <<
  /Root 1 0 R
  /Size 6
  /ID [<42841c13bbf709d79a200fa1691836f8><b1d8b5838eeafe16125317aa78e666aa>]
>>
startxref
478
%%EOF
```

- O final do arquivo mostra que a tabela de referência começa no *byte offset* (BO) 478,
logo após a palavra-chave **startxref**.

- O trailer informa através de `/Root` que o objeto ponto de entrada lógico é 1, 
indicado por `1 0 R`.

- A tabela de referências demonstra que o objeto 1 começa no *byte offset* 9. 
(A tabela começa após o `xref`.)

- O catálogo de objetos diz que a raíz da árvore de páginas está no objeto 2, de
acordo com a tabela de referências o objeto 2 está no *byte offset* 62.

- Revelado estar no BO 133 de acordo com a TR, o objeto 3 contém a carga `/MediaBox` e
indica que o conteúdo se encontra no objeto 4.

- Objeto 4, que está no BO 277, inclui os comandos para colocar a palavra "Batata" em uma
localização específica da página. 

Existe muito mais a ser analisado no código, mas esse é o básico essencial. 

Como podemos observar: é muito código. Diversas regras, referenciamentos e objetos que 
comandam um arquivo PDF, a ponto que uma simples página contendo a palavra "Batata"
representa muita análise e hostilidade para analistas não familiarizados com seu
funcionamento. 

Caso o analista espere ser capaz de abrir um arquivo PDF em um editor de texto de livre 
escolha e acompanhar sua estrutura como no exemplo acima, decepção o aguarda. Isso 
pois a maioria dos arquivos PDF contém dados e comprimidos, além de dados inerentemente
em binário como imagens. No mínimo, o conteúdo de objetos [stream] são majoritariamente
comprimidos. Comumente, o dicionário de stream deve conter um `/Filter` para palavra-chave
que descreve qual algoritmo(s) está sendo aplicado. Em casos assim, a forma mais simples
de inspecionar o arquivo é através de uma ferramenta voltada para descomprimir streams e
mostrar os dados de forma "crua", ferramentas como o [qpdf] são úteis para isso. (Mais
uma camada de complicação para o analista.)

<!--último parágrafo-->
PDFs representam um desafio para o analista de dados, pois são muito comuns e difundidos 
e ao mesmo tempo, são problemáticos como uma fonte de dados. Relatórios governamentais, 
laudos, balanços financeiros, anuários estátisticos e artigos acadêmicos costumam ser 
publicados em PDF porque o foco é a leitura humana, padronização visual e impressão. Para 
análise, porém, o dado está "fixo" numa representação gráfica, o que dificulta muito a 
extração.

A seguir exploramos como então podemos extrair dados de um PDF.

## PDF na análise de dados
<!--https://medium.com/@bojjasharanya/automating-pdf-data-extraction-your-ultimate-guide-for-choosing-the-suitable-library-d87a3dcf27e5-->
Diversos são os fatores que complicam a extração de dados de um arquivo PDF, porém
existem meios que podem facilitar a vida do analista quando lidar com o formato.
Destacamos o uso de bibliotecas criadas justamente para aplicação em PDFs. A ferramenta
correta pode simplificar o processo de extração, melhorar a precisão e poupar tempo.

### Importância da biblioteca

Primeiramente, o analista deve ter conhecimento de quais desafios o PDF que deseja
extrair irá apresentar, pois diferentes bibliotecas lidam diferentemente com certos
aspectos do PDF, ou seja, o primeiro passo é reconher qual biblioteca é a correta
para a situação. 

A seguir, listamos algumas das mais populares e úteis bibliotecas para extração de 
dados PDF:

- **PyMuPDF (fitz)**: União do Python com [MuPDF], um leve visualizador de PDF. Oferece 
extração de texto eficiente, extração de imagem e processamento de dados por página. 
Eficiente para atividades "simples" e feitas com rapidez, como extração de texto.

- **PyPDF2**: Uma biblioteca python que pode dividir, cortar e transformar páginas 
PDF. Eficiente para extração de texto e manipulação de múltiplos PDF, pois é simples
de realizar `merges` entre PDFs.

- **PDFMiner**: Ferramenta voltada a extração de dados em texto no PDF, suporta 
personalização robusta. Bilblioteca relativamente complexa, mas oferece controle preciso
sobre o processo de extração, excelente para mais controle e obtenção de dados.

O analista, essencialmente, deve experimentar e testar diferentes bibliotecas para as
diferentes situações em que deva extrair dados de um PDF, priorizando fatores como
velocidade, precisão e facilidade de uso. Não deve ter medo ou receio de consultar
[guias] e [fóruns] para tirar dúvidas.





[fóruns]: https://dev.to/mhamzap10/5-best-python-pdf-libraries-every-net-developer-should-know-25b9
[guias]: https://onlyoneaman.medium.com/i-tested-7-python-pdf-extractors-so-you-dont-have-to-2025-edition-c88013922257
[MuPDF]: https://mupdf.readthedocs.io/en/1.27.1/?_gl=1*ment2z*_ga*MTUzNTMzMTIwMS4xNzcwOTk1MDYx*_ga_JZTN4VTL9M*czE3NzA5OTUwNjAkbzEkZzAkdDE3NzA5OTUwNjMkajYwJGwwJGgw
[qpdf]: https://qpdf.sourceforge.io/
[stream]: https://pikepdf.readthedocs.io/en/latest/topics/streams.html
[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados