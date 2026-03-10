#### Conjunto de dados à primeira vista



Para começar, vamos trabalhar com uma versão reduzida do Conjunto de dados de varejo online do Repositório de aprendizado de máquina da Universidade da Califórnia em Irvine:  https://archive.ics.uci.edu/dataset/352/online+retail





Vamos tentar entender as informações desse conjunto de dados.



Primeiro, vamos carregar o conjunto de dados e exibir as primeiras linhas:



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**

**print(df.head())**



result:

**InvoiceNo StockCode                          Description  Quantity  \\**

**0    536520     21123  SET/10 IVORY POLKADOT PARTY CANDLES       1.0**

**1    536520     21124   SET/10 BLUE POLKADOT PARTY CANDLES       1.0**

**2    536520     21122   SET/10 PINK POLKADOT PARTY CANDLES       1.0**

**3    536520     84378        SET OF 3 HEART COOKIE CUTTERS       1.0**

**4    536520     21985    PACK OF 12 HEARTS DESIGN TISSUES       12.0**



            \*\*InvoiceDate UnitPrice  CustomerID         Country\*\*



**0  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**1  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**2  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**3  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**4  2010-12-01T12:43:00Z      0.29     14729.0  United Kingdom**



Vamos explorar um pouco mais os dados. Começaremos imprimindo os valores mínimo e máximo da coluna 'StockCode':



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**

**print(df\['StockCode'].min(), df\['StockCode'].max())**



result:

**10002 m**



Quando imprimimos as primeiras linhas, parecia que os dados de 'StockCode' eram números inteiros. No entanto, agora vemos que o valor máximo é 'm', que definitivamente não é um número inteiro.



Na verdade, a coluna representa valores de ID, alguns dos quais parecem números inteiros. Se você observar outros valores nessa coluna, vai encontrar valores como '82494L', '84029G', '35004G', etc.



Python é perfeitamente capaz de comparar valores de string usando os operadores > e <. No entanto, as regras para comparar strings são diferentes daquelas para comparar números. O comportamento de um operador depende dos tipos de dados dos operandos.



é importante observar que números inteiros se tornando flutuantes é comum em sistemas de exportação de dados, como ao converter arquivos do Excel para o formato CSV.



Os principais tópicos aprendidos nesta lição são:



* Não devemos presumir que o Python atribui os tipos de dados que esperamos ao ler os dados.
* É sempre uma ótima ideia chamar info() no DataFrame antes de mergulhar em cálculos e modificações. Faça disso uma rotina.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Strings e números na pandas



Quando a pandas lê dados de um arquivo de texto, converte automaticamente os dados brutos das colunas em tipos de dados pandas.



No geral, a conversão é simples: uma coluna que contém apenas números no arquivo CSV ou Excel será lida automaticamente como tipo de dados float ou int. Se uma coluna contiver apenas palavras, será lida como um tipo de dados object. A Pandas usa esse tipo de dados para dados de string, e também quando uma coluna contém uma mistura de tipos de dados entre seus valores.



Às vezes, a pandas não consegue inferir o tipo de dados correto. Quando isso acontece, precisamos intervir e converter os valores para o tipo correto.



##### Conversão para um tipo de dados específico



O método **astype()** da pandas permite fazer cast (ou converter) entre tipos de dados. Esse método existe para DataFrames e Series.



Abaixo, vamos criar um DataFrame simples de duas colunas a partir de um dicionário Python. As chaves do dicionário serão usadas como os nomes das colunas no DataFrame. Em seguida, exibiremos as informações sobre os tipos de dados das colunas:



**import pandas as pd**



**d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}**

**df = pd.DataFrame(data=d)**



**print(df)**

**print()**

**print('Output of df.info():')**

**df.info()**



result:

**col1  col2**

**0   1.0     3**

**1   2.0     4**



**Output of df.info():**

**<class 'pandas.core.frame.DataFrame'>**

**RangeIndex: 2 entries, 0 to 1**

**Data columns (total 2 columns):**

**#   Column  Non-Null Count  Dtype**

**---  ------  --------------  -----**

**0   col1    2 non-null      float64**

**1   col2    2 non-null      int64**

**dtypes: float64(1), int64(1)**

**memory usage: 160.0 bytes**



Agora vamos usar o método **astype()** para converter todos os valores em strings e exibir o DataFrame.



Ao usá-lo, você precisa especificar o tipo de dados que deseja converter entre parênteses. Por exemplo, **df\['column'] = df\['column'].astype('int')** converte a coluna 'column' para o tipo de dados de números inteiros.



Esteja ciente de que nem todos os tipos de dados podem ser convertidos entre si usando **astype()**.  Todas as operações de conversão que possam alterar significativamente os dados originais devem ser executadas com cuidado.



**import pandas as pd**



**d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}**

**df = pd.DataFrame(data=d)**



**df\_str\_dtype = df.astype('str')**

**print(df\_str\_dtype)**



result:

**col1 col2**

**0  1.0    3**

**1  2.0    4**



Os dados impressos do DataFrame parecem iguais a antes, mas o que o método **info()** nos diz?



**df\_str\_dtype.info()**



result:

**<class 'pandas.core.frame.DataFrame'>**

**RangeIndex: 2 entries, 0 to 1**

**Data columns (total 2 columns):**

**#   Column  Non-Null Count  Dtype**

**---  ------  --------------  -----**

**0   col1    2 non-null      object**

**1   col2    2 non-null      object**

**dtypes: object(2)**

**memory usage: 160.0+ bytes**





Também podemos usar o método **astype()** em colunas individuais (ou seja, objetos Series). Vamos fazer com que **'col1'** fique com o tipo de dados int em vez de float.



**import pandas as pd**



**d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}**

**df = pd.DataFrame(data=d)**



**df\['col1'] = df\['col1'].astype('int')**

**print(df)**



result:

**col1  col2**

**0     1     3**

**1     2     4**



As colunas em um DataFrame podem ter diferentes tipos de dados ou **dtypes**.



**import pandas as pd**



**d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}**

**df = pd.DataFrame(data=d)**



**df\['col1'] = df\['col1'].astype('int')**

**print(df.dtypes)**



result:

**col1    int64**

**col2    int64**

**dtype: object**



E assim, alteramos o tipo de dados de uma única coluna. Observe que o 64 em int64 se refere ao tamanho em bits dos números inteiros na memória de um computador. Você também pode ter outros tipos inteiros como int32, int16, etc. Basicamente, o tipo com 64 bits pode conter muito mais dígitos do que os tipos com menos bits, mas todos eles se comportam de forma idêntica aos tipos de dados int.



##### Cuidado com as pegadinhas



Digamos que você carregue um conjunto de dados, e parece que todos os valores de ponto flutuante em uma coluna deveriam ser números inteiros, mas há muitos deles para examinar manualmente. Como saber se é seguro realizar a conversão de float para int ou se você perderá algumas informações após fazer isso?



Para ilustrar, veja o exemplo a seguir.



**import pandas as pd**



**d = {'col1': \[1.0, 2.0, 3.0, 4.0], 'col2': \[5.0, 6.01, 7.0, 8.0]}**

**df = pd.DataFrame(data=d)**



**print(df)**



result:

**col1  col2**

**0   1.0  5.00**

**1   2.0  6.01**

**2   3.0  7.00**

**3   4.0  8.00**



Strings e números na pandas

Quando a pandas lê dados de um arquivo de texto, converte automaticamente os dados brutos das colunas em tipos de dados pandas.



No geral, a conversão é simples: uma coluna que contém apenas números no arquivo CSV ou Excel será lida automaticamente como tipo de dados float ou int. Se uma coluna contiver apenas palavras, será lida como um tipo de dados object. A Pandas usa esse tipo de dados para dados de string, e também quando uma coluna contém uma mistura de tipos de dados entre seus valores.



Às vezes, a pandas não consegue inferir o tipo de dados correto. Quando isso acontece, precisamos intervir e converter os valores para o tipo correto.



Outras vezes, a pandas fará exatamente o que foi projetada para fazer, mas isso talvez não seja o que queremos.



Nesta lição, você vai aprender a lidar com essas duas situações.



Conversão para um tipo de dados específico

O método astype() da pandas permite fazer cast (ou converter) entre tipos de dados.



Tal como acontece com muitos outros métodos pandas, esse método existe para DataFrames e Series.



Vamos demonstrar como isso funciona usando alguns exemplos.



Abaixo, vamos criar um DataFrame simples de duas colunas a partir de um dicionário Python. As chaves do dicionário serão usadas como os nomes das colunas no DataFrame. Em seguida, exibiremos as informações sobre os tipos de dados das colunas:



import pandas as pd



d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}

df = pd.DataFrame(data=d)



print(df)

print()

print('Output of df.info():')

df.info()

   col1  col2

0   1.0     3

1   2.0     4



Output of df.info():

<class 'pandas.core.frame.DataFrame'>

RangeIndex: 2 entries, 0 to 1

Data columns (total 2 columns):

 #   Column  Non-Null Count  Dtype

---

 0   col1    2 non-null      float64

 1   col2    2 non-null      int64

dtypes: float64(1), int64(1)

memory usage: 160.0 bytes

Agora vamos usar o método astype() para converter todos os valores em strings e exibir o DataFrame.



Ao usá-lo, você precisa especificar o tipo de dados que deseja converter entre parênteses. Por exemplo, df\['column'] = df\['column'].astype('int') converte a coluna 'column' para o tipo de dados de números inteiros.



Esteja ciente de que nem todos os tipos de dados podem ser convertidos entre si usando astype().  Todas as operações de conversão que possam alterar significativamente os dados originais devem ser executadas com cuidado.



import pandas as pd



d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}

df = pd.DataFrame(data=d)



df\_str\_dtype = df.astype('str')

print(df\_str\_dtype)

  col1 col2

0  1.0    3

1  2.0    4

Os dados impressos do DataFrame parecem iguais a antes, mas o que o método info() nos diz?



import pandas as pd



d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}

df = pd.DataFrame(data=d)



df\_str\_dtype = df.astype('str')

df\_str\_dtype.info()

<class 'pandas.core.frame.DataFrame'>

RangeIndex: 2 entries, 0 to 1

Data columns (total 2 columns):

 #   Column  Non-Null Count  Dtype

---

 0   col1    2 non-null      object

 1   col2    2 non-null      object

dtypes: object(2)

memory usage: 160.0+ bytes

Podemos ver que, apesar da exibição do DataFrame não ter mudado, as informações sobre os tipos de dados mudaram.



Também podemos usar o método astype() em colunas individuais (ou seja, objetos Series). Vamos fazer com que 'col1' fique com o tipo de dados int em vez de float.



import pandas as pd



d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}

df = pd.DataFrame(data=d)



df\['col1'] = df\['col1'].astype('int')

print(df)

   col1  col2

0     1     3

1     2     4

Dessa vez, em vez do método info(), vamos conferir o atributo dtypes. Lembra disso?



As colunas em um DataFrame podem ter diferentes tipos de dados ou dtypes.



import pandas as pd



d = {'col1': \[1.0, 2.0], 'col2': \[3, 4]}

df = pd.DataFrame(data=d)



df\['col1'] = df\['col1'].astype('int')

print(df.dtypes)

col1    int64

col2    int64

dtype: object

E assim, alteramos o tipo de dados de uma única coluna. Observe que o 64 em int64 se refere ao tamanho em bits dos números inteiros na memória de um computador. Você também pode ter outros tipos inteiros como int32, int16, etc. Basicamente, o tipo com 64 bits pode conter muito mais dígitos do que os tipos com menos bits, mas todos eles se comportam de forma idêntica aos tipos de dados int.



Cuidado com as pegadinhas

Digamos que você carregue um conjunto de dados, e parece que todos os valores de ponto flutuante em uma coluna deveriam ser números inteiros, mas há muitos deles para examinar manualmente. Como saber se é seguro realizar a conversão de float para int ou se você perderá algumas informações após fazer isso?



Para ilustrar, veja o exemplo a seguir.



import pandas as pd



d = {'col1': \[1.0, 2.0, 3.0, 4.0], 'col2': \[5.0, 6.01, 7.0, 8.0]}

df = pd.DataFrame(data=d)



print(df)

   col1  col2

0   1.0  5.00

1   2.0  6.01

2   3.0  7.00

3   4.0  8.00



Se você não tomar cuidado, vai executar **df.astype('int')** (que passará de 6,01 para 6) sem perceber que acabou de alterar os valores em seu conjunto de dados.



**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Numpy



NumPy é uma biblioteca Python potente usada para computação científica. Ela apresenta uma nova estrutura de dados chamada vetor, que é semelhante a uma lista, mas com várias vantagens, incluindo a capacidade de realizar operações vetorizadas em vetores inteiros rapidamente. NumPy também fornece muitas funções e ferramentas matemáticas úteis para trabalhar com vetores, como ordenação, indexação e transmissão.



Quando estiver lidando com centenas ou milhares (ou mais!) de valores, você pode empregar a ajuda do método **array\_equal()** da biblioteca numpy. Ele aceita dois vetores e retorna **True** se eles tiverem a mesma forma e elementos e **False** caso contrário. Vamos tentar com **'col1'**:



**import numpy as np**

**import pandas as pd**



**d = {'col1': \[1.0, 2.0, 3.0, 4.0], 'col2': \[5.0, 6.01, 7.0, 8.0]}**

**df = pd.DataFrame(data=d)**



\# verifique se a conversão de 'col1' é segura

**np.array\_equal(df\['col1'], df\['col1'].astype('int'))**



result:

**True**



Ótimo, não teremos problemas aqui! Agora vamos tentar '**col2'**:



**import numpy as np**

**import pandas as pd**



**d = {'col1': \[1.0, 2.0, 3.0, 4.0], 'col2': \[5.0, 6.01, 7.0, 8.0]}**

**df = pd.DataFrame(data=d)**



\# verifique se a conversão de 'col2' é segura

**np.array\_equal(df\['col2'], df\['col2'].astype('int'))**



result:

**False**



Agora sabemos que não podemos converter 'col2' de float para int sem perder alguns detalhes dos dados.



* Tenha cuidado ao converter entre tipos de dados e considere se a operação de conversão pode levar a alterações significativas.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

##### Conversão de strings em valores numéricos



**read\_csv()** sempre lê tudo o que se parece com números inteiros como um tipo de dados int, mesmo se você quiser que sejam strings.





Outras vezes, acontece o oposto: a pandas confunde valores numéricos com strings e precisamos realizar algumas etapas para fazer a conversão.



Em alguns casos simples, o método **astype()** funciona bem. Abaixo, ilustramos como converter strings que representam números inteiros em inteiros reais:



**import pandas as pd**



**d = {'col1': \['1.0', '2.0'], 'col2': \['3', '4']}**

**df = pd.DataFrame(data=d)**



\# converter col2 em int

**df\['col2'] = df\['col2'].astype('int')**

**print(df.dtypes)**



result:

**col1    object**

**col2     int64**

**dtype: object**





Em outros casos, as strings não podem ser convertidas em números dessa maneira. Veja o que acontece quando tentamos converter strings que representam valores de ponto flutuante em inteiros:



**import pandas as pd**



**d = {'col1': \['1.0', '2.0'], 'col2': \['3', '4']}**

**df = pd.DataFrame(data=d)**



**# converter col1 dá um erro**

**df\['col1'] = df\['col1'].astype('int')**



result:

**---------------------------------------------------------------------------**

**ValueError                                Traceback (most recent call last)**

**...**

**...**

**...**

**ValueError: invalid literal for int() with base 10: '1.0'**





Para resolver esse problema, usamos um método mais flexível chamado **to\_numeric()**:



**import pandas as pd**



**d = {'col1': \['1.0', '2.0'], 'col2': \['3', '4']}**

**df = pd.DataFrame(data=d)**



**df\['col2'] = df\['col2'].astype('int')**

**df\['col1'] = pd.to\_numeric(df\['col1'])**

**print(df.dtypes)**



result:

**col1    float64**

**col2      int64**

**dtype: object**



Ele funciona muito bem se você tiver strings semelhantes a números, como **'72'** ou **'1.394'**. Mas, por padrão, **to\_numeric()** não consegue converter strings com caracteres não numéricos ou decimais em um número. Em vez disso, ele mostra um erro.



Mas há boas notícias: **to\_numeric()** tem um parâmetro **errors=!** O valor desse parâmetro determina o que **to\_numeric()** fará se encontrar um valor inválido:



* **errors='raise'**: é o argumento padrão, onde valores inválidos geram erros, interrompendo a conversão para números em toda a coluna.
* **errors='coerce'**: os valores inválidos são substituídos por **NaN**.
* **errors='ignore'**: os valores inválidos são ignorados e deixados inalterados.

Desta forma:



**import pandas as pd**



**d = {'col1': \['1.0', 'B.0'], 'col2': \['3', '4']}**

**df = pd.DataFrame(data=d)**



**df\['col2'] = df\['col2'].astype('int')**

**df\['col1'] = pd.to\_numeric(df\['col1'], errors='coerce')**



**print(df.dtypes)**

**print(df)**



result:

**col1    float64**

**col2      int64**

**dtype: object**

**col1  col2**

**0   1.0     3**

**1   NaN     4**





#### Recapitulação



* Use **astype()** para converter um tipo de dados em outro.
* Nem todos os tipos de dados podem ser convertidos usando **astype()**; algumas conversões (por exemplo, converter uma string com números float em um número inteiro) podem gerar erros.
* Tenha cuidado ao realizar operações de conversão que possam resultar em alterações significativas nos dados originais.

\-  Ao converter de strings para números, o método **to\_numeric()** pode ser uma escolha melhor devido à sua flexibilidade (fornecida pelo parâmetro **errors=**).

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Como trabalhar com datas e horas



A biblioteca pandas é útil para mais do que apenas converter entre strings e números. Ela também pode lidar com dados de data e hora.



quando quiser ter certeza de que suas datas e horas estão armazenadas corretamente, ou quiser fazer cálculos com elas, você vai precisar usar o formato **datetime** do Python.



##### Conversão para datetime



Vamos novamente carregar o conjunto de dados e exibir algumas linhas aleatórias.



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**

**print(df.head())**



result:

**InvoiceNo StockCode                          Description  Quantity  \\**

**0    536520     21123  SET/10 IVORY POLKADOT PARTY CANDLES       1.0**

**1    536520     21124   SET/10 BLUE POLKADOT PARTY CANDLES       1.0**

**2    536520     21122   SET/10 PINK POLKADOT PARTY CANDLES       1.0**

**3    536520     84378        SET OF 3 HEART COOKIE CUTTERS       1.0**

**4    536520     21985    PACK OF 12 HEARTS DESIGN TISSUES       12.0**



            \*\*InvoiceDate UnitPrice  CustomerID         Country\*\*



**0  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**1  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**2  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**3  2010-12-01T12:43:00Z      1.25     14729.0  United Kingdom**

**4  2010-12-01T12:43:00Z      0.29     14729.0  United Kingdom**



A coluna **'InvoiceDate'** contém a data e hora em que a fatura do pedido foi emitida.



Sabemos que a coluna **'InvoiceDate'** tem o tipo de dados object devido às lições anteriores.



Vamos confirmar isso novamente olhando para o atributo **dtype**:



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**

**print(df\['InvoiceDate'].dtype)**



result:

**object**



A partir de agora, **'InvoiceDate'** armazena os valores como um tipo de **string**. Mas para realizar qualquer tipo de operação de data ou hora com esses valores, precisamos convertê-los para o tipo **datetime**. Podemos fazer isso usando o método **to\_datetime()** da pandas.



##### Formatação de valores datetime



O método **to\_datetime()** é usado para converter datas de um tipo de dados string para o tipo de dados datetime. Ao chamar o método, precisamos usar o parâmetro **format=**, que recebe uma string especificando como as datas são formatadas. Os códigos de formatação designados pelo símbolo **%** são usados para especificar o formato.



Por exemplo, podemos converter a string **2010-12-17T12:38:00Z** (um formato ISO 8601) em um objeto datetime passando a string de formatação correta para o parâmetro **format=: %Y-%m-%dT% H:%M:%SZ**.



**import pandas as pd**



**string\_date = '2010-12-17T12:38:00Z'**

**datetime\_date = pd.to\_datetime(string\_date, format='%Y-%m-%dT%H:%M:%SZ')**



**print(type(string\_date))**

**print(type(datetime\_date))**

**print(datetime\_date)**



result:

**<class 'str'>**

**<class 'pandas.\_libs.tslibs.timestamps.Timestamp'>**

**2010-12-17 12:38:00**



Após chamar **to\_datetime()** com nossa data baseada em string e a string de formatação apropriada, obtivemos um objeto com o tipo de dados **Timestamp**. O tipo **Timestamp** da pandas é equivalente ao tipo datetime do Python, então vamos usar o termo **"datetime"** para nos referirmos a ambos.



Observe que o formato do objeto datetime difere daquele da string original: **2010-12-17T12:38:00Z**. No objeto datetime, temos **YYYY-MM-DD HH:MM:SS — 2010-12-17 12:38:00**. Não importa como a string original foi formatada, o tipo datetime terá esse formato consistente.





##### Como funcionam os códigos de formatação



Como sabemos que o padrão de código para **'2010-12-17T12:38:00Z'** deveria ser **'%Y-%m-%dT%H:%M:%SZ'**? Porque o código representa uma generalização da string. Cada elemento de data ou hora se torna um símbolo que diz à pandas como interpretá-lo.



Existem muitos símbolos de formatação diferentes, mas apenas alguns que você vai usar regularmente. Entre eles:



* %d — dia do mês (01 a 31)
* %m — mês (01 a 12)
* %Y — ano de quatro dígitos (2019)
* %y — ano de dois dígitos (19)
* %H — hora no formato de 24 horas
* %I — hora no formato de 12 horas
* %M — minutos (00 a 59)
* %S — segundos (00 a 59)





##### Verificar a presença de objetos datetime



Agora vamos ver se nossos dados mudam em termos de aparência e tipo depois de usar **to\_datetime()**:



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**

**df\['InvoiceDate'] = pd.to\_datetime(df\['InvoiceDate'], format='%Y-%m-%dT%H:%M:%SZ')**



**print(df.head())**

**print()**

**df.info()**



result:

**InvoiceNo StockCode                          Description  Quantity  \\**

**0    536520     21123  SET/10 IVORY POLKADOT PARTY CANDLES       1.0**

**1    536520     21124   SET/10 BLUE POLKADOT PARTY CANDLES       1.0**

**2    536520     21122   SET/10 PINK POLKADOT PARTY CANDLES       1.0**

**3    536520     84378        SET OF 3 HEART COOKIE CUTTERS       1.0**

**4    536520     21985    PACK OF 12 HEARTS DESIGN TISSUES       12.0**



          \*\*InvoiceDate UnitPrice  CustomerID         Country\*\*



**0 2010-12-01 12:43:00      1.25     14729.0  United Kingdom**

**1 2010-12-01 12:43:00      1.25     14729.0  United Kingdom**

**2 2010-12-01 12:43:00      1.25     14729.0  United Kingdom**

**3 2010-12-01 12:43:00      1.25     14729.0  United Kingdom**

**4 2010-12-01 12:43:00      0.29     14729.0  United Kingdom**



**<class 'pandas.core.frame.DataFrame'>**

**RangeIndex: 50000 entries, 0 to 49999**

**Data columns (total 8 columns):**

**#   Column       Non-Null Count  Dtype**

**---  ------       --------------  -----**

**0   InvoiceNo    50000 non-null  object**

**1   StockCode    50000 non-null  object**

**2   Description  49857 non-null  object**

**3   Quantity     50000 non-null  float64**

**4   InvoiceDate  50000 non-null  datetime64\[ns]**

**5   UnitPrice    50000 non-null  object**

**6   CustomerID   31599 non-null  float64**

**7   Country      50000 non-null  object**

**dtypes: datetime64\[ns](1), float64(2), object(5)**

**memory usage: 3.1+ MB**





Observe como os valores impressos parecem ser diferentes de antes. Isso porque a coluna **'InvoiceDate'** agora é **datetime**. Embora os valores pareçam diferentes, todas as informações das strings originais continuam lá.



Agora, o tipo de dados da coluna datetime é um objeto **datetime64\[ns]**. O **\[ns]** significa o formato de hora baseado em nanossegundos que especifica a precisão do objeto **DateTime**.





Processe os dados de tempo na coluna 'timestamp' convertendo-os de string para datetime. Em seguida, imprima as cinco primeiras linhas na tabela position com o método head().



**import pandas as pd**



**position = pd.read\_csv('/datasets/position.csv')**



**position\['timestamp'] = pd.to\_datetime(position\['timestamp'], format='%Y-%m-%dT%H:%M:%S')**

**print(position.head())**



result:

            \*\*timestamp  level   page\\\_id\*\*



**0 2019-02-04 13:22:34      1  6ukoNykP**

**1 2019-02-06 15:30:54      1  Q62R1Ue7**

**2 2019-02-08 14:53:45      1  GmfSxgPs**

**3 2019-02-10 16:50:22      2  M56OK96C**

**4 2019-02-12 14:16:28      1  q2fqVqPc**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Acesso a atributos de coluna datetime usando **.dt**





Lembre-se de que, na pandas, os objetos datetime são representados pelo tipo de dados Timestamp. Para obter, por exemplo, o atributo year do primeiro valor de Timestamp na coluna **'InvoiceDate'**, use o seguinte código:



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**



\# converte 'InvoiceDate' para datetime

**df\['InvoiceDate'] = pd.to\_datetime(df\['InvoiceDate'], format='%Y-%m-%dT%H:%M:%SZ')**



**print(df\['InvoiceDate']\[0].year)** # retorna o ano do primeiro InvoiceDate



result:

**2010**



Embora possamos acessar todos os atributos de valores Timestamp individuais dessa maneira, não podemos fazer isso para um Series com valores Timestamp. Veja o que acontece quando tentamos obter o atributo **day** de toda a coluna **'InvoiceDate'**:



**import pandas as pd**



**df = pd.read\_csv('OnlineRetail.csv')**

**df\['InvoiceDate'] = pd.to\_datetime(df\['InvoiceDate'], format='%Y-%m-%dT%H:%M:%SZ')**

**df\['day'] = df\['InvoiceDate'].day**



result:

**---------------------------------------------------------------------------**

**AttributeError                            Traceback (most recent call last)**

**...**

**...**

**...**

**AttributeError: 'Series' object has no attribute 'day'**





Obtemos um erro, porque **df\['InvoiceDate']** é um objeto **Series**, que não tem um atributo **day**, embora os valores individuais de **Timestamp** no objeto Series tenham.



Para obter atributos para colunas inteiras de dados datetime, use o método acessor **.dt**.



Por exemplo, podemos criar um DataFrame **'df\_days'** que contém o atributo **day** para cada valor na coluna **'InvoiceDate'**:



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**

**df\['InvoiceDate'] = pd.to\_datetime(df\['InvoiceDate'], format='%Y-%m-%dT%H:%M:%SZ')**



**df\_days = df\['InvoiceDate'].dt.day**

**print(df\_days.sample(5, random\_state=42))**



result:

**33553    17**

**9427      6**

**199       1**

**12447     6**

**39489    21**

**Name: InvoiceDate, dtype: int64**



Sem erros dessa vez! Lembre-se de que, se quiser acessar os atributos de uma coluna inteira de dados datetime, você vai precisar usar o método acessor .dt da coluna datetime e não a própria coluna.





##### Como trabalhar com fusos horários





Você precisa saber como converter entre diferentes fusos horários sem se confundir. É aí que **.dt.tz\_localize()** e **.dt.tz\_convert()** são úteis. O primeiro permite que você atribua um fuso horário a uma coluna **datetime** para que os dados estejam **"cientes"** do próprio fuso horário. O segundo permite que você converta uma coluna **"que reconhece o próprio fuso"** em um diferente.



Vamos ver como isso funciona na prática atribuindo o fuso horário **UTC** à coluna **'InvoiceDate'**:



**import pandas as pd**



**df = pd.read\_csv('/datasets/OnlineRetail.csv')**

**df\['InvoiceDate'] = pd.to\_datetime(df\['InvoiceDate'], format='%Y-%m-%dT%H:%M:%SZ')**



**df\['InvoiceDate'] = df\['InvoiceDate'].dt.tz\_localize('UTC')**



**print(df\['InvoiceDate'].sample(5, random\_state=42))**



result:

**33553   2010-12-17 12:38:00+00:00**

**9427    2010-12-06 09:58:00+00:00**

**199     2010-12-01 13:21:00+00:00**

**12447   2010-12-06 16:57:00+00:00**

**39489   2010-12-21 15:19:00+00:00**

**Name: InvoiceDate, dtype: datetime64\[ns, UTC]**



E se precisássemos mostrar nossos dados para alguém que mora em Nova York que prefere ver os valores datetime no fuso horário local?



Nesse caso, passamos **'America/New\_York'** para o método **.dt.tz\_convert()**:



**import pandas as pd**



**df = pd.read\_csv('OnlineRetail.csv')**

**df\['InvoiceDate'] = pd.to\_datetime(df\['InvoiceDate'], format='%Y-%m-%dT%H:%M:%SZ')**



**df\['InvoiceDate'] = df\['InvoiceDate'].dt.tz\_localize('UTC')**



**df\['InvoiceDate\_NYC'] = df\['InvoiceDate'].dt.tz\_convert('America/New\_York')**



**print(df\['InvoiceDate\_NYC'].sample(5, random\_state=42))**



result:

**33553   2010-12-17 07:38:00-05:00**

**9427    2010-12-06 04:58:00-05:00**

**199     2010-12-01 08:21:00-05:00**

**12447   2010-12-06 11:57:00-05:00**

**39489   2010-12-21 10:19:00-05:00**

**Name: InvoiceDate\_NYC, dtype: datetime64\[ns, America/New\_York]**



Agora vemos duas coisas:



* O **dtype** da nova coluna **'InvoiceDate\_NYC'** agora contém informações de **America/New\_York** em vez de UTC.
* Os próprios valores datetime são diferentes. Eles são diferentes em exatamente 5 horas, porque Nova York, como a maioria do leste da América do Norte, usa o Horário Padrão do Leste **(UTC-5)** durante o inverno e o Horário de Verão do Leste **(UTC-4)** no resto do ano.



Consulte este artigo da Wikipédia para obter uma lista de todos os nomes de fuso horário padrão que você pode usar com **.dt.tz\_convert()**.  https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List







* Usando a coluna 'timestamp', crie um novo DataFrame chamado 'dt\_toronto' que contém todos os datetimes que estão no fuso horário 'America/Toronto'. Imprima as primeiras 5 linhas do resultado



**import pandas as pd**



**position = pd.read\_csv('/datasets/position.csv')**

**position\['timestamp'] = pd.to\_datetime(position\['timestamp'], format='%Y-%m-%dT%H:%M:%S')**



**dt\_toronto = position\['timestamp'].dt.tz\_localize('America/Toronto')**



**print(dt\_toronto.head())**





* Use o novo DataFrame 'dt\_toronto' para converter o datetime para o fuso horário 'Australia/Brisbane' e armazene o resultado em uma nova coluna chamada 'dt\_brisbane'. Imprima as 5 primeiras linhas.



**import pandas as pd**



**position = pd.read\_csv('/datasets/position.csv')**

**position\['timestamp'] = pd.to\_datetime(position\['timestamp'], format='%Y-%m-%dT%H:%M:%S')**

**dt\_toronto = position\['timestamp'].dt.tz\_localize('America/Toronto')**



**dt\_brisbane = dt\_toronto.dt.tz\_convert('Australia/Brisbane')**



**print(dt\_brisbane.head())**

