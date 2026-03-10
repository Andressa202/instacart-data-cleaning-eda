Vamos dar mais uma olhada no conjunto de dados de vendas de videogames usando **dropna()** para excluir valores ausentes mais uma vez.





**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**print(df.head())**



result:

                    \*\*name platform  year\\\_of\\\_release     genre publisher  \\\\\*\*



**0             Wii Sports      Wii           2006.0    Sports  Nintendo**

**2         Mario Kart Wii      Wii           2008.0    Racing  Nintendo**

**3      Wii Sports Resort      Wii           2009.0    Sports  Nintendo**

**6  New Super Mario Bros.       DS           2006.0  Platform  Nintendo**

**7               Wii Play      Wii           2006.0      Misc  Nintendo**



**developer  na\_sales  eu\_sales  jp\_sales  critic\_score  user\_score**

**0  Nintendo     41.36     28.96      3.77          76.0         8.0**

**2  Nintendo     15.68     12.76      3.79          82.0         8.3**

**3  Nintendo     15.61     10.93      3.28          80.0         8.0**

**6  Nintendo     11.28      9.14      6.50          89.0         8.5**

**7  Nintendo     13.96      9.18      2.93          58.0         6.6**





Digamos que queremos obter a pontuação média de críticos para cada gênero:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**mean\_score = df.groupby('genre')\['critic\_score'].mean()**

**print(mean\_score)**





result:

**genre**

**Action          67.852100**

**Adventure       66.422053**

**Fighting        69.693931**

**Misc            67.414508**

**Platform        70.000000**

**Puzzle          70.694915**

**Racing          69.621160**

**Role-Playing    72.823776**

**Shooter         70.968894**

**Simulation      69.983444**

**Sports          74.110643**

**Strategy        72.949458**

**Name: critic\_score, dtype: float64**



O índice do Series **mean\_score** é a "chave de **groupby()**", nesse caso, os valores unívocos da coluna **'genre'**. Fazer uma operação **groupby()** altera o índice de linha dos dados para as chaves que usamos para agrupamento.



Para agrupar os dados por várias colunas diferentes, passamos uma lista para o método **groupby()**:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**grp = df.groupby(\['platform', 'genre'])**

**print(grp\['critic\_score'].mean())**



result:

**platform  genre**

**3DS       Action          63.727273**

          \*\*Adventure       68.333333\*\*

&nbsp;         \\\*\\\*Fighting        68.857143\\\*\\\*

          \\\*\\\*Misc            69.100000\\\*\\\*

          \\\*\\\*Platform        72.444444\\\*\\\*

                            \\\*\\\*...\\\*\\\*    





**XOne      Role-Playing    80.777778**

          \*\*Shooter         77.903226\*\*

&nbsp;         \\\*\\\*Simulation      53.000000\\\*\\\*

          \\\*\\\*Sports          71.935484\\\*\\\*

          \\\*\\\*Strategy        70.000000\\\*\\\*





**Name: critic\_score, Length: 197, dtype: float64**



Agora temos a pontuação média de críticos para cada gênero em cada plataforma.



Aqui, a variável **grp** é um objeto que contém o DataFrame agrupado antes de processarmos cada grupo com o método **mean()**. Ele é conhecido como um objeto "em espera". Se tentarmos imprimir o objeto **grp**, uma representação textual do objeto será exibida.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**grp = df.groupby(\['platform', 'genre'])**

**print(grp)**



result:

**<pandas.core.groupby.generic.DataFrameGroupBy object at 0x0000022EA82A75B0>**



Quando imprimimos **df.groupby('column\_name')**, não vemos a impressão de uma tabela como veríamos ao imprimir **df**. Em vez disso, vemos o tipo de dados do objeto agrupado **(DataFrameGroupBy)** e uma string **(0x0000022EA82A75B0)** representando o local na memória do computador onde o objeto está armazenado. Não há resultado a ser exibido até processarmos os grupos.





#### **Split-apply-combine**





O objeto **DataFrameGroubBy** é uma parte de um framework de processamento de dados chamado **split-apply-combine** (ou seja, "divida-aplique-combine"):



* divida os dados em grupos
* aplique uma função de agregação estatística a cada grupo
* combine os resultados de cada grupo



No código abaixo, ilustramos cada um dos três componentes desse padrão:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**grp = df.groupby(\['platform', 'genre'])**

**mean\_scores = grp\['critic\_score'].mean()**

**print(mean\_scores)**



result:

**platform  genre**

**3DS       Action          63.727273**

          \*\*Adventure       68.333333\*\*

&nbsp;         \\\*\\\*Fighting        68.857143\\\*\\\*

          \\\*\\\*Misc            69.100000\\\*\\\*

          \\\*\\\*Platform        72.444444\\\*\\\*

                            \\\*\\\*...\\\*\\\*





**XOne      Role-Playing    80.777778**

          \*\*Shooter         77.903226\*\*

&nbsp;         \\\*\\\*Simulation      53.000000\\\*\\\*

          \\\*\\\*Sports          71.935484\\\*\\\*

          \\\*\\\*Strategy        70.000000\\\*\\\*





**Name: critic\_score, Length: 197, dtype: float64**



Claro, podemos pular a criação dos objetos **grp** e **mean\_scores** e deixar a pandas fazer todos os três passos em uma linha de código:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**print(df.groupby(\['platform', 'genre'])\['critic\_score'].mean())**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### O método **agg()**



Até agora, aplicamos apenas uma única função aos grupos. Mas e se quisermos calcular estatísticas de resumo diferentes para colunas diferentes? Como exemplo, a pontuação média dos críticos e o total de vendas japonesas de cada grupo. Podemos fazer isso usando o método **agg()**, que é a abreviação de "**aggregate**" (agregar).



O método **agg()** usa um dicionário como parâmetro de entrada, no qual as chaves são nomes das colunas e os valores correspondentes são as funções de agregação que você quer aplicar a elas:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**agg\_dict = {'critic\_score': 'mean', 'jp\_sales': 'sum'}**



**grp = df.groupby(\['platform', 'genre'])**

**print(grp.agg(agg\_dict))**



result:

                       \*\*critic\\\_score  jp\\\_sales\*\*



**platform genre**

**3DS      Action           63.727273      6.60**

         \*\*Adventure        68.333333      0.66\*\*

&nbsp;        \\\*\\\*Fighting         68.857143      0.46\\\*\\\*

         \\\*\\\*Misc             69.100000      1.22\\\*\\\*

         \\\*\\\*Platform         72.444444      5.94\\\*\\\*





**...                             ...       ...**

**XOne     Role-Playing     80.777778      0.01**

         \*\*Shooter          77.903226      0.13\*\*

&nbsp;        \\\*\\\*Simulation       53.000000      0.00\\\*\\\*

         \\\*\\\*Sports           71.935484      0.02\\\*\\\*

         \\\*\\\*Strategy         70.000000      0.00\\\*\\\*













**\[197 rows x 2 columns]**





Isso é incrível, não é? Dessa forma, você pode até aplicar suas próprias funções personalizadas com **agg()**. Confira:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**def double\_it(sales):**

    \*\*sales = sales.sum() \\\* 2\*\* # multiplica a soma anterior por 2

&nbsp;   \\\*\\\*return sales\\\*\\\*













**agg\_dict = {'jp\_sales': double\_it}**



**grp = df.groupby(\['platform', 'genre'])**

**print(grp.agg(agg\_dict))**



result:

                                                    \*\*jp\\\_sales\*\*



**platform genre**

**3DS      Action           13.20**

         \*\*Adventure         1.32\*\*

&nbsp;        \\\*\\\*Fighting          0.92\\\*\\\*

         \\\*\\\*Misc              2.44\\\*\\\*

         \\\*\\\*Platform         11.88\\\*\\\*





**...                        ...**

**XOne     Role-Playing      0.02**

         \*\*Shooter           0.26\*\*

&nbsp;        \\\*\\\*Simulation        0.00\\\*\\\*

         \\\*\\\*Sports            0.04\\\*\\\*

         \\\*\\\*Strategy          0.00\\\*\\\*













**\[197 rows x 1 columns]**





##### Recapitulação

Vamos lembrar de tudo o que vimos nesta lição:



* Método **groupby()**: usado para agrupar dados de acordo com uma ou mais colunas em um DataFrame
* **grp = df.groupby(\['platform', 'genre'])** agrupa por duas colunas ao mesmo tempo
* Método **agg()**: usado para computar estatísticas agregadas para cada grupo em um DataFrame agrupado. Passe um dicionário para ele com itens **'column':'function'**
* Framework de divisão-aplicação-combinação **(split-apply-combine)**: um framework de processamento de dados que descreve o processo de dividir um conjunto de dados em grupos, aplicar uma função a cada grupo e, em seguida, combinar os resultados de volta em uma única estrutura de dados
* Método **mean()**: usado para calcular a média de um conjunto de valores
* Método **sum()**: usado para calcular a soma de um conjunto de valores
* Método **dropna()**: usado para remover valores ausentes **(NaN)** de um DataFrame
* Objeto **DataFrameGroupBy**: possui o DataFrame agrupado antes de ser processado com uma função de agregação





E agora você vai:



* Criar um dicionário para calcular para cada gênero:
* Soma das vendas totais
* Média de vendas na América do Norte (NA)
* Média de vendas na Europa (EU)
* Média de vendas no Japão (JP)
* Atribuir o dicionário a uma variável chamada agg\_dict com as tuplas descritas acima.
* Atribuir o resultado de agg() a uma variável chamada genre.
* Imprimir genre.





**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**



**grp = df.groupby('genre')**



**agg\_dict = {'total\_sales': 'sum', 'na\_sales': 'mean', 'eu\_sales':'mean', 'jp\_sales':'mean'}**



**genre = grp.agg(agg\_dict)**



**print(genre)**



result:



              \*\*total\\\_sales  na\\\_sales  eu\\\_sales  jp\\\_sales\*\*



**genre**

**Action            1559.58  0.260834  0.154045  0.047905**

**Adventure          221.10  0.080783  0.048764  0.040138**

**Fighting           411.17  0.263086  0.118174  0.103039**

**Misc               728.12  0.232726  0.121566  0.061777**

**Platform           776.68  0.501689  0.225619  0.147331**

**Puzzle             230.19  0.211845  0.086224  0.098810**

**Racing             652.57  0.287710  0.189359  0.045404**

**Role-Playing       874.98  0.220540  0.125807  0.236973**

**Shooter            948.34  0.447649  0.239864  0.029297**

**Simulation         359.51  0.208455  0.129886  0.072998**

**Sports            1196.76  0.291495  0.160473  0.057726**

**Strategy           163.38  0.100366  0.066135  0.072709**



Suponha que queremos determinar o total de vendas europeias para cada gênero em cada plataforma. As tabelas dinâmicas fornecem um método rápido e conveniente para isso. Primeiro, vamos examinar o código e depois analisá-lo. Para ficar mais simples, removeremos as linhas com valores ausentes.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**pivot\_data = df.pivot\_table(index='genre',**

                            \*\*columns='platform',\*\*

&nbsp;                           \\\*\\\*values='eu\\\\\\\_sales',\\\*\\\*

                            \\\*\\\*aggfunc='sum'\\\*\\\*

                           \\\*\\\*)\\\*\\\*





**print(pivot\_data)**

**print()**

**print(type(pivot\_data))**



result:



**DS    GBA    GC     PC     PS    PS2    PS3  \\**

**genre**

**Action        8.41   NaN  11.25   5.95  6.11  15.32  21.15  63.97  94.17**

**Adventure     0.46  0.24   1.17   1.07  1.01   1.58   0.32   3.27   6.34**

**Fighting      0.84  0.00   0.28   0.85  2.73   0.11   5.63  15.46  13.69**

**Misc          1.21   NaN  25.10   2.19  1.90   1.19   1.69   9.73   7.62**

**Platform      8.41  0.00  13.00  10.62  5.19   0.33   6.09  17.11   7.26**

**Puzzle        1.02   NaN  15.72   1.34  0.74   0.13   0.10   1.11   0.04**

**Racing        4.45  0.00   8.43   3.43  2.13   2.59  13.03  36.59  28.09**

**Role-Playing  3.10  0.00   6.00   3.96  2.36  24.32   8.70  16.33  16.81**

**Shooter       0.27  0.00   0.43   0.38  2.79  18.28   2.31  31.38  65.43**

**Simulation    4.40  0.00  12.21   0.56  1.74  22.35   0.44  10.08   2.71**

**Sports        0.56  0.05   0.94   1.52  3.75   6.47   5.66  47.85  30.62**

**Strategy      0.20   NaN   1.29   0.73  0.48  14.81   0.10   2.26   1.04**



**platform        PS4    PSP   PSV    Wii  WiiU   X360     XB   XOne**

**genre**

**Action        35.66  13.08  2.79  21.87  4.81  64.30   9.82  10.11**

**Adventure      1.55   0.85  0.56   2.43  0.05   3.50   0.46   0.76**

**Fighting       2.40   2.10  0.44   4.86  0.34   8.17   2.13   0.48**

**Misc           1.15   0.83  0.31  47.91  3.39  15.13   0.80   0.99**

**Platform       2.84   3.02  1.22  21.13  6.27   3.58   2.21   0.28**

**Puzzle         0.02   0.76  0.03   2.61  0.38   0.02   0.02    NaN**

**Racing         5.49   9.30  0.44  17.47  2.15  21.14   6.45   4.11**

**Role-Playing   8.01   3.47  1.06   1.88  0.42  17.32   2.83   2.75**

**Shooter       28.83   5.12  1.49   4.49  1.57  70.02  14.56  14.03**

**Simulation     0.47   1.79   NaN   6.21  0.04   3.56   1.31   0.01**

**Sports        25.81   7.46  0.43  73.08  0.68  25.74   9.23   8.65**

**Strategy       0.21   1.35   NaN   0.20  0.32   2.04   0.42   0.06**



**<class 'pandas.core.frame.DataFrame'>**



Criamos uma tabela dinâmica usando o método **pivot\_table()**. Os seguintes parâmetros foram usados:



* **index=** — a coluna cujos valores se tornam o índice na tabela dinâmica
* **columns=** — a coluna cujos valores se tornam colunas na tabela dinâmica
* **values=** — a coluna cujos valores queremos agregar na tabela dinâmica
* **aggfunc=** — a função de agregação que queremos aplicar a valores em cada grupo linha-coluna





Usar uma tabela dinâmica é conveniente aqui porque nos permite excluir facilmente todas as colunas do df que não interessam para a análise. Pode ser mais fácil também para as pessoas lerem do que o resultado equivalente do uso de groupby(), como você pode ver abaixo.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df.dropna(inplace=True)**



**groupby\_data = df.groupby(\['genre', 'platform'])\['eu\_sales'].mean()**

**print(groupby\_data)**

**print()**

**print(type(groupby\_data))**



result:

**genre         platform**

**Action        3DS         0.152909**

              \*\*DS          0.150000\*\*

&nbsp;             \\\*\\\*GBA         0.116667\\\*\\\*

              \\\*\\\*GC          0.088551\\\*\\\*

              \\\*\\\*PC          0.115188\\\*\\\*

              \\\*\\\*PS          0.783333\\\*\\\*

              \\\*\\\*PS2         0.263251\\\*\\\*

              \\\*\\\*PS3         0.409435\\\*\\\*

              \\\*\\\*PS4         0.424524\\\*\\\*

              \\\*\\\*PSP         0.176757\\\*\\\*

              \\\*\\\*PSV         0.075405\\\*\\\*

              \\\*\\\*Wii         0.179262\\\*\\\*

              \\\*\\\*WiiU        0.123333\\\*\\\*

              \\\*\\\*X360        0.274786\\\*\\\*

              \\\*\\\*XB          0.082521\\\*\\\*

              \\\*\\\*XOne        0.198235\\\*\\\*





**Adventure     3DS         0.051111**

              \*\*DC          0.120000\*\*

&nbsp;             \\\*\\\*DS          0.037742\\\*\\\*

              \\\*\\\*GBA         0.118889\\\*\\\*

              \\\*\\\*GC          0.077692\\\*\\\*

              \\\*\\\*PC          0.043889\\\*\\\*

              \\\*\\\*PS          0.064000\\\*\\\*

              \\\*\\\*PS2         0.081750\\\*\\\*

              \\\*\\\*PS3         0.317000\\\*\\\*

              \\\*\\\*PS4         0.110714\\\*\\\*

              \\\*\\\*PSP         0.065385\\\*\\\*

              \\\*\\\*PSV         0.056000\\\*\\\*

              \\\*\\\*Wii         0.110455\\\*\\\*

              \\\*\\\*WiiU        0.050000\\\*\\\*

              \\\*\\\*X360        0.233333\\\*\\\*

              \\\*\\\*XB          0.025556\\\*\\\*

              \\\*\\\*XOne        0.152000\\\*\\\*





**Fighting      3DS         0.120000**

              \*\*DC          0.000000\*\*

&nbsp;             \\\*\\\*DS          0.020000\\\*\\\*

              \\\*\\\*GBA         0.065385\\\*\\\*

              \\\*\\\*GC          0.113750\\\*\\\*

              \\\*\\\*PC          0.036667\\\*\\\*

              \\\*\\\*PS          0.375333\\\*\\\*

              \\\*\\\*PS2         0.211781\\\*\\\*

              \\\*\\\*PS3         0.240175\\\*\\\*

              \\\*\\\*PS4         0.218182\\\*\\\*

              \\\*\\\*PSP         0.070000\\\*\\\*

              \\\*\\\*PSV         0.048889\\\*\\\*

              \\\*\\\*Wii         0.186923\\\*\\\*

              \\\*\\\*WiiU        0.113333\\\*\\\*

              \\\*\\\*X360        0.145893\\\*\\\*

              \\\*\\\*XB          0.068710\\\*\\\*

              \\\*\\\*XOne        0.096000\\\*\\\*





**Misc          3DS         0.121000**

              \*\*DS          0.660526\*\*

&nbsp;             \\\*\\\*GBA         0.182500\\\*\\\*

              \\\*\\\*GC          0.118750\\\*\\\*

              \\\*\\\*PC          0.396667\\\*\\\*

              \\\*\\\*PS          0.422500\\\*\\\*

              \\\*\\\*PS2         0.180185\\\*\\\*

              \\\*\\\*PS3         0.185854\\\*\\\*

              \\\*\\\*PS4         0.143750\\\*\\\*

              \\\*\\\*PSP         0.039524\\\*\\\*

              \\\*\\\*PSV         0.038750\\\*\\\*

              \\\*\\\*Wii         0.684429\\\*\\\*

              \\\*\\\*WiiU        0.282500\\\*\\\*

              \\\*\\\*X360        0.244032\\\*\\\*

              \\\*\\\*XB          0.053333\\\*\\\*

              \\\*\\\*XOne        0.082500\\\*\\\*





**Platform      3DS         0.467222**

              \*\*DC          0.000000\*\*

&nbsp;             \\\*\\\*DS          0.371429\\\*\\\*

              \\\*\\\*GBA         0.221250\\\*\\\*

              \\\*\\\*GC          0.117955\\\*\\\*

              \\\*\\\*PC          0.033000\\\*\\\*

              \\\*\\\*PS          0.380625\\\*\\\*

              \\\*\\\*PS2         0.244429\\\*\\\*

              \\\*\\\*PS3         0.330000\\\*\\\*

              \\\*\\\*PS4         0.258182\\\*\\\*

              \\\*\\\*PSP         0.151000\\\*\\\*

              \\\*\\\*PSV         0.203333\\\*\\\*

              \\\*\\\*Wii         0.586944\\\*\\\*

              \\\*\\\*WiiU        0.418000\\\*\\\*

              \\\*\\\*X360        0.170476\\\*\\\*

              \\\*\\\*XB          0.085000\\\*\\\*

              \\\*\\\*XOne        0.070000\\\*\\\*





**Puzzle        3DS         0.127500**

              \*\*DS          0.302308\*\*

&nbsp;             \\\*\\\*GBA         0.223333\\\*\\\*

              \\\*\\\*GC          0.123333\\\*\\\*

              \\\*\\\*PC          0.032500\\\*\\\*

              \\\*\\\*PS          0.050000\\\*\\\*

              \\\*\\\*PS2         0.185000\\\*\\\*

              \\\*\\\*PS3         0.040000\\\*\\\*

              \\\*\\\*PS4         0.020000\\\*\\\*

              \\\*\\\*PSP         0.069091\\\*\\\*

              \\\*\\\*PSV         0.030000\\\*\\\*

              \\\*\\\*Wii         0.186429\\\*\\\*

              \\\*\\\*WiiU        0.126667\\\*\\\*

              \\\*\\\*X360        0.010000\\\*\\\*

              \\\*\\\*XB          0.020000\\\*\\\*





**Racing        3DS         0.890000**

              \*\*DC          0.000000\*\*

&nbsp;             \\\*\\\*DS          0.468333\\\*\\\*

              \\\*\\\*GBA         0.163333\\\*\\\*

              \\\*\\\*GC          0.068710\\\*\\\*

              \\\*\\\*PC          0.057556\\\*\\\*

              \\\*\\\*PS          0.685789\\\*\\\*

              \\\*\\\*PS2         0.279313\\\*\\\*

              \\\*\\\*PS3         0.445873\\\*\\\*

              \\\*\\\*PS4         0.366000\\\*\\\*

              \\\*\\\*PSP         0.238462\\\*\\\*

              \\\*\\\*PSV         0.088000\\\*\\\*

              \\\*\\\*Wii         0.582333\\\*\\\*

              \\\*\\\*WiiU        2.150000\\\*\\\*

              \\\*\\\*X360        0.297746\\\*\\\*

              \\\*\\\*XB          0.084868\\\*\\\*

              \\\*\\\*XOne        0.293571\\\*\\\*





**Role-Playing  3DS         0.119231**

              \*\*DC          0.000000\*\*

&nbsp;             \\\*\\\*DS          0.067416\\\*\\\*

              \\\*\\\*GBA         0.127742\\\*\\\*

              \\\*\\\*GC          0.107273\\\*\\\*

              \\\*\\\*PC          0.293012\\\*\\\*

              \\\*\\\*PS          0.310714\\\*\\\*

              \\\*\\\*PS2         0.139573\\\*\\\*

              \\\*\\\*PS3         0.215513\\\*\\\*

              \\\*\\\*PS4         0.333750\\\*\\\*

              \\\*\\\*PSP         0.049571\\\*\\\*

              \\\*\\\*PSV         0.034194\\\*\\\*

              \\\*\\\*Wii         0.072308\\\*\\\*

              \\\*\\\*WiiU        0.210000\\\*\\\*

              \\\*\\\*X360        0.298621\\\*\\\*

              \\\*\\\*XB          0.166471\\\*\\\*

              \\\*\\\*XOne        0.305556\\\*\\\*





**Shooter       3DS         0.135000**

              \*\*DC          0.000000\*\*

&nbsp;             \\\*\\\*DS          0.015926\\\*\\\*

              \\\*\\\*GBA         0.029231\\\*\\\*

              \\\*\\\*GC          0.069750\\\*\\\*

              \\\*\\\*PC          0.140615\\\*\\\*

              \\\*\\\*PS          0.192500\\\*\\\*

              \\\*\\\*PS2         0.247087\\\*\\\*

              \\\*\\\*PS3         0.540744\\\*\\\*

              \\\*\\\*PS4         0.847941\\\*\\\*

              \\\*\\\*PSP         0.170667\\\*\\\*

              \\\*\\\*PSV         0.298000\\\*\\\*

              \\\*\\\*Wii         0.154828\\\*\\\*

              \\\*\\\*WiiU        0.314000\\\*\\\*

              \\\*\\\*X360        0.454675\\\*\\\*

              \\\*\\\*XB          0.136075\\\*\\\*

              \\\*\\\*XOne        0.452581\\\*\\\*





**Simulation    3DS         0.488889**

              \*\*DC          0.000000\*\*

&nbsp;             \\\*\\\*DS          0.330000\\\*\\\*

              \\\*\\\*GBA         0.070000\\\*\\\*

              \\\*\\\*GC          0.174000\\\*\\\*

              \\\*\\\*PC          0.272561\\\*\\\*

              \\\*\\\*PS          0.088000\\\*\\\*

              \\\*\\\*PS2         0.224000\\\*\\\*

              \\\*\\\*PS3         0.150556\\\*\\\*

              \\\*\\\*PS4         0.235000\\\*\\\*

              \\\*\\\*PSP         0.223750\\\*\\\*

              \\\*\\\*Wii         0.238846\\\*\\\*

              \\\*\\\*WiiU        0.040000\\\*\\\*

              \\\*\\\*X360        0.127143\\\*\\\*

              \\\*\\\*XB          0.062381\\\*\\\*

              \\\*\\\*XOne        0.010000\\\*\\\*





**Sports        3DS         0.140000**

              \*\*DC          0.050000\*\*

&nbsp;             \\\*\\\*DS          0.042727\\\*\\\*

              \\\*\\\*GBA         0.080000\\\*\\\*

              \\\*\\\*GC          0.056818\\\*\\\*

              \\\*\\\*PC          0.202187\\\*\\\*

              \\\*\\\*PS          0.353750\\\*\\\*

              \\\*\\\*PS2         0.244133\\\*\\\*

              \\\*\\\*PS3         0.266261\\\*\\\*

              \\\*\\\*PS4         0.629512\\\*\\\*

              \\\*\\\*PSP         0.124333\\\*\\\*

              \\\*\\\*PSV         0.071667\\\*\\\*

              \\\*\\\*Wii         1.001096\\\*\\\*

              \\\*\\\*WiiU        0.113333\\\*\\\*

              \\\*\\\*X360        0.183857\\\*\\\*

              \\\*\\\*XB          0.076281\\\*\\\*

              \\\*\\\*XOne        0.279032\\\*\\\*





**Strategy      3DS         0.066667**

              \*\*DS          0.047778\*\*

&nbsp;             \\\*\\\*GBA         0.121667\\\*\\\*

              \\\*\\\*GC          0.068571\\\*\\\*

              \\\*\\\*PC          0.118480\\\*\\\*

              \\\*\\\*PS          0.025000\\\*\\\*

              \\\*\\\*PS2         0.059474\\\*\\\*

              \\\*\\\*PS3         0.130000\\\*\\\*

              \\\*\\\*PS4         0.052500\\\*\\\*

              \\\*\\\*PSP         0.096429\\\*\\\*

              \\\*\\\*Wii         0.033333\\\*\\\*

              \\\*\\\*WiiU        0.320000\\\*\\\*

              \\\*\\\*X360        0.107368\\\*\\\*

              \\\*\\\*XB          0.032308\\\*\\\*

              \\\*\\\*XOne        0.030000\\\*\\\*





**Name: eu\_sales, dtype: float64**



**<class 'pandas.core.series.Series'>**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



Filtramos o conjunto de dados de videogames de modo que ele contenha apenas os jogos lançados a partir de 2000. Crie uma tabela dinâmica a partir do conjunto de dados filtrado que possui o valor médio das vendas no Japão para cada combinação de gênero e ano de lançamento.



* Os gêneros são o índice
* As colunas da tabela dinâmica são os anos de lançamento
* Use a coluna apropriada como os valores a serem agregados
* Use a função de agregação apropriada
* Atribua o resultado à variável chamada df\_pivot e depois a imprima.



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df = df\[df\['year\_of\_release'] >= 2000]**



**df\_pivot = pd.pivot\_table(df,**

                          \*\*index='genre',\*\*

&nbsp;                        \\\*\\\*columns='year\\\\\\\_of\\\\\\\_release',\\\*\\\*

                         \\\*\\\*values='jp\\\\\\\_sales',\\\*\\\*

                         \\\*\\\*aggfunc='mean'\\\*\\\*

                         \\\*\\\*)\\\*\\\*













**print(df\_pivot)**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



Queremos saber algumas estatísticas gerais sobre distribuidoras de jogos:



* A pontuação média dada a elas por críticos
* O total de vendas



Como você já viu, podemos fazer isso usando **groupby()**. Primeiro, vamos obter a pontuação média de avaliações de cada distribuidora:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**mean\_score = df.groupby('publisher')\['critic\_score'].mean()**

**print(mean\_score)**



result:

**publisher**

**10TACLE Studios                 42.000000**

**1C Company                      73.000000**

**20th Century Fox Video Games          NaN**

**2D Boy                          90.000000**

**3DO                             57.470588**

                                  \*\*...\*\*



**id Software                     85.000000**

**imageepoch Inc.                       NaN**

**inXile Entertainment            81.000000**

**mixi, Inc                             NaN**

**responDESIGN                          NaN**

**Name: critic\_score, Length: 581, dtype: float64**





Vamos também obter o número de vendas. A maneira mais simples de fazer isso é usar mais um **groupby():**





**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**

**num\_sales = df.groupby('publisher')\['total\_sales'].sum()**

**print(num\_sales)**



result:



**publisher**

**10TACLE Studios                 0.11**

**1C Company                      0.08**

**20th Century Fox Video Games    1.92**

**2D Boy                          0.03**

**3DO                             9.52**

                                \*\*...\*\*



**id Software                     0.02**

**imageepoch Inc.                 0.04**

**inXile Entertainment            0.09**

**mixi, Inc                       0.87**

**responDESIGN                    0.13**

**Name: total\_sales, Length: 581, dtype: float64**



Já que ambos os resultados têm o mesmo índice, podemos juntar os resultados em um DataFrame com facilidade usando a função **concat()** da pandas:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**mean\_score = df.groupby('publisher')\['critic\_score'].mean()**



**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**

**num\_sales = df.groupby('publisher')\['total\_sales'].sum()**



**df\_concat = pd.concat(\[mean\_score, num\_sales], axis='columns')**

**print(df\_concat)**



result:

                              \*\*critic\\\_score  total\\\_sales\*\*



**publisher**

**10TACLE Studios                  42.000000         0.11**

**1C Company                       73.000000         0.08**

**20th Century Fox Video Games           NaN         1.92**

**2D Boy                           90.000000         0.03**

**3DO                              57.470588         9.52**

**...                                    ...          ...**

**id Software                      85.000000         0.02**

**imageepoch Inc.                        NaN         0.04**

**inXile Entertainment             81.000000         0.09**

**mixi, Inc                              NaN         0.87**

**responDESIGN                           NaN         0.13**



**\[581 rows x 2 columns]**



Em geral, **concat()** espera uma lista de objetos Serie e/ou DataFrame. Para obter esse resultado, passamos uma lista de variáveis Series para **concat()** e definimos **axis='columns'** para garantir que elas foram combinadas como colunas.



Podemos renomear colunas com o método columns. Ele pode ser chamado em um DataFrame e passar uma lista de novos nomes de coluna para substituir os existentes. Os novos nomes devem ser passados na mesma ordem que os nomes das colunas originais.



Vamos renomear a coluna **'critic\_score'** já que agora ela representa uma média:



import pandas as pd



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**mean\_score = df.groupby('publisher')\['critic\_score'].mean()**



**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**

**num\_sales = df.groupby('publisher')\['total\_sales'].sum()**



**df\_concat = pd.concat(\[mean\_score, num\_sales], axis='columns')**

**df\_concat.columns = \['avg\_critic\_score', 'total\_sales']**

**print(df\_concat)**



result:

                              \*\*avg\\\_critic\\\_score  total\\\_sales\*\*



**publisher**

**10TACLE Studios                      42.000000         0.11**

**1C Company                           73.000000         0.08**

**20th Century Fox Video Games               NaN         1.92**

**2D Boy                               90.000000         0.03**

**3DO                                  57.470588         9.52**

**...                                        ...          ...**

**id Software                          85.000000         0.02**

**imageepoch Inc.                            NaN         0.04**

**inXile Entertainment                 81.000000         0.09**

**mixi, Inc                                  NaN         0.87**

**responDESIGN                               NaN         0.13**



**\[581 rows x 2 columns]**



Você deve ter notado que podemos obter o mesmo resultado acima usando **agg()**. No entanto, **concat()** é bastante versátil. Podemos usá-lo para concatenar DataFrames juntos:



* Por linhas, assumindo que eles possuem o mesmo número de colunas
* Por colunas, se eles tiverem o mesmo número de linhas



Para concatenar linhas de DataFrames diferentes, podemos usar **concat()** e definir **axis='index'** (ou excluir o parâmetro, já que **axis='index'** é o argumento padrão). Como alternativa, podemos usar números inteiros para o argumento **index=**, onde **index=0** concatena em linha e **index=1**, em coluna.



Aqui está um exemplo onde filtramos os dados para obter dois DataFrames diferentes de acordo com gênero e depois os recombinamos em um DataFrame:



**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**



**rpgs = df\[df\['genre'] == 'Role-Playing']**

**platformers = df\[df\['genre'] == 'Platform']**



**df\_concat = pd.concat(\[rpgs, platformers])**

**print(df\_concat\[\['name', 'genre']])**



result:

                                                   \*\*name         genre\*\*



**4                              Pokemon Red/Pokemon Blue  Role-Playing**

**12                          Pokemon Gold/Pokemon Silver  Role-Playing**

**20                        Pokemon Diamond/Pokemon Pearl  Role-Playing**

**25                        Pokemon Ruby/Pokemon Sapphire  Role-Playing**

**27                          Pokemon Black/Pokemon White  Role-Playing**

**...                                                 ...           ...**

**16356                                    Strider (2014)      Platform**

**16358                                Goku Makaimura Kai      Platform**

**16603  The Land Before Time: Into the Mysterious Beyond      Platform**

**16710                Woody Woodpecker in Crazy Castle 5      Platform**

**16715                                  Spirits \& Spells      Platform**





Pronto, os dois DataFrames foram costurados em um! Lembre-se de que isso funciona aqui porque os dois DataFrames pequenos têm as mesmas colunas.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



Sua tarefa é calcular o número total de distribuidoras que criaram um jogo em cada plataforma usando **nunique()**. Atribua o resultado à variável chamada num\_pubs, depois a imprima.





**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**



**total\_sales = df.groupby('platform')\['total\_sales'].sum()**



**num\_pubs = df.groupby('platform')\['publisher'].nunique()**



**print(num\_pubs)**





Combine **total\_sales** e **num\_pubs** por colunas em um DataFrame chamado platforms usando **concat()**. Altere os nomes das colunas em platforms para **'total\_sales'** e **'num\_publishers'**, respectivamente, e imprima platforms.





**import pandas as pd**



**df = pd.read\_csv('/datasets/vg\_sales.csv')**

**df\['total\_sales'] = df\['na\_sales'] + df\['eu\_sales'] + df\['jp\_sales']**



**total\_sales = df.groupby('platform')\['total\_sales'].sum()**

**num\_pubs = df.groupby('platform')\['publisher'].nunique()**



**platforms = pd.concat(\[total\_sales, num\_pubs], axis='columns')**



**platforms.columns = \['total\_sales', 'num\_publishers']**

**print(platforms)**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



Nesta lição, você vai aprender como combinar DataFrames usando o método **merge()** de forma que afete a quantidade de dados com os quais você está trabalhando.



Considere o seguinte exemplo: dois estudantes de literatura concordam que um deles vai escrever metade da lista de leitura do quadro enquanto o outro assiste ao YouTube. Depois, o primeiro vai até a lanchonete da escola enquanto o segundo copia o resto da lista. Por fim, eles vão juntar as listas. Trabalho em equipe! Vamos ver como ficou:



**import pandas as pd**



**first\_pupil\_df = pd.DataFrame(**

    \*\*{\*\*

&nbsp;       \\\*\\\*'author': \\\\\\\['Alcott', 'Fitzgerald', 'Steinbeck', 'Twain', 'Hemingway'],\\\*\\\*

        \\\*\\\*'title': \\\\\\\['Little Women',\\\*\\\*

                  \\\*\\\*'The Great Gatsby',\\\*\\\*

                  \\\*\\\*'Of Mice and Men',\\\*\\\*

                  \\\*\\\*'The Adventures of Tom Sawyer',\\\*\\\*

                  \\\*\\\*'The Old Man and the Sea'\\\*\\\*

                 \\\*\\\*],\\\*\\\*

    \\\*\\\*}\\\*\\\*





**)**

**second\_pupil\_df = pd.DataFrame(**

    \*\*{\*\*

&nbsp;       \\\*\\\*'author': \\\\\\\['Steinbeck', 'Twain', 'Hemingway', 'Salinger', 'Hawthorne'],\\\*\\\*

        \\\*\\\*'title': \\\\\\\['East of Eden',\\\*\\\*

                  \\\*\\\*'The Adventures of Huckleberry Finn',\\\*\\\*

                  \\\*\\\*'For Whom the Bell Tolls',\\\*\\\*

                  \\\*\\\*'The Catcher in the Rye',\\\*\\\*

                  \\\*\\\*'The Scarlett Letter'\\\*\\\*

                 \\\*\\\*],\\\*\\\*

    \\\*\\\*}\\\*\\\*





**)**



**print(first\_pupil\_df)**

**print()**

**print(second\_pupil\_df)**





result:



       \*\*author                         title\*\*



**0      Alcott                  Little Women**

**1  Fitzgerald              The Great Gatsby**

**2   Steinbeck               Of Mice and Men**

**3       Twain  The Adventures of Tom Sawyer**

**4   Hemingway       The Old Man and the Sea**



      \*\*author                               title\*\*



**0  Steinbeck                        East of Eden**

**1      Twain  The Adventures of Huckleberry Finn**

**2  Hemingway             For Whom the Bell Tolls**

**3   Salinger              The Catcher in the Rye**

**4  Hawthorne                 The Scarlett Letter**



#### Junção interna **(inner merge)**



Vamos usar o método **merge()** para combinar entradas com os mesmos autores. O nome da coluna na qual vai ocorrer a junção é passado para o parâmetro **on=**, nesse caso o **'author'**:



**both\_pupils = first\_pupil\_df.merge(second\_pupil\_df, on='author')**

**print(both\_pupils)**



result:



      \*\*author                       title\\\_x                             title\\\_y\*\*



**0  Steinbeck               Of Mice and Men                        East of Eden**

**1      Twain  The Adventures of Tom Sawyer  The Adventures of Huckleberry Finn**

**2  Hemingway       The Old Man and the Sea             For Whom the Bell Tolls**



Como os dois DataFrames originais possuem uma coluna chamada **'title'**, a pandas adicionou os sufixos **\_x** e **\_y** para diferenciá-las no DataFrame unido. Observe que o DataFrame unido tem apenas 9 células, em comparação com as 20 células dos DataFrames originais: a quantidade de dados mudou.



Esse modo de junção é chamado de junção interna. Há outros tipos de junção que podem ser especificados usando o parâmetro **how=** em **merge()**. **'inner'** é o argumento padrão para **how=**, então não precisamos incluí-lo acima.



#### Junção externa **(outer merge)**



A diferença entre junção externa e a interna é que, no caso da externa, todos os valores na coluna especificada são mantidos a partir de ambos os DataFrames originais, mas o novo DataFrame tem valores ausentes nos lugares onde não há correspondências. É melhor ilustrar isso com um exemplo:



**both\_pupils = first\_pupil\_df.merge(second\_pupil\_df, on='author', how='outer')**

**print(both\_pupils)**



result:

       \*\*author                       title\\\_x  \\\\\*\*



**0      Alcott                  Little Women**

**1  Fitzgerald              The Great Gatsby**

**2   Steinbeck               Of Mice and Men**

**3       Twain  The Adventures of Tom Sawyer**

**4   Hemingway       The Old Man and the Sea**

**5    Salinger                           NaN**

**6   Hawthorne                           NaN**



                              \*\*title\\\_y\*\*



**0                                 NaN**

**1                                 NaN**

**2                        East of Eden**

**3  The Adventures of Huckleberry Finn**

**4             For Whom the Bell Tolls**

**5              The Catcher in the Rye**

**6                 The Scarlett Letter**





Há 7 autores únicos em ambos os DataFrames originais; cada um deles é representado por uma linha no DataFrame unido. Para os autores no primeiro DataFrame que não se encontram no segundo (no caso, **'Alcott'** e **'Fitzgerald'**), há valores **NaN** na coluna que veio do segundo DataFrame ( **'title\_y'**) e vice-versa. Observe também que agora temos 21 células de dados.



#### Junção à esquerda **(left merge)**





Podemos fazer passando **how='left'** para **merge()**. Em uma junção à esquerda, todos os valores do DataFrame esquerdo (aquele no qual nós chamamos merge()) estão presentes no DataFrame unido. Os valores do DataFrame direito (aquele que passamos como entrada para merge()) só são mantidos se tiverem correspondência aos valores da coluna especificada no DataFrame esquerdo. exemplo:





**both\_pupils = first\_pupil\_df.merge(second\_pupil\_df, on='author', how='left')**

**print(both\_pupils)**



result:

       \*\*author                       title\\\_x  \\\\\*\*



**0      Alcott                  Little Women**

**1  Fitzgerald              The Great Gatsby**

**2   Steinbeck               Of Mice and Men**

**3       Twain  The Adventures of Tom Sawyer**

**4   Hemingway       The Old Man and the Sea**



                              \*\*title\\\_y\*\*



**0                                 NaN**

**1                                 NaN**

**2                        East of Eden**

**3  The Adventures of Huckleberry Finn**

**4             For Whom the Bell Tolls**





Como você pode ver, todos os autores e títulos do primeiro estudante estão presentes no DataFrame unido, enquanto as linhas com **'Salinger'** e **'Hawthorne'** do segundo estudante não se encontram aqui, porque esses autores não estavam no DataFrame do primeiro estudante.



Observe que existe também uma junção à direita **(how='right')**. No entanto, ela funciona de forma idêntica a uma junção à esquerda, exceto que o DataFrame unido mantém todos os valores do DataFrame direito em vez do esquerdo. O mesmo resultado pode ser obtido executando uma junção à esquerda e alternando a ordem dos DataFrames.

#### 

#### Consideração sobre os nomes das colunas



Há dois aspectos sobre todas as junções que fizemos até agora que precisamos abordar:



* Os sufixos **\_x** e **\_y** foram adicionados aos nomes das colunas **'title'** no DataFrame unido
* A coluna na qual fizemos a junção tinha o mesmo nome em ambos os DataFrames — **'author'**



Ao unir DataFrames na pandas, é importante garantir que as colunas não tenham os mesmos nomes. Se for o caso, a pandas vai adicionar automaticamente os sufixos **\_x** e **\_y**. No entanto, esses sufixos não são muito descritivos. Para definir sufixos melhores, passe uma lista de strings de sufixos para o parâmetro **suffixes=** em **merge()**:





**both\_pupils = first\_pupil\_df.merge(second\_pupil\_df,**

                                   \*\*on='author',\*\*

&nbsp;                                  \\\*\\\*suffixes=\\\\\\\['\\\\\\\_1st\\\\\\\_student', '\\\\\\\_2nd\\\\\\\_student']\\\*\\\*

                                  \\\*\\\*)\\\*\\\*





**print(both\_pupils)**



result:

      \*\*author             title\\\_1st\\\_student                   title\\\_2nd\\\_student\*\*



**0  Steinbeck               Of Mice and Men                        East of Eden**

**1      Twain  The Adventures of Tom Sawyer  The Adventures of Huckleberry Finn**

**2  Hemingway       The Old Man and the Sea             For Whom the Bell Tolls**





nem sempre é o caso que as colunas em que você quer fazer junção têm o mesmo nome. Você poderia mudar os nomes para serem iguais antes de junção, mas isso pode causar confusão.



Em vez disso, a função **merge()** tem os parâmetros **left\_on=** e **right\_on=**, que você pode usar em vez de **on=** se as colunas tiverem nomes diferentes. Para ilustrar como isso funciona, vamos recriar os DataFrames para que um tenha uma coluna **'authors'** e o outro tenha uma coluna **'author'**:



**import pandas as pd**



**first\_pupil\_df = pd.DataFrame(**

    \*\*{\*\*

&nbsp;       \\\*\\\*'authors': \\\\\\\['Alcott', 'Fitzgerald', 'Steinbeck', 'Twain', 'Hemingway'],\\\*\\\*

        \\\*\\\*'title': \\\\\\\['Little Women',\\\*\\\*

                  \\\*\\\*'The Great Gatsby',\\\*\\\*

                  \\\*\\\*'Of Mice and Men',\\\*\\\*

                  \\\*\\\*'The Adventures of Tom Sawyer',\\\*\\\*

                  \\\*\\\*'The Old Man and the Sea'\\\*\\\*

                 \\\*\\\*],\\\*\\\*

    \\\*\\\*}\\\*\\\*





**)**

**second\_pupil\_df = pd.DataFrame(**

    \*\*{\*\*

&nbsp;       \\\*\\\*'author': \\\\\\\['Steinbeck', 'Twain', 'Hemingway', 'Salinger', 'Hawthorne'],\\\*\\\*

        \\\*\\\*'title': \\\\\\\['East of Eden',\\\*\\\*

                  \\\*\\\*'The Adventures of Huckleberry Finn',\\\*\\\*

                  \\\*\\\*'For Whom the Bell Tolls',\\\*\\\*

                  \\\*\\\*'The Catcher in the Rye',\\\*\\\*

                  \\\*\\\*'The Scarlett Letter'\\\*\\\*

                 \\\*\\\*],\\\*\\\*

    \\\*\\\*}\\\*\\\*





**)**



**both\_pupils = first\_pupil\_df.merge(second\_pupil\_df,**

                                   \*\*left\\\_on='authors',\*\*

&nbsp;                                  \\\*\\\*right\\\\\\\_on='author'\\\*\\\*

                                  \\\*\\\*)\\\*\\\*





**print(both\_pupils)**



result:

     \*\*authors                       title\\\_x     author  \\\\\*\*



**0  Steinbeck               Of Mice and Men  Steinbeck**

**1      Twain  The Adventures of Tom Sawyer      Twain**

**2  Hemingway       The Old Man and the Sea  Hemingway**



                              \*\*title\\\_y\*\*



**0                        East of Eden**

**1  The Adventures of Huckleberry Finn**

**2             For Whom the Bell Tolls**





Dessa vez, a pandas vai resultar em **'authors'** e **'author'**, mas eles estarão alinhados e bem posicionados para que você possa entender os resultados e como eles se relacionam.



#### O método **drop()**



Se quisermos remover a informação duplicada, podemos usar o método **drop()** com **axis='columns'** para indicar que queremos remover uma coluna e não uma linha:



**both\_pupils = first\_pupil\_df.merge(second\_pupil\_df,**

                                   \*\*left\\\_on='authors',\*\*

&nbsp;                                  \\\*\\\*right\\\\\\\_on='author'\\\*\\\*

                                  \\\*\\\*)\\\*\\\*





**print(both\_pupils.drop('author', axis='columns'))**



result:

     \*\*authors                       title\\\_x                             title\\\_y\*\*



**0  Steinbeck               Of Mice and Men                        East of Eden**

**1      Twain  The Adventures of Tom Sawyer  The Adventures of Huckleberry Finn**

**2  Hemingway       The Old Man and the Sea             For Whom the Bell Tolls**





#### Recapitulação



* **Junção interna**: mantém apenas as linhas com valores compartilhados na coluna especificada de ambos os DataFrames originais. É feita com **how='inner'** (argumento padrão).
* **Junção externa**: mantém todos os valores na coluna especificada de ambos os DataFrames originais, com valores ausentes onde não há correspondência. É feita com **how='outer'**.
* **Junção à esquerda**: mantém todos os valores do DataFrame esquerdo, com valores do DataFrame direito mantidos apenas para valores que correspondem à coluna especificada no esquerdo. É feita com **how='left'**.
* **Junção à direita**: mantém todos os valores do DataFrame direito, com valores do DataFrame esquerdo mantidos apenas para valores que correspondem à coluna especificada no direito. É feita com **how='right'.**
* **Método drop()**: remove uma coluna de um DataFrame usando o parâmetro **axis='columns'.**
