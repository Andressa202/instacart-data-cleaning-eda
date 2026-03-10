#### Gráficos de linha



Lembra quando usamos o método **plot()** sem especificar um argumento para o parâmetro **kind=**? Isso gera um gráfico de linha, em que cada ponto dos dados é conectado sequencialmente por uma linha.



Parecia um pouco com isso:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**df.sort\_values('height').plot(x='height', y='weight')**

**plt.show()**





Tiramos esse conjunto de dados do 'Yahoo Finance' https://finance.yahoo.com/quote/SBUX/history/?p=SBUX



Vamos dar uma olhada nos dados:



**import pandas as pd**



**df = pd.read\_csv('/datasets/sbux.csv')**



**print(df.head())**

**print()**

**df.info()**



result:

         **date       open      close    volume**

**0  2015-01-02  41.064999  40.720001   6886000**

**1  2015-01-05  40.070000  39.939999  11623800**

**2  2015-01-06  40.169998  39.615002   7664400**

**3  2015-01-07  39.875000  40.590000   9732600**

**4  2015-01-08  41.165001  41.244999  13170600**



**<class 'pandas.core.frame.DataFrame'>**

**RangeIndex: 1258 entries, 0 to 1257**

**Data columns (total 4 columns):**

 **#   Column  Non-Null Count  Dtype**  

**---  ------  --------------  -----**  

 **0   date    1258 non-null   object** 

 **1   open    1258 non-null   float64**

 **2   close   1258 non-null   float64**

 **3   volume  1258 non-null   int64**  

**dtypes: float64(2), int64(1), object(1)**

**memory usage: 39.4+ KB**





vamos construir um gráfico de linhas para visualizar o preço de abertura das ações da Starbucks durante a vida útil do conjunto de dados:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/sbux.csv')**



**df.plot(x='date', y='open')**

**plt.show()**



Tudo o que tivemos que fazer para construir um gráfico de linhas foi chamar **plot()** no DataFrame sem o argumento **kind=** e especificar quais colunas queríamos desenhar nos eixos X e Y, assim como já fizemos com outros gráficos. Você não precisa especificar o argumento kind= como "line" porque esse já é o valor padrão.



Agora vamos usar as nossas habilidades para melhorar o gráfico 



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/sbux.csv')**



**df.plot(x='date',**

        **y='open',**

        **legend=False,**

        **title='Starbucks market open',**

        **xlabel='Date',**

        **ylabel='Share price / USD',**

        **rot=45)**



**plt.show()**





**rot=.** Esse argumento aplica uma rotação nas legendas das marcas do eixo X, em graus.Ele é usado para que as legendas não se sobreponham umas às outras, e agora elas estão presentes em um ângulo de 45% do eixo X. 

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



Nesta lição, criaremos gráficos de barras para visualizar dados do Departamento do Censo dos Estados Unidos sobre o histórico da população na Califórnia, no Oregon e em Washington. https://www.census.gov/



Vamos começar imprimindo informações gerais sobre o conjunto de dados:



**import pandas as pd**



**df = pd.read\_csv('/datasets/west\_coast\_pop.csv')**

**df.info()**



result:

**<class 'pandas.core.frame.DataFrame'>**

**RangeIndex: 10 entries, 0 to 9**

**Data columns (total 4 columns):**

 **#   Column  Non-Null Count  Dtype**  

**---  ------  --------------  -----**  

 **0   year    10 non-null     int64**  

 **1   ca\_pop  10 non-null     float64**

 **2   or\_pop  10 non-null     float64**

 **3   wa\_pop  10 non-null     float64**

**dtypes: float64(3), int64(1)**

**memory usage: 448.0 bytes**



Para visualizar esses dados em um gráfico de barras, chamamos o método **plot()** no DataFrame e especificamos **kind='bar'** como argumento. Mas precisamos especificar que queremos a coluna **'year'** no eixo **X**, caso contrário o gráfico não fará muito sentido:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/west\_coast\_pop.csv')**



**df.plot(x='year', kind='bar')**

**plt.show()**



Se você não especificar nenhuma coluna para o parâmetro y=, a pandas vai criar automaticamente uma barra para cada coluna no DataFrame que não está no eixo X; nesse caso, uma barra para a população de cada estado.



No entanto, se quisermos apresentar esses dados para outras pessoas, precisamos deixá-los mais profissionais:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/west\_coast\_pop.csv')**



**df.plot(x='year',**

        **kind='bar',**

        **title='West coast USA population growth',**

        **xlabel='Year',**

        **ylabel='Population (millions)')**



**plt.legend(\['CA', 'OR', 'WA'])**

**plt.show()**



Para melhorar o nosso gráfico, usamos uma nova função do Matplotlib chamada **legend()**. Essa função nos permite especificar manualmente as marcas das legendas passando uma lista de nomes, em vez do comportamento padrão de usar os nomes das colunas do DataFrame.



Perceba que a ordem da legenda na lista vai corresponder à ordem das colunas no DataFrame, então é importante ordenar as legendas corretamente. Além disso, **plt.legend()** precisa vir depois de chamarmos **plot()** no DataFrame.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Histogramas



Um histograma é um gráfico que mostra a frequência em que diferentes valores aparecem em uma variável no conjunto de dados.

Embora ele possa se parecer com gráficos de barras, existem algumas diferenças cruciais entre os dois:



* Gráficos de barras são usados para comparar valores de variáveis discretas, e histogramas são usados para construir distribuições de variáveis numéricas contínuas.
* A ordem das barras no gráfico de barras pode ser modificada por estilo ou objetivo de comunicação. Já a ordem das barras nos histogramas não pode ser modificada.



Em um histograma, o eixo X representa a variável e seu intervalo de valores. O eixo Y representa a frequência de ocorrência de cada valor. E é por isso que não faz sentido alterar a ordem das barras.



##### Construção de histogramas com DataFrames



Na pandas, há duas maneiras de construir histogramas:



* Método **hist()**
* Método **plot()** com o argumento **kind='hist'**

Vamos examinar o **hist()** primeiro:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**

**df.hist()**



**plt.show()**





Se hist() for chamado em um DataFrame sem nenhum argumento, ele cria um gráfico separado para cada coluna numérica



Agora vamos chamar **plot()** no DataFrame para fazer histogramas:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**df.plot(kind='hist')**



**plt.show()**



O comportamento padrão de **plot()** difere significativamente do de **hist()**.



Podemos usar **plot()** para desenhar todos os histogramas no mesmo gráfico.





##### Construção de histogramas com colunas



Podemos construir um histograma com apenas uma coluna usando **hist()** e passando o nome da coluna como argumento no parâmetro **column=**

ex:


**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**df.hist(column='height')**



**plt.show()**



Como alternativa, podemos traçar o mesmo histograma chamando **hist()** apenas na coluna **'height'**:

ex:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**df\['height'].hist()**



**plt.show()**



Por padrão, **hist()** usa 10 barras. Entretanto, para esses dados, 10 barras parecem ser insuficientes para capturar o nível de precisão que queremos. Para lidar com isso, podemos usar o parâmetro **bins=** e ajustar o número de barras usadas:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**df.hist(column='height', bins=30)**



**plt.show()**



Podemos fazer o mesmo histograma usando **plot()** e o chamando apenas na coluna **'height'**:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**df\['height'].plot(kind='hist', bins=30)**



**plt.show()**



Para analisar um histograma de apenas uma variável, recomendamos usar **plot(),** porque **hist()** não suporta todos os parâmetros de formatação que você aprendeu, como **title=, xlabel=** e **ylabel=**



##### Comparação de histogramas de subconjuntos de dados



Tudo o que precisamos fazer é filtrar os dados por sexo e chamar **plot()** na coluna **'height'**. Se fizermos isso duas vezes (uma vez para cada sexo), a pandas vai construir automaticamente os dois no mesmo gráfico, com o segundo histograma à frente do primeiro:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



\# isso imprime o gráfico do sexo masculino

**df\[df\['male'] == 1]\['height'].plot(kind='hist', bins=30)**



\# isso imprime o gráfico do sexo feminino

\# incluindo um valor alpha para podermos ver ambos os histogramas

**df\[df\['male'] == 0]\['height'].plot(kind='hist', bins=30, alpha=0.8)**



**plt.legend(\['Male', 'Female'])** # legenda, seguindo a mesma ordem usada no gráfico anterior

**plt.show()**





Se precisar, você pode usar os índices para dividir o histograma, usando a mesma lógica usada para criar o filtro da variável **'male'**. Para isso, ordene os dados da variável e redefina os índices para que os dados das mulheres sejam os primeiros no DataFrame. Em seguida, corte o DataFrame pela metade para dar uma olhada na metade inferior dos dados.



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**df = df.sort\_values('male').reset\_index(drop=True)**



**df = df\[df.index < 5000]**



**df.hist(column='height', bins=50)**



**plt.show()**



#### Recapitulação



Histogramas nos dão uma ideia de como nossos dados são distribuídos. Podemos visualizar a distribuição para ter noções cruciais dos dados, como, por exemplo, os valores mais frequentes e a presença de valores atípicos.



* Podemos usar os métodos **hist()** ou **plot()** para fazer histogramas das nossas colunas numéricas.
* Chamar **hist()** em um DataFrame cria um histograma separado para cada coluna numérica.
* Chamar **plot()** com **kind='hist'**cria um histograma para cada coluna numérica no mesmo gráfico.
* Chamar qualquer método várias vezes também cria cada novo histograma no mesmo gráfico. Isso é útil para comparar distribuições de diferentes subconjuntos de dados, como as alturas de homens e mulheres.





























































































