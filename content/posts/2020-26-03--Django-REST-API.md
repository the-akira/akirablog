---
title: Desenvolvendo uma REST API com Django
date: "2020-03-26T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/django-rest-api/"
category: "Django"
tags:
  - "Python"
  - "Programação"
  - "Web Development"
  - "Django"
description: "Neste tutorial vamos aprender como desenvolver uma REST API com Django REST framework. Ferramenta capaz de nos prover funcionalidades que nos permite construir API's com eficiência e rapidez."
---

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/Imagens/Capas/DjangoRest.png)

## Conteúdo

1. [Introdução](#introdução)
2. [Iniciando o Projeto](#iniciando-o-projeto)
3. [Desenvolvendo a API](#desenvolvendo-a-api)
4. [Conclusão](#conclusão)
5. [Referências](#referências)

## Introdução

Django REST framework é um kit de ferramentas poderoso e flexível para a construção de **Web API's**, também muito conhecidas como **REST API's**.

É fundamental que tenhamos a compreensão do conceito **REST** de forma que assim consigamos aproveitar o máximo do potencial deste framework para a construção de API's que estejam de acordo com as restrições **REST**.

### REST

REST é acrônimo para **REpresentational State Transfer**. É um estilo arquitetônico para sistemas hipermídia distribuídos e foi apresentado pela primeira vez por **Roy Fielding** em 2000 em sua famosa [dissertação](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm).

REST define um conjunto de restrições a serem usadas para a criação de serviços Web. Os serviços Web em conformidade com o estilo de arquitetura REST, chamados serviços da Web RESTful, fornecem interoperabilidade entre sistemas de computadores na Internet. Os serviços da Web RESTful permitem que os sistemas solicitantes acessem e manipulem representações textuais de recursos da Web usando um conjunto uniforme e predefinido de operações *stateless*.

Em um serviço da Web RESTful, as solicitações feitas ao **URI** de um recurso provocam uma resposta com um *payload* formatado em **HTML**, **XML**, **JSON** ou algum outro formato. A resposta pode confirmar que alguma alteração foi feita no recurso armazenado, e a resposta pode fornecer links de hipertexto para outros recursos ou coleções de recursos relacionados. Quando o HTTP é usado, as operações (métodos HTTP) disponíveis são **GET**, **HEAD**, **POST**, **PUT**, **PATCH**, **DELETE**, **CONNECT**, **OPTIONS** e **TRACE**.

A figura a seguir ilustra com uma visão de alto nível o funcionamento de uma REST API

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/HTTP/REST.png)

Ao utilizar um protocolo **stateless** e suas operações padrão, os sistemas RESTful visam desempenho rápido, confiabilidade e capacidade de expansão reutilizando componentes que podem ser gerenciados e atualizados sem afetar o sistema como um todo, mesmo enquanto estiver em execução.

Como qualquer outro estilo arquitetural, o REST também possui seus próprios guias restritivos que devem ser atendidas caso uma interface tenha o desejo de ser referida como RESTful.

### Princípios Orientadores do REST

1. **Client-Server**: Ao separar os aspectos da interface do usuário dos aspectos de armazenamento de dados, aprimoramos a portabilidade da interface do usuário em várias plataformas e melhoramos a escalabilidade, simplificando os componentes do servidor.
2. **Stateless**: Cada solicitação do cliente para o servidor deve conter todas as informações necessárias para entender a solicitação e não pode tirar proveito de nenhum contexto armazenado no servidor. O estado da sessão é, portanto, mantido inteiramente no cliente.
3. **Cacheable**: As restrições de cache exigem que os dados em uma resposta a uma solicitação sejam implicitamente ou explicitamente rotulados como armazenáveis em cache ou não em cache. Se uma resposta for armazenável em cache, será concedido ao cache do cliente o direito de reutilizar esses dados de resposta para solicitações equivalentes posteriores.
4. **Uniform Interface**: Ao aplicar o princípio de generalidade da engenharia de software à interface do componente, a arquitetura geral do sistema é simplificada e a visibilidade das interações é aprimorada. Para obter uma interface uniforme, são necessárias várias restrições de arquitetura para orientar o comportamento dos componentes. O REST é definido por quatro restrições de interface: **identificação de recursos**; **manipulação de recursos através de representações**; **mensagens auto-descritivas**; e, **hipermídia como o mecanismo do estado do aplicativo**.
5. **Layered System**: O estilo do sistema em camadas permite que uma arquitetura seja composta de camadas hierárquicas, restringindo o comportamento do componente, de modo que cada componente não possa "ver" além da camada imediata com a qual está interagindo.
6. **Code on demand (optional)**: O REST permite que a funcionalidade do cliente seja estendida baixando e executando o código na forma de applets ou scripts. Isso simplifica os clientes, reduzindo o número de recursos necessários para a pré-implementação.

### Recursos

A abstração chave das informações na arquitetura REST são os **recursos**. Qualquer informação que possa ser nomeada pode ser um recurso: um **documento** ou **imagem**, um **serviço temporal**, uma **coleção de outros recursos**, um **objeto não virtual** (por exemplo, uma pessoa) e assim por diante. O REST usa um **identificador de recurso** para identificar o recurso específico envolvido em uma interação entre componentes.

O estado do recurso em qualquer data/hora específica é conhecido como **representação do recurso**. Uma representação consiste em dados, metadados que descrevem os links de dados e hipermídia, que podem ajudar os clientes na transição para o próximo estado desejado.

O formato dos dados de uma representação é conhecido como um **[media type](https://www.iana.org/assignments/media-types/media-types.xhtml)**. O media type identifica uma especificação que define como uma representação deve ser processada. 

O acesso aos recursos é fornecido pelo **servidor REST** no qual o **cliente REST** é usado para acessar e também para modificar recursos. Todos os recursos são identificados via [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)(Uniform Resource Identifier).

### Projeto

A **REST API** que vamos desenvolver oferecerá os recursos **filmes** e **usuários**, estes últimos que serão os usuários da aplicação, cadastrados em nosso banco de dados, capazes de: 

- Cadastrar filmes (**POST**)
- Solicitar filmes (**GET**)
- Atualizar filmes (**PUT**)
- Deletar filmes (**DELETE**)

Contaremos com um mecanismo de autenticação através de um **Token**. Será necessário o envio de um **Token** válido para que seja possível a interação com determinados **endpoints** de nossa API.

O **endpoint**(ou rota) é a URL que solicitamos. Teremos os seguintes **endpoints**:

| Endpoint    | Ação |
|-----------  |---------------|
| filmes/     | Obter lista de filmes, cadastrar novos filmes  |
| filmes/1/   | Obter filme específico, atualizar ou deletar um filme |
| usuarios/   | Obter usuários cadastrados no banco de dados       |
| usuarios/1/ | Obter usuário específico cadastrado no banco de dados      |
| login/ | Autenticar na API através de login e senha válidos, receberá um Token em retorno |

Com essa breve introdução podemos finalmente iniciar as preparações para o desenvolvimento do projeto, de forma que tudo fique mais claro e compreensivo!

Você pode encontrar o código-fonte desse projeto no **[GitHub](https://github.com/the-akira/Django-REST-Tutorial)** caso tenha alguma dúvida específica.

## Iniciando o Projeto

Começaremos do princípio, criando o diretório principal de nosso projeto

```
mkdir projeto
```

Uma vez que ele foi criado, navegamos até ele

```
cd projeto/
```

Devemos agora criar um ambiente virtual de forma a isolarmos os pacotes que vamos utilizar e suas versões específicas, vamos então digitar

```
python3 -m venv env
```

É necessário ativarmos o ambiente virtual para que ele tenha efeito

```
source env/bin/activate
```

Finalmente instalaremos os pacotes necessários para trabalharmos

```
pip install django
pip install djangorestframework
```

Com os pacotes instalados com sucesso, agora temos acesso aos comandos Django, vamos então criar a nossa aplicação!

```
django-admin startproject movies
```

Perceba que chamei o projeto de **movies**, você pode escolher o nome que for mais conveniente para você, vamos agora navegar até o projeto

```
cd movies/
```

Definiremos um **app** que será chamado de **resources**, este que representará efetivamente os recursos de nossa API, onde estarão localizados os **models**, **serializers** e **views**

```
django-admin startapp resources
```

Neste ponto o nosso projeto conta com a seguinte estrutura

```
movies/
├── movies
│   ├── asgi.py
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── resources
    ├── admin.py
    ├── apps.py
    ├── __init__.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py

3 directories, 13 files
```

Editaremos agora o arquivo `settings.py`, vamos apenas alterar uma pequena parte dele com o seguinte conteúdo

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework.authtoken',
    'rest_framework',
    'resources',
]

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',  
    ],
}
```

Registramos os **apps** que precisaremos ao longo do desenvolvimento e também o mecanismo de autenticação por **Tokens**, e agora iremos sincronizar o banco de dados através do comando

```
python manage.py migrate
```

Criaremos um usuário para que em breve possamos testar a API com ele

```
python manage.py createsuperuser
```

Escolha um **username** e uma **senha**. Dados estes que serão utilizados para autenticarmos na API futuramente.

Estão definidas as configurações básicas de nossa API, agora podemos efetivamente partir para as etapas de desenvolvimento.

## Desenvolvendo a API

### Modelo

Um modelo é uma classe que representa tabela ou coleção em nosso banco de dados e onde cada atributo da classe é um campo da tabela ou coleção.

Os modelos do Django fornecem um Object-Relational Mapping(ORM) para o banco de dados subjacente. ORM é uma poderosa técnica de programação que facilita muito o trabalho com dados e banco de dados.

Sendo assim, vamos precisar apenas da síntaxe do Python para definir o nosso **model**, editaremos então o arquivo `resources/models.py` com o seguinte conteúdo

```python
from django.db import models

class Filme(models.Model):
    titulo = models.CharField(max_length=100)
    diretor = models.CharField(max_length=100)
    sinopse = models.TextField()
    genero = models.CharField(max_length=100)
    owner = models.ForeignKey('auth.User', 
        related_name='filmes', on_delete=models.CASCADE)

    def __str__(self):
        return self.title
```

Perceba que nosso **model** conta com os seguintes campos:

- **id**: Definido **implicitamente** e auto-incrementado, ele é a **chave primária** identificadora de cada Filme
- **titulo**: Representa o título do Filme
- **sinopse**: Representa a sinopse do Filme
- **genero**: Representa o gênero do Filme
- **owner**: Representa o usuário que criou o Filme

O método __str__() nos traz uma representação do modelo como string, nos retornando o título do Filme.

Uma vez que o modelo está definido, agora devemos executar as migrações para que o Django possa sincronizar nosso banco de dados corretamente, vamos então executar

```
python manage.py makemigrations
```

E finalmente, sincronizar o banco de dados com os modelos atualizados

```
python manage.py migrate
```

### Serializers

Os serializadores permitem que dados complexos, como **querysets** e **models**, sejam convertidos em tipos de dados Python nativos, que podem ser facilmente renderizados em JSON, XML ou outros tipos de conteúdo. 

Os serializadores também fornecem desserialização, permitindo que os dados analisados sejam convertidos novamente em tipos complexos, após a primeira validação dos dados recebidos.

Para definirmos o nosso **serializador**, vamos criar o arquivo em `resources/serializers.py` com o seguinte conteúdo

```python
from resources.models import Filme
from django.contrib.auth.models import User
from rest_framework import serializers

class FilmeSerializer(serializers.ModelSerializer):
    class Meta:
        model = Filme
        owner = serializers.ReadOnlyField(source='owner.username')
        fields = ['id', 'titulo', 'diretor', 'sinopse', 'genero', 'owner']

class UserSerializer(serializers.ModelSerializer):
    filmes = serializers.PrimaryKeyRelatedField(many=True, queryset=Filme.objects.all())

    class Meta:
        model = User
        fields = ['id', 'username', 'filmes']
```

Observe que importamos os modelos **Filme** e **User** que serão serializados e definimos duas classes:

- **FilmeSerializer**: Responsável por serializar o modelo Filme, com o campo **owner** definido apenas para leitura.
- **UserSerializer**: Responsável por serializar o modelo User, com o campo **filmes** definido como uma chave primária relacional, representando todos os filmes cadastrados por um usuário.

Com nossos serializadores estabelecidos, devemos agora trabalhar nas **views** de nossa API

### Views

Uma view é um mecanismo Python que recebe uma requisição Web e retorna uma resposta. Para trabalharmos com as views, editaremos o arquivo `resources/views.py`

```python
from resources.models import Filme
from django.contrib.auth.models import User
from rest_framework.permissions import IsAuthenticated
from resources.serializers import FilmeSerializer, UserSerializer
from rest_framework import generics

class FilmeList(generics.ListCreateAPIView):
    queryset = Filme.objects.all()
    serializer_class = FilmeSerializer
    permission_classes = [IsAuthenticated]

    def perform_create(self, serializer):
        serializer.save(owner=self.request.user)

class FilmeDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Filme.objects.all()
    serializer_class = FilmeSerializer
    permission_classes = [IsAuthenticated]

class UserList(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class UserDetail(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

Em nossas **views** importamos os models **Filme** e **User** e também os serializadores que definimos anteriormente (FilmeSerializer e UserSerializer), também importamos a funcionalidade **IsAuthenticated** para garantir que somente usuários autenticados possam acessar ou modificar os filmes. 

Além disso estamos importando as funções **generics** que nos fornecem funcionalidades poderosas de **criação**, **atualização**, **solicitação** e **remoção** sem que precisemos escrever diversas linhas de código.

Finalmente, definimos quatro classes, cada uma representando uma view específica:

- **FilmeList**: Traz a lista de todos os filmes em nosso banco de dados, também nos permite inserir novos filmes, somente usuários autenticados podem interagir com esse endpoint.
- **FilmeDetail**: Traz detalhes de um filme específico no banco de dados, também podemos atualizar ou deletar um filme específico, somente usuários autenticados podem interagir com esse endpoint.
- **UserList**: Traz a lista de usuários cadastrados em nosso projeto. Este endpoint pode ser acessado por qualquer um, uma vez que não estamos protegendo ele.
- **UserDetail**: Traz detalhes sobre um usuário específico, assim como os filmes que ele cadastrou. Este endpoint pode ser acessado por qualquer um, uma vez que não estamos protegendo ele.

Tendo as nossas **views** definidas, agora precisamos mapear as **URL's** de nossa API, de forma que ao visitarmos uma determinada URL, sua view referente seja acionada, nos respondendo assim de maneira adequada. Vamos então alterar o arquivo `movies/urls.py`

```python
from django.contrib import admin
from django.urls import path
from resources import views as resources_views
from rest_framework.authtoken.views import obtain_auth_token

urlpatterns = [
    path('admin/', admin.site.urls),
    path('filmes/', resources_views.FilmeList.as_view()),
    path('filmes/<int:pk>/', resources_views.FilmeDetail.as_view()),
    path('usuarios/', resources_views.UserList.as_view()),
    path('usuarios/<int:pk>/', resources_views.UserDetail.as_view()),
    path('login/', obtain_auth_token, name='api_token_auth'),
]
```

Observe que esse padrão de URL's se parece muito com a tabela que definimos anteriormente, representando assim cada um de nossos **endpoints**, agora finalmente podemos começar a testar nossa API, mas antes, vamos executar o projeto

```
python manage.py runserver
```

"Starting development server at `http://127.0.0.1:8000/`"

### Testando os Endpoints

Para os testes irei utilizar [curl](https://curl.haxx.se/), ferramenta de linha de comando e biblioteca para transferir dados com URL's que acredito ser muito eficaz. Existem muitas outras opções excelentes de ferramentas para essas tarefas, como [HTTPie](https://httpie.org/) por exemplo e obviamente **[Requests](https://requests.readthedocs.io/en/master/)**, sinta-se livre para escolher sua solução favorita.

O próprio Django REST framework oferece um mecanismo construído que nos permite interagir diretamente como nossa API, visite por exemplo o endereço: `http://127.0.0.1:8000/usuarios/` e verás os usuários cadastrados no projeto.

#### Autenticando

Você deve lembrar que no início de nosso projeto, registramos um super usuário com permissões especiais, utilizaremos ele para autenticar em nossa API, para que possamos então interagir com todos os recursos disponíveis.

Executaremos então o seguinte comando no endpoint `login/` em que:

* **-d** = dados enviados para o endpoint
* **-H** = Header enviado
* **-X** = Tipo de requisição

```
curl -d '{"username":"username", "password":"senha"}' -H "Content-Type: application/json" -X POST http://localhost:8000/login/
```

O **username** é o username escolhido por você e **senha** é a senha que você definiu, substitua os valores corretamente.

Receberemos um **Token** similar a esse:

```
{"token":"75c0bba76256298d8d7ea5f204cfad83fe9a174d"}
```

Iremos guardá-lo para utilizarmos ele nas próximas requisições.

#### Enviando Dados

Através do método **POST** podemos cadastrar filmes no endpoint `filmes/`

```
curl -d '{"titulo":"The Lord of the Rings","diretor":"Peter Jackson","sinopse":"The Lord of the Rings is a film series of three epic fantasy adventure films directed by Peter Jackson, based on the novel written by J. R. R. Tolkien.","genero":"Adventure","owner":1}' -H "Content-Type: application/json" -X POST http://localhost:8000/filmes/ -H 'Authorization: Token 75c0bba76256298d8d7ea5f204cfad83fe9a174d'
```

#### Obtendo Dados

Através do método **GET** podemos obter os filmes cadastrados na aplicação

```
curl -X GET http://localhost:8000/filmes/ -H 'Authorization: Token 75c0bba76256298d8d7ea5f204cfad83fe9a174d'
```

Ou até mesmo um filme específico

```
curl -X GET http://localhost:8000/filmes/2/ -H 'Authorization: Token d1781baac8f55daf296f6f32144ba2e107d949b5'
```

Observe que especificamos o **id** do filme desejado. Também podemos solicitar os usuários

```
curl -X GET http://localhost:8000/usuarios/
```

Ou até mesmo um usuário específico

```
curl -X GET http://localhost:8000/usuarios/1/ 
```

Note que não foi preciso especificarmos o **Token** para que possámos obter os usuários, uma vez que esse recurso não foi protegido por nós.

#### Atualizando Dados

O método HTTP **PUT** nos permite atualizarmos os dados de nossa API, vejamos como podemos atualizar um filme

```
curl -d '{"titulo":"Fight Club","diretor":"David Fincher","sinopse":"Fight Club is a 1999 American film directed by David Fincher","genero":"cult","owner":1}' -H "Content-Type: application/json" -X PUT http://localhost:8000/filmes/2/ -H 'Authorization: Token 75c0bba76256298d8d7ea5f204cfad83fe9a174d'
```

Perceba que estou alterando o filme de `ID=2`.

#### Deletando Dados

O método **DELETE** nos permite remover recursos específicos de nossa API

```
curl -H "Content-Type: application/json" -X DELETE http://localhost:8000/filmes/1/ -H 'Authorization: Token 75c0bba76256298d8d7ea5f204cfad83fe9a174d'
```

Perceba que estou removendo o filme de `ID=1`.

Finalmente conseguimos atingir as funcionalidades básicas fundamentais de uma REST API.

## Conclusão

Através desse pequeno guia fomos capazes de aprender os aspectos básicos da arquitetura REST, bem como sua importância no desenvolvimento de Softwares. 

Experimentamos de forma breve o Django REST framework, explorando funcionalidades essenciais que ele oferece para a construção rápida de API's consistentes.

Por fim utilizamos a ferramenta **curl** para testar os endpoints de nossa aplicação, validando assim cada um deles.

Você pode aprimorar o projeto adicionando funcionalidades como a de **pesquisa**, **paginação**, para isso a [documentação oficial](https://www.django-rest-framework.org/) do Django REST framework pode ajudá-lo com maestria, uma vez que ela é muito detalhada e organizada. 

Considere também a escrita de testes automatizados para validar cada **endpoint**.

Desejo a você uma boa exploração!

## Referências

- [Django REST framework](https://www.django-rest-framework.org/)
- [Representational state transfer (REST)](https://en.wikipedia.org/wiki/Representational_state_transfer)
- [What is REST](https://restfulapi.net/)
- [Understanding And Using REST APIs](https://www.smashingmagazine.com/2018/01/understanding-using-rest-api/)
- [How to Implement Token Authentication](https://simpleisbetterthancomplex.com/tutorial/2018/11/22/how-to-implement-token-authentication-using-django-rest-framework.html)
