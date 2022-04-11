---
title: Desenvolvendo uma GraphQL API com Django
date: "2020-03-17T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/django-graphql-api/"
category: "Django"
tags:
  - "Python"
  - "Programação"
  - "Django"
  - "Web"
description: "Através desse tutorial seremos capazes de aprender como desenvolver uma GraphQL API com Django e Graphene-Python."
---

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/Imagens/Capas/Django-Graph-QL.png)

# Conteúdo

1. [Introdução](#introdução)
2. [Configurando a Aplicação](#configurando-a-aplicação)
3. [Desenvolvendo o Projeto](#desenvolvendo-o-projeto)
4. [Conclusão](#conclusão)
5. [Referências](#referências)

# Introdução

As **[APIs (Application Programming Interfaces)](https://developer.mozilla.org/en-US/docs/Glossary/API)** são construções disponibilizadas nas linguagens de programação para permitir que os desenvolvedores criem funcionalidades complexas com mais facilidade. Eles abstraem o código mais complexo, fornecendo uma sintaxe mais fácil de usar em seu lugar.

Elas permitem que os desenvolvedores integrem duas partes de um aplicativo ou aplicativos diferentes juntos. Ela consiste de vários elementos, como funções, protocolos e ferramentas que permitem aos desenvolvedores criar aplicativos. Um objetivo comum de todos os tipos de APIs é acelerar o desenvolvimento de aplicativos, fornecendo uma parte de sua funcionalidade pronta para uso, para que os desenvolvedores não precisem implementá-la. Existem APIs para todos os tipos de sistemas, incluindo sistemas operacionais, bibliotecas e a Web.

## Web APIs

Uma **Web API** é um tipo exclusivo de interface em que a comunicação ocorre usando a Internet e protocolos específicos da Web. Assim como as APIs remotas fazem os recursos remotos parecerem locais, as APIs da Web fazem o mesmo com os recursos disponíveis na Web. De fato, as Web APIs começaram a se popularizar com o advento dos serviços de Internet que permitiram aos usuários armazenar conteúdo online. 

De modo geral, você serve Web APIs por meio de uma interface **[HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)**. A própria API define um conjunto de terminais, solicita mensagens e estruturas de resposta. É uma abordagem padrão também para identificar os tipos de mídia de resposta suportados. XML e JSON são dois exemplos favoritos de tipos de mídia de resposta que podem ser facilmente interpretados pelos consumidores da API. Embora inicialmente as Web APIs também fossem chamadas de serviços da Web, hoje em dia o uso deste último formulário sinaliza que a API é RESTful, em vez de seguir o padrão SOAP.

O GraphQL é um novo padrão de API que fornece uma alternativa mais eficiente, poderosa e flexível ao REST. Foi desenvolvido pelo Facebook e é de código aberto e atualmente é mantido por uma grande comunidade de empresas e indivíduos de todo o mundo.

## Sobre GraphQL

GraphQL é uma linguagem de consulta para sua API.

Ele fornece uma maneira padrão de:

- Descrever dados fornecidos por um servidor em um *[statically typed Schema](https://graphql.org/learn/schema/)*
- Solicitar dados em uma *[Query](https://en.wikipedia.org/wiki/Query_language)* que descreva exatamente seus requisitos de dados
- Receber dados em uma *Response* contendo apenas os dados solicitados

O GraphQL não é uma arquitetura de API como o REST, é uma linguagem que nos permite compartilhar dados relacionados de uma maneira muito mais fácil. Na sua essência, o GraphQL permite a busca declarativa de dados, onde um cliente pode especificar exatamente quais dados precisa de uma API. Em vez de vários *endpoints* que retornam estruturas de dados fixas, um servidor GraphQL expõe apenas um único *endpoint* e responde exatamente com os dados solicitados pelo cliente.

Este **[artigo](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/)** ilustra muito bem a diferença entre **GraphQL** e **REST**

## A Biblioteca Graphene

O Graphene é uma biblioteca que fornece ferramentas para implementar uma API GraphQL em Python.

Graphene é totalmente caracterizado com integrações para frameworks da Web e ORMs mais populares. Ele produz schemas que são totalmente compatíveis com as especificações do GraphQL e fornece ferramentas e padrões para a criação de uma API compatível com retransmissão.

Com o Graphene, não precisamos usar a sintaxe do GraphQL para criar um *schema*, usamos apenas o Python! Esta biblioteca open-source também foi integrada ao Django para que possamos criar esquemas referenciando os modelos de nossos aplicativos!

## Graphene-Django

O **Graphene-Django** é construído sob o Graphene. O Graphene-Django fornece algumas abstrações adicionais que facilitam a adição da funcionalidade GraphQL ao nosso projeto Django.

Agora que estamos brevemente familiarizados com as tecnologias que iremos utilizar nesse tutorial, podemos dar início à configuração de nosso projeto e iniciar o desenvolvimento de nossa GraphQL API, projeto que irá expor dados sobre artistas musicais.

# Configurando a Aplicação

## Diretórios

Iniciando do princípio vamos então criar o diretório de nosso projeto

```
mkdir projeto
```

Navegamos até nosso projeto através do comando `cd`

```
cd projeto/
```

## Ambiente Virtual

Agora que estamos dentro do diretório `projeto`, vamos criar um **ambiente virtual**, de forma que possamos manter todas as dependências de nosso projeto isolados de nosso sistemas ou até mesmo de outros projetos.

Você pode utilizar este **[Guia](https://github.com/the-akira/Python-Iluminado/blob/master/Capitulos/29.PIP.md)** para mais detalhes sobre os ambientes virtuais.

Assumindo que estamos agora com a versão do Python 3.6 ou superior, vamos criar um ambiente virtual através do seguinte comando

```
python -m venv ambientev
```

Agora que temos nosso ambiente criado, é necessário ativá-lo para assim podermos instalar as bibliotecas de nosso projeto

```
source ambientev/bin/activate
```

Imediatamente nosso terminal de comandos será prefixado com `(ambientev)` significando que estamos com este ambiente virtual ativado e toda biblioteca Python será instalada nele. Caso queriamos desativar este ambiente devemos digitar o comando `deactivate`.

## Instalando Bibliotecas

Com o nosso ambiente virtual ativado, vamos então utilizar `pip` para instalar o framework Django e a biblioteca Graphene, ferramentas fundamentais de nosso projeto.

```
pip install django
pip install graphene-django
```

## Criando o Projeto

Agora que temos Django instalado em nosso ambiente virtual, vamos inicializar a criação do projeto com o seguinte comando

```
django-admin startproject graphqlapi
```

Uma aplicação Django consiste de vários **apps**, estes que são componentes reutilizáveis dentro de um projeto, sendo assim, vamos criar um **app** específico para os recursos de nossa API. Primeiramente vamos navegar até o diretório principal de nosso projeto

```
cd graphqlapi/
```

Criamos então nosso **app** resources

```
django-admin startapp resources
```

Uma vez que o **app** foi criado, agora vamos sincronizar nosso banco de dados pela primeira vez

```
python manage.py migrate
```

Com o banco de dados inicializado, agora podemos começar a editar os arquivos do projeto.

## Configurando os Arquivos

Dentro do arquivo `graphqlapi/settings.py`, vamos buscar uma lista Python com o nome `INSTALLED_APPS` e vamos adicionar o seguinte a ela

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'resources',
    'graphene_django',
]
```

E no final do arquivo vamos adicionar

```python
GRAPHENE = {
    'SCHEMA': 'graphqlapi.schema.schema',
}
```

Finalmente podemos então executar o projeto com o comando `python manage.py runserver` e visitá-lo no endereço `http://127.0.0.1:8000/`. Agora que temos uma base de projeto definida e configurada, podemos dar início a criação de nossos **modelos** e **schemas**.

# Desenvolvendo o Projeto

## Definindo o Modelo

Os modelos do Django descrevem o layout do banco de dados do nosso projeto. Cada modelo é uma classe Python que geralmente é mapeada para uma tabela de banco de dados. As propriedades da classe são mapeadas para as colunas do banco de dados.

Vamos então editar o arquivo `resources/models.py` para criar nossos modelos

```python
from django.db import models

class Disco(models.Model):
    titulo = models.CharField(max_length=100)
    ano_lancamento = models.DateField(null=True, blank=True)

    def __str__(self):
        return self.titulo

    class Meta:
        ordering = ['titulo']

class Artista(models.Model):
    nome = models.CharField(max_length=100)
    origem = models.CharField(max_length=100)
    discos = models.ManyToManyField(Disco)

    def __str__(self):
        return self.nome

    class Meta:
        ordering = ['nome']
```

Observe que definimos duas classes, respectivamente:

- **Disco**: Representa um disco lançado por um artista, possui os campos `titulo` e `ano_lancamento`.

- **Artista**: Representa um artista musical, com os campos `nome`, `origem` e `discos`, que por sua vez está relacionado com a classe **Disco** através de uma **[Many-to-many relationships](https://docs.djangoproject.com/en/3.0/topics/db/examples/many_to_many/)**, dessa maneira um Artista pode possuir diversos discos.

Tendo nosso modelo compreendido, agora devemos aplicar as migrações para que o Django reconheça as mudanças que fizemos no banco de dados, executamos então

```
python manage.py makemigrations
```

Nossos modelos serão criados, finalmente devemos executar as migrações

```
python manage.py migrate
```

Django irá automaticamente sincronizar o banco de dados, precisamos agora registrar os nossos modelos no painel administrativo do Django para que possamos inserir dados em nossa aplicação por meio de uma interface gráfica amigável.

Para esta tarefa precisamos editar o arquivo `resources/admin.py` com o seguinte conteúdo

```python
from django.contrib import admin
from resources.models import Artista, Disco

classes = [Artista, Disco]

for c in classes:
	admin.site.register(c)
```

E vamos criar um super-usuário para acessarmos a área administrativa privada de nosso projeto

```
python manage.py createsuperuser
```

Preencha seus dados, escolha uma senha segura e vamos executar nosso servidor

```
python manage.py runserver
```

Visitamos o endereço `127.0.0.1:8000/admin/` para fazer login no painel administrativo e imediatamente veremos que nossos modelos estão todos registrados, já podemos começar a inserir dados experimentais para os testes que iremos fazer com GraphQL, então escolha seus artistas e discos favoritos e vamos para a próxima etapa.

## GraphQL - Schema e Object Types

Para executarmos *queries* ao nosso projeto Django, precisaremos de elementos essenciais:

- O **Schema** com object types definidos
- Uma **view** que receberá *queries* como input e nos trará os dados como resultado

O GraphQL apresenta nossos objetos ao mundo como uma estrutura de grafo, em vez de uma estrutura mais hierárquica à qual você pode estar acostumado. Para criar essa representação, Graphene precisa conhecer cada tipo de objeto que aparecerá no gráfo.

Este grafo também possuirá um *root type* no qual todos os acessamos iniciarão, ele será representado pela classe `Query`.

Para cada um de nossos modelos iremos criar um *type* que herdará o `DjangoObjectType`.

Criados os dois *types*, vamos listá-los como campos na classe `Query`.

De forma a cumprir todas essas tarefas precisamos criar um arquivo `resources/schema.py` com o seguinte conteúdo

```python
import graphene
from graphene_django.types import DjangoObjectType
from resources.models import Artista, Disco

class ArtistaType(DjangoObjectType):
    class Meta:
        model = Artista

class DiscoType(DjangoObjectType):
    class Meta:
        model = Disco

class Query(object):
    artista = graphene.Field(ArtistaType, id=graphene.Int())
    disco = graphene.Field(DiscoType, id=graphene.Int())
    artistas = graphene.List(ArtistaType)
    discos = graphene.List(DiscoType)

    def resolve_artista(self, info, **kwargs):
        id = kwargs.get('id')

        if id is not None:
            return Artista.objects.get(pk=id)
        return None

    def resolve_disco(self, info, **kwargs):
        id = kwargs.get('id')

        if id is not None:
            return Disco.objects.get(pk=id)
        return None

    def resolve_artistas(self, info, **kwargs):
        return Artista.objects.all()

    def resolve_discos(self, info, **kwargs):
        return Disco.objects.all()
```

Veja que a classe **Query** acima é um mixin, herdado do **objeto**. Isso ocorre porque agora criaremos uma classe de query no nível do projeto que combinará todos os nossos mixins no nível do aplicativo.

Definimos os dois types `ArtistaType` e `DiscoType` respectivamente e perceba também que cada propriedade da classe `Query` corresponse à uma **query GraphQL**.

- As propriedades **artista** e **disco** retornam um valor de `ArtistaType` e `DiscoType` e ambas requerem um **ID** que é um número inteiro
- As propriedades **artistas** e **discos** uma lista dos types respectivos

Por fim, os quatro métodos que criamos são chamados de **resolvers**, eles conectam as *queries* no *schema* às ações reais realizadas pelo banco de dados. Como é padrão no Django, interagimos com nosso banco de dados através de modelos.

Considere o método `resolve_artista`, nós capturamos O ID do parâmetro da *query* e retornamos o artista com o ID respectivo de nosso banco de dados. Já o método `resolve_artistas` trará todos os registros de artistas em nosso banco de dados como uma lista.

No diretório principal de nossa aplicação `graphqlapi`

```
graphqlapi/
├── asgi.py
├── __init__.py
├── settings.py
├── urls.py
└── wsgi.py
```

Vamos criar o arquivo `schema.py` com o conteúdo

```python
import graphene
import resources.schema

class Query(resources.schema.Query, graphene.ObjectType):
    # Essa classe irá herdar de múltiplas Queries
    # Ao começarmos a adicionar mais apps ao nosso projeto
    pass

schema = graphene.Schema(query=Query)
```

Podemos imaginar esse arquivo como similar ao arquivo `urls.py` de nível superior, agora finalmente podemos registrar nossas **views** para posteriormente testarmos as *queries*.

## Criando GraphiQL Views

O GraphiQL é um IDE GraphQL interativo gráfico no navegador. Em outras palavras, um playground para executarmos nossos testes. 

Diferente de uma API RESTful, existe apenas uma URL a partir da qual o GraphQL é acessado. As solicitações para este URL são tratadas pela visualização `GraphQLView` do **Graphene**.

Essa **view** servirá como *endpoint* do GraphQL. Como queremos ter o GraphiQL acima mencionado, especificamos nos parâmetros como `graphiql = True`.

Editamos então o arquivo `graphiqlapi/urls.py` com o conteúdo

```python
from django.contrib import admin
from django.urls import path
from graphene_django.views import GraphQLView

urlpatterns = [
    path('admin/', admin.site.urls),
    path('graphql/', GraphQLView.as_view(graphiql=True)),
]
```

Certificamos-nos que nosso servidor está executando normalmente e nos dirigimos então ao endereço `http://127.0.0.1:8000/graphql/` e agora podemos testar nossa API!

## Executando Queries

Lembre de criar dados de teste para que nossas consultas não retornem vazio, feito isso, vamos iniciar selecionando os Discos de nosso banco de dados

```
query Discos {
  discos{
    id
    titulo 
    anoLancamento
  }
}
```

Podemos selecionar também um Disco específico, para isso precisamos informar o parâmetro **id**

```
query Disco {
  disco(id: 2) {
    id
    titulo 
    anoLancamento
  }
}
```

Podemos também selecionar todos os artistas ou apenas um artistas específico

```
query Artistas {
  artistas {
    id
    nome
  }
}
```

Selecionado o artista de `id = 1`

```
query Artistas {
  artista(id: 1) {
    id
    nome
    origem
  }
}
```

Ou até mesmo selecionar os artistas e seus respectivos discos

```
query Artistas {
  artistas {
    id
    nome
    origem
    discos {
      titulo 
      anoLancamento
    }
  }
}
```

Perceba que estamos conseguindo selecionar os dados com sucesso, nossas *queries* estão funcionando, nosso objetivo agora é criar *mutations* que nos permitirão enviar dados para o servidor e inserí-los em nosso banco de dados.

## Criando Mutations

As **Mutations queries** modificam os dados no banco de dados e retornam um valor. Pode ser usado para inserir, atualizar ou excluir dados. Mutações são definidas como parte do schema, vamos agora implementá-las para possamos inserir artistas e discos em nosso projeto.

Primeiramente começaremos definindo os **inputs**, para isso vamos novamente editar o arquivo `resources/schema.py` e anexar o seguinte conteúdo

```python
class DiscoInput(graphene.InputObjectType):
    id = graphene.ID()
    titulo = graphene.String()
    ano_lancamento = graphene.String()

class ArtistaInput(graphene.InputObjectType):
    id = graphene.ID()
    nome = graphene.String()
    origem = graphene.String()
    discos = graphene.List(DiscoInput)
```

Cada uma dessas classes define quais campos podem ser utilizados para alterar dados na API.

Vamos agora criar uma mutação para Discos, no mesmo arquivo anexe

```python
class CreateDisco(graphene.Mutation):
    class Arguments:
        input = DiscoInput(required=True)

    disco = graphene.Field(DiscoType)

    @staticmethod
    def mutate(root, info, input=None):
        disco_instance = Disco(titulo=input.titulo)
        disco_instance.save()
        return CreateDisco(disco=disco_instance)
```

Observe que esta classe herda de `graphene.Mutation`, definimos os argumentos que ela irá receber, instanciamos um type disco e criamos um método estático que irá inserir os dados passados como parâmetro.

Agora vamos criar nossa última mutação, anexe no mesmo arquivo

```python
class CreateArtista(graphene.Mutation):
    class Arguments:
        input = ArtistaInput(required=True)

    artista = graphene.Field(ArtistaType)

    @staticmethod
    def mutate(root, info, input=None):
        discos = []
        if input.discos:
            for disco_input in input.discos:
                disco = Disco.objects.get(pk=disco_input.id)
                if disco is None:
                    return CreateArtista(disco=None)
                discos.append(disco)
        artista_instance = Artista(
            nome=input.nome,
            origem=input.origem
          )
        artista_instance.save()
        artista_instance.discos.set(discos)
        return CreateArtista(artista=artista_instance)
```

Estamos seguindo o mesmo procedimento dos Discos, porém dessa vez estamos também inserindo um **id** referente ao disco, caso não seja passado nenhum **id** de disco como parâmetro, utilizaremos **None**.

Por fim precisamos apenas registrar a **Mutation type**, anexe nesse mesmo arquivo

```python
class Mutation(graphene.ObjectType):
    create_disco = CreateDisco.Field()
    create_artista = CreateArtista.Field()
```

Para que nossas mutações possam finalmente funcionar, precisamos editar o arquivo `schema.py root`, vamos deixá-lo da seguinte forma

```python
import graphene
import resources.schema

class Query(resources.schema.Query, graphene.ObjectType):
    pass

class Mutation(resources.schema.Mutation, graphene.ObjectType):
    pass

schema = graphene.Schema(query=Query, mutation=Mutation)
```

De forma a garantir que nossas mutações estão funcionando corretas, vamos escrever alguns testes!

## Escrevendo Mutations

As mutações seguem um modelo similar às *queries*, vamos adicionar um disco ao nosso banco de dados para entendermos melhor

```
mutation createDisco {
  createDisco(input: {
    titulo: "Octavarium"
  }) {
    disco {
      id
      titulo
    }
  }
}
```

Observe como o parâmetro de input corresponde às propriedades de entrada das classes `Arguments` que criamos anteriormente.

Agora podemos adicionar um disco que o Dream Theater lançou

```
mutation createArtista {
  createArtista(input: {
    nome: "Dream Theater",
    discos: [
      {
        id: 2
      }
    ]
    origem: "US"
  }) {
    artista{
      id
      nome
      discos {
        id
        titulo 
      }
    }
  }
}
```

Veja que conseguimos inserir dados com sucesso em nossa aplicação, nosso objetivo está finalmente concluído e temos uma GraphQL API executando com sucesso e com a funcionalidade de inserção de dados.

# Conclusão

Através desse tutorial estivemos aptos a aprender sobre a nova tecnologia GraphQL e como utilizar ela em conjunto com o framework Django através da biblioteca Graphene que nos permitiu facilmente criarmos uma API para assim consultarmos dados de nosso banco de dados e até mesmo inserí-los. Você pode aprimorar este projeto adicionando a funcionalidade de atualizar os dados ou até mesmo deletá-los.

O código fonte do projeto pode ser encontrado no GitHub: **[Django GraphQL Tutorial](https://github.com/the-akira/Django-GraphQL-Tutorial)**

Bons estudos!

# Referências

- [How to GraphQL](https://www.howtographql.com)
- [What are Web APIs](https://hackernoon.com/what-are-web-apis-c74053fa4072)
- [Graphene-Python](https://docs.graphene-python.org/en/latest/quickstart/)
- [Building a GraphQL API with Django](https://stackabuse.com/building-a-graphql-api-with-django/)
- [Building a GraphQL API with Python](https://ednsquare.com/story/building-a-graphql-api-with-python-django------u6KWaC)
- [GraphQL](https://en.wikipedia.org/wiki/GraphQL)
