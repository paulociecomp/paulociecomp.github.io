---
layout: post
title: Entendendo Feature Toggles
---

![https://unsplash.com/@markusspiske](https://images.unsplash.com/photo-1556217994-22de7face210?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1236&q=80)

Feature flags ou **feature toggles** podem ser artimanhas poderosas para nos auxiliar tanto na gestão de produtos quanto na gestão do ciclo de vida do software, e nos ajudar a alcançar o que eu considero o ***Valhalla*** do desenvolvimento de software, o ***continuous delivery***. 

Atingir o ***continuous delivery*** é uma tarefa árdua e impossível para algumas organizações. No entanto, feature toggles podem ser uma das armas que ajudam a eliminar inimigos como o branches hell, mas primeiro vamos mostrar como elas podem nos auxiliar no gerenciamento de features a nível de usuário, agindo como ferramentas poderosas para uso de estratégias de lançamento de novas funcionalidades, e depois veremos como elas agem no nível da engenharia de software.

## Features toggles e gestão de produto

Os desenvolvedores de produtos criam soluções baseadas, muitas das vezes, no que eles acreditam que as pessoas irão querer. Quem nunca teve aquela ideia revolucionária de aplicativo que mudaria o mundo para sempre? Bem… essas ideias nada mais são que hipóteses, e hipóteses merecem ser testadas.


Existem inúmeros métodos que nos auxiliam a testar hipóteses em nossos produtos. Para produtos novos, existe o **Lean Startup** e **Customer development** com conceitos como **Mínimo Produto Viável** e **Pivotar**. Quando já se possui uma base de usuários relevante, pode ser interessante usar **testes A/B** que é um tipo de teste em produção.

![https://unsplash.com/@markusspiske](https://blog.launchdarkly.com/wp-content/uploads/2015/10/ld_multivariate.png)

Imagine que seu time tem uma ideia de uma nova *feature*, mas não tem certeza de como seus usuários irão se comportar ao usá-la. Pode ser bem ruim para o produto se toda a sua base de usuários for para as redes sociais reclamar de uma feature da qual eles detestaram. Uma boa estratégia é colher feedbacks de apenas uma parcela desses usuários. É aqui que entra o poder das feature toggles.

Com os toggles, seu time pode lançar features em produção ativadas para apenas alguns grupos de usuários. Imagine que exista a necessidade de habilitar uma determinada feature apenas para usuários que morem em São Paulo, ou seu time deseje colher feedbacks apenas de usuários de um determinado perfil, como idade, profissão, interesses, etc.

Assim, ao invés de tentar adivinhar as necessidades dos usuários, colhemos feedbacks dos usuários reais, e através desses feedbacks tomamos alguma decisão que pode ser desde lançar a feature para todos os usuários, melhorar a feature ou, até mesmo, remover a feature. Desta forma, evoluímos nossos produtos através de dados reais e não somente hipóteses.

## Testes em Produção

Quando falarmos em teste de produção, pode nos vir à cabeça algo extremamente perigoso e amador. No entanto, quando estamos devidamente preparados, testes em produção podem ser bastante eficientes. 

Em muitos cenários pode ser extremamente trabalhoso e caro construir um ambiente de testes o mais parecido possível com o ambiente de produção. Pode ser muito difícil e complexo simular os dados reais, além de simular o comportamento dos usuários.

![https://unsplash.com/@markusspiske](https://blog.launchdarkly.com/wp-content/uploads/2015/10/ld_overview2.png)

Testes em produção podem nos auxiliar a receber feedbacks rápidos de usuários reais, ao invés de dados *mockados*, e nos auxiliam a encontrar e corrigir bugs de forma rápida. Para isso, podemos usar a mesma estratégia dos testes A/B, ativar a funcionalidade para apenas uma parcela dos usuários. Colhemos as métricas, corrigimos possíveis bugs que irão afetar apenas uma parte dos usuários, e podemos melhorar a feature com base nos feedbacks destes usuários, caso seja necessaŕio.

No entanto, para que esses testes possam ser eficientes é necessário um excelente sistema de **alerta** e **monitoramento**.

## Gestão de ciclo de vida de features

Existem cenários em que temos várias pessoas ou times trabalhando de forma concorrente em um mesmo projeto. O que normalmente ocorre é a criação de filas, onde temos de esperar um time concluir todo o ciclo de vida de uma feature para que possamos realizar o merge do próximo *pull request*. Isso não é muito eficiente, gera estoque, o que encarece o processo de desenvolvimento, além de potencializar conflitos entre pessoas.

Feature toggles no auxiliam a eliminar filas de deploys e até mesmo evitar o uso de *branches*. Podemos fazer deploy em produção de features desativadas que não causaram *side effects* na aplicação. Assim, poderemos *mergear* código na *branch* principal sem a necessidade de esperar que outras features que estejam na branch sejam lançadas em produção. Sem isso, jamais poderíamos pensar em continuous delivery. 

```java
  function reticulateSplines(){
    if( featureIsEnabled("use-new-SR-algorithm") ){
        return enhancedSplineReticulation();
    }else{
        return oldFashionedSplineReticulation();
    }
  }
```

As features toggle também ajudam quando ocorre algum erro no deploy. Neste cenário, não é necessário fazer *rollback* do deploy ao acionar a equipe de infraestrutura, pois, dependendo do sistema gerenciador de toggles que você usa, basta um clique em um botão para desativar a feature que foi *deployada*. Assim, até mesmo pessoas de negócio podem desativar a feature em casos de problemas.

É importante entender que as feature toggles são complexidade inserida no código. Podemos encará-las como **débitos técnicos** que precisam ser tratados pelo time. Quando o toggle não é mais necessário, é recomendável sua remoção do código. Logo, é preciso gerenciar o ciclo de vida das feature toggles.
Também é muito importante escrever testes automatizados para cenários em que as feature toggles estejam ativadas e desativadas. Isso garante segurança no processo de remoção dos toggles, além de garantir o comportamento da aplicação em ambos os estados do toggle.

## Conclusão

O uso de feature toggles é muito importante para testar hipóteses de negócio de uma forma eficiente, além de garantir testes em produção de maneira mais segura. As toggles também nos auxiliam no ciclo de vida da aplicação, ajudando a evitar o uso de branches e alcançar o **continuous delivery**. No entanto, elas geram uma complexidade extra no código, sendo necessário a criação de uma gestão de feature toggles. Apesar disso, elas se mostram uma poderosa ferramenta para desenvolvimento ágil de produtos de software.
