#### Contagem de valores ausentes



Uma boa maneira de começar a verificar se há valores ausentes é chamar o método **info()** no DataFrame



###### Contagem de valores ausentes com **value\_counts()**



Em vez somar os valores retornados por **isna()**, podemos contar os valores ausentes diretamente usando o método v**alue\_counts()**



Esse método tem um parâmetro chamado **dropna=**, definido como True por padrão. Isso significa que **value\_counts()** exclui os valores None ou NaN a não ser que você configure **dropna=False**



ex:

**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv')**

**print(df\_logs\['source'].value\_counts(dropna=False))**



Como você pode ver, quando imprimimos **df\_logs\['source'].value\_counts(dropna=False)**, o resultado é armazenado em ordem decrescente com base na contagem de cada valor. Como alternativa, podemos ordenar o resultado em ordem alfabética com base nos nomes dos valores. Para fazer isso, podemos usar o método **sort\_index()**



ex:

**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv')**

**print(df\_logs\['source'].value\_counts(dropna=False).sort\_index())**





Aplique o método **value\_counts()** à coluna 'email' e armazene o resultado na variável email\_values. Dessa vez, não inclua os valores ausentes no resultado



**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv')**



**email\_values = df\_logs\['email'].value\_counts(dropna=True)**



**print(email\_values)**





###### Reescreva a variável email\_values usando a ordenação



**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv')**



**email\_values = df\_logs\['email'].value\_counts()**

**email\_values = email\_values.sort\_index()**



**print(email\_values)**



**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Filtragem de DataFrames para lidar com valores ausentes



Há várias maneiras de filtrar um DataFrame para extrair linhas com esses valores. Uma delas é usar o método **isna()**, que produz um objeto Series para cada linha. Nesse objeto Series, True indica um valor ausente, e False indica um valor não ausente

se quisermos extrair linhas em que a coluna 'source' tem valores ausentes, podemos fazer isso usando o seguinte código:

ex:

**print(df\_logs\[df\_logs\['source'].isna()])**



Como alternativa, podemos filtrar o DataFrame e extrair apenas as linhas em que 'source' não está ausente. A abordagem que usamos anteriormente funciona com apenas uma pequena modificação:

ex:

**print(df\_logs\[~df\_logs\['source'].isna()])**



para criar um DataFrame que não tem valores ausentes na coluna 'email' e contém apenas o valor 'email' na coluna 'source', podemos usar o seguinte código:

ex:

**print(df\_logs\[(~df\_logs\['email'].isna()) \& (df\_logs\['source'] == 'email')])**



###### exercício:

Filtre o DataFrame df\_logs para que ele contenha apenas linhas onde não existam valores ausentes na coluna 'email'. Atribua os resultados filtrados a uma variável chamada df\_emails, depois imprima as primeiras 10 linhas.

**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv')**



**df\_emails = df\_logs\[(~df\_logs\['email'].isna())]**

**print(df\_emails.head(10))**



Verifique se há alguma linha em que ambas as colunas 'source' e 'email' têm valores ausentes



**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv')**



**df\_emails = df\_logs\[(df\_logs\['email'].isna()) \& (df\_logs\['source'] == 'email')]**

**print(df\_emails)**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

##### 

##### Preenchimento de valores categóricos ausentes



###### Variáveis quantitativas e categóricas



As variáveis quantitativas têm valores numéricos que podemos usar para cálculos aritméticos

Já as variáveis categóricas representam um conjunto de valores possíveis que uma observação específica pode ter. Coisas como cor, marca e modelo de um carro são exemplos de variáveis categóricas.



Use o método **fillna()** para substituir os valores ausentes em 'email' por strings vazias



**import pandas as pd**

**df\_logs = pd.read\_csv('/datasets/visit\_log.csv')**



**df\_logs\['email'] = df\_logs\['email'].fillna(value='')**

**print(df\_logs.head())**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

##### 

##### O parâmetro keep\_default\_na=



podemos fazer com que read\_csv() leia valores ausentes como strings vazias em vez de NaN ao definir o parâmetro keep\_default\_na= como False

ex:

**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv', keep\_default\_na=False)**



**print(df\_logs.head())**

**print()**

**df\_logs.info()**



Esteja ciente de que definir keep\_default\_na=False converte todos os valores ausentes em strings vazias, mesmo nas colunas numéricas



Leia os dados usando keep\_default\_na=False e imprima todos os valores unívocos na coluna 'source'



**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv', keep\_default\_na=False)**



**df\_sources = df\_logs\['source'].unique()**

**print(df\_sources)**



Use replace() para substituir os valores ausentes na coluna 'source' pela string 'email'.

Verifique o seu trabalho chamando o método unique() na coluna 'source' e imprimindo os resultados.





**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv', keep\_default\_na=False)**



**df\_logs\['source'] = df\_logs\['source'].replace('','email')**



**print(df\_logs\['source'].unique())**



determine o número de visitas em que uma compra foi feita para cada fonte, calculando a soma da coluna 'purchase' para cada agrupamento de fonte. Depois disso, atribua os resultados à variável purchases e a imprima



**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv', keep\_default\_na=False)**

**df\_logs\['source'] = df\_logs\['source'].replace('', 'email')**



**purchases = df\_logs.groupby('source')\['purchase'].sum()**

**print(purchases)**



Calcule a taxa de conversão de cada fonte de tráfego, salve os resultados em conversion e a imprima. A taxa de conversão é a proporção de visitas em que uma compra foi feita. O pré-código tem as visitas e compras do exercício anterior.



**import pandas as pd**



**df\_logs = pd.read\_csv('/datasets/visit\_log.csv', keep\_default\_na=False)**

**df\_logs\['source'] = df\_logs\['source'].replace('', 'email')**



**visits = df\_logs.groupby('source')\['user\_id'].count()**

**purchases = df\_logs.groupby('source')\['purchase'].sum()**



**conversion = purchases / visits**

**print(conversion)**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

##### 

##### Preenchimento de valores quantitativos ausentes



Para decidir qual é o melhor valor representativo entre a média e a mediana, podemos seguir os passos abaixo:



-Determine se os dados têm valores atípicos significativos.

-Se não há valores atípicos significativos, calcule a média usando o método **mean()**.

-Se seus dados têm valores atípicos significativos, calcule a mediana usando o método **median()**.

-Substitua valores ausentes pela média ou pela mediana usando o método **fillna().**



Antes de começar a preencher valores ausentes quantitativos, sempre verifique se há valores atípicos

**Usar a média só é apropriado quando não há valores atípicos nos dados.**



###### exercícios

###### 

###### queremos comparar o tempo médio gasto no site por usuários de computadores desktop e de dispositivos móveis

###### 

ex:



**import pandas as pd**



**analytics\_data = pd.read\_csv('/datasets/web\_analytics\_data.csv')**



**age\_avg = analytics\_data\['age'].mean()**

**analytics\_data\['age'] = analytics\_data\['age'].fillna(age\_avg)**



**desktop\_data = analytics\_data\[analytics\_data\['device\_type'] == 'desktop']**

**mobile\_data = analytics\_data\[analytics\_data\['device\_type'] == 'mobile']**



**desktop\_data.info()**

**print()**

**mobile\_data.info()**

 

###### 

###### Atribua o tempo médio de visita de desktop à variável desktop\_avg e a média de usuários de dispositivos móveis à mobile\_avg

ex:



**import pandas as pd**



**analytics\_data = pd.read\_csv('/datasets/web\_analytics\_data.csv')**



**age\_avg = analytics\_data\['age'].mean()**

**analytics\_data\['age'] = analytics\_data\['age'].fillna(age\_avg)**



**desktop\_data = analytics\_data\[analytics\_data\['device\_type'] == 'desktop']**

**mobile\_data =  analytics\_data\[analytics\_data\['device\_type'] == 'mobile']**



**desktop\_avg = desktop\_data\['time'].mean()**

**mobile\_avg =  mobile\_data\['time'].mean()**



**print(f"Tempo médio de desktop: {desktop\_avg:.2f} segundos")**

**print(f"Tempo médio dos dispositivos móveis: {mobile\_avg:.2f} segundos")**





###### Use o tempo médio das visitas de desktop para preencher os valores ausentes da coluna 'time' de desktop\_data e a média de tempo de visita de dispositivos móveis para preenchê-los em mobile data

ex:



**import pandas as pd**



**analytics\_data = pd.read\_csv('/datasets/web\_analytics\_data.csv')**



**age\_avg = analytics\_data\['age'].mean()**

**analytics\_data\['age'] = analytics\_data\['age'].fillna(age\_avg)**



**desktop\_data = analytics\_data\[analytics\_data\['device\_type'] == 'desktop']**

**mobile\_data =  analytics\_data\[analytics\_data\['device\_type'] == 'mobile']**



**desktop\_avg = desktop\_data\['time'].mean()**

**mobile\_avg = mobile\_data\['time'].mean()**



**desktop\_data\['time'] = desktop\_data\['time'].fillna(desktop\_avg)**

**mobile\_data\['time'] = mobile\_data\['time'].fillna(mobile\_avg)**



**desktop\_data.info()**

**print()**

**mobile\_data.info()**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Como lidar com duplicados



No sprint de Python básico, você aprendeu sobre os métodos **duplicated()** e **drop\_duplicates()**. Nesta lição, vamos revisar esses conceitos e apresentar outras formas de tratar dados duplicados



Existem duas técnicas boas para encontrar dados duplicados:



**técnica 1**

Podemos usar o método **duplicated().sum()** para obter o número de valores duplicados em uma única coluna ou linhas duplicadas em um DataFrame.



**import pandas as pd**

**df = pd.DataFrame({'col\_1': \['A', 'B', 'A', 'A'], 'col\_2': \[1, 2, 2, 1]})**



**print('É assim que fica o conjunto de dados:')**

**print(df)**

**print('É assim que fica um objeto Séries booleano:')**

**print(df.duplicated())**

**print('É assim que fica um resultado do método duplicated() seguido por sum():')**

**print(df.duplicated().sum())**



**É assim que fica o conjunto de dados:**

&nbsp;   \*\*col\\\_1  col\\\_2\*\*


**0     A      1**

**1     B      2**

**2     A      2**

**3     A      1**



**É assim que fica um objeto Séries booleano:**

**0    False**

**1    False**

**2    False**

**3     True**

**dtype: bool**



**É assim que fica um resultado do método duplicated() seguido por sum():**

**1**



A propósito, você pode ver os duplicados usando uma filtragem simples:



**import pandas as pd**

**df = pd.DataFrame({'col\_1': \['A', 'B', 'A', 'A'], 'col\_2': \[1, 2, 2, 1]})**



**print(df\[df.duplicated()])**



resultado :

&nbsp;   \*\*col\\\_1  col\\\_2\*\*


**3     A      1**





**Técnica 2**

Chame o método **value\_counts()**. Esse método identifica todos os valores unívocos em uma coluna e calcula quantas vezes cada um aparece





**import pandas as pd**

**df = pd.DataFrame({'col\_1': \['A', 'B', 'A', 'A'], 'col\_2': \[1, 2, 2, 1]})**



**print('É assim que fica o conjunto de dados:')**

**print(df)**

**print()**

**print('Aqui está o resultado da chamada do método value\_counts() para col\_1:')**

**print(df\['col\_1'].value\_counts())**



resultado:



**É assim que fica o conjunto de dados:**

**col\_1  col\_2**

**0     A      1**

**1     B      2**

**2     A      2**

**3     A      1**



**Aqui está o resultado da chamada do método value\_counts() para col\_1:**

**A    3**

**B    1**

**Name: col\_1, dtype: int64**



##### Como lidar com duplicados



podemos lidar com linhas completamente duplicadas, como a primeira e a última no DataFrame do exemplo acima, usando o método **drop\_duplicates()**





Se você só quiser considerar duplicados em uma (ou algumas) das colunas em vez de linhas totalmente duplicadas, use o parâmetro **subset=**

ex:

**import pandas as pd**

**df = pd.DataFrame({'col\_1': \['A', 'B', 'A', 'A'], 'col\_2': \[1, 2, 2, 1]})**



**print(df)**

**print()**

**print(df.drop\_duplicates(subset='col\_1'))**



A maneira mais simples de trabalhar com entradas duplicadas  é tornar todas as letras nas strings minúsculas chamando o método **lower()**

ex:

**import pandas as pd**

**df = pd.DataFrame({'col\_1': \['A', 'B', 'A', 'A'], 'col\_2': \[1, 2, 2, 1]})**



**print('col\_1 original no conjunto de dados:')**

**print(df\['col\_1'])**

**print()**

**print('Conjunto de dados com valores em minúsculas em col\_1')**

**print(df\['col\_1'].str.lower())**



Vamos verificar o número de duplicados verdadeiros na coluna 'col\_1' após transformar os valores em minúsculas



**import pandas as pd**

**df = pd.DataFrame({'col\_1': \['A', 'B', 'A', 'A'], 'col\_2': \[1, 2, 2, 1]})**



**df\['col\_1'] = df\['col\_1'].str.lower()**

**print(df\['col\_1'].duplicated().sum())**

resultado:

**2**



se o método **replace()** para mudar todas as ocorrências de 'GB' para 'gb' na coluna 'item' dos dados sobre celulares:



**import pandas as pd**



**stock = pd.read\_csv('/datasets/phone\_stock.csv')**

**stock\['item'] = stock\['item'].str.replace('GB', 'gb')**



**print(stock.head())**



resultado:



&nbsp;                   \*\*id                     item  count\*\*


**0  100480924     Apple iPhone Xr 64gb     10**

**1  100480924     Apple iPhone Xr 64gb     19**

**2  100480959     Xiaomi Redmi 6A 16gb     44**

**3  100480975          HUAWEI P30 lite     38**

**4  100480988  Samsung Galaxy A30 32gb     49**



Se você não tem certeza sobre qual abordagem é melhor, pode manter a coluna original e criar uma coluna adicional onde as strings foram modificadas



**import pandas as pd**



**stock = pd.read\_csv('/datasets/phone\_stock.csv')**

**stock\['item\_modified'] = stock\['item'].str.replace('GB', 'gb')**



**print(stock.head())**



resultado:



&nbsp;                   \*\*id                     item  count            item\\\_modified\*\*


**0  100480924     Apple iPhone Xr 64gb     10     Apple iPhone Xr 64gb**

**1  100480924     Apple iPhone Xr 64gb     19     Apple iPhone Xr 64gb**

**2  100480959     Xiaomi Redmi 6A 16gb     44     Xiaomi Redmi 6A 16gb**

**3  100480975          HUAWEI P30 lite     38          HUAWEI P30 lite**

**4  100480988  Samsung Galaxy A30 32gb     49  Samsung Galaxy A30 32gb**





**exercícios:**

Usando sua coluna recém-criada 'item\_lowercase' e o método sum(), calcule o número total dos dois modelos de celulares:



'apple iphone xr 64gb'

'samsung galaxy a30 32gb'



**import pandas as pd**



**df\_stock = pd.read\_csv('/datasets/phone\_stock.csv')**

**df\_stock\['item\_lowercase'] = df\_stock\['item'].str.lower()**



**apple = df\_stock\[df\_stock\['item\_lowercase'] == 'apple iphone xr 64gb']\['count'].sum()**

**samsung = df\_stock\[df\_stock\['item\_lowercase'] == 'samsung galaxy a30 32gb']\['count'].sum()**



Agora exclua as linhas com celulares duplicados chamando **drop\_duplicates()** em df\_stock. Precisamos excluir linhas com base apenas na coluna item\_lowercase, então certifique-se de usar esse nome como valor para o parâmetro **subset=** também precisamos chamar o método **reset\_index()** com o parâmetro **drop=True**



**import pandas as pd**



**df\_stock = pd.read\_csv('/datasets/phone\_stock.csv')**

**df\_stock\['item\_lowercase'] = df\_stock\['item'].str.lower()**



**df\_stock = df\_stock.drop\_duplicates(subset='item\_lowercase').reset\_index(drop=True)**

**print(df\_stock.head())**





Para atualizar o valor na primeira linha (índice 0) e a coluna count' para o modelo Apple iPhone, podemos usar loc\[]. Passamos dois valores para loc\[] para especificar o índice da linha e o nome da coluna e, em seguida, usamos o sinal = para definir o valor desejado:

ex:

**df\_stock.loc\[0,'count'] = 33**



No exemplo anterior, o valor 33 foi definido, mas nós queremos definir o valor da variável apple que anteriormente calculamos e salvamos.



Esse foi um exemplo para Apple iPhone. Para Samsung, o procedimento é o mesmo, exceto que passamos diferentes valores para o atributo loc\[].



**import pandas as pd**



**df\_stock = pd.read\_csv('/datasets/phone\_stock.csv')**

**df\_stock\['item\_lowercase'] = df\_stock\['item'].str.lower()**



**apple = df\_stock\[df\_stock\['item\_lowercase'] == 'apple iphone xr 64gb']\['count'].sum()**

**samsung = df\_stock\[df\_stock\['item\_lowercase'] == 'samsung galaxy a30 32gb']\['count'].sum()**



**df\_stock = df\_stock.drop\_duplicates(subset='item\_lowercase').reset\_index(drop=True)**



**df\_stock.loc\[0, 'count'] = apple**

**df\_stock.loc\[3, 'count'] = samsung**



**print(df\_stock)**



extraindo as linhas que contêm valores ausentes



**import pandas as pd**



**df = pd.read\_csv('/datasets/products\_data.csv')**

**mis\_booleans = df\['price'].isna()**



**mis\_rows = df\[mis\_booleans]**

**print(mis\_rows)**



vamos preencher o único valor ausente que temos



**import pandas as pd**



**df = pd.read\_csv('/datasets/products\_data.csv')**



**avg\_per\_category = df.groupby('category')\['price'].mean()**



**print(avg\_per\_category)**



Extraia o preço médio da categoria que nos interessa



**import pandas as pd**



**df = pd.read\_csv('/datasets/products\_data.csv')**

**avg\_per\_category = df.groupby('category')\['price'].mean()**



**mean\_val = avg\_per\_category\[2]**



**print(mean\_val)**



Agora é hora de substituir o valor ausente por uma média na sua categoria. Em Python, você pode usar o atributo loc\[] para fazer isso



informações:

&nbsp;   \*\*category      product  price\*\*


**7  hair care  conditioner    NaN**

código:



**import pandas as pd**



**df = pd.read\_csv('/datasets/products\_data.csv')**

**avg\_per\_category = df.groupby('category')\['price'].mean()**



**mean\_val = avg\_per\_category\[2]**



**df.loc\[7, 'price'] = mean\_val**

**print(df)**

