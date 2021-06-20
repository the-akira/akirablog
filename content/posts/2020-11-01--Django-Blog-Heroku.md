---
title: Desenvolvendo um Blog com Django e Deployment no Heroku
date: "2020-01-11T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/django-blog-heroku/"
category: "Django"
tags:
  - "Python"
  - "Programação"
  - "Django"
  - "Web"
description: "Neste guia vamos desenvolver um Blog completo com o framework Django e no final vamos fazer o deployment de nosso projeto na plataforma Heroku utilizando o sistema gerenciador de banco de dados PostgreSQL."
---

# Conteúdo

1. [Introdução](#introducao)
2. [Primeiros Passos](#primeiros-passos)
3. [Construindo o Blog](#construindo-o-blog)
4. [Deployment no Heroku](#deployment-no-heroku)
5. [Conclusão](#conclusao)

# Introducao

O **[Django](https://www.djangoproject.com)** é um framework Web Python de alto nível que incentiva o desenvolvimento rápido e o design limpo e pragmático. Construído por desenvolvedores experientes, ele cuida de grande parte do aborrecimento do desenvolvimento Web, para que você possa se concentrar em escrever seu aplicativo sem precisar reinventar a roda. É de código aberto e gratuito.

O Django pode ser (e tem sido) usado para criar praticamente qualquer tipo de site - desde sistemas de gerenciamento de conteúdo e wikis, até redes sociais e sites de notícias. Ele pode funcionar com qualquer estrutura do lado do cliente e pode fornecer conteúdo em quase qualquer formato (incluindo HTML, feeds RSS, JSON, XML, etc.).

Suas três mais importantes características são:

- Velocidade
- Segurança
- Escalabilidade

Sites conhecidos que usam o **Django** incluem o Public Broadcasting Service, Instagram, Mozilla, The Washington Times, Disqus, Bitbucket, e Nextdoor.

- Neste guia vamos desenvolver juntos o seguinte projeto: **https://akiradjango.herokuapp.com**
- Você pode encontrar o código fonte em: **https://github.com/the-akira/Django-Blog-Tutorial**

## Funcionamento

Em um site tradicional orientado a dados, uma aplicação Web aguarda solicitações **HTTP** do navegador da Web (ou outro cliente). Quando uma solicitação é recebida, o aplicativo calcula o que é necessário com base na **URL** e, possivelmente, nas informações dos dados `POST` ou `GET`. Dependendo do que for necessário, ele poderá ler ou gravar informações em um banco de dados ou executar outras tarefas necessárias para atender à solicitação. O aplicativo retornará uma resposta ao navegador da Web, geralmente criando dinamicamente uma **página HTML** para exibição do navegador, inserindo os dados recuperados nos espaços reservados em um **template HTML**.

Os aplicativos da web Django geralmente agrupam o código que lida com cada uma dessas etapas em arquivos separados, vejamos a estrutura: 

![img](https://i.ibb.co/YNq9TS2/django.png)

- **URL's:** Um mapeador de URL é usado para redirecionar solicitações HTTP para a visualização apropriada com base na URL de solicitação.
- **View:** Uma **view** é uma função **request handler**, que recebe solicitações HTTP e retorna respostas HTTP. As **views** acessam os dados necessários para satisfazer solicitações por meio de modelos e delegam a formatação da resposta aos modelos.
- **Models:** Models são objetos Python que definem a estrutura dos dados de um aplicativo e fornecem mecanismos para gerenciar (adicionar, modificar, excluir) e consultar registros no banco de dados.
- **Templates:** Um template é um arquivo de texto que define a estrutura ou o layout de um arquivo (como uma página HTML), com espaços reservados para representar o conteúdo real. Uma **view** pode criar dinamicamente uma página HTML usando um template HTML, preenchendo-a com dados de um modelo.

Django também conta com o poderoso **[Django admin site](https://docs.djangoproject.com/en/3.0/ref/contrib/admin/)**:

Uma das partes mais poderosas do Django é a interface de administração automática. Ele lê os metadados de seus modelos para fornecer uma interface rápida e centrada no modelo, na qual usuários confiáveis podem gerenciar o conteúdo do seu site. 

Agora que obtivemos informações essenciais sobre os fundamentos básicos do Django e como ele opera, vamos iniciar o nosso projeto prático.

Lembre-se que é necessário termos o **[Python](https://www.python.org)** em nosso computador para que possamos utilizar o Django!

# Primeiros Passos

Iniciaremos nosso projeto criando um diretório, chamaremos ele de **projeto**, vamos utilizar o comando **[mkdir](https://en.wikipedia.org/wiki/Mkdir)** para criá-lo:

```
mkdir projeto
```

Navegaremos até nosso projeto através do comando **[cd](https://en.wikipedia.org/wiki/Cd_(command))**:

```
cd projeto/
```

Uma vez que estamos dentro do diretório `projeto`, vamos agora criar um [ambiente virtual](https://github.com/the-akira/Python-Iluminado/blob/master/Capitulos/29.PIP.md) para isolarmos nossas dependências.

```
python3 -m venv ambientev
```

Nosso ambiente virtual foi criado, agora precisamos ativá-lo, vamos executar o seguinte comando:

```
source ambientev/bin/activate
```

Finalmente podemos instalar o framework Django, para esta tarefa vamos utilizar o **[pip](https://pypi.org/project/pip/)**, porém sinta-se livre caso queira utilizar o [pipenv](https://github.com/pypa/pipenv) ou outra solução:

```
pip install django
```

Agora que temos o Django instalado em nosso ambiente virtual e ele está ativado, nos está disponível o comando **django-admin startproject** que nos permite iniciar um projeto Django.

```
django-admin startproject webapp
```

Observe que chamamos nosso projeto de **webapp**, nome arbitrário escolhido por mim, sinta-se livre para escolher o nome que desejar. Veja também que Django nos criou a seguinte estrutura de diretórios e arquivos:

```
webapp/
├── manage.py
└── webapp
    ├── asgi.py
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py

1 directory and 6 files
```

Neste ponto já temos uma aplicação funcionando, só precisamos navegar até o diretório `webapp`:

```
cd webapp/
```

E executar o seguinte comando:

```
python manage.py runserver
```

Podemos agora através de nosso web browser navegar até o endereço `http://127.0.0.1:8000` e veremos que a nossa instalação funcionou corretamente e o Django já está operacional.

Chegou o momento de aplicarmos as migrações, mas antes disso, o que são elas?

As migrações são a maneira do Django de propagar alterações que você faz nos seus **models** (adicionar um campo, excluir um modelo etc.) no esquema do banco de dados. Eles foram projetados para serem principalmente automáticos, mas você precisa saber quando fazer migrações, quando executá-las e os problemas comuns em que poderá se deparar.

Existem vários comandos que você usará para interagir com as migrações e o tratamento do Django do esquema do banco de dados:

- **migrate**, responsável por aplicar e cancelar a aplicação de migrações.
- **makemigrations**, responsável por criar novas migrações com base nas alterações que você fez em seus modelos.
- **sqlmigrate**, que exibe as instruções SQL para uma migração.
- **showmigrations**, que lista as migrações de um projeto e seu status.

No momento vamos lidar apenas com o comando **migrate**, vamos então executá-lo:

```
python manage.py migrate
```

Uma série de operações serão executadas e veremos muitos **OK's**, significando que nossos **models** foram migrados com sucesso, agora já podemos criar um super-usuário com poderes administrativos para acessar o **Django Admin** back-end, para esta tarefa executaremos:

```
python manage.py createsuperuser
```

Escolha o nome do seu usuário, preencha seu endereço de email e escolha uma senha. Feito, agora já possuímos um usuário capaz de acessar o painel administrativo do Django. Você agora já pode navegar até o endereço `http://127.0.0.1:8000/admin/` e fazer o login com suas credenciais escolhidas, aproveite e divirta-se experimentando o painel Django.

Estes foram os primeiros passos básicos para iniciarmos uma aplicação Django, agora vamos efetivamente iniciar a construção de nosso Blog!


# Construindo o Blog

Um conceito muito importante no framework Django são os **apps**, Um Django **app** é uma pequena biblioteca que representa uma parte discreta de um projeto maior. É muito comum um projeto Django ser composto de vários **apps**, cada um contendo uma solução específica, por exemplo, é possível ter um **app** que vá lidar apenas com o gerenciamento dos usuários de nossa aplicação.

Para o nosso projeto, vamos construir apenas um **app** e chamaremos ele de **blog**, novamente, nome arbitrário escolhido por nós, sinta-se livre para escolher o seu nome favorito, vamos então executar:

```
python manage.py startapp blog
```

Nesse ponto é importante termos nosso projeto aberto em um editor de textos, uma vez que nosso **app blog** foi criado, precisamos imediamente registrá-lo no arquivo `settings.py` do nosso projeto, este arquivo guarda todas as configurações de nossa aplicação, vamos editar a seguinte parte dele, de forma a cadastrarmos nosso **app**:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```

## Model

Agora que nosso **app** já está registrado, vamos criar nosso **model** que é a fonte única e definitiva de informações sobre seus dados. Ele contém os campos e comportamentos essenciais dos dados que você está armazenando. Geralmente, cada modelo é mapeado para uma única tabela de banco de dados, manteremos nosso **model** simples para fins de aprendizado, vamos então editar o arquivo `models.py` localizado dentro do diretório `blog/`, ele contará com o seguinte conteúdo:

```python
from django.db import models
from django.utils import timezone

class Post(models.Model): 
	titulo = models.CharField(max_length=100)
	conteudo = models.TextField()
	data = models.DateTimeField(default=timezone.now)
	autor = models.CharField(max_length=100)

	def __str__(self):
		return self.titulo
```

No topo de nosso arquivo fazemos os **imports** necessários para criarmos o nosso **model**, criamos uma classe chamada **Post** que será uma tabela no banco de dados com os campos: **titulo**, **conteudo**, **data** e **autor** e por fim definimos um método mágico que imprime o titulo do Post como string. É importante destacar que o campo data será preenchido automaticamente de acordo com o horário da postagem (timezone).

Nosso modelo está criado, devemos agora registrá-lo no arquivo `admin.py` de forma que nosso modelo apareça no painel administrativo do Django, fazendo com que seja possível adicionarmos dados em nossa aplicação através dessa interface. Vamos então editar o arquivo `admin.py`

```python
from django.contrib import admin
from blog.models import Post 

admin.site.register(Post)
```

Observe que importamos o model **Post** do arquivo `models.py` que pertence ao **app blog**, por fim fazemos o seu registro no painel administrativo Django.

Precisamos agora executar alguns comandos para que as mudanças em nosso banco de dados sejam feitas:

```
python manage.py makemigrations
```

**makemigrations** criará novas migrações com base nas alterações que fizemos, podemos agora executar:

```
python manage.py migrate
```

Com o comando `python manage.py runserver` executamos nosso servidor, visitamos o endereço `http://127.0.0.1:8000/admin/` e veremos que já podemos cadastrar os nossos Posts através do painel de administrativo do Django. Muito legal, não?

## Templates

Um **Django template** é um documento de texto ou uma sequência de caracteres Python **marcada usando** a linguagem de modelo do Django. Algumas construções são reconhecidas e interpretadas pelo mecanismo de modelo. Os principais são variáveis e tags.

Um modelo é renderizado com um contexto. A renderização substitui as variáveis pelos seus valores, que são pesquisados no contexto, e executa as tags. De forma simplificada, **templates** são arquivos **HTML** que podemos utilizar para apresentar nossos dados aos usuários de nossa aplicação, eles são um mecanismo poderoso e fundamental do framework Django.

Dentro do diretório `blog` vamos criar um novo diretório chamado de `templates` e dentro do diretório `templates` vamos criar um diretório chamado `blog`, dentro de blog vamos criar os arquivos `base.html` e `index.html`. Os diretórios estão sendo criados dessa maneira pois estamos seguindo a convenção obrigatória do Django.

Nosso arquivo `base.html` contará com o seguinte conteúdo:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>Blog Django</title>
</head>
<body>
    {% block content %}{% endblock content %}
</body>
</html>
```

E o arquivo `index.html` contará com o seguinte conteúdo:

```html
{% extends "blog/base.html" %}

{% block content %}
	<h1>Django Blog</h1>
	{% for post in posts %}
	<div>
	     <h2>{{ post.titulo }}</h2>
	     <p>{{ post.conteudo }}</p>
              <small>{{ post.data|date:"F d, Y" }}</small>
	</div>
	{% endfor %}
{% endblock content %}
```

O arquivo `base.html` será o nosso layout principal, os demais arquivos HTML serão apenas um extensão dele, ele é nosso arquivo mestre. Veja que nele foram definidas as tags `{% block content %}{% endblock content %}`, elas definem um bloco de conteúdo que pode ser inserido através de outro arquivo, e é exatamente o que fazemos no arquivo `index.html`, todas as tags HTML dentro de `{% block content %}{% endblock content %}` são injetadas no arquivo `base.html` fazendo com que seja muito mais fácil organizarmos e mantermos o código de nossa aplicação, sem precisarmos repetir código.

## Atualizando views e urls

Começaremos alterando o arquivo `views.py` com o seguinte conteúdo:

```python
from django.shortcuts import render
from django.views.generic import ListView
from blog.models import Post

class PostListView(ListView):
	model = Post 
	template_name = 'blog/index.html'
	context_object_name = 'posts'
	ordering = ['-data']
	paginate_by = 4
```

Observe que importamos **ListView**, que é uma view genérica baseada em classes feita especialmente para facilitar a nossa vida e o **model Post**, em seguida definimos nossa view chamada de **PostListView**, este nome é definido pelo Django como convenção, esta **view** será responsável por retornar os nossos **Posts** utilizando o template definido por nós como `index.html` e com as datas ordendadas de forma decrescente e com paginação a cada 4 **Posts**. Também setamos o **context\_object\_name** como **posts** definindo assim o nome pelo qual podemos acessar nossos dados no template com o **for loop**. 

Vamos agora editar o arquivo `urls.py` com o seguinte conteúdo:

```python
from django.contrib import admin
from django.urls import path
from blog.views import PostListView

urlpatterns = [
    path('', PostListView.as_view(), name='blog-index'),
    path('admin/', admin.site.urls),
]
```

Além dos imports já presentes no arquivo, importamos também o **PostListView** de nosso arquivos `views.py`, com o **PostListView** em nossa disposição, podemos definir uma URL que ativará essa **view**, o endereço será o '/', por isso deixamos as aspas vazias, também definimos o parâmatro **name** como **blog-index** para termos um nome a referenciarmos em nossos templates.

Nesse ponto já podemos visitar nosso blog em `http://127.0.0.1:8000/` e ver os **Posts** apresentados em nosso index. Caso não haja nenhum post apresentado, visite o back-end de nossa aplicação (painel administrativo Django) e cadastre novos posts!

## Arquivos Static

Os sites geralmente precisam exibir arquivos adicionais, como imagens, JavaScript ou CSS. No Django, nos referimos a esses arquivos como "arquivos static".

Dentro do diretório `blog` vamos criar um novo diretório chamado de `static` e dentro do diretório `static` vamos criar um diretório chamado `blog`, dentro de blog vamos criar o arquivo `style.css`

Em nosso arquivo `style.css` vamos adicionar um estilo básico para testes:

```css
body{
    background-color: #13101a;
    color: #beb8cc;
}
```

Agora precisamos atualizar nosso arquivo `base.html` para que nosso CSS seja carregado:

```html
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" type="text/css" href="{% static 'blog/style.css' %}">
    <title>Blog Django</title>
</head>
<body>
    {% block content %}{% endblock content %}
</body>
</html>
```

Observe que usamos o comando `{% load static %}` para carregar os arquivos static e depois utilizamos a função `{% static 'blog/style.css' %}` para carregar nosso arquivo `style.css`. Como podemos ver, nossos estilos foram aplicados com sucesso, caso não tenha atualizado para você, certifique-se de ter seguido todos os passos corretamente e não esqueça de sempre fazer uma limpeza no cache de seu navegador.

## Página de Detalhes para cada Post e Paginação no Index

Vamos agora adicionar uma página específica para cada post de nosso blog, para isso, precisaremos alterar nosso arquivo `views.py` e nosso arquivo `urls.py` e também devemos criar um novo template chamado `post_detail.html`

Começamos editando nosso arquivo `views.py`:

```python
from django.shortcuts import render
from django.views.generic import ListView, DetailView
from blog.models import Post

class PostListView(ListView):
	model = Post 
	template_name = 'blog/index.html'
	context_object_name = 'posts'
	ordering = ['-data']
	paginate_by = 4

class PostDetailView(DetailView):
	model = Post
```

Veja que importamos mais uma view genérica, dessa vez **DetailView** e posteriormente definimos a nossa view com o nome PostDetailView pré-determinado pelo Django, ela basicamente nos retornará um **Post** individual baseado em um determinado **id**.

Podemos agora alterar nosso arquivo `urls.py`:

```python
from django.contrib import admin
from django.urls import path
from blog.views import PostListView, PostDetailView

urlpatterns = [
    path('', PostListView.as_view(), name='blog-index'),
    path('post/<int:pk>/', PostDetailView.as_view(), name="post-detail"),
    path('admin/', admin.site.urls),
]
```

Simplesmente importamos nossa view **PostDetailView** e criamos uma URL que irá ativar essa view: **post/<int:pk>/**, para entendermos melhor:

**int** significa que essa URL vai receber um número inteiro e **pk** indica que deve ser uma chave primária, nesse caso um **id** único que representa um ÚNICO **Post**.

Por exemplo, se visitarmos o seguinte endereço `http://127.0.0.1:8000/post/1/` nos será retorna apenas o **Post** com `id = 1` em nosso banco de dados. 

Vamos criar agora nosso template para a página específica de cada post, dentro do diretório `templates/blog` criaremos então o arquivo com o nome `post_detail.html` que contará com o seguinte conteúdo:

```html
{% extends "blog/base.html" %}

{% block content %}
	<h1>Django Blog</h1>
	<div>
		<h2>{{ post.titulo }}</h2>
		<p>{{ post.conteudo }}</p>
		<small>{{ post.data|date:"F d, Y" }}</small>
	</div>
{% endblock content %}
```

Por fim, vamos adicionar paginação em nossa página principal e também vamos conectar cada post da página principal com sua página específica de detalhes. Para esta tarefa, precisamos editar o arquivo `index.html` com o seguinte conteúdo:

```html
{% extends "blog/base.html" %}

{% block content %}
	<h1>Django Blog</h1>
	{% for post in posts %}
	<div>
		<a href="{% url 'post-detail' post.id %}"><h1>{{ post.titulo }}</h1></a>
		<p>{{ post.conteudo }}</p>
		<small>{{ post.data|date:"F d, Y" }}</small>
	</div>
	{% endfor %}
	
	{% if is_paginated %}
		{% if page_obj.has_previous %}
			<a href="?page=1">First</a>
			<a href="?page={{ page_obj.previous_page_number }}">Previous</a>
		{% endif %}

		{% for num in page_obj.paginator.page_range %}
			{% if page_obj.number == num %}
		  <a href="?page={{ num }}">{{ num }}</a>  	
		{% elif num > page_obj.number|add:'-3' and num < page_obj.number|add:'3' %}  
		  <a href="?page={{ num }}">{{ num }}</a>  	
			{% endif %}
		{% endfor %}

		{% if page_obj.has_next %}
			<a href="?page={{ page_obj.next_page_number }}">Next</a>
			<a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
		{% endif %}	  
	{% endif %}

{% endblock content %}
```

## Adicionando Formulário para busca de Posts

Vamos adicionar um formulário de pesquisa em nossa aplicação de forma a permitir que nossos usuários executem buscas em nosso banco de dados. Começaremos editando nosso arquivo `index.html`:

```html
{% extends "blog/base.html" %}

{% block content %}
	<h1>Django Blog</h1>
	<form action="{% url 'search' %}" method="GET">
	  	<input type="text" name="q" id="id_q" placeholder="Buscar for posts...">
	</form>
	{% for post in posts %}
	<div>
		<a href="{% url 'post-detail' post.id %}"><h1>{{ post.titulo }}</h1></a>
		<p>{{ post.conteudo }}</p>
		<small>{{ post.data|date:"F d, Y" }}</small>
	</div>
	{% endfor %}
	
	{% if is_paginated %}
		{% if page_obj.has_previous %}
			<a href="?page=1">First</a>
			<a href="?page={{ page_obj.previous_page_number }}">Previous</a>
		{% endif %}

		{% for num in page_obj.paginator.page_range %}
			{% if page_obj.number == num %}
		  <a href="?page={{ num }}">{{ num }}</a>  	
		{% elif num > page_obj.number|add:'-3' and num < page_obj.number|add:'3' %}  
		  <a href="?page={{ num }}">{{ num }}</a>  	
			{% endif %}
		{% endfor %}

		{% if page_obj.has_next %}
			<a href="?page={{ page_obj.next_page_number }}">Next</a>
			<a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
		{% endif %}	  
	{% endif %}

{% endblock content %}
```

Adicionamos um formulário no topo de nossa aplicação, este que fará um **GET request** para a url **search**, que vamos definir agora, juntamente com a **view** responsável por processar os dados enviados pelo formulário, além disso, vamos também criar um novo **template** dentro do diretório `templates/blog` que vamos chamar de `search.html`.

Vamos começar então editando nosso arquivo `views.py`:

```python
from django.shortcuts import render
from django.views.generic import ListView, DetailView
from blog.models import Post
from operator import attrgetter
from django.db.models import Q

class PostListView(ListView):
	model = Post 
	template_name = 'blog/index.html'
	context_object_name = 'posts'
	ordering = ['-data']
	paginate_by = 4

class PostDetailView(DetailView):
	model = Post

def get_post_queryset(query=None):
    queryset = []
    queries = query.split(' ') # python install 2019 = ['python', 'install', '2019']
    for q in queries:
        posts = Post.objects.filter(Q(titulo__icontains=q)| Q(conteudo__icontains=q) ).distinct()
        for post in posts:
            queryset.append(post)
    return list(set(queryset))

def search(request):
    context = {}
    query = ""
    if request.GET:
        query = request.GET.get('q', '')
        context['query'] = str(query)
    posts = sorted(get_post_queryset(query), key=attrgetter('titulo'), reverse=True)
    context['posts'] = posts
    return render(request, 'blog/search.html', context)
```

Perceba que importamos duas funções relevantes para nosso mecanismo de busca: `attrgetter` e `Q`. Definimos uma função `get_post_queryset()` responsável por processar os dados digitados pelo usuário e retornar uma lista de valores únicos, filtrando-os em relação ao título e conteúdo de nossos Posts guardados no banco de dados. Por fim, definimos a **view** que chamamos de **search**, ela será responsável por receber o **GET request** e retornar os Posts organizados pelo título em ordem reversa, passando-os para o template.

Uma vez que nossas **views** estão definidas, agora precisamos atualizar nosso arquivo `urls.py`, de forma que quando executarmos o **GET request** através do formulário, nossas **views** sejam ativadas! Vamos então editá-lo:

```python
from django.contrib import admin
from django.urls import path
from blog.views import PostListView, PostDetailView
from blog import views

urlpatterns = [
    path('', PostListView.as_view(), name='blog-index'),
    path('post/<int:pk>/', PostDetailView.as_view(), name="post-detail"),
    path('search/', views.search, name='search'),
    path('admin/', admin.site.urls),
]
```

Observe que simplesmente importamos as **views** do package **blog** e definimos um novo caminho para nossa **url search**. Agora precisamos apenas criar o arquivo `search.html` dentro do diretório `templates/blog`, ele contará com o seguinte conteúdo:

```html
{% extends "blog/base.html" %}

{% block content %}
	<h1>Django Blog</h1>
	<form action="{% url 'search' %}" method="GET">
	  	<input type="text" name="q" id="id_q" placeholder="Buscar for posts...">
	</form>
	{% for post in posts %}
	<div>
		<a href="{% url 'post-detail' post.id %}"><h1>{{ post.titulo }}</h1></a>
		<p>{{ post.conteudo }}</p>
		<small>{{ post.data|date:"F d, Y" }}</small>
	</div>
	{% endfor %}
	
	{% if is_paginated %}
		{% if page_obj.has_previous %}
			<a href="?page=1">First</a>
			<a href="?page={{ page_obj.previous_page_number }}">Previous</a>
		{% endif %}

		{% for num in page_obj.paginator.page_range %}
			{% if page_obj.number == num %}
		  <a href="?page={{ num }}">{{ num }}</a>  	
		{% elif num > page_obj.number|add:'-3' and num < page_obj.number|add:'3' %}  
		  <a href="?page={{ num }}">{{ num }}</a>  	
			{% endif %}
		{% endfor %}

		{% if page_obj.has_next %}
			<a href="?page={{ page_obj.next_page_number }}">Next</a>
			<a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
		{% endif %}	  
	{% endif %}

{% endblock content %}
```

Finalmente podemos executar nosso servidor com o comando `python manage.py runserver` para testarmos nossa aplicação! Caso não tenha inserido posts em seu blog, visite o Back-end `http://127.0.0.1:8000/admin/` e insira alguns para que você possa experimentar com o formulário de busca.

## Melhorando a aparência de nossa aplicação!

Para finalizar, vamos fazer algumas edições em nossos arquivos `.html` e `.css`, de forma aperfeiçoarmos o aspecto visual de nossa aplicação.

Começamos editando o arquivo `base.html`:

```html
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" type="text/css" href="{% static 'blog/style.css' %}">
    <script src="https://kit.fontawesome.com/abb211c046.js"></script>
	<title>Blog Django</title>
</head>
<body>
	<div class="container">
		{% block content %}{% endblock content %}
	</div>
</body>
</html>
```

Vamos também editar os arquivos `index.html` e `search.html`, ambos ficarão da seguinte forma:

```html
{% extends "blog/base.html" %}

{% block content %}
	<div class="header">
		<a class="title" href="{% url 'blog-index' %}"><h1>Django Blog</h1></a>
		<div class="social">
		    <a href="https://github.com" target="_blank"><i class="fab fa-github fa-3x"></i></a>
		    <a href="https://twitter.com/" target="_blank"><i class="fab fa-twitter fa-3x"></i></a>
	    </div>
		<form action="{% url 'search' %}" method="GET">
		  	<input class="inpt" type="text" name="q" id="id_q" placeholder="Buscar por posts...">
		</form>
	</div>
	<div class="posts-container">
		{% for post in posts %}
		<div class="posts">
			<a class="post-title" href="{% url 'post-detail' post.id %}"><h1>{{ post.titulo }}</h1></a>
			<p>{{ post.conteudo }}</p>
			<small>{{ post.data|date:"F d, Y" }}</small>
			<p>Autor: <b>{{ post.autor }}</b></p>
		</div>
		{% endfor %}
	</div>
	
	<div class="paginas">
	{% if is_paginated %}
		{% if page_obj.has_previous %}
			<a href="?page=1">First</a>
			<a href="?page={{ page_obj.previous_page_number }}">Previous</a>
		{% endif %}

		{% for num in page_obj.paginator.page_range %}
			{% if page_obj.number == num %}
		  		<a class="pagina-atual" href="?page={{ num }}">{{ num }}</a>  	
			{% elif num > page_obj.number|add:'-3' and num < page_obj.number|add:'3' %}  
		  		<a href="?page={{ num }}">{{ num }}</a>  	
			{% endif %}
		{% endfor %}

		{% if page_obj.has_next %}
			<a href="?page={{ page_obj.next_page_number }}">Next</a>
			<a href="?page={{ page_obj.paginator.num_pages }}">Last</a>
		{% endif %}	  
	{% endif %}
	</div>

{% endblock content %}
```

Por fim, temos que editar nosso arquivo `style.css`:

```css
body{
	background-color: #13101a;
	color: #beb8cc;
}

.container{
	margin: 65px auto;
	max-width: 1210px;
	padding: 35px;
	border: 2px solid #beb8cc;	
	background-color: black;
	border-radius: 10px;
}

p{
	text-align: justify;
}

.title, form{
	text-align: center;
}

.title{
	font-size: 1.33rem;
	margin-bottom: 30px;
	margin-top: 15px;
	color: #beb8cc;
	text-decoration: none;
}

.posts-container{
	list-style-type: none;
	margin-left: 0;
	margin-top: 30px;
	margin-bottom: 10px;
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(400px, 2fr));
	grid-template-rows: auto;
	justify-content: space-evenly;
	padding: 20px;
	grid-column-gap: 10px;
	overflow-x: hidden;		
	text-align: center;
	box-shadow: 3.6px 3.6px 3.6px 3.6px rgba(0,0,0,.7);
}

.posts{
	border: 2px solid #beb8cc;
	display: block;
	width: 80%;
	padding: 26px;
	margin: auto;
	margin-bottom: 20px;
	opacity: 0.87;
	background-color: #13101a;
	border-radius: 10px;
}

.post-title{
	color: #beb8cc;	
	text-decoration: none;
	font-weight: bold;
}

.post-title:hover{
	color: black;	
	text-decoration: none;
	font-weight: bold;
}

.header{
	display: block;
	margin: auto;
	background-color: #13101a;
	border: 2px solid #beb8cc;
	padding: 15px 0 25px 0;
	width: 91%;
	border-radius: 10px;
}

input{
	margin-top: 8px;
	margin-bottom: 8px;
	width: 80%;
	height: 40px;
	border: 2px solid #beb8cc;
	background-color: black;
	color: #beb8cc;
	border-radius: 8px;
	font-size: 24px;
}

.social{
	text-align: center;
	margin-bottom: 17px;
}

i{
	color: #beb8cc;
	margin-left: 13px;
	margin-right: 13px;
}

.paginas{
	text-align: center;
}

.paginas > a {
	text-decoration: none;
	border: 2px solid #beb8cc;
	padding: 10px;
	color: #beb8cc;
	background-color: #13101a;
}

.pagina-atual{
	background-color: black !important;
}

@media screen and (max-width: 600px) {
    .posts-container {
        list-style-type: none;
        margin-left: 0;
        margin-top: 0;
        display: flex;
        flex-direction: column;
        padding: 5px;
    }
    .posts{
        width: 95%;
        padding: 7px;
    }
}
```

Nosso projetado está finalizado, agora podemos fazer o deployment dele na plataforma Heroku.

# Deployment no Heroku

Nosso primeiro passo no tutorial é fazer o download e instalar a interface de linha de comandos do Heroku para o seu Sistema Operacional, não vamos abordar essa etapa em detalhe, uma vez que ela é particular para cada sistema.

Visite este **[Link](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)** para fazer o download, lembrando que é necessário ter o **[git](https://git-scm.com/)** instalado em sua máquina!

Uma vez que o Heroku esteja instalado em sua máquina com sucesso, agora é necessário fazermos o login, para isso vamos abrir nossa Interface de Linha de Comandos e digitar o seguinte comando:

```
heroku login -i
```

Preencha os seus dados e você será autenticado com sucesso na plataforma Heroku.

Agora que estamos autenticados, vamos acessar o diretório principal de nosso projeto Django, que chamamos de `webapp` e executaremos o seguinte comando:

```
git init
```

Precisamos agora instalar duas novas dependências, certifique-se de que o seu ambiente virtual está ativado e execute:

```
pip install gunicorn
```

```
pip install django-heroku
```

Vamos agora editar o arquivo `settings.py`

Adicionamos ao topo do arquivo:

```python
import django_heroku
```

Adicionamos no fim do arquivo:

```python
django_heroku.settings(locals())
```

Devemos também criar um arquivo chamado de `Procfile` com o seguinte conteúdo:

```
web: gunicorn webapp.wsgi
```

Estamos quase lá, precisamos agora executar o comando:

```
pip freeze > requirements.txt 
```

Nossas dependências estão congeladas no arquivo `requirements.txt`, agora o Heroku já sabe o que deve instalar para nossa aplicação funcionar corretamente.

Precisamos agora criar o nosso aplicativo no Heroku, para esta tarefa vamos executar o seguinte comando:

```
heroku create akiradjango
```

Veja que escolhi o nome `akiradjango`, escolha o nome que mais lhe agradar. Vamos agora enviar nossa aplicação para o Heroku com os seguintes conteúdos:

```
git add .
git commit -m "Arquivos adicionados"
git push heroku master
```

Se eventualmente você se deparar com um erro sobre o package **pkg-resources==0.0.0**, acesse o arquivo `requirements.txt` e remova a linha referente a esta package. Executa os passos anteriores de envio e nossa aplicação estará no ar!

Porém... Veja que se acessarmos nossa aplicação, veremos um erro, algo como: *"relation "blog_post" does not exist"*, este erro é referente ao banco de dados, precisamos executar as migrações para ele funcionar, e depois devemos também criar um super-usuário para que possamos acessar o nosso painel administrativo, exatamente como fizemos no começo de nosso tutorial! Sendo assim, vamos executar os comandos:

```
heroku run python manage.py migrate
```

Com as migrações feitas, nossa aplicação já pode ser visitada em `https://akiradjango.herokuapp.com`, agora precisamos apenas criar o super-usuário, vamos executar:

```
heroku run bash
```

Ganhamos uma interface de linha de comandos que nos fornece acesso direto ao nosso projeto Django, vamos então executar:

```
python manage.py createsuperuser
```

Escolha seu **nome de usuário**, **endereço de email** e defina uma **senha**. Sucesso! Agora podemos navegar até `https://akiradjango.herokuapp.com/admin/` e começar a adicionar **Posts** em nosso Blog.

# Conclusao

Através desse pequeno tutorial fomos capazes de aprender conceitos fundamentais do framework Django e através deles criar um simples Blog no qual fizemos o deploy na plataforma Heroku.

Com essa base, agora podemos avançar criando aplicações mais complexas, com **models** mais elaborados e consultas diversificadas.

Como desafio a você, convido-te a implementar [Markdown](https://en.wikipedia.org/wiki/Markdown) em nosso Blog, para isso, você pode utilizar os seguintes **apps**:

- [Django Markdownify - A Django Markdown filter](https://django-markdownify.readthedocs.io/en/latest/)
- [Django MarkdownX](https://neutronx.github.io/django-markdownx/)

Para fortalecer seus conhecimentos, recomendo fortemente você seguir os seguintes tutoriais:

- [Tutorial Oficial Django: Escrevendo seu primeiro Django app](https://docs.djangoproject.com/en/3.0/intro/tutorial01/)
- [Django Introduction por Mozilla](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Introduction)
- [Tutoriais Django: RealPython](https://realpython.com/tutorials/django/)
- [Curso Completo de Django para Iniciantes](https://www.youtube.com/watch?v=JT80XhYJdBw)
- [Série de Tutoriais Django: Corey Schafer](https://www.youtube.com/playlist?list=PL-osiE80TeTtoQCKZ03TU5fNfx2UY6U4p)

Bons estudos!
