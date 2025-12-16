### Métodos e atributos da estrutura de dataframe

O objeto mais comum de um dataframe pandas é o dataframe object, um dado bidimensional rotulado de diferentes datatypes possiveis. Conceitualmente, entenda o pandas dataframe como uma tabela SQL, um dicionário ou uma série de objetos, quqlaquer um que seja familiar para você. É interessante os diversos métodos que facilitam o entendimento e visualização dos dados para o desenvolvedor, um desses métodos já demonstramos aqui, o ```data.head()```, que mostra as primeiras 5 linhas de um dataset.

Certos parãmetros ajudam o dev a listar informações úteis do dataframe, a seguir alguns deles: 

```data.columns``` que informa rapidamente o nome das colunas presentes no dataset

```data.info()``` apresenta um resumo com informações gerais do dataframe, como RangeIndex, número de colunas, datatype etc.  

```data.describe()``` cria descrições estatísticas do dataframe, que resumem a tendência central, disperção e formato de distribuição dos dados

com essas informaçôes é muito mais fácil filtrar e realizar escolhas bem informadas do que fazer com os dados presentes. 





### Como lidar com datasets grandes

Ao longo da procura de dados você irá se deparar com datasets gigantescos, cuja visualização em software como o Excel pode ser imprática, travada, quebrada ou todos os mencionados. Visto isso se faz necessário carregar apenas 'pedaços' (chunks) do arquivo para o tornar legível e manipulável, para isso usamos o parâmetro ```chunksize```, útil principalmente quando o dataset trabalhado excede os limites da memória de sua máquina. 

Importando grande dataset, definindo seus chunks e performando operações em cada um
```python
import pandas as pd

file_path='data/large_dataset.csv'
chunk_size = 1000 #representa a quantidade de linhas por chunk

for chunk in pd.reda_csv(file_path, chunksize=chunk_size):
    #operações feitas para cada chunk
    print(f'Processando chunk com {len(chunk) linhas}')
    print(chunk.describe())
```

Se seu desejo é fazer operações através de todo o dataset, por exemplo somar todos os valores de uma dada coluna, você pode agregar os resultados enquanto itera pelos chunks
```python
total_sum = 0

for chunk in pd.read_csv(file_path, chunksize=chunk_size):
    total_sum += chunk['column_name'].sum()

print(f'Soma total: {total_sum}')
```
