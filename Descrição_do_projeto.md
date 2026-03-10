#### Neste projeto, você vai trabalhar com dados da Instacart.



Neste projeto, você vai precisar criar gráficos para apresentar seus resultados. Certifique-se de que todos os gráficos que você criar têm um título, eixos rotulados e uma legenda, se necessário; e inclua **plt.show()** no final de cada célula com um gráfico.



Sua missão é limpar os dados e preparar um relatório que forneça informações sobre os hábitos de compra dos clientes da Instacart. Após responder a cada pergunta, escreva uma breve explicação dos seus resultados em uma célula Markdown no seu notebook Jupyter.





#### Dicionário de dados



Há cinco tabelas no conjunto de dados, e você vai precisar usar todas elas para pré-processar os dados e fazer a AED. Abaixo está um dicionário que lista as colunas de cada tabela e descreve os dados contidos nelas.



* **instacart\_orders.csv**: cada linha corresponde a um pedido no aplicativo da Instacart
* **'order\_id'**: é o número de identificação exclusivo de cada pedido
* **'user\_id'**: é o número de identificação exclusivo da conta de cada cliente
* **'order\_number'**: é o número de vezes que o cliente fez um pedido
* **'order\_dow'**: é o dia da semana em que o pedido foi feito (0 é domingo)
* **'order\_hour\_of\_day'**: é a hora do dia em que o pedido foi feito
* **'days\_since\_prior\_order'**: é o número de dias desde que o cliente fez seu pedido anterior
* **products.csv**: cada linha corresponde a um produto exclusivo que os clientes podem comprar
* **'product\_id'**: é o número de identificação exclusivo de cada produto
* **'product\_name'**: é o nome do produto
* **'aisle\_id'**: é o número de identificação exclusivo de cada categoria de corredor do supermercado
* **'department\_id'**: é o número de identificação exclusivo de cada categoria de departamento do supermercado
* **order\_products.csv**: cada linha corresponde a um item incluído em um pedido
* **'order\_id'**: é o número de identificação exclusivo de cada pedido
* **'product\_id'**: é o número de identificação exclusivo de cada produto
* **'add\_to\_cart\_order'**: é a ordem sequencial em que cada item foi colocado no carrinho
* **'reordered':** 0 se o cliente nunca comprou o produto antes, 1 se já o comprou
* **aisles.csv**
* **'aisle\_id'**: é o número de identificação exclusivo de cada categoria de corredor do supermercado
* **'aisle':** é o nome do corredor
* **departments.csv**
* **'department\_id'**: é o número de identificação exclusivo de cada categoria de departamento do supermercado
* **'department'**: é o nome do departamento





#### Instruções para concluir o projeto



* Etapa 1: abra os arquivos de dados (**/datasets/instacart\_orders.csv**, **/datasets/products.csv**, **/datasets/aisles.csv**, **/datasets/departments.csv** e **/datasets/order\_products.csv**) e dê uma olhada no conteúdo geral de cada tabela.



* Observe que os arquivos têm formatação fora do padrão, então você vai precisar definir certos argumentos em **pd.read\_csv()** para ler os dados corretamente. Dê uma olhada nos arquivos CSV para ter uma ideia de quais deveriam ser esses argumentos.



* Observe que **order\_products.csv** contém muitas linhas de dados. Quando um DataFrame tem muitas linhas, i**nfo()** não imprime as contagens de valores não nulos por padrão. Se quiser imprimi-las, inclua **show\_counts=True** quando chamar **info()**.



* Etapa 2: faça o pré-processamento dos dados da seguinte maneira:



* Verifique e corrija os tipos de dados (por exemplo, certifique-se de que as colunas de ID sejam números inteiros)
* Identifique e preencha valores ausentes
* Identifique e remova valores duplicados
* Certifique-se de explicar que tipos de valores ausentes e duplicados você encontrou, como você os preencheu ou removeu, por que escolheu esses métodos e por que você acha que esses valores ausentes e duplicados estavam presentes no conjunto de dados.



* Etapa 3: quando os dados estiverem processados e prontos, execute a seguinte análise:



**\[A] (é necessário concluir tudo para passar)**



* Verifique se os valores nas colunas **'order\_hour\_of\_day'** e **'order\_dow'** na tabela orders fazem sentido (ou seja, os valores da coluna **'order\_hour\_of\_day'** variam de 0 a 23 e os da **'order\_dow'** variam de 0 a 6).
* Crie um gráfico que mostre quantas pessoas fazem pedidos a cada hora do dia.
* Crie um gráfico que mostre em que dia da semana as pessoas fazem compras.
* Crie um gráfico que mostre quanto tempo as pessoas esperam até fazer seu próximo pedido e comente sobre os valores mínimo e máximo.

**\[B] (é necessário concluir tudo para passar)**



* Há diferenças entre as distribuições de **'order\_hour\_of\_day'** na quarta-feira e no sábado? Construa histogramas para ambos os dias no mesmo gráfico e descreva as diferenças que você notou.
* Construa um gráfico de distribuição para o número de pedidos que os clientes fazem (ou seja, quantos clientes fizeram apenas 1 pedido, quantos fizeram apenas 2, quantos apenas 3, etc.)
* Quais são os 20 produtos comprados com mais frequência? Exiba os números de identificação (ID) e nomes.

**\[C] (é necessário concluir pelo menos duas perguntas para passar)**



* Quantos itens as pessoas normalmente compram em um pedido? Como fica a distribuição?
* Quais são os 20 principais itens incluídos mais frequentemente em pedidos repetidos? Exiba os números de identificação (ID) e nomes.
* Para cada produto, que proporção de pedidos em que ele aparece são pedidos repetidos? Crie uma tabela com colunas para ID do produto, nome do produto e a proporção de pedidos repetidos.
* Para cada cliente, que proporção dos produtos comprados são pedidos repetidos?
* Quais são os 20 principais itens que as pessoas colocam nos carrinhos antes de todos os outros? Exiba o ID do produto, nome e o número de vezes que ele foi o primeiro a ser adicionado a um carrinho.

























































































































































































































