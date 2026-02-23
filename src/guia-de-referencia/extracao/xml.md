# XML

## Consumir XMLs
<!-- https://www.ibm.com/docs/en/streamsets-legacy-chcloud?topic=formats-reading-processing-xml-data -->
Existem diversas formas de consumir arquivos XML, o maior fator do método utilizado seria 
o quão bem formado é o arquivo, isso é, respeita as regras estruturais do XML, parsers padrão 
podem ser utilizados para interpretar a hierarquia e transformar os dados em estruturas 
manipuláveis. Entretanto, na prática é comum lidar com XMLs malformados ou datados, oriundos
de sistemas legado por exemplo. Nestes casos, é necessário tratar o conteúdo como texto, aplicando
delimitadores ou outros processos de limpeza.