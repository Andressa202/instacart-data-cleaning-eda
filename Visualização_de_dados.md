#### visão geral

##### Por que visualizar dados?





* Para facilitar a análise de grandes quantidades de dados e revelar visualmente suas propriedades; em outras palavras, visualizar para analisar. Ao visualizar os dados, você pode obter instantaneamente um entendimento melhor da situação, o que seria impossível trabalhando com números brutos.
* Para comunicar os resultados da análise aos seus colegas de uma maneira compreensível e efetiva.





##### Escolher a visualização correta

Existem muitos tipos de gráficos. Qual deles usar depende de muitos fatores, inclusive o tipo e a dimensão dos dados(ou seja, o número de variáveis representadas nele) e outros fatores que vamos abordar ao longo deste capítulo. Cada tipo pode ser útil para os analistas e leitores à sua maneira.





##### Elementos de um bom gráfico

Bons gráficos nos ajudam a tirar conclusões a partir dos dados. Eles fornecem uma boa compreensão inicial dos dados sem exigir uma análise profunda.



##### Títulos

Todo gráfico deve ter um título descritivo que informa imediatamente ao leitor por que ele foi criado.

##### 

##### Eixos

Os eixos devem sempre ser legendados para evitar ambiguidade sobre a quantidade ou as categorias mostradas.



Geralmente, a melhor opção é representar a variável independente no eixo X e a variável dependente, no eixo Y.



* A variável independente é a causa da variação no gráfico. O valor dela é independente das outras variáveis no gráfico.
* Já a variável dependente é o efeito disso. O valor dela depende das mudanças na variável independente.



##### Legendas de gráfico

As legendas de gráfico são úteis quando você está usando mais do que um conjunto de dados no mesmo gráfico.



Nem todos os gráficos necessitam de legenda. Ainda assim, ela pode ajudar a reforçar as informações.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### O que podemos aprender com visualizações ruins



* ###### Uso desnecessário do eixo X
* ###### Gráfico de pizza sem sentido

Não existe quase nenhum caso em que é melhor usar gráficos de pizza e não gráficos de barras para comunicar informações com clareza.

* ###### Gráfico de barras enganoso
* ###### Sobrecarga de informação
* ###### Gráfico de linha enganoso



#### exemplo de código para gráfico de linhas 'Casos de Covid'



    **import pandas as pd**

    **import numpy as np**

    **import plotly.express as px**

    

    **cases = \[33, 61, 86, 112, 116, 129, 192, 174, 344, 304, 327, 246, 320, 339, 376]**

    

    **dates = \['March<br>'] \* len(cases)**

    **day = 18**

    **for i in range(len(dates)):**

        **dates\[i] = dates\[i] + str(day)**

        **day = day + 1**

    **dates\[-1] = 'April<br>1'**

    

    **labels = dict(date="Date", cases="Number of cases")**

    **markers = dict(size=30, line=dict(width=2, color='black'), color='white')**

    **title = dict(text='New Cases Per Day', font=dict(color='white', size=30))**

    **yaxis = dict(tickmode='linear', tick0=30, dtick=30)**

    

    **df = pd.DataFrame({'cases': cases, 'date': dates})**

    

    **fig = px.line(df, y='cases', x='date', text='cases', markers=True, labels=labels, title="New Cases Per Day")**

    

    **fig.update\_xaxes(showgrid=False, color='white', tickangle=0)**

    **fig.update\_yaxes(color='white', gridcolor='#5c5a5c', gridwidth=2, range=\[15, 400])**

    **fig.update\_traces(marker=markers, line\_color='white', line\_width=6)**

    **fig.update\_layout(title=title,**

                      **title\_x=0.5,**

                      **paper\_bgcolor='#070230',**

                      **plot\_bgcolor='#070230',**

                      **yaxis=yaxis,**

                      **xaxis\_type='category')**

    **fig.add\_annotation(text='TOTAL CASES',** 

                        **align='right',**

                        **showarrow=False,**

                        **font=dict(color='white', size=12),**

                        **xref='paper',**

                        **yref='paper',**

                        **x=1.08,**

                        **y=1.25)**

    **fig.add\_annotation(text='3,342',** 

                        **align='right',**

                        **showarrow=False,**

                        **font=dict(color='white', size=23),**

                        **xref='paper',**

                        **yref='paper',**

                        **x=1.071,**

                        **y=1.2)**

    

    **fig.show()**

    

* ###### Uso confuso de cor  
* 
**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



#### Nesta lição, você vai aprender a fazer gráficos usando a **pandas** e a exibi-los usando a **Matplotlib**.



##### O método **plot()** e **plt.show()**



Aqui está um exemplo de como podemos chamar **plot()** para construir um gráfico na pandas e depois usar **show()** para exibir o gráfico em toda a sua beleza:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**

**print(df)**



**df.plot()**

**plt.show()**



result:

   **a   b**

**0  2   4**

**1  3   9**

**2  4  16**

**3  5  25**



O método **plot()** da pandas prepara a imagem com base nos dados, enquanto a função **show()** de pyplot apresenta a imagem ao usuário.



Por padrão, o método **plot()** cria um gráfico em que todas as colunas numéricas do DataFrame são traçadas nos mesmos eixos. Os índices ficam no eixo X, e os valores das colunas ficam no eixo Y.



Se for necessário, você também pode traçar uma única coluna:

ex:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df\['b'].plot()**

**plt.show()**



Em alguns ambientes de desenvolvimento, como no Jupyter Notebook, você não precisa usar **plt.show()** para exibir o gráfico. No entanto, é uma boa prática sempre incluir **plt.show()** para que o gráfico seja exibido em qualquer ambiente que alguém possa executar o seu código.



Outra opção útil é escrever um código que salva os gráficos como arquivos de imagem. Por exemplo, podemos salvar o gráfico acima no formato **PNG** usando a função **savefig()** da **matplotlib**:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df\['b'].plot()**

**plt.savefig('myplot.png')**



Se você rodar esse código na sua máquina, um arquivo **PNG** nomeado **myplot.png** será criado no diretório do seu script Python ou Notebook Jupyter. O Python sabe em qual formato salvar o arquivo com base na extensão na string de entrada para **savefig()**.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Personalização de gráficos com parâmetros **plot()**



* ###### Títulos

Títulos para os gráficos são passados no parâmetro **title=** como strings:

ex:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df.plot(title='A e B')**

**plt.show()**





* ###### Estilo de linha

Para mudar o marcador de estilo, use o parâmetro **style=**. Vamos passar a ele o argumento **'o'** para fazer cada valor ser marcado como um ponto.

ex:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df.plot(title='A e B', style='o')**

**plt.show()**



Existem outros marcadores de estilo também, por exemplo, **style='x'** marca cada ponto com um x:

ex:



**df.plot(title='A e B', style='x')**

**plt.show()**



Diferentes estilos podem até ser combinados. Por exemplo, quando queremos exibir tanto linhas quanto pontos, podemos usar **style='o-'**:



**df.plot(title='A e B', style='o-')**

**plt.show()**



* ###### Eixos

Em todos os gráficos que você viu até agora, o eixo horizontal tinha os valores dos índices do DataFrame. Mas e se você quiser desenhar duas colunas, uma em relação a outra? Podemos usar os parâmetros **x=** e **y=** para conseguir isso:

ex:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df.plot(x='b', y='a', title='A vs B', style='o')**

**plt.show()**



Aqui a coluna **'a'** está no eixo **y** e a coluna **'b'**está no eixo **X**. Perceba que a pandas nomeou o eixo horizontal automaticamente com **'b'**e a legenda padrão do gráfico agora faz referência apenas à coluna **'a'**.



Para mudar a legenda dos eixos, podemos usar os parâmetros **xlabel=** e **ylabel=** do método **plot()**:

ex:



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df.plot(x='b',**

        **y='a',**

        **title='A vs B',**

        **style='o',**

        **xlabel="Oi, sou B",**

        **ylabel="Oi, sou A")**



**plt.show()**



no método **plot()**, podemos definir legendas com **plt.xlabel()** e **plt.ylabel()**. Aqui está como:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df.plot(x='b',**

        **y='a',**

        **title='A vs B',**

        **style='o',**

        **legend=False)**



**plt.xlabel("Olá, eu sou B")** # configurando a legenda de x

**plt.ylabel("Olá, eu sou A")** # configurando a legenda de y

**plt.show()**



Depois, podemos também definir os limites dos eixos usando os parâmetros **xlim=** e **ylim=**



Vamos expandir o intervalo do eixo horizontal de 0 a 30 e definir apenas o valor mínimo do eixo vertical como 0:



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df.plot(x='b', y='a', title='A vs B', style='o', xlim=\[0, 30], ylim=0)**

**plt.show()**





##### Outras opções de personalização

Outras funcionalidades podem ser adicionadas aos gráficos se você achar que eles ficariam mais fáceis de ler. Vamos adicionar linhas de grade para ajudar a observar quais valores estamos desenhando. Para fazer isso, defina o parâmetro **grid=** com **True**. Por padrão, esse parâmetro é definido como **False**:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



**df.plot(x='b', y='a', title='A vs B', style='o', grid=True)**

**plt.show()**



Também podemos gerenciar o tamanho do gráfico com o parâmetro **figsize=**

A largura e o comprimento em polegadas são passados como uma lista: **figsize=\[largura, altura]**. Vamos comparar dois gráficos de tamanhos diferentes:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.DataFrame({'a':\[2, 3, 4, 5], 'b':\[4, 9, 16, 25]})**



\# construindo um gráfico pequeno

**df.plot(x='b', y='a', style='o', xlim=\[0, 30], figsize=\[2, 2])**



\# construindo um gráfico grande

**df.plot(x='b', y='a', style='o', xlim=\[0, 30], figsize=\[10, 4])**



**plt.show()**



##### Recapitulação



* ###### **plot()**: é o método da pandas que prepara uma imagem a partir dos dados.
* ###### **show()**: é a função da biblioteca pyplot da Matplotlib que apresenta uma imagem ao usuário.
* ###### **savefig()**: é a função da Matplotlib que salva o gráfico como um arquivo de imagem.
* ###### **title=**: é o parâmetro para definir o título do gráfico.
* ###### **style=**: é o parâmetro para alterar o estilo do marcador do gráfico.
* ###### **x=** e **y=**: são os parâmetros para construir duas colunas relacionadas entre si.
* ###### **xlabel=** e **ylabel**=: são os parâmetros para definir as legendas dos eixos.
* ###### **xlim**= e **ylim=**: são os parâmetros para definir os limites dos eixos.
* ###### **grid=**: é o parâmetro para adicionar linhas de grade.
* ###### **figsize=**: é o parâmetro para definir o tamanho do gráfico.
* ###### **legend=**: é o parâmetro para adicionar ou remover a legenda do gráfico.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Cálculo do coeficiente de correlação



Um valor de -1 representa uma perfeita correlação negativa, enquanto 1 representa uma perfeita correlação positiva. Em geral, o coeficiente de correlação funciona assim:



* Se um valor aumenta e o outro também aumenta, o coeficiente de correlação é positivo.
* Se um permanece o mesmo enquanto o outro muda, o coeficiente é 0.
* Se um diminui enquanto o outro aumenta, o coeficiente é negativo.



Quanto mais perto o coeficiente estiver de -1 ou 1, mais forte será a correlação. Por outro lado, um valor 0 significa que não existe correlação ou que existe uma conexão complexa não linear que o coeficiente não consegue mostrar.



##### Calculando o coeficiente de correlação



Na pandas, você pode calcular o coeficiente de correlação de Pearson usando o método **corr()**



Para fazer isso, aplique-o à coluna que contém a primeira variável e passe a coluna com a segunda variável como um parâmetro. A ordem das variáveis não importa. Por exemplo:



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**print(df\['height'].corr(df\['weight']))**



result:

**0.9165261045538688**



Com um coeficiente em torno de 0.9, vemos que a altura e o peso têm uma forte correlação positiva nesse conjunto de dados.

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_**



##### Construção de matrizes de dispersão



Na pandas, podemos construir matrizes de dispersão usando a função **plotting.scatter\_matrix():**



**import pandas as pd**

**from matplotlib import pyplot as plt**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**pd.plotting.scatter\_matrix(df, figsize=(9, 9))**

**plt.show()**





Em uma tarefa da última lição, você chamou o método **corr()** no DataFrame e obteve este resultado:



**import pandas as pd**



**df = pd.read\_csv('/datasets/height\_weight.csv')**



**print(df.corr())**



result:

          **height    weight       age      male**

**height  1.000000  0.916526  0.010042  0.760690**

**weight  0.916526  1.000000  0.228538  0.785218**

**age     0.010042  0.228538  1.000000  0.004750**

**male    0.760690  0.785218  0.004750  1.000000**



A tabela resultante é chamada de matriz de correlação. Essa matriz contém os coeficientes de correlação para cada par de colunas numéricas no DataFrame.



Coeficientes na diagonal do canto superior esquerdo ao canto inferior direito são sempre iguais a 1

Os elementos das diagonais acima (e abaixo) da diagonal principal contêm os coeficientes para todos os outros pares de colunas. 









































