# JSON

## Escrevendo JSON com python

Python suporta o formato JSON nativamente através do módulo `json`, que foi criado
especificamente para ler e escrever strings formatadas como JSON. Portanto, é muito
conveniente converter *data types* python para dados JSON e vice-versa.

O formato JSON pode ser útil quando se quer salvar dados fora do seu programa python,
ao invés de criar toda uma database, pode ser utilizado um arquivo JSON para armazenar 
esses dados. Para escrever dados python a um arquivo JSON se utilza o `json.dump()`.
Mantenha a atenção, pois diferentes *data types* python como listas e tuples são convertidas
para o mesmo tipo de JSON array, o que pode causar problemas caso se deseje transformar 
os dados de volta ao python. 

<!--Escolhi não incluir um código exemplo, para manter o foco teórico, sem transformar
essa seção em um how-to ou guia prático-->


## Como consumir JSON

Ao consumir um JSON, é boa prática começar validando sua estrutura básica e entendendo 
qual é o elemento raíz com `json.loads`, evite assumir que esses dados já estão no formato 
esperado. Sempre trate campos opcionais e valores `null`, pois os JSONs reais raramente 
são perfeitamente consistentes. Também é recomendável evitar acessar chaves diretamente 
sem verificação, use métodos que lidem melhor com ausências como `dict.get()` e a 
normalização de estruturas aninhadas antes de qualquer análise mais profunda. 