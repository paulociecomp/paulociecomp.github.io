---
layout: post
title: Postgres com docker
---

![https://unsplash.com/@markusspiske](https://images.unsplash.com/photo-1531355618229-d19728b4581e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1374&q=80)

Quando desenvolvemos software, precisamos configurar o ambiente em nossas máquinas para que possamos trabalhar. Para isso, precisamos instalar uma série de dependências necessárias para o desenvolvimento. Lotamos nosso *HD* com várias instalações e, muitas das vezes, perdemos o controle das diversas versões de aplicativos e bibliotecas instalados, pois quando trabalhamos em muitos projetos diferentes, teremos versões diferentes das dependências desses projetos.

Hoje, existem diversas ferramentas que nos auxiliam com o gerenciamento de versões de bibliotecas. Mas, não só de bibliotecas dependem nossos projetos. Também teremos outras aplicações como dependência, e uma dessas aplicações é o **sistema gerenciador de banco de dados**.

Neste post, iremos descobrir como gerenciar versões do Postgres, usando o **docker**, que não é um gerenciador de versões, mas nos auxilia nessa questão, fazendo assim, que não seja necessário instalar o Postgres em nossa máquina, deixando ele rodando dentro de um *container*.

## Docker

Não iremos focar na instalação do docker aqui. Você pode instalá-lo através deste [link](https://docs.docker.com/engine/install/){:target="_blank"} no site oficial. Após a instalação, precisaremos selecionar uma imagem.

O [Docker Hub](https://hub.docker.com/){:target="_blank"} é o repositório oficial de imagens docker. Podemos, inclusive, subir nossas próprias imagens customizadas lá. Muitas empresas criam imagens oficiais e as publicam no **docker hub**, mas também existem diversas imagens criadas pela comunidade. As imagens docker são como imagens de disco, já pré-configuradas com diversas ferramentas. O Postgres possui sua imagem oficial com o PostgreSQL instalado e pronto para uso, e é ela que vamos utilizar aqui.

Na [página da imagem](https://hub.docker.com/_/postgres){:target="_blank"}, está disponível uma documentação de como utilizá-la. Nela, vemos as diversas versões da imagem que pode ser identificada através de tags. Cada tag identifica qual versão do Postgres aquela versão da imagem docker está utilizando.

![alt text](/images/docker_hub_page.png "Tags")

Pode ser que, por algum motivo, nosso projeto use uma versão específica do Postgres. Para manter a compatibilidade, vamos usar a mesma versão, bastando identificar a tag. Neste exemplo, vamos usar a versão **10.12**.

```shell
  docker run --name meu-postgres -e POSTGRES_PASSWORD=senhalouca -d -p 5432:5432 postgres:10.12
```

O comando docker run vai fazer **pull** da imagem a partir do repositório e vai criar e executar um container.

- **--name** é o nome do container que, nesse caso é **meu-postgres**.

- **-e** indica que vamos usar variáveis de ambiente. Essas variáveis estão disponíveis na documentação da imagem. Nesse caso, estamos *setando* a senha da conexão na variável **POSTGRES_PASSWORD**.

- **-d** significa que o *console* vai ser liberado ao fim do comando e o container irá rodar como um *daemon*.

- **-p** indica que vamos fazer ***bind*** de portas no formato ***-p porta_do_host:porta_do_container***. Significa que a porta do **host** vai *escutar* a porta do container. Sem isso, só é possível conectar ao Postgres de dentro do container. As portas do **host** e do **container** não precisam ser iguais.

- **postgres:10.12** indica qual imagem e sua respectiva versão será usada pelo container.

Com o comando **docker ps**, podemos listar todos os containers em execução. Agora, basta conectar à instância do Postgres. O usuário padrão dessa imagem é o usuário **postgres** e a senha foi definida na variável de ambiente **POSTGRES_PASSWORD**.

![alt text](/images/conexao.png "Conexão com o postgres")

O comando para **parar** a execução do container:

```shell
  docker stop id_do_conainer
```

Comando para **listar** todos os containers:

```shell
  docker ps -a
```

Comando para **iniciar** um container:

```shell
  docker start id_do_container
```

Ao reiniciar a máquina, ou ***stopar*** o container com o comando **stop**, os dados desse container se perderão, ou seja, tudo que salvamos no banco de dados vai ser perder. Para a evitar isso, usaremos um recurso do docker chamado **volumes**, passando um novo parâmetro no comando.

Caso já exista um container com o mesmo nome, será preciso removê-lo com o comando:

```shell
  docker rm id_do_container
```

Lembre-se que para listar containers que não estão em execução, basta o comando **docker ps -a**.

Eis o comando com o volume de dados do Postgres:

```shell
  docker run --name meu-postgres -e POSTGRES_PASSWORD=senhalouca -d -p 5432:5432 -v $HOME/projetos/docker-postgres/data:/var/lib/postgresql/data postgres:10.12
```

O comando **-v** monta em **$HOME/projetos/docker-postgres/data**, que é um *path* da máquina host, o volume **/var/lib/postgresql/data** que é um diretório localizado dentro do container. Dessa forma, os dados dos Postgres serão **persistidos** mesmo após o container ser removido. Esses dados ficarão em **$HOME/projetos/docker-postgres/data** que pode ser qualquer diretório que você escolher em sua máquina (host).

## Conclusão

Com isso temos uma instância do Postgres rodando dentro de um container isolado, sem a necessidade de instalar e configurar o sistema de banco de dados em nossa máquina.