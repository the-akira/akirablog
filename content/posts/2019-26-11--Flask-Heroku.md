---
title: Deploy de uma Aplicação Flask com Heroku
date: "2019-11-26T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/flask-heroku/"
category: "Flask"
tags:
  - "Fundamentos"
  - "Flask"
  - "Python"
description: "Neste guia vamos aprender como fazer Deploy de uma simples aplicação na plataforma Heroku"
---

# Introdução

Heroku é uma **plataforma em nuvem como serviço** ([PaaS](https://en.wikipedia.org/wiki/Platform_as_a_service)) que suporta várias linguagens de programação. Uma das primeiras plataformas em nuvem, o Heroku está em desenvolvimento desde junho de 2007, quando suportava apenas a linguagem de programação Ruby, mas agora suporta Java, Node.js, Scala, Clojure, Python, PHP e Go. Por esse motivo, o Heroku é considerado uma plataforma poliglota, pois possui recursos para um desenvolvedor criar, executar e dimensionar aplicativos de maneira semelhante na maioria dos idiomas.

Neste guia vamos fazer o **[Deploy](https://www.fullstackpython.com/deployment.html)** de uma simples Aplicação Web escrita com o microframework Flask, para isso vamos utilizar a plataforma Heroku. O **deployment** envolve empacotar seu aplicativo Web e colocá-lo em um ambiente de produção que possa executar o aplicativo para que todos no mundo possam vê-lo.

## Tutorial

1. Nosso primeiro passo no tutorial é **fazer o download e instalar** a **interface de linha de comandos** do Heroku para o seu Sistema Operacional, não vamos abordar essa etapa em detalhe, uma vez que ela é particular para cada sistema.

Visite este **[Link](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)** para fazer o download, lembrando que é necessário ter o **[git](https://git-scm.com/)** instalado em sua máquina!

2. Uma vez que o **Heroku** esteja instalado em sua máquina com sucesso, agora é necessário fazermos o **login**, para isso vamos abrir nossa Interface de Linha de Comandos e digitar o seguinte comando:

```
heroku login
```

Uma página em seu navegador será aberta, faça o **login** e sua autenticação estará completa.

3. Agora que estamos autenticados no Heroku, vamos fazer o clone do nosso projeto para podermos fazer o **Deployment**. Para esta tarefa vamos executar o seguinte comando

```
git clone https://github.com/the-akira/Flask-Blog.git
```

Navegamos dentro do diretório principal de nosso projeto e agora vamos reinicializar o repositório com o comando

```
git init
```

Vamos criar um Ambiente Virtual

```
python -m venv myvenv
```

Ativamos o Ambiente Virtual

```
source myvenv/bin/activate
```

Instalamos as dependências necessárias de nosso projeto

```
pip install -r requirements.txt
```

Vamos precisar instalar agora o famoso `gunicorn`. Ele é um servidor Web Python para sistemas operacionais baseados em UNIX. É necessário tê-lo instalado em seu código com o ambiente virtual ativado para iniciar o aplicativo Flask no Heroku.

```
pip install gunicorn
```

Uma vez instalado o **gunicorn**, não podemos esquecer de adicioná-lo ao arquivo `requirements.txt`, sendo assim, vamos executar

```
pip freeze > requirements.txt
```

4. Agora, na verdade, criamos sua instância de aplicativo nos servidores Heroku. É aqui que você especifica o nome do aplicativo. Irei chamar o aplicativo de `akiraflask`

```
heroku create akiraflask
```

5. Uma vez que nosso aplicativo foi criado, agora precisamos criar um arquivo especial chamado `Procfile`. O Procfile será o comando que o Heroku executa para iniciar seu código. Seria como você executando python `app.py`. Crie um arquivo no diretório **root** do seu aplicativo com o nome **Procfile** e insira o seguinte conteúdo

```
web: gunicorn app:app
```

Substitua o primeiro "app" no código acima pelo nome do script que você deseja executar. O script Python que inicia meu aplicativo Flask é chamado app.py, portanto, o Procfile contém app:app. Se o script fosse intitulado run.py, o Procfile exibiria:

```
web: gunicorn run:app
```

6. Agora podemos adicionar, fazer o commit e enviar o código para o Heroku

```
git add .
git commit -m "Procfile Adicionada"
git push heroku master
```

Observe que o envio não apenas envia as alterações, mas também resulta em uma reconstrução. O arquivo `requirements.txt` é verificado mais uma vez e assim por diante.

Nossa aplicação já está **online**, podemos visitá-la em `https://akiraflask.herokuapp.com`

Podemos concluir então que Heroku é uma plataforma muito interessante e que facilita muito o processo de Deployment de aplicações Python e até mesmo de outras linguagens.
