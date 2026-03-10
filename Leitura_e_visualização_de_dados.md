#### Como resolver problemas com arquivos CSV



###### Nesta lição, você vai aprender a ler dados de arquivos CSV que não estão em conformidade com o formato padrão que vimos no Python Básico.

Lembre-se de que CSV é uma sigla do inglês que significa valores separados por vírgula. No entanto, os arquivos CSV não precisam usar apenas vírgulas como delimitadores;qualquer caractere pode ser usado.

Às vezes, eles podem ser encontrados como arquivos **.tsv** ou **.tab** (também conhecidos como arquivos TSV), além de **.csv**.



arquivo que vamos ler:

**data = pd.read\_csv('/datasets/gpp\_modified.csv')**



Você se lembra dos arquivos **.tsv** que mencionamos mais cedo? Os valores nesses arquivos são separados por tabulações ou caracteres **\\\\t** na terminologia da ciência da computação.



##### O parâmetro **sep=**

\# **sep=** - para especificar o delimitador correto



**import pandas as pd**



**data = pd.read\_csv('/datasets/gpp\_modified.csv', sep='|')**  #Vamos usar '|' como argumento e ver como isso afeta o DataFrame:



**print(data.head())**



Outro exemplo envolve a leitura de um arquivo **.tsv**. Para fazer isso, precisamos definir o parâmetro **sep=** como **'\\\\t'**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**

##### Os parâmetros **header=** e **names=**

\# **header=None** - para especificar que o arquivo não contem nomes de colunas

\# **names=** - para definir nomes de colunas manualmente





**read\_csv()** pode manipular arquivos sem cabeçalhos se usarmos o argumento correto para o parâmetro **header=**.



O argumento padrão é **header='infer'**, que diz à biblioteca pandas para tentar adivinhar quais são os nomes das colunas. Nesse caso, a pandas achou que a primeira linha de dados tinha os nomes das colunas. Podemos corrigir isso passando **header=None.**

**ex:**

**import pandas as pd**



**data = pd.read\_csv('/datasets/gpp\_modified.csv', sep='|', header=None)**



**print(data.head())**





Como o arquivo não tem nomes de coluna e não especificamos nenhum, a pandas usa números inteiros consecutivos (0, 1, 2 ...) para nomear as colunas.



Poderíamos renomear as colunas depois de ler os dados usando o atributo **columns** do DataFrame ou o método **rename()** mas também podemos definir os nomes enquanto lemos os dados usando o parâmetro opcional **names=** em **read\_csv()**.



**ex:**

**import pandas as pd**



**column\_names = \[**

    \*\*'country',

    'name',

    'capacity\\\\\\\\\\\\\\\_mw',

&nbsp;   'latitude',

    'longitude',

    'primary\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\_fuel',

    'owner'\\\\\\\*\\\\\\\*



**]**

**data = pd.read\_csv('/datasets/gpp\_modified.csv', sep='|', header=None, names=column\_names)**



**print(data.head())**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### O parâmetro **decimal=**

\# **decimal=** - para ler os decimais corretamente



É a convenção padrão no Brasil usar a vírgula como separador de decimal, mas o Python exige que você use o ponto como separador de decimal em números de ponto flutuante.



Em **read\_csv()** podemos usar o parâmetro **decimal=** e passar **','** como argumento. Por padrão, **read\_csv()** usa **decimal='.'**



**ex:**



**import pandas as pd**



**column\_names = \[**

    \*\*'country',\*\*

    \\\*\\\*'name',\\\*\\\*

    \\\\\\\*\\\\\\\*'capacity\\\\\\\\\\\\\\\_mw',\\\\\\\*\\\\\\\*

&nbsp;   \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*'latitude',\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*

    \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*'longitude',\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*

    \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*'primary\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\_fuel',\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*

    \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*'owner'\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*





















**]**

**data = pd.read\_csv(**

    \*\*'/datasets/gpp\\\_modified.csv',\*\*

    \\\*\\\*sep='|',\\\*\\\*

    \\\\\\\*\\\\\\\*header=None,\\\\\\\*\\\\\\\*

&nbsp;   \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*names=column\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\_names,\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*

    \\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*decimal=',',\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\*





















**)**



**print(data.head())**



**extra: no Jupyter notebook, você pode digitar o nome de um método e pressionar Shift+Tab para exibir a documentação.**

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Como ler arquivos do Excel



**df = pd.read\_excel()**

Vamos ler o arquivo Excel usando apenas o caminho do arquivo **'/datasets/product\_reviews.xlsx'**

A planilha reviews foi lida em df por padrão porque é a primeira planilha do arquivo Excel





##### O parâmetro **sheet\_name=**



Se você quer acessar uma planilha diferente do padrão, precisa passar dois argumentos: o caminho do arquivo (string) e **sheet\_name=** com o nome da planilha (também uma string).



**ex:
import pandas as pd**



**df = pd.read\_excel('/datasets/product\_reviews.xlsx', sheet\_name='reviewers')**



**print(df.head())**



Também podemos usar o índice da planilha em vez do nome como segundo argumento: **sheet\_name=0** mostra a primeira planilha, **sheet\_name=1** a segunda, e assim por diante



**ex:**

**import pandas as pd**



**df\_products = pd.read\_excel('/datasets/product\_reviews.xlsx',**

                           \*\*sheet\\\_name='products',\*\*



**)**



**sorted\_df\_products = df\_products.sort\_values('id')**  #classifica o DataFrame pela coluna 'id' em ordem crescente usando o método sort\_values()





Nunca é uma má ideia chamar o método **info()** quando você começar a trabalhar com um novo DataFrame.

Observação: você não pode acessar diretamente as informações exibidas por **info()** para armazenar em variáveis ou usar em cálculos porque **info()** não retorna nada, apenas imprime na tela



Se você deseja armazenar o número de linhas e colunas como variáveis, pode usar o atributo **shape**

ex:



**import pandas as pd**



**column\_names = \[**

    \*\*'country',\*\*

    \\\*\\\*'name',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*'capacity\\\\\\\\\\\\\\\_mw',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'latitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'longitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'primary\\\\\\\\\\\\\\\_fuel',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'owner'\\\\\\\*\\\\\\\*










**]**

**data = pd.read\_csv(**

    \*\*'/datasets/gpp\\\_modified.csv',\*\*

    \\\*\\\*sep='|',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*header=None,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*names=column\\\\\\\\\\\\\\\_names,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*decimal=',',\\\\\\\*\\\\\\\*










**)**



**n\_rows, n\_cols = data.shape**



**print(f"O DataFrame tem {n\_rows} linhas e {n\_cols} colunas")**



**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### O método **sample()**



O método **sample()** seleciona linhas aleatórias do DataFrame em vez de linhas consecutivas do início ou do final dele



ex:



**import pandas as pd**



**column\_names = \[**

    \*\*'country',\*\*

    \\\*\\\*'name',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*'capacity\\\\\\\\\\\\\\\_mw',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'latitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'longitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'primary\\\\\\\\\\\\\\\_fuel',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'owner'\\\\\\\*\\\\\\\*










**]**

**data = pd.read\_csv(**

    \*\*'/datasets/gpp\\\_modified.csv',\*\*

    \\\*\\\*sep='|',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*header=None,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*names=column\\\\\\\\\\\\\\\_names,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*decimal=',',\\\\\\\*\\\\\\\*










**)**



**print(data.sample(5))**





Se você quiser usar sample() e sempre obter o mesmo resultado ao executar o código, precisará usar o parâmetro random\_state= e configurá-lo para algum valor inteiro de sua escolha (qualquer número inteiro entre 0 e 4294967295)



ex:



**import pandas as pd**



**column\_names = \[**

    \*\*'country',\*\*

    \\\*\\\*'name',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*'capacity\\\\\\\\\\\\\\\_mw',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'latitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'longitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'primary\\\\\\\\\\\\\\\_fuel',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'owner'\\\\\\\*\\\\\\\*










**]**

**data = pd.read\_csv(**

    \*\*'/datasets/gpp\\\_modified.csv',\*\*

    \\\*\\\*sep='|',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*header=None,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*names=column\\\\\\\\\\\\\\\_names,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*decimal=',',\\\\\\\*\\\\\\\*










**)**



**print(data.sample(5, random\_state=1369))**



**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### o método **describe()**



**describe()** retorna do DataFrame as colunas numéricas da variável data. por padrão, colunas não numéricas são ignoradas.



###### Chamando **describe()** em colunas não numéricas

###### 

**import pandas as pd**



**column\_names = \[**

    \*\*'country',\*\*

    \\\*\\\*'name',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*'capacity\\\\\\\\\\\\\\\_mw',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'latitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'longitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'primary\\\\\\\\\\\\\\\_fuel',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'owner'\\\\\\\*\\\\\\\*










**]**

**data = pd.read\_csv(**

    \*\*'/datasets/gpp\\\_modified.csv',\*\*

    \\\*\\\*sep='|',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*header=None,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*names=column\\\\\\\\\\\\\\\_names,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*decimal=',',\\\\\\\*\\\\\\\*










**)**



**print(data\['country'].describe())**





Não temos erro! Em vez disso, obtemos as seguintes informações úteis:



'count': O número de valores não nulos

'unique': O número de valores unívocos

'top': O valor que ocorre com mais frequência

'freq': O número de vezes que o valor mais frequente ocorre



Para as colunas numéricas e não numéricas, **describe()** retorna uma contagem do número de valores não nulos, mas as outras quantidades ('unique', 'top' e 'freq') são retornadas apenas para as colunas não numéricas.



**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### O parâmetro **include=**



Podemos usar o parâmetro **include=** para dizer ao método **describe()** quais colunas queremos incluir no resultado. No entanto, não passamos uma lista de nomes de colunas como argumento. Em vez disso, passamos o tipo de dados das colunas que queremos como uma string (ou uma lista de strings se quisermos mais de um tipo de dados)



ex:

**import pandas as pd**



**column\_names = \[**

    \*\*'country',\*\*

    \\\*\\\*'name',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*'capacity\\\\\\\\\\\\\\\_mw',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'latitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'longitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'primary\\\\\\\\\\\\\\\_fuel',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'owner'\\\\\\\*\\\\\\\*










**]**

**data = pd.read\_csv(**

    \*\*'/datasets/gpp\\\_modified.csv',\*\*

    \\\*\\\*sep='|',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*header=None,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*names=column\\\\\\\\\\\\\\\_names,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*decimal=',',\\\\\\\*\\\\\\\*










**)**



**print(data.describe(include='object'))**





Por fim, podemos passar 'all' para obter as informações sobre cada coluna, seja qual for o tipo de dados:



ex:

**import pandas as pd**



**column\_names = \[**

    \*\*'country',\*\*

    \\\*\\\*'name',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*'capacity\\\\\\\\\\\\\\\_mw',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'latitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'longitude',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'primary\\\\\\\\\\\\\\\_fuel',\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*'owner'\\\\\\\*\\\\\\\*










**]**

**data = pd.read\_csv(**

    \*\*'/datasets/gpp\\\_modified.csv',\*\*

    \\\*\\\*sep='|',\\\*\\\*

&nbsp;   \\\\\\\*\\\\\\\*header=None,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*names=column\\\\\\\\\\\\\\\_names,\\\\\\\*\\\\\\\*

    \\\\\\\*\\\\\\\*decimal=',',\\\\\\\*\\\\\\\*










**)**



**print(data.describe(include='all'))**

