# CSV

> Esta página ainda está sendo construída. Se quiser contribuir com o guia, [acesse o nosso repositório](https://github.com/seplan-rr/guia-do-analista-de-dados).



O CSV é um dos formatos mais simples para representar dados estruturados, por isso é uma extensão de arquivo comum para planilhas. Porque é tão comum encontrar dados em formato de planilha CSV, é importante entender a extensão de forma compreensiva, sua estrutura, métodos de extração, melhores jeitos de consumir o arquivo etc.

CSV significa "valor separado por vírgula" que referencia o estilo de filtro aplicado para o csv estruturar os dados, por debaixo dos panos é possível observar os dados sem o embezelamento de programas como o microsoft excel e vemos a informação de forma crua, o resultado é uma bagunça de dados difícil de discernir, estaria estruturado de forma similar a seguir "Roraima,190229,2023,21398,verde..." difícil de interpretar. Com o auxílio de programas como o google sheets organizando os dados para melhor visualização, inclusive podemos escolher o filtro de dados que desejamos, chamados de separadores (, ; . etc).

#### Separadores
Tambem chamados de delimitadores, são os caracteres ou padrão regex que o csv utiliza para diferenciar um dado de outro, diferentes arquivos podem utilizar diferentes separadores dependendo dos dados presentes, a vírgula é o sep padrão mas se por exemplo o dataset armazena blocos de texto que neles contém vírgula, o separador deve ser diferente para não "quebrar" as informações da planilha, nesse caso é normal esperar que o sep mude par a ponto e vígula (;). É possível que seja utilizado um delimitador 'indicado' que é quando o dado existe dentro de por exemplo aspas e então um separador como a vírgula. Ex:

"O desmatamento está aumentando", "mesmo assim, seguimos", "VASCO",...

Dessa forma as informações armazenadas podem ser textos que possuem vírgula e mesmo assim não comprometer o entendimento do programa ao fazer a separação de um dado para o outro.

#### Encoding
De forma simples é um método de tradução dos caracteres na planilhas para os bytes que o computador entende, cada método de encoding funciona como se fosse uma chave, que permite ao computador entender os dados dentro do arquivo, se o separador serve para distinguir um dado de outro o encoding serve para o computador conseguir entender a informação em si. Por via de regra, sempre use o encoding UTF-8 se possível, é o padrão na maior parte dos arquivos pela web e o mais completo, praticamente todo caractere que você imaginar o UTF-8 vai traduzir para bytes sem problema, sejam em outras linguas, emojis, qualquer coisa. 

É importante sempre estar ciente do encoding utilizado pelo csv que você queira manipular, pois utilizar o errado é como tentar destrancar uma porta com a chave de outra, o computador não vai entender alguns caracteres e isso pode comprometer seriamente a análise do csv. 

Para checar o encoding de um arquivo uso o seguinte comando
```python
with open('file_name.csv') as f:
    print (f)
```
A saída esperada deve ser algo assim
```python
<_io.TextIOWrapper name='file_name.csv' mode='r' encoding='utf8'>
```

#### Byte order mark (BOM)
Seria uma sequência de 2 a 4 bytes no começo de um arrquivo de texto que indicam o seu enconding de Unicode, como o UTF-8, UTF-16 ou UTF-32 etc. O UTF-8 é o padrão de hoje em dia, porém é necessário estar pronto para encontrar arquivos com unicodes diferentes que possam gerar incompatibilidades ao tentar ser visualizado por softwares como Microsoft Excel, por exemplo o latin-1.

Para checar a BOM presente utilize do comando -k a seguir:
```python
file -k unicode-example-with-bom.csv
```


#### CSV.zip
Ao encontrar um arquivo CSV zipado, muito provavelmente o motivo é que ele é muito grande, normalmente você desziparia o arquivo e trabalharia normalmente com ele dentro do python, mas quando a file é grande demais, fazendo com que o mero ato de deszipar seja uma demanda grande demais na memória, outras alternativas devem ser adotadas, como o streaming e o chunksize.

Inspencionando o zip
```python
from zipfile import ZipFile

zip_path = 'big.csv.zip'
with ZipFile(zip_path) as z:
    print(z.namelist()) #lista os arquivos no zip
```

Ler só o cabeçalho e obter os nomes das colunas
```python
import io 
from zipfile import ZipFile

zip_path = 'big.csv.zip'
with ZipFile(zip_path) as z:
    member = z.namelist()[0] #exemplo escolhendo a col pelo index
    with z.open(member, 'r') as fbytes:
        text = io.TextIOWrapper(fbytes, encoding='utf-8', errors='replace')
        header_line = text.readline()
        cols = header_line.strip().split(',') #setando o separador, ajuste conforme necessário
print(cols)
```

Usando o pandas.read_csv() streaming (chunks) e pedindo colunas específicas
```python
import io 
import pandas as pd
from zipfile import ZipFile

zip_path = 'big.csv.zip'
member_name = None

with ZipFile(zip_path) as z:
    member_name = z.namelist()[0]
    with z.open(member_name, 'r') as fbytes:
        text_stream = io.TextIOWrapper(fbytes, encoding='utf-8', errors='replace')
        #leitura em chunks de colunas selecionadas
        chunks= pd.read_csv(
            text_stream,
            usecols=['colA','colB'],
            chunksize = 1000, #ajuste conforme a memória
            dtype={'colA':'int64','colB':'float32'},
            low_memory=False
        )
        for i, chunk inn enumerate(chunks):
            print('chunk',i,'lines', len(chunk))
```


 