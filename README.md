# GPU-KNN Classification



## Conjunto de dados

O conjunto de dados é relacionado a presença ou não do câncer de mama em diversos pacientes, e contém diversos atributos, na maioria numméricos. O conjunto de dados foi retirado do Kaggle e pode ser obtido no seguinte link

[https://www.kaggle.com/datasets/yasserh/breast-cancer-dataset](https://www.kaggle.com/datasets/yasserh/breast-cancer-dataset)

Abaixo podemos ver alguns valores do conjunto de dados


![Screenshot from 2023-05-22 15-35-25](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/65ac9d28-037b-4b6e-bd05-eea01872cb87)



Além disso, algumas informações sobre o conjunto de dados foram descobertas:

O conjunto possuia apenas valores numéricos, oque iria ajudar muito o classificador que seria utilizado. O conjunto não possui valores NULOS e possuia em torno de 600 linhas de 32 atributos.

## Analisando atributos

Após importar o conjunto de dados foi necessário realizar uma análise nos atributos para evitar qualquer problema.

Inicialmente foi verificado se havia um desbalanceamento entre as classes por meio da visualização. Felizmente, como se pode ver abaixo, não existia.

![Screenshot from 2023-05-22 15-38-55](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/bb825c06-2454-44cf-98f3-f2c0f07ea215)


### Análise de correlação

Também, no projeto foi verificado a correlação entre os atributos para verificar se haviam atributos redundantes, ou seja, atributos que poderiam ser removidos sem que haja perda de informação. Para isso estimulou-se um limite de 0.9 de correlação. Assim pode-se remover os seguintes atributos:

![Screenshot from 2023-05-22 15-40-23](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/bc837410-ded5-4ca0-a80f-db8687d3aa64)



### Análise de outliers

Para verificar os outliers, foi realizado uma visualização bem por cima, utilizando o desvio padrão dos atributos do conjunto de dados. Apenas um deles se destacava bastante, possuindo um desvio padrão de mais de 500. Assim foi necessário verificá-lo para entender como estava distribuido e quais eram suas características.

Inicialmente a distribuição do atributo foi plotada para podermos entender como funcionava a distribuição numérica do atributo. Foi possível analisar que a maioria dos valores era menores que 1000 e muitos poucos valores estava acima disso.

![Screenshot from 2023-05-22 15-43-31](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/a1a9866a-8c2c-4526-82ee-9f7e0e381822)



Mesmo que ista pareça outliers, foi possível compreender que isto estava relacionada com a classe alvo do exemplo. Com uma rápida verificação foi possível entender que a maioria dos exemplos com classe positiva, ou seja, com câncer de mama continham este atributo com valor superiror a 1000, como se pode verificar abaixo:

![Screenshot from 2023-05-22 15-44-43](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/5bf46810-5293-47c4-9166-5ccacc7dcbb5)


Isto representava quase 60% dos valores com a classe alvo positiva. Assim, foi decidido que este atributo não sofreria nenhuma alteração visto que era importante haver essa diferença no atributo.

## Preparativos para classificação

Após a análise e pré-processamento do conjunto de dados eram necessários mais duas 2 tarefas: Separar em treino e teste e normalizar os dados.

### Normalização

Esta tarefa era necessária pois como o algoritmo que estava sendo utiilizado trabalha com cálculo de distâncias e similaridade caso existam discrepâncias muito grandes entre os possíveis valores dos atributos o modelo pode entender que um atributo deve predominar e desta maneira perder a capacidade de generalização, assim foi realizado um minmaxscaler.

### Separar em treino e teste

Como estamos lidando com aprendizado de máquina supervisionado, foi necessário dividir o conjunto de dados. Para isso foi utilizado 30% do conjunto de dados para teste e 70% para treino. Além disso, é importante falar que a normalização foi realizada localmente nos conjuntos de dados.

## Classificação

Para realizar a classificação foi utilizado o KNN, que é o modelo que classifica um ponto baseado em sua vizinhança, ou seja, com base nos seus k vizinhos mais próximos. Neste projeto foram apresentados 3 algoritmos que encontravam os k vizinhos mais próximos diferentes, sendo dois deles métodos aproximados e um deles um método exato. Isso foi realizado para mostrar a diferença entre cada um e também entender que métodos aproximados tem o benefício de conseguir trabalhar com conjuntos de dados mais volumosos, o que torná a maioria deles escaláveis. Além disso, estes algoritmos estavam implementados em GPU e utilizaram da paralelização para serem melhorados. Além disso, a ferramenta utilizada consegue facilmente ser adaptada para um sistema multi-GPU promovendo uma classificação para conjuntos muito grandes, acima de 100 milhões de exemplos.

Foi possível entender que o algoritmo IVFPQ utilizava menos memória que os outros e mesmo assim manteve a qualidade na classificação. Logo todos os futuros experimentos foram realizados com este algoritmo.

O primeiro foi uma busca pelo hiperparâmetro, variando alguns parâmetros, o principal deles foi o número de vizinhos ‘’k’’

![Screenshot from 2023-05-22 15-53-53](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/a4802495-1fcf-4772-a784-d9c445b2bece)


## Experimento final

Após descobrir os hiperparâmetros realizou-se um teste final onde iria ser compreender melhor o resultado. Para isso foi escolhido k = 13. e iniciou-se o teste.

Nesse caso utilizou-se de matriz de confusão e classification report para se analisar o resultado.

![Screenshot from 2023-05-22 15-54-55](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/092e721b-731e-499e-8fb3-06918df43b18)
![Screenshot from 2023-05-22 15-55-04](https://github.com/gorlando04/GPU-KNN-Classification/assets/91696970/9776ecb9-2ced-4c76-bac0-ff032c4507b7)


Com isso, foi possível entender que o modelo conseguiu obter um bom resultado mesmo sendo aproximado. 

## Conclusão

Após a realização deste projeto foi possível compreender o funcionamento do algoritmo de kNN para a classificação binária de dados. Além disso, foi possível entender a capacidade que métodos aproximados tem de conseguir substituir a altura métodos exatos, utilizando muito menos tempo e memória. Também, pode-se verificar que GPUs são extremamante benéficas para tarefas de aprendizado de máquina já que permitem uma aceleração do modelo. Finalmente, pode-se realizar uma pequena busca de hiperparâmetros para verificar qual era o mais adequado neste caso.

É importante ressaltar que a estrutura utilizada pode ser adequada para um sistema multi-GPU Que iria mostrar uma capacidade de processar rapidamente um conjunto de dados e conjuntos de dados extensos, em torno de mais de 100 milhões de amostras poderiam ser processados.
