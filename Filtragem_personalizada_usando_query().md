#### Filtragem personalizada usando **query()**

#### 

Você já aprendeu técnicas básicas de filtragem que usam operadores lógicos como >, <, >=, <= e ==. Nesta lição, você vai aprender como filtrar dados usando dois novos métodos da pandas:



**isin()**, que verifica a existência de valores

**query()**, que permite filtrar dados usando strings personalizadas de consulta





Vamos trabalhar com um conjunto de dados que contém informações históricas sobre vendas de consoles de videogames. Os dados originais são de  VGChartz, caminho ''https://www.vgchartz.com/gamedb/ ''(os materiais estão em inglês), e foram modificados para os nossos propósitos aqui.



Vamos dar uma olhada nos dados:



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**print(df.head())**

**print()**

**print(df.info())**



result:



                       **name platform  year\_of\_release         genre publisher  \\**

**0                Wii Sports      Wii           2006.0        Sports  Nintendo**   

**1         Super Mario Bros.      NES           1985.0      Platform  Nintendo**   

**2            Mario Kart Wii      Wii           2008.0        Racing  Nintendo**   

**3         Wii Sports Resort      Wii           2009.0        Sports  Nintendo**   

**4  Pokemon Red/Pokemon Blue       GB           1996.0  Role-Playing  Nintendo**   



  **developer  na\_sales  eu\_sales  jp\_sales  critic\_score  user\_score**  

**0  Nintendo     41.36     28.96      3.77          76.0         8.0**  

**1       NaN     29.08      3.58      6.81           NaN         NaN**  

**2  Nintendo     15.68     12.76      3.79          82.0         8.3**  

**3  Nintendo     15.61     10.93      3.28          80.0         8.0**  

**4       NaN     11.27      8.89     10.22           NaN         NaN**  



**<class 'pandas.core.frame.DataFrame'>**

**RangeIndex: 16717 entries, 0 to 16716**

**Data columns (total 11 columns):**

 **#   Column           Non-Null Count  Dtype**  

**---  ------           --------------  -----**  

 **0   name             16717 non-null  object** 

 **1   platform         16717 non-null  object** 

 **2   year\_of\_release  16448 non-null  float64**

 **3   genre            16717 non-null  object** 

 **4   publisher        16663 non-null  object** 

 **5   developer        10096 non-null  object** 

 **6   na\_sales         16717 non-null  float64**

 **7   eu\_sales         16717 non-null  float64**

 **8   jp\_sales         16717 non-null  float64**

 **9   critic\_score     8137 non-null   float64**

 **10  user\_score       7590 non-null   float64**

**dtypes: float64(6), object(5)**

**memory usage: 1.4+ MB**



##### Filtragem com strings de consulta e o método **query()**



Vamos filtrar os dados de forma que apenas selecionemos os jogos cujas vendas no Japão foram de pelo menos um milhão de dólares:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**mask = df\['jp\_sales'] >= 1**

**print(df\[mask]\[\['name', 'jp\_sales']])**



result:



                                             **name  jp\_sales**

**0                                      Wii Sports      3.77**

**1                               Super Mario Bros.      6.81**

**2                                  Mario Kart Wii      3.79**

**3                               Wii Sports Resort      3.28**

**4                        Pokemon Red/Pokemon Blue     10.22**

**...                                           ...       ...**

**1970                  Tag Team Match M.U.S.C.L.E.      1.05**

**1971                            Derby Stallion 96      1.04**

**1972                             Adventure Island      1.05**

**2051    Oshare Majo Love and Berry: DS Collection      1.01**

**2065  Jissen Pachi-Slot Hisshouhou: Hokuto no Ken      1.00**



**\[243 rows x 2 columns]**



Para simplificar, estamos visualizando apenas as colunas 'name' e 'jp\_sales'. É claro, poderíamos ter feito isso sem criar a variável mask e simplesmente colocado a expressão de máscara na nossa linha de filtragem no código.

Podemos fazer a mesma filtragem usando o método **query()**.



Esse método é chamado em um DataFrame e espera uma string como parâmetro. A string representa a consulta que você quer fazer no DataFrame, o que significa que ela diz ao Python quais linhas precisam ser filtradas:



ex:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**print(df.query("jp\_sales > 1")\[\['name', 'jp\_sales']])**



result:



                                           **name  jp\_sales**

**0                                    Wii Sports      3.77**

**1                             Super Mario Bros.      6.81**

**2                                Mario Kart Wii      3.79**

**3                             Wii Sports Resort      3.28**

**4                      Pokemon Red/Pokemon Blue     10.22**

**...                                         ...       ...**

**1885                              Densha De Go!      1.02**

**1970                Tag Team Match M.U.S.C.L.E.      1.05**

**1971                          Derby Stallion 96      1.04**

**1972                           Adventure Island      1.05**

**2051  Oshare Majo Love and Berry: DS Collection      1.01**



**\[239 rows x 2 columns]**



Para filtrar usando **query()** com base nas comparações de strings, você precisa colocar a string entre aspas. Por exemplo, vamos selecionar apenas os jogos publicados pela Nintendo:





**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**print(df.query("publisher == 'Nintendo'")\[\['name', 'publisher']].head())**



result:

                       **name publisher**

**0                Wii Sports  Nintendo**

**1         Super Mario Bros.  Nintendo**

**2            Mario Kart Wii  Nintendo**

**3         Wii Sports Resort  Nintendo**

**4  Pokemon Red/Pokemon Blue  Nintendo**



Agora use query() para filtrar os dados. Mantenha apenas as linhas em que as colunas 'publisher' e 'developer' têm os mesmos valores



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**cols = \['name', 'publisher', 'developer']**



**df\_filtered = df.query("publisher == developer")\[cols]**

**print(df\_filtered.head())**



result:

                    **name publisher developer**

**0             Wii Sports  Nintendo  Nintendo**

**2         Mario Kart Wii  Nintendo  Nintendo**

**3      Wii Sports Resort  Nintendo  Nintendo**

**6  New Super Mario Bros.  Nintendo  Nintendo**

**7               Wii Play  Nintendo  Nintendo**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Filtragem usando o método **isin()**



Em vez de usar os operadores lógicos que você conhece, **isin()** verifica se os valores em uma coluna correspondem a algum dos valores em outro vetor, como uma lista ou um dicionário.



Podemos usar uma lista de consoles de videogames portáteis para obter apenas as linhas de jogos em um desses consoles:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**handhelds = \['3DS', 'DS', 'GB', 'GBA', 'PSP']**

**print(df\[df\['platform'].isin(handhelds)]\[\['name', 'platform']])**



result:

                                                    **name platform**

**4                               Pokemon Red/Pokemon Blue       GB**

**5                                                 Tetris       GB**

**6                                  New Super Mario Bros.       DS**

**10                                            Nintendogs       DS**

**11                                         Mario Kart DS       DS**

**...                                                  ...      ...**

**16702                           Mezase!! Tsuri Master DS       DS**

**16703  Eiyuu Densetsu: Sora no Kiseki Material Collec...      PSP**

**16706                                           Plushees       DS**

**16710                 Woody Woodpecker in Crazy Castle 5      GBA**

**16715                                   Spirits \& Spells      GBA**



**\[4801 rows x 2 columns]**



A propósito, você se lembra do símbolo de til **(~)**, que inverte o resultado? Podemos usá-lo aqui também. Por exemplo, podemos filtrar o DataFrame original extraindo apenas as linhas em que os valores na coluna 'platform' não estão na lista handhelds:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**handhelds = \['3DS', 'DS', 'GB', 'GBA', 'PSP']**

**print(df\[~df\['platform'].isin(handhelds)]\[\['name', 'platform']])**

result:

&nbsp;                                                                                                  **name platform**

**0                                            Wii Sports      Wii**

**1                                     Super Mario Bros.      NES**

**2                                        Mario Kart Wii      Wii**

**3                                     Wii Sports Resort      Wii**

**7                                              Wii Play      Wii**

**...                                                 ...      ...**

**16711  SCORE International Baja 1000: The Official Game      PS2**

**16712                     Samurai Warriors: Sanada Maru      PS3**

**16713                                  LMA Manager 2007     X360**

**16714                           Haitaka no Psychedelica      PSV**

**16716                               Winning Post 8 2016      PSV**



**\[11916 rows x 2 columns]**



Também podemos verificar a existência de algo usando o método **query()** com a palavra-chave **in** na string de consulta.



Vamos ver como isso funciona em uma filtragem igual à anterior:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**handhelds = \['3DS', 'DS', 'GB', 'GBA', 'PSP']**

**print(df.query("platform in @handhelds")\[\['name', 'platform']]**



result:

                                                   **name platform**

**4                               Pokemon Red/Pokemon Blue       GB**

**5                                                 Tetris       GB**

**6                                  New Super Mario Bros.       DS**

**10                                            Nintendogs       DS**

**11                                         Mario Kart DS       DS**

**...                                                  ...      ...**

**16702                           Mezase!! Tsuri Master DS       DS**

**16703  Eiyuu Densetsu: Sora no Kiseki Material Collec...      PSP**

**16706                                           Plushees       DS**

**16710                 Woody Woodpecker in Crazy Castle 5      GBA**

**16715                                   Spirits \& Spells      GBA**



**\[4801 rows x 2 columns]**



Como alternativa, você pode encontrar as linhas que não estão na lista usando a palavra-chave **not in**:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**handhelds = \['3DS', 'DS', 'GB', 'GBA', 'PSP']**

**print(df.query("platform not in @handhelds")\[\['name', 'platform']])**



result:

                                                                                                    **name platform**

**0                                            Wii Sports      Wii**

**1                                     Super Mario Bros.      NES**

**2                                        Mario Kart Wii      Wii**

**3                                     Wii Sports Resort      Wii**

**7                                              Wii Play      Wii**

**...                                                 ...      ...**

**16711  SCORE International Baja 1000: The Official Game      PS2**

**16712                     Samurai Warriors: Sanada Maru      PS3**

**16713                                  LMA Manager 2007     X360**

**16714                           Haitaka no Psychedelica      PSV**

**16716                               Winning Post 8 2016      PSV**



**\[11916 rows x 2 columns]**



Como a variável handhelds é externa ao DataFrame, temos que iniciá-la com o símbolo **@** na string de consulta. Caso contrário, a pandas vai tentar encontrar uma coluna chamada 'handhelds' e vai exibir um erro quando não conseguir encontrá-la.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Uso de estruturas de dados externas para filtrar DataFrames



Nesta lição, você vai aprender como filtrar usando dicionários, Series e até outros DataFrames

Para descobrir se os valores da coluna 'a' estão na lista our\_list, vamos escrever a consulta "a in @our\_list" (a em @our\_list).



**import pandas as pd**



**our\_list = \[2, 5, 10]**

**df = pd.DataFrame(**

    **{**

        **'a': \[2, 3, 10, 11, 12],**

        **'b': \[5, 4, 3, 2, 1],**

        **'c': \['X', 'Y', 'Y', 'Y', 'Z'],**

    **}**

**)**

**print(df)**

**print()**

**print(our\_list)**

**print()**

**print(df.query("a in @our\_list"))**



result:

    **a  b  c**

**0   2  5  X**

**1   3  4  Y**

**2  10  3  Y**

**3  11  2  Y**

**4  12  1  Z**



**\[2, 5, 10]**



    **a  b  c**

**0   2  5  X**

**2  10  3  Y**



Como podemos ver, **query()** retornou todas as linhas de df onde os valores na coluna 'a' estão presentes em our\_list. Esses valores são 2 e 10; a coluna 'a' não contém o valor 5.



#### Filtragem com um dicionário



Lembre-se de que dicionários consistem em pares chave-valor.



Vamos criar um dicionário e atribuí-lo à variável our\_dict. Para verificar a presença dos valores da coluna 'a' entre os valores do dicionário, precisamos usar o método **values()** do dicionário na consulta **"a in @our\_dict.values()":**



**import pandas as pd**



**our\_dict = {0: 10, 3: 11, 12: 12}**

**df = pd.DataFrame(**

    **{**

        **'a': \[2, 3, 10, 11, 12],**

        **'b': \[5, 4, 3, 2, 1],**

        **'c': \['X', 'Y', 'Y', 'Y', 'Z'],**

    **}**

**)**

**print(df)**

**print()**

**print(our\_dict)**

**print()**

**print(df.query("a in @our\_dict.values()"))**



result:

    **a  b  c**

**0   2  5  X**

**1   3  4  Y**

**2  10  3  Y**

**3  11  2  Y**

**4  12  1  Z**



**{0: 10, 3: 11, 12: 12}**



    **a  b  c**

**2  10  3  Y**

**3  11  2  Y**

**4  12  1  Z**



Cada um dos valores do dicionário (10, 11 e 12) aparece na coluna 'a' apenas uma vez, então recebemos três linhas de **query()**. Os valores de índice no DataFrame filtrado permanecem inalterados. A filtragem com valores de dicionário é muito parecida à filtragem com listas.



No entanto, para verificar a presença dos valores da coluna 'a' entre as chaves do dicionário (0, 3 e 12), precisamos usar a consulta **"a in @our\_dict.keys()"**. A consulta mais curta **"a in @our\_dict"** também funciona, porque verifica as chaves por padrão:



**import pandas as pd**



**our\_dict = {0: 10, 3: 11, 12: 12}**

**df = pd.DataFrame(**

    **{**

        **'a': \[2, 3, 10, 11, 12],**

        **'b': \[5, 4, 3, 2, 1],**

        **'c': \['X', 'Y', 'Y', 'Y', 'Z'],**

    **}**

**)**

**print(df)**

**print()**

**print(our\_dict)**

**print()**

**print(df.query("a in @our\_dict"))**



result:



    **a  b  c**

**0   2  5  X**

**1   3  4  Y**

**2  10  3  Y**

**3  11  2  Y**

**4  12  1  Z**



**{0: 10, 3: 11, 12: 12}**



    **a  b  c**

**1   3  4  Y**

**4  12  1  Z**





#### Filtragem com um objeto Series



Da mesma forma que dicionários armazenam pares chave-valor, objetos Series armazenam pares índice-valor. Entretanto, os valores para objetos Series são verificados por padrão.



A consulta **"a in @our\_series"** verifica a presença de valores na coluna **'a'** entre os valores de our\_series em vez de entre os índices.



**import pandas as pd**



**our\_series = pd.Series(\[10, 11, 12])**

**df = pd.DataFrame(**

    **{**

        **'a': \[2, 3, 10, 11, 12],**

        **'b': \[5, 4, 3, 2, 1],**

        **'c': \['X', 'Y', 'Y', 'Y', 'Z'],**

    **}**

**)**

**print(df)**

**print()**

**print(our\_series)**

**print()**

**print(df.query("a in @our\_series"))**



result:

    **a  b  c**

**0   2  5  X**

**1   3  4  Y**

**2  10  3  Y**

**3  11  2  Y**

**4  12  1  Z**



**0    10**

**1    11**

**2    12**

**dtype: int64**



    **a  b  c**

**2  10  3  Y**

**3  11  2  Y**

**4  12  1  Z**



Se precisarmos verificar se os valores da coluna **'a'** estão presentes entre os valores de índice do Series, então teremos que usar o atributo **index**: **"a in @our\_series.index"**.



ex:



**import pandas as pd**



**our\_series = pd.Series(\[10, 11, 12], index=\['X', 'Y', 'T'])**

**df = pd.DataFrame(**

    **{**

        **'a': \[2, 3, 10, 11, 12],**

        **'b': \[5, 4, 3, 2, 1],**

        **'c': \['X', 'Y', 'Y', 'Y', 'Z'],**

    **}**

**)**



**print(df.query("c in @our\_series.index"))**



Result:

    **a  b  c**

**0   2  5  X**

**1   3  4  Y**

**2  10  3  Y**

**3  11  2  Y**



#### Filtragem com um DataFrame



Também podemos usar um DataFrame externo para filtrar os dados de duas maneiras:



* Filtrar usando seus valores de índice
* Filtrar usando valores em colunas específicas



&nbsp;fazemos a verificação da mesma maneira que fizemos para os índices de um objeto Series: acessamos o atributo **index** na consulta.



ex:



**import pandas as pd**



**df = pd.DataFrame(**

    **{**

        **'a': \[2, 3, 10, 11, 12],**

        **'b': \[5, 4, 3, 2, 1],**

        **'c': \['X', 'Y', 'Y', 'Y', 'Z'],**

    **}**

**)**

**our\_df = pd.DataFrame(**

    **{**

        **'a1': \[2, 4, 6],**

        **'b1': \[3, 2, 2],**

        **'c1': \['A', 'B', 'C'],**

    **},**

    **index=\['Z', 'X', 'P']**

**)**



**print(df)**

**print()**

**print(our\_df)**

**print()**

**print(df.query("c in @our\_df.index"))**



result:

    **a  b  c**

**0   2  5  X**

**1   3  4  Y**

**2  10  3  Y**

**3  11  2  Y**

**4  12  1  Z**



   **a1  b1 c1**

**Z   2   3  A**

**X   4   2  B**

**P   6   2  C**



    **a  b  c**

**0   2  5  X**

**4  12  1  Z**





Para verificar se os valores de uma coluna do DataFrame que queremos filtrar (neste caso, **df**) também estão presentes em uma coluna de um DataFrame externo (**our\_df**), também precisamos especificar a coluna externa na consulta usando a notação de ponto.



A consulta **"a in @our\_df.a1"** verifica se algum valor da coluna **'a'** de **df** está presente na coluna **'a1'** de **our\_df.**



Podemos usar a notação de ponto para acessar as colunas de um DataFrame como atributos em vez da notação de colchetes, então **our\_df\['a1']** é equivalente a **our\_df.a1**. No entanto, em strings de consulta, apenas a notação de ponto funciona.



ex:



**import pandas as pd**



**df = pd.DataFrame(**

    **{**

        **'a': \[2, 3, 10, 11, 12],**

        **'b': \[5, 4, 3, 2, 1],**

        **'c': \['X', 'Y', 'Y', 'Y', 'Z'],**

    **}**

**)**

**our\_df = pd.DataFrame(**

    **{**

        **'a1': \[2, 4, 6],**

        **'b1': \[3, 2, 2],**

        **'c1': \['A', 'B', 'C'],**

    **},**

    **index=\['Z', 'X', 'P']**

**)**



**print(df.query("a in @our\_df.b1"))**



Result:

   **a  b  c**

**0  2  5  X**

**1  3  4  Y**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Filtragem com base em várias condições



não podemos usar operadores lógicos, como **and, or** ou **not** nas expressões de filtragem. Em vez disso, temos que usar as versões que comparam bit a bit: **\&, |** e **~** , respectivamente.



Digamos que queremos verificar todos os jogos de Wii que não são de esporte:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['user\_score'] = pd.to\_numeric(df\['user\_score'], errors='coerce')**



**print(df\[(df\['platform'] == 'Wii') \& ~(df\['genre'] == 'Sports')].head())**



result:

                         **name platform  year\_of\_release     genre publisher  \\**

**2              Mario Kart Wii      Wii           2008.0    Racing  Nintendo**   

**7                    Wii Play      Wii           2006.0      Misc  Nintendo**   

**8   New Super Mario Bros. Wii      Wii           2009.0  Platform  Nintendo**   

**39    Super Smash Bros. Brawl      Wii           2008.0  Fighting  Nintendo**   

**49         Super Mario Galaxy      Wii           2007.0  Platform  Nintendo**   



    **developer  na\_sales  eu\_sales  jp\_sales  critic\_score  user\_score**  

**2    Nintendo     15.68     12.76      3.79          82.0         8.3**  

**7    Nintendo     13.96      9.18      2.93          58.0         6.6**  

**8    Nintendo     14.44      6.94      4.70          87.0         8.4**  

**39  Game Arts      6.62      2.55      2.66          93.0         8.9**  

**49   Nintendo      6.06      3.35      1.20          97.0         8.9**



Há três componentes importantes para a condição de filtragem (**df\['platform'] == 'Wii') \& ~(df\['genre'] == 'Sports')**:



* Cada condição individual é separada por parênteses.
* O operador **\&** é usado em vez de **and**.
* O operador **~** é usado em vez do **not** (e ele precede a condição que queremos negar).





Agora vamos explorar uma condição **"or"** (ou) obtendo todos os jogos que superaram $1 milhão em vendas em pelo menos uma das três regiões:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['user\_score'] = pd.to\_numeric(df\['user\_score'], errors='coerce')**



**print(df\[(df\['na\_sales'] >= 1) | (df\['eu\_sales'] >= 1) | (df\['jp\_sales'] >= 1)].head())**



result:

                       **name platform  year\_of\_release         genre publisher  \\**

**0                Wii Sports      Wii           2006.0        Sports  Nintendo**   

**1         Super Mario Bros.      NES           1985.0      Platform  Nintendo**   

**2            Mario Kart Wii      Wii           2008.0        Racing  Nintendo**   

**3         Wii Sports Resort      Wii           2009.0        Sports  Nintendo**   

**4  Pokemon Red/Pokemon Blue       GB           1996.0  Role-Playing  Nintendo**   



  **developer  na\_sales  eu\_sales  jp\_sales  critic\_score  user\_score**  

**0  Nintendo     41.36     28.96      3.77          76.0         8.0**  

**1       NaN     29.08      3.58      6.81           NaN         NaN**  

**2  Nintendo     15.68     12.76      3.79          82.0         8.3**  

**3  Nintendo     15.61     10.93      3.28          80.0         8.0**  

**4       NaN     11.27      8.89     10.22           NaN         NaN**



cada condição individual é separada por parênteses, e usamos o operador **|** em vez de **or**.



condição de filtragem para aplicar a df de modo que mantenhamos apenas as linhas nas quais a pontuação de usuários (**'user\_score'**) ou a dos críticos (**'critic\_score'**) é igual ou maior que 90% e o gênero (**'genre'**) é **'Role-Playing', 'Strategy' ou 'Puzzle'**



**((df\['critic\_score'] >= 90) | (df\['user\_score'] >= 9)) \& (df\['genre'].isin(\['Role-Playing', 'Strategy', 'Puzzle']))**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Múltiplas condições com **query()**



Vamos filtrar os dados para obter apenas os jogos de Wii que não sejam de esportes, mas desta vez usando uma string de consulta:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['user\_score'] = pd.to\_numeric(df\['user\_score'], errors='coerce')**



**print(df.query("platform == 'Wii' and genre != 'Sports'").head())**



result:

&nbsp;                        **name platform  year\_of\_release     genre publisher  \\**

**2              Mario Kart Wii      Wii           2008.0    Racing  Nintendo**   

**7                    Wii Play      Wii           2006.0      Misc  Nintendo**   

**8   New Super Mario Bros. Wii      Wii           2009.0  Platform  Nintendo**   

**39    Super Smash Bros. Brawl      Wii           2008.0  Fighting  Nintendo**   

**49         Super Mario Galaxy      Wii           2007.0  Platform  Nintendo**   



    **developer  na\_sales  eu\_sales  jp\_sales  critic\_score  user\_score**  

**2    Nintendo     15.68     12.76      3.79          82.0         8.3**  

**7    Nintendo     13.96      9.18      2.93          58.0         6.6**  

**8    Nintendo     14.44      6.94      4.70          87.0         8.4**  

**39  Game Arts      6.62      2.55      2.66          93.0         8.9**  

**49   Nintendo      6.06      3.35      1.20          97.0**         



Atribua sua string de consulta à variável chamada q\_string, depois imprima as primeiras 5 linhas do resultado de **query()** em df com q\_string como parâmetro.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['user\_score'] = pd.to\_numeric(df\['user\_score'], errors='coerce')**



**q\_string = "na\_sales >= 1 or eu\_sales >= 1 or jp\_sales >= 1"**

**print(df.query(q\_string).head())**



Result:

                       **name platform  ...  critic\_score user\_score**

**0                Wii Sports      Wii  ...          76.0        8.0**

**1         Super Mario Bros.      NES  ...           NaN        NaN**

**2            Mario Kart Wii      Wii  ...          82.0        8.3**

**3         Wii Sports Resort      Wii  ...          80.0        8.0**

**4  Pokemon Red/Pokemon Blue       GB  ...           NaN        NaN**



**\[5 rows x 11 columns]**





&nbsp;A variável developers contém uma lista de empresas. Filtre o DataFrame df para incluir apenas os jogos que atendam às seguintes condições:



* O jogo é vendido nas três regiões (América do Norte, Europa e Japão).
* As vendas no Japão foram maiores que as vendas combinadas na América do Norte e na Europa.
* A empresa que desenvolveu o jogo está na lista developers.



Use a variável cols para selecionar apenas as colunas 'name', 'developer', 'na\_sales', 'eu\_sales' e 'jp\_sales' do DataFrame filtrado e atribua o resultado à variável df\_filtered



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['user\_score'] = pd.to\_numeric(df\['user\_score'], errors='coerce')**



**developers = \['SquareSoft', 'Enix Corporation', 'Square Enix']**

**cols = \['name', 'developer', 'na\_sales', 'eu\_sales', 'jp\_sales']**



**q\_string = "jp\_sales > (na\_sales + eu\_sales) and jp\_sales > 0 and na\_sales > 0 and eu\_sales > 0 and developer in @developers"**

**df\_filtered = df.query(q\_string)\[cols]**



Result:

                                                  **name  ... jp\_sales**

**175                                   Final Fantasy IX  ...     2.78**

**633                              Final Fantasy Tactics  ...     1.34**

**785                                       Parasite Eve  ...     1.05**

**1296                                         Xenogears  ...     0.89**

**1780                              Brave Fencer Musashi  ...     0.65**

**2362              Dissidia 012: Duodecim Final Fantasy  ...     0.46**

**2666                                    Unlimited Saga  ...     0.56**

**3029   Final Fantasy Crystal Chronicles: Ring of Fates  ...     0.42**

**3433                                    Romancing SaGa  ...     0.47**

**3757               Tactics Ogre: Let Us Cling Together  ...     0.27**

**4195  Final Fantasy Crystal Chronicles: Echoes of Time  ...     0.27**

**4523                                      Dawn of Mana  ...     0.29**

**5587                                  Final Fantasy XI  ...     0.15**



**\[13 rows x 5 columns]**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Substituição de valores com **where()**



Quando o processamento de dados envolve a modificação de valores de colunas, podemos usar o método where() para filtrar e modificar ao mesmo tempo, para que só modifiquemos valores com base em condições específicas.



Para entender o **where()**, vamos compará-lo com o método **replace()**



Digamos que queremos modificar todos os valores **'NES'** na coluna **'platform'** para o nome completo, **'Nintendo Entertainment System'**. Podemos fazer isso usando **replace():**

ex:


**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df\['platform'] = df\['platform'].str.replace('NES', 'Nintendo Entertainment System')**



**# imprimindo apenas as 2 primeiras colunas**

**print(df.iloc\[:, :2].head())**



result:

                       **name                       platform**

**0                Wii Sports                            Wii**

**1         Super Mario Bros.  Nintendo Entertainment System**

**2            Mario Kart Wii                            Wii**

**3         Wii Sports Resort                            Wii**

**4  Pokemon Red/Pokemon Blue                             GB**



Podemos alcançar o mesmo resultado com o método **where():**



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df\['platform'] = df\['platform'].where(df\['platform'] != 'NES', 'Nintendo Entertainment System')**

**print(df.iloc\[:, :2].head())**



result:

                       **name                       platform**

**0                Wii Sports                            Wii**

**1         Super Mario Bros.  Nintendo Entertainment System**

**2            Mario Kart Wii                            Wii**

**3         Wii Sports Resort                            Wii**

**4  Pokemon Red/Pokemon Blue                             GB**



O método **where()** verifica a condição em cada valor na coluna. Se a condição for True, **where()** não faz nada; se for False, **where()** substitui o valor atual pelo novo.

'''''''

Um total de vendas regionais igual a 0 na América do Norte e na Europa pode significar que o jogo nunca foi lançado fora do Japão, então queremos substituir esses valores de 0 pelo valor nulo None nessas colunas apenas quando ambas as colunas contiverem zeros ('na\_sales' e 'eu\_sales').



Podemos usar a condição de filtragem do quiz para fazer isso usando **where():**



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**df\[\['na\_sales', 'eu\_sales']] = df\[\['na\_sales', 'eu\_sales']].where((df\['na\_sales'] > 0) | (df\['eu\_sales'] > 0), None)**

**print(df\[\['name', 'na\_sales', 'eu\_sales']])**



result:

**name  na\_sales  eu\_sales**

**0                         Wii Sports     41.36     28.96**

**1                  Super Mario Bros.     29.08      3.58**

**2                     Mario Kart Wii     15.68     12.76**

**3                  Wii Sports Resort     15.61     10.93**

**4           Pokemon Red/Pokemon Blue     11.27      8.89**

**...                              ...       ...       ...**

**16712  Samurai Warriors: Sanada Maru       NaN       NaN**

**16713               LMA Manager 2007       NaN       NaN**

**16714        Haitaka no Psychedelica       NaN       NaN**

**16715               Spirits \& Spells       NaN       NaN**

**16716            Winning Post 8 2016       NaN       NaN**



**\[16717 rows x 3 columns]**





Agora vamos dar uma olhada em como o método where() funciona quando precisamos verificar uma inclusão





Queremos substituir essas distribuidoras pelo valor 'other' (outro). Veja como podemos fazer isso usando o método **where():**



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**rare\_publishers = \['Red Flagship', 'Max Five', '989 Sports']**

**df\['publisher'] = df\['publisher'].where(~df\['publisher'].isin(rare\_publishers), 'other')**



**print(df\[df\['publisher'] == 'other'].iloc\[:,1:5])**



result:

            **platform  year\_of\_release         genre publisher**

**5038        PS           1999.0        Sports     other**

**13365       PS           2001.0  Role-Playing     other**

**16646      PSV           2016.0        Action     other**









Para preencher os valores ausentes da coluna deck usando o método where() da biblioteca pandas, o código deve ser escrito assim:



**import pandas as pd**



**df = pd.read\_csv('/datasets/titanic.csv')**



**df\['deck'] = df\['deck'].where(df\['deck'].notna(),'Unknown')**



**print( df.head())**



Result:

   **survived  pclass     sex   age  ...     deck  embark\_town  alive  alone**

**0         0       3    male  22.0  ...  Unknown  Southampton     no  False**

**1         1       1  female  38.0  ...        C    Cherbourg    yes  False**

**2         1       3  female  26.0  ...  Unknown  Southampton    yes   True**

**3         1       1  female  35.0  ...        C  Southampton    yes  False**

**4         0       3    male  35.0  ...  Unknown  Southampton     no   True**



**\[5 rows x 15 columns]**





























