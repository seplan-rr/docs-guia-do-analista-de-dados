# PDF

## PDF na análise de dados
<!-- https://medium.com/@bojjasharanya/automating-pdf-data-extraction-your-ultimate-guide-for-choosing-the-suitable-library-d87a3dcf27e5 -->
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

- **PDFtotext**: Utilizado para extrair texto de PDFs para serem anexados ao objeto
desejado. Muito útil para entender como o texto foi armazenado, extrair relatórios 
públicos e e tratá-los via Python, R e etc. 

**Exemplo de caso de uso**:

Digamos que o dev possua um arquivo `relatorio_2024.pdf` e deseje extrair seu texto,
mantendo a formatação e trabalhando com o resultado no terminal. Para isso o analista deve 
utilizar `pdftotext relatorio_2024.pdf`, a partir desse ponto, diversas possibilidades de
manipulação se abrem, como buscar palavras `pdftotext relatorio_2024.pdf - | grep "violência"`,
ou mesmo uma simples conversão para `relatorio_2024.txt`, entre muitas [outras].


O analista, essencialmente, deve experimentar e testar diferentes bibliotecas para as
diferentes situações em que deva extrair dados de um PDF, priorizando fatores como
velocidade, precisão e facilidade de uso. Não deve ter medo ou receio de consultar
[guias] e [fóruns] para tirar dúvidas.


[MuPDF]: https://mupdf.readthedocs.io/en/1.27.1/?_gl=1*ment2z*_ga*MTUzNTMzMTIwMS4xNzcwOTk1MDYx*_ga_JZTN4VTL9M*czE3NzA5OTUwNjAkbzEkZzAkdDE3NzA5OTUwNjMkajYwJGwwJGgw
[fóruns]: https://dev.to/mhamzap10/5-best-python-pdf-libraries-every-net-developer-should-know-25b9
[guias]: https://onlyoneaman.medium.com/i-tested-7-python-pdf-extractors-so-you-dont-have-to-2025-edition-c88013922257
[outras]: https://manpages.ubuntu.com/manpages/trusty/man1/pdftotext.1.html