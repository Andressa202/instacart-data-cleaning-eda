#### Índices em DataFrames e Series



###### O atributo **index**





Objetos Series e DataFrames na Pandas sempre têm índices, armazenados no atributo **index**.  Toda vez que você criar um objeto Series ou DataFrame, o atributo index correspondente será criado automaticamente com valores padrão se você não especificar os valores de índice.





Vamos criar um objeto Series para demonstrar:



**import pandas as pd**



**oceans = pd.Series(\['Pacific', 'Atlantic', 'Indian', 'Southern', 'Arctic'])**



**print(oceans)**



result:



**0     Pacific**

**1    Atlantic**

**2      Indian**

**3    Southern**

**4      Arctic**

**dtype: object**



Aqui usamos a classe **Series()** da pandas para criar um objeto Series 



Não especificamos um índice quando criamos o Series, então temos o índice padrão: números inteiros consecutivos que começam em zero.



**import pandas as pd**



**oceans = pd.Series(\['Pacific', 'Atlantic', 'Indian', 'Southern', 'Arctic'])**



**print(oceans.index)**

**print(type(oceans.index))**



result:



**RangeIndex(start=0, stop=5, step=1)**

**<class 'pandas.core.indexes.range.RangeIndex'>**



Esse índice é do tipo **RangeIndex**

Esse é o tipo padrão de índices usados por DataFrames e Series, mas ele não é o único tipo de dados de índices na pandas



O tipo **RangeIndex** tem três parâmetros: **start=, stop= e step=**.



* **start=** é o primeiro índice de um objeto Series ou um DataFrame.
* **stop=** é o último índice. Como você pode ver, ele não está incluído em um objeto Series ou DataFrame.
* **step=** é o passo dado ao mover do primeiro índice ao último. Por padrão, é 1.



Como uma opção, podemos definir índices como quisermos. Para isso, atribuímos a nossa lista de números ao atributo index de oceans:



**import pandas as pd**



**oceans = pd.Series(\['Pacific', 'Atlantic', 'Indian', 'Southern', 'Arctic'])**



**oceans.index = \[1, 2, 3, 4, 5]**



**print(oceans.index)**

**print(type(oceans.index))**



result:



**Int64Index(\[1, 2, 3, 4, 5], dtype='int64')**

**<class 'pandas.core.indexes.numeric.Int64Index'>**



Também podemos definir o atributo index usando o parâmetro index= na chamada de Series(), o que é preferível quando não queremos o índice padrão e não planejamos modificar o índice mais tarde:



**import pandas as pd**



**oceans = pd.Series(\['Pacific', 'Atlantic', 'Indian', 'Southern', 'Arctic'],**

                   **index=\[1, 2, 3, 4, 5])**



**print(oceans.index)**

**print(type(oceans.index))**



Por fim, vamos definir os valores de índice como strings e ver qual tipo de dados teremos no índice:



**import pandas as pd**



**oceans = pd.Series(\['Pacific', 'Atlantic', 'Indian', 'Southern', 'Arctic'],**

                   **index=\['A', 'B', 'C', 'D', 'E'])**



**print(oceans)**

**print()**

**print(oceans.index)**

**print(type(oceans.index))**



result:



**A     Pacific**

**B    Atlantic**

**C      Indian**

**D    Southern**

**E      Arctic**

**dtype: object**



**Index(\['A', 'B', 'C', 'D', 'E'], dtype='object')**

**<class 'pandas.core.indexes.base.Index'>**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

##### 

##### Indexação usando **loc\[]**



* Índice: é um componente de um objeto Series ou DataFrame que pode ser acessado usando o atributo index.
* Indexação: é o processo de acessar valores em um objeto Series ou DataFrame usando os respectivos índices.



Vamos começar criando um DataFrame:



**import pandas as pd**



**states  = \['Alabama', 'Alaska', 'Arizona', 'Arkansas']**

**flowers = \['Camellia', 'Forget-me-not', 'Saguaro cactus blossom', 'Apple blossom']**

**insects = \['Monarch butterfly', 'Four-spotted skimmer dragonfly', 'Two-tailed swallowtail', 'European honey bee']**

**index   = \['state 1', 'state 2', 'state 3', 'state 4']**



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**



**print(df)**



result:



            **state                  flower                          insect**

**state 1   Alabama                Camellia               Monarch butterfly**

**state 2    Alaska           Forget-me-not  Four-spotted skimmer dragonfly**

**state 3   Arizona  Saguaro cactus blossom          Two-tailed swallowtail**

**state 4  Arkansas           Apple blossom              European honey bee**



Lembre-se de que podemos acessar elementos do DataFrame com **loc\[]** passando valores de índice e nomes de colunas, como **df.loc\[index\_value, col\_name]**. Assim, veja como podemos obter o inseto do estado de Arkansas:



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**



**print(df.loc\['state 4', 'insect'])**



result:



**European honey bee**





Também podemos usar **loc\[]** para obter vários elementos como um objeto Series ou outro DataFrame.

ex:



**df.loc\[\[index\_value\_1, index\_value\_1], \[col\_name\_1, col\_name\_2]]**



Agora pratique usando **loc\[]** dessa maneira para obter apenas as colunas 'flower' e 'insect' do Alabama e do Arizona



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**



**filtered\_df = df.loc\[\['state 1','state 3'],\['flower','insect']]**

**print(filtered\_df)**



result:



                         **flower                  insect**

**state 1                Camellia       Monarch butterfly**

**state 3  Saguaro cactus blossom  Two-tailed swallowtail**





Aqui está um exemplo que retorna as flores dos três primeiros índices de um DataFrame:



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**



**print(df.loc\['state 1': 'state 3', 'flower'])**



result:



**state 1                  Camellia**

**state 2             Forget-me-not**

**state 3    Saguaro cactus blossom**

**Name: flower, dtype: object**



De forma parecida, você pode selecionar várias colunas, bem como vários índices:



**print(df.loc\['state 1': 'state 3', 'flower': 'insect'])**



result:



                        **flower                          insect**

**state 1                Camellia               Monarch butterfly**

**state 2           Forget-me-not  Four-spotted skimmer dragonfly**

**state 3  Saguaro cactus blossom          Two-tailed swallowtail**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Indexação usando **iloc\[]**



Também podemos indexar um DataFrame usando as posições numéricas de suas linhas e colunas com **iloc\[]**.



Enquanto **loc\[]** usa rótulos de índices e colunas para acessar elementos, **iloc\[]** usa números inteiros para designar as posições dos elementos que você deseja obter.



**import pandas as pd**



**states  = \['Alabama', 'Alaska', 'Arizona', 'Arkansas']**

**flowers = \['Camellia', 'Forget-me-not', 'Saguaro cactus blossom', 'Apple blossom']**

**insects = \['Monarch butterfly', 'Four-spotted skimmer dragonfly', 'Two-tailed swallowtail', 'European honey bee']**

**index   = \['state 1', 'state 2', 'state 3', 'state 4']**



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**



**print(df)**

**print()**

**print(df.iloc\[3, 2])**



result:



            **state                  flower                          insect**

**state 1   Alabama                Camellia               Monarch butterfly**

**state 2    Alaska           Forget-me-not  Four-spotted skimmer dragonfly**

**state 3   Arizona  Saguaro cactus blossom          Two-tailed swallowtail**

**state 4  Arkansas           Apple blossom              European honey bee**



**European honey bee**



Ao contrário de **loc\[], iloc\[]** usa a indexação normal de Python a partir de 0 





Assim como em **loc\[]**, podemos acessar várias linhas e/ou colunas com **iloc\[]** ao passar listas das posições ou usar fatiamento





**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**



**print(df)**

**print()**

**print(df.iloc\[\[0, 2], 1:])**



result:



            **state                  flower                          insect**

**state 1   Alabama                Camellia               Monarch butterfly**

**state 2    Alaska           Forget-me-not  Four-spotted skimmer dragonfly**

**state 3   Arizona  Saguaro cactus blossom          Two-tailed swallowtail**

**state 4  Arkansas           Apple blossom              European honey bee**



                         **flower                  insect**

**state 1                Camellia       Monarch butterfly**

**state 3  Saguaro cactus blossom  Two-tailed swallowtail**



você também pode usar indexação negativa. Aqui está um exemplo que seleciona a última coluna (que tem o índice Python de -1) e a primeira e a terceira linhas (os índices Python 0 e 2):



**print(df.iloc\[\[0, 2], -1])**



result:



**state 1         Monarch butterfly**

**state 3    Two-tailed swallowtail**

**Name: insect, dtype: object**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Alteração do índice de um DataFrame usando o método **set\_index()**



Para DataFrames, existe outra forma de definir valores de índice usando o método **set\_index()**. Este método pega uma coluna existente de um DataFrame e substitui o índice pelos valores dessa coluna:



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**

**df = df.set\_index('state') # substitui o índice**



**print(df)**

**print()**

**print(df.index)**



result:

                         **flower                          insect**

**state**                                                           

**Alabama                 Camellia               Monarch butterfly**

**Alaska             Forget-me-not  Four-spotted skimmer dragonfly**

**Arizona   Saguaro cactus blossom          Two-tailed swallowtail**

**Arkansas           Apple blossom              European honey bee**



**Index(\['Alabama', 'Alaska', 'Arizona', 'Arkansas'], dtype='object', name='state')**





O nome do índice é 'state', porque **set\_index()** automaticamente dá o nome antigo da coluna ao novo índice.



Se você não quiser que o índice tenha um nome, pode removê-lo definindo o atributo **index\_name** de um DataFrame como **None**

ex:



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**

**df = df.set\_index('state')**



**df.index.name = None**

**print(df)**

**print()**

**print(df.index)**



result:

                          **flower                          insect**

**Alabama                 Camellia               Monarch butterfly**

**Alaska             Forget-me-not  Four-spotted skimmer dragonfly**

**Arizona   Saguaro cactus blossom          Two-tailed swallowtail**

**Arkansas           Apple blossom              European honey bee**



**Index(\['Alabama', 'Alaska', 'Arizona', 'Arkansas'], dtype='object')**



#### **exercicio**

Use **loc\[]** para extrair as flores do Alabama, Alaska e Arizona e salve o resultado na variável flowers





**import pandas as pd**



**states  = \['Alabama', 'Alaska', 'Arizona', 'Arkansas']**

**flowers = \['Camellia', 'Forget-me-not', 'Saguaro cactus blossom', 'Apple blossom']**

**insects = \['Monarch butterfly', 'Four-spotted skimmer dragonfly', 'Two-tailed swallowtail', 'European honey bee']**

**index   = \['state 1', 'state 2', 'state 3', 'state 4']**



**df = pd.DataFrame({'state': states, 'flower': flowers, 'insect': insects}, index=index)**

**df = df.set\_index('state')**



**flowers = df.loc\[' Alabama':'Arizona','flower']**

**print(flowers)**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

##### 



























































