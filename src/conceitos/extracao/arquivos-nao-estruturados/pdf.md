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
forma, pense no PDF como o papel eletrônico. Neste documento exploraremos a [estrutura](#estrutura-do-pdf) do formato e formas de [transformá-lo](#alterando-o-pdf) como analista de dados utilizando o PDF. 

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

## Alterando o PDF

### PDF/A

PDF/A é o formato padrão para o arquivamento de documentos eletrônicos, permitindo que
sejam visualizados em sua forma original independente do software. PDF/A é um subconjunto
de PDF que se difere na maneira como proíbe recursos inadequados para seu armazenamento
em longo-prazo. É auto-contido, ou seja pode ser visualizado e reproduzido inalterado
independete do aparelho utilizado. 

Algumas das exigências do PDF/A abaixo:

- Todas as fontes devem estar embutidas
- Não pode depender de conteúdo externo
- Não pode usar JavaScript
- Não pode conter criptografia
- Os metadados devem ser padronizados (XMP)

O PDF/A é um formato usado massivamente pelo setor acadêmico, tanto pela sua capacidade
de preservar os artigos científicos para prosperidade quanto pela sua compatibilidade com 
caracteres especiais de fórmulas matemáticas. Uma das vantagens do PDF/A seria a integração
global de seu conteúdo, pois o texto é apresentado corretamente em qualquer aparelho, além
de ser relativamente simples de ser convertido para Word, HTML e *e-Books*.

### OCR
<!--fonte: https://medium.com/@onepdf2023/what-is-ocr-and-how-can-it-improve-your-pdfs-aae17c672663-->
OCR (Optical Character Recognition) é um processo que escaneia e analisa automaticamente 
escrita a mão e converte para uma fonte digital. Entenda como um processo que transforma
imagens em texto e que pode muitas vezes ser útil ao se trabalhar com PDFs. Uma das principais
formas de uso do OCR seria para lidar com *scans* por exemplo escaneamentos de revistas, 
passaportes, cartas, assinaturas, entre outros diversos documentos. 

O primeiro passo para utilizar do OCR é selecionar uma ferramenta de transformação de PDF
com a funcionalidade embutida, o [OnePDF] é um bom exemplo, pois além do suporte ao OCR
ele é capaz de converter arquivos de PDF e para PDF sem comprometer a legibilidade. 




[OnePDF]: https://www.onepdf.online/br
[qpdf]: https://qpdf.sourceforge.io/
[stream]: https://pikepdf.readthedocs.io/en/latest/topics/streams.html
[repositório]: https://github.com/seplan-rr/guia-do-analista-de-dados