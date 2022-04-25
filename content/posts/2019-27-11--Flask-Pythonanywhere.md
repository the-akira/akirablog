---
title: Deploy de uma Aplicação Flask com PythonAnywhere
date: "2019-11-27T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/flask-pythonanywhere/"
category: "Flask"
tags:
  - "Fundamentos"
  - "Flask"
  - "Python"
  - "Web"
description: "Neste guia vamos aprender como fazer Deploy de uma simples aplicação na plataforma PythonAnywhere."
---

# Introdução

O **PythonAnywhere** é um ambiente de desenvolvimento integrado online ([IDE](https://pt.wikipedia.org/wiki/Ambiente_de_desenvolvimento_integrado)) e um serviço de hospedagem na web ([plataforma como serviço](https://pt.wikipedia.org/wiki/Plataforma_como_servi%C3%A7o)) baseado na linguagem de programação Python. Fundada por **Giles Thomas** e **Robert Smithson** em 2012, fornece acesso no navegador a interfaces de linha de comando Python e Bash baseadas em servidor, além de um editor de código com destaque de sintaxe. Os arquivos do programa podem ser transferidos de e para o serviço usando o navegador do usuário. Os aplicativos da Web hospedados pelo serviço podem ser gravados usando qualquer estrutura de aplicativos baseada em [WSGI](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface).

O **WSGI** não é um servidor, um módulo python, uma estrutura, uma API ou qualquer tipo de software. É apenas uma especificação de interface pela qual o servidor e o aplicativo se comunicam. Os lados da interface do servidor e do aplicativo são especificados no [PEP 3333](https://www.python.org/dev/peps/pep-3333/). Se um aplicativo (ou estrutura ou kit de ferramentas) for escrito na especificação WSGI, ele será executado em qualquer servidor escrito nessa especificação.

Neste guia vamos fazer o **[Deploy](https://www.fullstackpython.com/deployment.html)** de uma simples Aplicação Web escrita com o microframework Flask, para isso vamos utilizar a plataforma PythonAnywhere. O deployment envolve empacotar seu aplicativo Web e colocá-lo em um ambiente de produção que possa executar o aplicativo para que todos no mundo possam vê-lo.

## Tutorial

1. Nosso primeiro passo neste guia será fazer o registro na plataforma **PythonAnywhere**, para esta tarefa vamos navegar até este **[Link](https://www.pythonanywhere.com/registration/register/beginner/)** e fazer nosso cadastro. Importante, lembre-se de confirmar seu endereço de email.

Uma vez que estamos cadastrados com sucesso, agora podemos nos autenticar na plataforma para obtermos acesso a todas as suas funcionalidades. 

Vamos visualizar uma interface similar a essa

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/Imagens/Tutoriais/PythonAnyWhere.png)

2. Nesse passo, vamos clicar no botão **Open Web tab**, em seguida vamos clicar no botão **Add a new web app**.

Nos será apresentada a informação de que nossa aplicação viverá em determinado endereço, no meu caso vai ser `akiraflask.pythonanywhere.com`, uma vez que eu me cadastrei como o usuário **akiraflask**.

Tudo certo até então, vamos clicar no botão **Next**

PythonAnywhere nos pedirá para escolher o framework que vamos trabalhar, em nosso caso será o **Flask**, mas veja como existem diversas outras opções, por fim ele pedirá para escolhermos uma versão do Python, vamos escolher a última versão **3.8**.

Por fim ele nos pedirá para entrarmos com o caminho para o arquivo que irá guardar nossa aplicação, vamos fornecer a seguinte informação:

`/home/seu_usuario/mysite/app.py`

**Importante**: Não esqueça de substituir o seu usuário no caminho do arquivo

Nesse ponto, nossa aplicação já está **online** como um simples Hello World, porém agora precisamos configurar a nossa aplicação que reside no **GitHub**.

3. Vamos agora retornar para o painel de controle do PythonAnywhere (representado na primeira imagem) e vamos clicar no botão **$Bash** para acessarmos o terminal de comandos.

Primeiramente vamos digitar o comando `ls` para vermos o que há disponível

Veja que temos um arquivo `README.txt` e um diretório chamado `mysite` que contém um arquivo `app.py` que representa o simples Hello World, vamos remover esse diretório com o comando `rm -r mysite/` e vamos clonar o repositório de nossa aplicação com o seguinte comando:

```
git clone https://github.com/the-akira/Flask-Pokedex.git mysite
```

Uma vez que o clone foi feito, agora já temos nossa aplicação residindo no PythonAnywhere, agora só precisamos navegar até o diretório **mysite** com o comando `cd mysite/` e ativar o Ambiente Virtual, instalar as dependências e tudo deve funcionar corretamente. Vamos executar os seguintes comandos:

Para iniciar e ativar o Ambiente Virtual digite:

```
mkvirtualenv --python=/usr/bin/python3.8 my-virtualenv
```

Nesse ponto o ambiente já está ativado, caso eventualmente queira desativá-lo digite: `deactivate` e caso você queira reativá-lo digite: `workon my-virtualenv`.

Agora finalmente podemos instalar as dependências necessárias para nossa aplicação funcionar corretamente, vamos executar o seguinte comando:

```
pip install -r requirements.txt
```

4. Nosso último passo agora é muito simples, vamos retornar para o painel de controle e vamos clicar novamente no botão **Open Web tab**, por fim vamos clicar no botão verde **Reload** para recarregar nossa aplicação com as novas configurações.

Finalizado, nossa aplicação está online e reside no seguinte endereço `https://akiraflask.pythonanywhere.com`

Muito simples, não? PythonAnywhere facilita muito nosso workflow e nos permite trabalharmos com git, além de muitos outros frameworks Python.
