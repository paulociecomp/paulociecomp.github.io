---
layout: post
title: Complexidade de algoritmos - Estrutura de Dados
---

Na faculdade, aprendemos de uma forma, não muito didática, como analisar performance e complexidade de algoritmos. Muitos desenvolvedores não precisam se aprofundar em conceitos de matemática concreta para desenvolver softwares empresariais.

No entanto, fundamentos de ciência da computação são importantes, principalmente quando lidamos com estruturas de dados complexas. Nesta série, iremos abordar como identificar e lidar com complexidade de algoritmos de uma forma inclusiva e pouco acadêmica.

## Estrutura de Dados

Computação é sobre manipular dados. Um dos grandes desafios que temos como desenvolvedores é descobrir formas eficientes de lidar com esses dados. Estruturas de dados são maneiras de organizar esses dados. Vamos ver que, a maneira como organizamos esses dados, causa impacto na performance de nossos programas. Uma função lenta que é executada só uma vez, talvez não cause problemas, mas quando ela for executada por milhares de usuários de uma aplicação web, podemos ter dores de cabeça.

O array é uma estrutura de dados bem básica, no entanto é extremamente versátil e muito utilizada. Vamos trabalhar com ela aqui.

```ruby
  array = ["ipa", "apa", "pilsen", "larger", "stout"]
```

Temos um array de 5 elementos string. O index é um número que identifica onde um determinado elemento está posicionado no array. Geralmente o index começa por 0. Então o elemento de index 0 é a "ipa", o de index 1 é a "apa", e assim por diante.

Para que possamos iniciar o estudo de performance, precisamos, primeiro, compreender como interagir com dos dados. Na maioria das estruturas de dados, temos basicamente 4 operações:

-   Leitura (Read)

-   Pesquisa (Search)

-   Inserção (Insert)

-   Deleção (Delete)

Quando mensuramos a velocidade de uma operação, não medimos o tempo que a operação leva para ser executada, mas sim o número de etapas que ela leva para ser concluída. Se executarmos uma função em um computador e ela levar 10 segundos, em outro hardware ela pode levar mais tempo, em um supercomputador da NASA pode levar milissegundos. Isso não nos diz muita coisa.

Mas podemos dizer que, se uma operação A precisou de 5 etapas para ser concluída e uma operação B precisou de 1000 etapas, a operação A tem velocidade maior que B em qualquer modelo de hardware.

## Velocidade das operações

Olhando para o array de 5 elementos, podemos identificar facilmente quais são esses elementos e onde estão posicionados, mas o computador não possui olhos, então ele não possui conhecimento dos elementos do array. O que o computador conhece é onde o array começa e qual o seu tamanho.

O que vemos:

```ruby
  ["ipa", "apa", "pilsen", "larger", "stout"]
```

O que o computador "vê":

```ruby
  "[?, ?, ?, ?, ?]
```

Para se obter um elemento do array, basta indicar qual seu index. Por exemplo, a[2] retorna a string "pilsen". Essa é a operação de **leitura** e ela possui apenas uma etapa, pois o ponteiro vai direto à posição indicada pelo index.

Diferente da leitura, quando queremos **pesquisar** algo no array, precisamos passar por várias etapas. Por exemplo, digamos que queremos saber se o array possui a string "larger".

-   Etapa #1: o computador verifica o index 0. Ele identifica que o valor é "ipa", diferente de larger.

-   Etapa #2: o computador vai para o index 1 e verifica que não dá match.

-   Etapa #3: o computador vai para o index 2. Também é diferente.

-   Etapa 4#: o computador vai para o index 3 e finalmente encontra a string "larger".

Para o nosso array de 5 elementos, foram necessárias 4 etapas para encontrar um elemento. Mas, e se tivéssemos pesquisado por "stout"? Na pior das hipóteses, teremos que percorrer o array inteiro para encontrar um elemento. Para um array de 500 elementos, teremos 500 etapas. Para um array de 1.000 elementos, teremos 1.000 etapas. Para um array de N elementos, teremos, no máximo **N** etapas para encontrar um elemento. Isso é uma **pesquisa linear**. Enquanto a leitura precisa de 1 etapa, a pesquisa linear precisa de N etapas, logo a pesquisa linear é uma operação menos eficiente do que a leitura.

Assim como a operação de pesquisa, a eficiência de uma operação de **inserção** vai depender de onde queremos inserir o elemento no array.

Para inserir um elemento no final do array é muito simples. O computador sabe onde o array começa e sabe qual o tamanho do array. Então ele efetua um cálculo para saber em qual posição da memória ele irá inserir o novo elemento e ele faz isso em apenas uma etapa.

```ruby
  array<< "porter"
```

```ruby
  ["ipa", "apa", "pilsen", "larger", "stout", "porter"]
```

Mas e se quisermos inserir o elemento na posição 3? Será necessário arrastar várias partes do array para liberar espaço para o novo elemento.

```ruby
  ["ipa", "apa", "pilsen", _____, "larger", "stout"]
```

-   Etapa 1: mover "stout" para a direita.

    ```ruby
      ["ipa", "apa", "pilsen", "larger", ____, "stout"]
    ```

-   Etapa 2: mover "larger" para a direita.

    ```ruby
      ["ipa", "apa", "pilsen", ____, "larger", "stout"]
    ```

-   Etapa 3: inserir "porter" no index 3.

    ```ruby
      ["ipa", "apa", "pilsen", "porter", "larger", "stout"]
    ```

O pior cenário é ter que inserir o elemento no início do array, pois, será necessário mover todos os elementos do array com N etapas para mover os elementos e 1 etapa para a inserção. Logo, temos **N + 1** etapas.

E, finalmente, para deletar um elemento de um determinado index, por exemplo, do index 2:

-   Etapa #1: "pilsen" é removida.

    ```ruby
      ["ipa", "apa", ____, "larger", "stout"]
    ```

-   Etapa #2: movemos "larger" para a esquerda.
  
    ```ruby
      ["ipa", "apa", "larger",____, "stout"]
    ```

-   Etapa #3: movemos "stout" para a esquerda.
    
    ```ruby
      ["ipa", "apa", "larger", "stout"]
    ```

Assim como a inserção, o pior cenário e ter que remover o primeiro elemento do array. Para um array de N elementos, teremos uma etapa para a deleção e N - 1 etapas para mover os elementos, logo a deleção precisa de **N** etapas.

Existe um tipo de array que possui algumas peculiaridades que podem ser úteis para melhorar a performance de algumas operações, o array ordenado.

Imagine o seguinte cenário:

```ruby
  [34, 64, 78, 32, 14, 45, 82]
```

Queremos pesquisar o número 13 no array acima, para isso será necessário percorrer o array inteiro para descobrirmos que o elemento não existe. Agora, e se o mesmo array fosse ordenado?

```ruby
  [14, 32, 34, 45, 64, 78, 82]
```

Bastaria uma etapa para sabermos que o elemento não está presente, pois o primeiro elemento é maior que 13.

A pesquisa linear é mais eficiente em um array ordenado comparado a um array comum. No entanto, de nada adianta se o elemento pesquisado estiver na última posição do array. Para este cenário precisamos pensar em algoritmos mais performáticos. E temos um muito bom.

## Pesquisa Binária

Imagine ter que adivinhar um número de 1 a 100, tendo dicas se o número é maior ou menor em relação ao seu chute. Sendo sagaz, como sei que você é, você escolherá 50. Seu oponente dirá que o número correto é menor que 50. Assim você exclui metade das possibilidades. Agora você, seguindo a mesma estratégia, escolhe 25. Seu oponente diz que é maior que 25. Nesse momento você restringiu as possibilidades para o intervalo entre 25 e 50. Você escolhe 36 e seu oponente diz que é menor que 36. Você escolhe 31. Seu oponente diz que é maior que 31. O que sobra são os números entre 31 e 36. Você escolhe 33, seu oponente diz que é menor. Você escolhe 32 e o jogo termina. Em 6 etapas vc descobriu o número em uma lista de 100. Isso que você fez foi uma **pesquisa binária**.

Como o computador faria?

```ruby
  a = [?, ?, ?, ?, ?, ? ,? ,? ,?]
```

Em um array ordenado de 9 elementos, vamos pesquisar pelo valor 13.

-   Etapa #1: selecionamos o elemento mais central. Para isso basta dividir o tamanho do array por 2, sendo que 9 / 2 = 4. Logo o elemento mais central é a[4].

    ```ruby
      [?, ?, ?, ?, 25, ? ,? ,? ,?]
      #Sendo que o valor é 25 e 13 é menor, 13 deve estar à esquerda.
    ```

-   Etapa #2: entre os elementos da esquerda, selecionamos o mais central. Como o número de elementos é par, tanto faz qual index escolher. Arbitrariamente escolhemos o index 1 e descobrimos que o valor é 5.

  ```ruby
    [?, 5, ?, ?, 25, ? ,? ,? ,?]
    #5 é menor que 13, logo 13 deve estar à direita.
  ```  

-   Etapa #3: como sobrou 2 elementos, arbitrariamente escolhemos um deles.

    ```ruby
      [?, 5, 9, ?, 25, ? ,? ,? ,?]
      #9 é menor que 13. Só pode ser o próximo elemento.
    ```

-   Etapa #4: inspecionamos o próximo elemento, se não estiver lá, 13 não pertence a este array e finalizamos a execução do programa.

  ```ruby
    [?, 5, 9, 13, 25, ? ,? ,? ,?]
    #Comparamos o elemento com o valor 13 e deu match.
  ```

Em 4 etapas, encontramos o elemento no array ordenado através da pesquisa binária.

Dá pra perceber que em um array pequeno, a pesquisa binária não tem tantas vantagens sobre o pesquisa linear. Mas a medida que o número de elementos cresce, o diferença de performance é exorbitante.

-   100 elementos:  
    Pesquisa linear: 100 etapas  
    Pesquisa binária: 7 etapas 

-   10.000 elementos:  
    Pesquisa linear: 10.000 etapas  
    Pesquisa binária: 13 etapas  

Um array ordenado não necessariamente significa mais performance. A inserção em arrays ordenados é mais lenta em comparação com arrays não ordenados. No entanto, a pesquisa em arrays ordenados tem performance muito superior. Tudo vai depender da necessidade de sua aplicação.

## Conclusão

Aprendemos que velocidade não se mede por tempo e sim por etapas. Vimos algumas operações e seus respectivos graus de eficiência em arrays. E percebemos que não só o tipo de estrutura de dados causa influência na performance, mas também o algoritmo empregado para executar operações. 

No próximo post, vamos entender como funciona a análise propriamente dita da complexidade de algoritmos, sem *blá blá blá* acadêmico, usando a famigerada **Notação O**, o que meu professor com PHD falhou miseravelmente em me explicar, mas que consegui aprender lendo um simples tutorial na internet.

Até lá.