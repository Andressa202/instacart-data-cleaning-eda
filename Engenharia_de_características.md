conjunto de dados de vendas de videogames



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**print(df.head())**



#### Transformações de coluna usando operadores aritméticos



Observe que o DataFrame acima inclui vendas de três regiões: NA (América do Norte), EU (Europa) e JP (Japão). Para criar uma coluna **'total\_sales'**, precisamos gerá-la a partir das outras. Isso é fácil de fazer:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**

**print(df\['total\_sales'].head())**



result:

**0    74.09**

**1    39.47**

**2    32.23**

**3    29.82**

**4    30.38**

**Name: total\_sales, dtype: float64**





Isso funciona porque a maioria das funções matemáticas funcionam em formato vetorial: elas são aplicadas a colunas inteiras de uma só vez e não precisam percorrer cada valor em uma coluna





Com esse código simples, você pode criar uma nova coluna chamada **'total\_sales'** no DataFrame. O conteúdo dessa coluna será a soma das vendas nas três regiões, linha por linha.



Podemos usar esse método para gerar colunas a partir de fórmulas úteis. Por exemplo, se quisermos calcular a parcela das vendas da EU, basta fazer o seguinte:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

\# cria a coluna total\_sales e a preenche

**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**



\# cria a coluna eu\_sales\_share e a preenche

**df\['eu\_sales\_share'] = df\['eu\_sales'] / df\['total\_sales']**

**print(df\['eu\_sales\_share'].head())**



result:

**0    0.390876**

**1    0.090702**

**2    0.395904**

**3    0.366533**

**4    0.292627**

**Name: eu\_sales\_share, dtype: float64**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Criação de colunas booleanas



vamos criar uma coluna que verifica se a distribuidora é a Nintendo, Podemos criá-la usando os operadores de comparação ==, <, >=.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



\# cria a coluna is\_nintendo e a preenche

**df\['is\_nintendo'] = df\['publisher'] == 'Nintendo'**

**print(df\['is\_nintendo'].head())**



result:

**0    True**

**1    True**

**2    True**

**3    True**

**4    True**

**Name: is\_nintendo, dtype: bool**





Lembre-se de que também podemos fazer isso usando o método **isin()**, que verifica se um valor está em uma lista:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



\# Garante que estamos comparando letras minúsculas

**print(df\['platform'].str.lower().isin(\['gb', 'wii']).head())**



result:

**0     True**

**1    False**

**2     True**

**3     True**

**4     True**

**Name: platform, dtype: bool**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Colunas categóricas



Podemos converter **'platform'** de uma coluna string para uma coluna categórica usando o método **astype()** que você aprendeu no capítulo anterior:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df\['platform'] = df\['platform'].astype('category')**

**print(df\['platform'].head())**



result:

**0    Wii**

**1    NES**

**2    Wii**

**3    Wii**

**4     GB**

**Name: platform, dtype: category**

**Categories (31, object): \['2600', '3DO', '3DS', 'DC', ..., 'WiiU', 'X360', 'XB', 'XOne']**



Observe que existem apenas 31 categorias, embora existam 16.719 entradas. Quando a coluna é armazenada como strings, precisamos manter o texto completo das 16.719 entradas. Quando armazenada como uma categoria, armazenamos apenas um único número (o ID da categoria).

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



Vamos conferir de novo a coluna **'year\_of\_release'** do conjunto de dados de videogame. Em particular, queremos saber o intervalo de anos que nossos dados cobrem:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**print(df\['year\_of\_release'].min(), df\['year\_of\_release'].max())**



result:

**1980.0 2020.0**



Uau, 40 anos de jogos! Vamos dar uma olhada nesses valores contando quantos jogos temos para cada ano.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\_year\_of\_release = df\['year\_of\_release'].value\_counts()**



**print(df\_year\_of\_release)**



result:

**2008.0    1427**

**2009.0    1426**

**2010.0    1255**

**2007.0    1197**

**2011.0    1136**

**2006.0    1006**

**2005.0     939**

**2002.0     829**

**2003.0     775**

**2004.0     762**

**2012.0     653**

**2015.0     606**

**2014.0     581**

**2013.0     544**

**2016.0     502**

**2001.0     482**

**1998.0     379**

**2000.0     350**

**1999.0     338**

**1997.0     289**

**1996.0     263**

**1995.0     219**

**1994.0     121**

**1993.0      60**

**1981.0      46**

**1992.0      43**

**1991.0      41**

**1982.0      36**

**1986.0      21**

**1983.0      17**

**1989.0      17**

**1990.0      16**

**1987.0      16**

**1988.0      15**

**1984.0      14**

**1985.0      14**

**1980.0       9**

**2017.0       3**

**2020.0       1**

**Name: year\_of\_release, dtype: int64**



vamos ordenar o resultado por índice para exibi-lo em ordem cronológica.



Para isso, usaremos o método **sort\_index()**. Ele funciona da mesma forma que o bom e velho **sort\_values()**, mas é aplicado aos índices em vez dos valores:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\_year\_of\_release = df\['year\_of\_release'].value\_counts().sort\_index()**



**print(df\_year\_of\_release)**



result:

**1980.0       9**

**1981.0      46**

**1982.0      36**

**1983.0      17**

**1984.0      14**

**1985.0      14**

**1986.0      21**

**1987.0      16**

**1988.0      15**

**1989.0      17**

**1990.0      16**

**1991.0      41**

**1992.0      43**

**1993.0      60**

**1994.0     121**

**1995.0     219**

**1996.0     263**

**1997.0     289**

**1998.0     379**

**1999.0     338**

**2000.0     350**

**2001.0     482**

**2002.0     829**

**2003.0     775**

**2004.0     762**

**2005.0     939**

**2006.0    1006**

**2007.0    1197**

**2008.0    1427**

**2009.0    1426**

**2010.0    1255**

**2011.0    1136**

**2012.0     653**

**2013.0     544**

**2014.0     581**

**2015.0     606**

**2016.0     502**

**2017.0       3**

**2020.0       1**

**Name: year\_of\_release, dtype: int64**





Que conclusões podemos tirar desses dados? A análise dos jogos de 2008 seria significativamente diferente da análise dos jogos de 2009? Se quisermos comparar os dados de 2000 com outros anos, precisaremos compará-los com os dados de 1986 e de 1987 separadamente, ou seria melhor agrupar essas comparações para obter dados sobre os jogos lançados antes de 2000 comparados aos lançados depois de 2000?



#### Categorização



Precisamos realizar uma categorização, o que significa agrupar dados em novas categorias que vamos criar. Nesse caso, vamos agrupar os jogos em quatro categorias com base em geração.



* Lançamentos de antes de 2000 vão para a categoria **'retro'**
* Lançamentos entre 2000 e 2009 (incluindo esse ano) vão para a categoria **'modern'**
* Lançamentos a partir de 2010 vão para a categoria **'recent'**
* Aqueles sem dados do ano de lançamento vão para a categoria **'unknown'**



Queremos colocar cada jogo em uma dessas quatro categorias e armazenar o resultado em uma nova coluna.



Não há na pandas uma função pronta para fazer isso. No entanto, podemos escrever nossa própria função, adaptada especificamente às necessidades aqui. A função deve aceitar o ano de lançamento como entrada e retornar como saída a categoria da geração para aquele ano.



Veja como ficará nossa função personalizada, **era\_group():**



**def era\_group(year):**

&nbsp;   """


    A função retorna a geração dos jogos conforme o ano de lançamento, usando as seguintes regras:

    —'retro'   for year < 2000

    —'modern'  for 2000 <= year < 2010

    —'recent'  for year >= 2010

    —'unknown' for missing year values (NaN)

    """



&nbsp;   \*\*if year < 2000:\*\*

        \*\*return 'retro'\*\*

    \*\*elif year < 2010:\*\*

        \*\*return 'modern'\*\*

    \*\*elif year >= 2010:\*\*

        \*\*return 'recent'\*\*

    \*\*else:\*\*

        \*\*return 'unknown'\*\*










Agora precisaremos testar a função para cada regra. Então, vamos verificar em quais categorias três jogos de anos diferentes se enquadram:



**print(era\_group(1983))**

**print(era\_group(2009))**

**print(era\_group(2021))**

**print(era\_group(np.nan))**



result:

**retro**

**modern**

**recent**

**unknown**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### O método apply()



Para criar uma coluna com as gerações usando a função personalizada **era\_group()**, precisaremos chamar o método **apply()**, que recebe valores de uma coluna DataFrame e aplica uma função a eles.



Nesse caso, **apply()** deve ser aplicada à coluna **'year\_of\_release'**, porque **'year\_of\_release'** contém os dados que a função usa como parâmetro. A própria função **era\_group()** torna-se então o argumento que passamos para o método **apply()**.



**import pandas as pd**



**def era\_group(year):**

    """

    A função retorna a geração dos jogos conforme o ano de lançamento, usando as seguintes regras:

    —'retro'   for year < 2000

    —'modern'  for 2000 <= year < 2010

    —'recent'  for year >= 2010

    —'unknown' for missing year values (NaN)

    """



    **if year < 2000:**

&nbsp;       \*\*return 'retro'\*\*

    \*\*elif year < 2010:\*\*

        \*\*return 'modern'\*\*

    \*\*elif year >= 2010:\*\*

        \*\*return 'recent'\*\*

    \*\*else:\*\*

        \*\*return 'unknown'\*\*






**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df\['era\_group'] = df\['year\_of\_release'].apply(era\_group)**

**print(df.head())**



result:

&nbsp;            \*\*name platform  year\\\_of\\\_release         genre publisher  \\\\\*\*


**0                Wii Sports      Wii           2006.0        Sports  Nintendo**

**1         Super Mario Bros.      NES           1985.0      Platform  Nintendo**

**2            Mario Kart Wii      Wii           2008.0        Racing  Nintendo**

**3         Wii Sports Resort      Wii           2009.0        Sports  Nintendo**

**4  Pokemon Red/Pokemon Blue       GB           1996.0  Role-Playing  Nintendo**



**developer  na\_sales  eu\_sales  jp\_sales  critic\_score  user\_score era\_group**

**0  Nintendo     41.36     28.96      3.77          76.0         8.0    modern**

**1       NaN     29.08      3.58      6.81           NaN         NaN     retro**

**2  Nintendo     15.68     12.76      3.79          82.0         8.3    modern**

**3  Nintendo     15.61     10.93      3.28          80.0         8.0    modern**

**4       NaN     11.27      8.89     10.22           NaN         NaN     retro**





Agora vamos analisar os dados das gerações com o método **value\_counts():**



**import pandas as pd**



**def era\_group(year):**

    """

    A função retorna a geração dos jogos conforme o ano de lançamento, usando as seguintes regras:

    —'retro'   for year < 2000

    —'modern'  for 2000 <= year < 2010

    —'recent'  for year >= 2010

    —'unknown' for missing year values (NaN)

    """



    **if year < 2000:**

&nbsp;       \*\*return 'retro'\*\*

    \*\*elif year < 2010:\*\*

        \*\*return 'modern'\*\*

    \*\*elif year >= 2010:\*\*

        \*\*return 'recent'\*\*

    \*\*else:\*\*

        \*\*return 'unknown'\*\*






**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df\['era\_group'] = df\['year\_of\_release'].apply(era\_group)**

**print(df\['era\_group'].value\_counts())**



result:

**modern     9193**

**recent     5281**

**retro      1974**

**unknown     269**

**Name: era\_group, dtype: int64**





Mas e se quisermos criar categorias baseadas em valores de mais de uma coluna? Nesse caso, podemos escrever uma função que receba uma linha inteira como entrada e extraia os valores necessários para criar novas categorias.



Ilustraremos isso com o conjunto de dados de videogames. Mas, desta vez, vamos simplificar as coisas eliminando todas as linhas que tenham valores ausentes:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df.dropna(inplace=True)**

**df.info()**



result:

**<class 'pandas.core.frame.DataFrame'>**

**Int64Index: 7943 entries, 0 to 16707**

**Data columns (total 11 columns):**

**#   Column           Non-Null Count  Dtype**

**---  ------           --------------  -----**

**0   name             7943 non-null   object**

**1   platform         7943 non-null   object**

**2   year\_of\_release  7943 non-null   float64**

**3   genre            7943 non-null   object**

**4   publisher        7943 non-null   object**

**5   developer        7943 non-null   object**

**6   na\_sales         7943 non-null   float64**

**7   eu\_sales         7943 non-null   float64**

**8   jp\_sales         7943 non-null   float64**

**9   critic\_score     7943 non-null   float64**

**10  user\_score       7943 non-null   object**

**dtypes: float64(5), object(6)**

**memory usage: 744.7+ KB**



Descartamos muitas linhas, mas agora não precisamos nos preocupar com valores ausentes. Além disso, sempre podemos adicioná-las novamente após nossa análise preliminar para ver como isso afeta os resultados.



Agora vamos escrever uma função chamada **era\_sales\_group()** que coloca os jogos nas seguintes categorias conforme o ano de lançamento e as vendas totais:



* Lançamentos de antes de 2000 com menos de US$ 1 milhão em vendas vão para a categoria 'retro'
* Lançamentos entre 2000 e 2009 (incluindo esse ano) com menos de US$ 1 milhão em vendas vão para a categoria 'modern'
* Lançamentos de antes de 2010 com US$ 1 milhão ou mais em vendas vão para a categoria 'classic'
* Lançamentos a partir de 2010 com menos de US$ 1 milhão em vendas vão para a categoria 'recent'
* Lançamentos a partir de 2010 com US$ 1 milhão ou mais em vendas vão para a categoria 'big hit'



Veja como a função fica, e também uma amostra do resultado:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df.dropna(inplace=True)**



**def era\_sales\_group(row):**

    """

    A função retorna a geração dos jogos conforme o ano de lançamento, usando as seguintes regras:

    —'retro'   para ano < 2000 e o total de vendas < $1 milhões

    —'modern'  para 2000 <= ano < 2010 e o total de vendas < $1 milhões

    —'recent'  para ano >= 2010 e o total de vendas < $1 milhões

    —'classic' para ano < 2010 e o total de vendas >= $1 milhões

    —'big hit' para ano >= 2010 e o total de vendas >= $1 milhões

    """



&nbsp;   \*\*year = row\\\['year\\\_of\\\_release']\*\*

    \*\*na\\\_sales = row\\\['na\\\_sales']\*\*

    \*\*eu\\\_sales = row\\\['eu\\\_sales']\*\*

    \*\*jp\\\_sales = row\\\['jp\\\_sales']\*\*

    

    \*\*total\\\_sales = na\\\_sales + eu\\\_sales + jp\\\_sales\*\*

    

    \*\*if year < 2000:\*\*

        \*\*if total\\\_sales < 1:\*\*

            \*\*return 'retro'\*\*

        \*\*else:\*\*

            \*\*return 'classic'\*\*

    \*\*if year < 2010:\*\*

        \*\*if total\\\_sales < 1:\*\*

            \*\*return 'modern'\*\*

        \*\*else:\*\*

            \*\*return 'classic'\*\*

    \*\*if year >= 2010:\*\*

        \*\*if total\\\_sales < 1:\*\*

            \*\*return 'recent'\*\*

        \*\*else:\*\*

            \*\*return 'big hit'\*\*






**row = df.iloc\[0]** # use a primeira linha como exemplo de parâmetro de entrada



**print(row)**

**print()**

**print('Esse jogo é', era\_sales\_group(row))**



result:

**name               Wii Sports**

**platform                  Wii**

**year\_of\_release        2006.0**

**genre                  Sports**

**publisher            Nintendo**

**developer            Nintendo**

**na\_sales                41.36**

**eu\_sales                28.96**

**jp\_sales                 3.77**

**critic\_score             76.0**

**user\_score                  8**

**Name: 0, dtype: object**



**Esse jogo é classic**



Assim como a função **era\_group()** da lição anterior, a função **era\_sales\_group()** também tem um parâmetro, **row**. Mas, nesse caso, espera-se que o parâmetro row seja uma única linha inteira do DataFrame.



Observe que a função usa os nomes das colunas de **df** para extrair esses valores, então ela não funcionaria em outro DataFrame com colunas diferentes. Ela é específica para os dados em **vg\_sales.csv**.



Usamos **df.iloc\[0]** para pegar a primeira linha de df e testar a função, que produziu a saída **'classic'**. Se verificarmos as regras de categorização, isso faz sentido. O Wii Sports foi lançado antes de 2010 e certamente teve mais de US$ 1 milhão em vendas – muito, muito mais!

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Usos da função com linhas personalizadas



Podemos criar nossas próprias linhas com os valores que queremos testar. Basta converter uma lista de nomes de colunas e uma lista de valores de linhas em um objeto Series:



**column\_names = \['year\_of\_release', 'na\_sales', 'eu\_sales', 'jp\_sales']**

**row\_values = \[2000, 0.1, 0.25, 0]**



**row = pd.Series(row\_values, index=column\_names)**



**print(row)**

**print()**

**print('Esse jogo é', era\_sales\_group(row))**



result:

**year\_of\_release    2000.00**

**na\_sales              0.10**

**eu\_sales              0.25**

**jp\_sales              0.00**

**dtype: float64**



**Esse jogo é modern**





Para finalizar os testes, vamos verificar a função com os vários valores de entrada que criamos:



**cols = \['year\_of\_release', 'na\_sales', 'eu\_sales', 'jp\_sales']**



**row\_1 = pd.Series(\[1989, 0, 0, 0.6], index=cols) # esperado 'retro'**

**row\_2 = pd.Series(\[1989, 1, 2, 0], index=cols)   # esperado 'classic'**

**row\_3 = pd.Series(\[2006, 0.3, 0, 0], index=cols) # esperado 'modern'**

**row\_4 = pd.Series(\[2020, 0, 0.4, 0], index=cols) # esperado 'recent'**

**row\_5 = pd.Series(\[2020, 1, 1, 1], index=cols)   # esperado 'big hit'**



**print(row\_1, row\_2, row\_3, row\_4, row\_5, sep='\\n\\n')**

**print()**



**rows = \[row\_1, row\_2, row\_3, row\_4, row\_5]**



**for row in rows:**

&nbsp;   \*\*print('Esse jogo é', era\\\_sales\\\_group(row))\*\*






result:

**year\_of\_release    1989.0**

**na\_sales              0.0**

**eu\_sales              0.0**

**jp\_sales              0.6**

**dtype: float64**



**year\_of\_release    1989**

**na\_sales              1**

**eu\_sales              2**

**jp\_sales              0**

**dtype: int64**



**year\_of\_release    2006.0**

**na\_sales              0.3**

**eu\_sales              0.0**

**jp\_sales              0.0**

**dtype: float64**



**year\_of\_release    2020.0**

**na\_sales              0.0**

**eu\_sales              0.4**

**jp\_sales              0.0**

**dtype: float64**



**year\_of\_release    2020**

**na\_sales              1**

**eu\_sales              1**

**jp\_sales              1**

**dtype: int64**



**Esse jogo é retro**

**Esse jogo é classic**

**Esse jogo é modern**

**Esse jogo é recent**

**Esse jogo é big hit**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Criação de colunas



Assim como na lição anterior, chamaremos o método apply(). Desta vez, no entanto, existem duas grandes diferenças:



* O método **apply()** é chamado em todo o DataFrame **df** em vez de em uma única coluna
* Precisamos usar o parâmetro **axis=** ao chamar o método **apply()**



Por padrão, o parâmetro **axis=** é definido como **0**, que significa que **apply()** passa valores de colunas para a função usada como parâmetro de entrada. Se quisermos que **apply()** passe valores de linhas para a função, precisamos definir **axis=1**.



Assim, a nova coluna **'game\_category'** pode ser criada da seguinte maneira:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**def era\_sales\_group(row):**

    """

    A função retorna a geração dos jogos conforme o ano de lançamento, usando as seguintes regras:

    —'retro'   para ano < 2000 e o total de vendas < $1 milhões

    —'modern'  para 2000 <= ano < 2010 e o total de vendas < $1 milhões

    —'recent'  para ano >= 2010 e o total de vendas < $1 milhões

    —'classic' para ano < 2010 e o total de vendas >= $1 milhões

    —'big hit' para ano >= 2010 e o total de vendas >= $1 milhões

    """



    **year = row\['year\_of\_release']**

&nbsp;   \*\*na\\\_sales = row\\\['na\\\_sales']\*\*

    \*\*eu\\\_sales = row\\\['eu\\\_sales']\*\*

    \*\*jp\\\_sales = row\\\['jp\\\_sales']\*\*

    

    \*\*total\\\_sales = na\\\_sales + eu\\\_sales + jp\\\_sales\*\*

    

    \*\*if year < 2000:\*\*

        \*\*if total\\\_sales < 1:\*\*

            \*\*return 'retro'\*\*

        \*\*else:\*\*

            \*\*return 'classic'\*\*

    \*\*if year < 2010:\*\*

        \*\*if total\\\_sales < 1:\*\*

            \*\*return 'modern'\*\*

        \*\*else:\*\*

            \*\*return 'classic'\*\*

    \*\*if year >= 2010:\*\*

        \*\*if total\\\_sales < 1:\*\*

            \*\*return 'recent'\*\*

        \*\*else:\*\*

            \*\*return 'big hit'\*\*






**df\['game\_category'] = df.apply(era\_sales\_group, axis=1)**

**print(df.sample(5, random\_state=321))**



result:

&nbsp;                                   \*\*name platform  year\\\_of\\\_release      genre  \\\\\*\*


**7654          Dave Mirra Freestyle BMX 2       XB           2001.0     Sports**

**6815          Burnout 2: Point of Impact       XB           2003.0     Racing**

**9824   NPPL: Championship Paintball 2009     X360           2008.0    Shooter**

**34            Call of Duty: Black Ops II      PS3           2012.0    Shooter**

**14940  Agatha Christie's The ABC Murders      PS4           2016.0  Adventure**



&nbsp;                  \*\*publisher        developer  na\\\_sales  eu\\\_sales  jp\\\_sales  \\\\\*\*


**7654   Acclaim Entertainment     Z-Axis, Ltd.      0.15      0.04      0.00**

**6815   Acclaim Entertainment  Criterion Games      0.18      0.05      0.00**

**9824        Activision Value         FUN Labs      0.10      0.01      0.00**

**34                Activision         Treyarch      4.99      5.73      0.65**

**14940               Microids         Microids      0.01      0.01      0.00**



&nbsp;      \*\*critic\\\_score  user\\\_score game\\\_category\*\*  


**7654           76.0         8.2        modern**

**6815           88.0         5.1        modern**

**9824           44.0         7.3        modern**

**34             83.0         5.3       big hit**

**14940          67.0         6.2        recent**







Podemos ver que a coluna foi criada e que podemos começar a analisá-la. Por exemplo, podemos usar o método **value\_counts()** para obter o número de jogos que pertencem a cada categoria:



**print(df\['game\_category'].value\_counts())**





result:

**modern     3907**

**recent     1784**

**classic     748**

**big hit     407**

**retro        43**

**Name: game\_category, dtype: int64**







* Crie uma nova função chamada **average\_sales()** que aplica a fórmula que usamos antes: **('na\_sales' + 'eu\_sales' + 'jp\_sales') / 3**
* Crie uma nova coluna chamada **'avg\_sales'** aplicando a função **average\_sales()** ao DataFrame.

Imprima as 5 primeiras linhas para ter certeza de que está tudo certo com a nova coluna.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**def average\_sales(row):**

&nbsp;   \*\*na\\\_sales = row\\\['na\\\_sales']\*\*

    \*\*eu\\\_sales = row\\\['eu\\\_sales']\*\*

    \*\*jp\\\_sales = row\\\['jp\\\_sales']\*\*

    

    \*\*return (na\\\_sales + eu\\\_sales + jp\\\_sales) / 3\*\*






**df\['avg\_sales'] = df.apply(average\_sales, axis=1)**

**print(df.head())**



Result:

&nbsp;                   \*\*name platform  ...  user\\\_score  avg\\\_sales\*\*


**0             Wii Sports      Wii  ...           8  24.696667**

**2         Mario Kart Wii      Wii  ...         8.3  10.743333**

**3      Wii Sports Resort      Wii  ...           8   9.940000**

**6  New Super Mario Bros.       DS  ...         8.5   8.973333**

**7               Wii Play      Wii  ...         6.6   8.690000**



**\[5 rows x 12 columns]**

