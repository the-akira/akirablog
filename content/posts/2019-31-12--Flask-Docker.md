---
title: Transformando uma Aplicação Flask em um Docker Container e Deployment com Heroku
date: "2019-12-31T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/flask-docker/"
category: "Flask"
tags:
  - "Fundamentos"
  - "Flask"
  - "Python"
  - "Web"
description: "Neste guia vamos aprender como transformar uma aplicação Flask em um Docker Container, assim como, fazer Deploy desta aplicação na plataforma Heroku"
---

# Introdução

Neste guia buscamos como objetivo transformar uma aplicação **Flask** em um **[Docker Container](https://www.docker.com/resources/what-container)** e em seguida fazer deploy desta aplicação na plataforma Heroku. Para estas tarefas, é importante, primeiramente, entendermos o que é a tecnologia Docker e como ela pode ajudar os desenvolvedores.

## Docker

O Docker é um conjunto de produtos de plataforma como serviço [(PaaS)](https://en.wikipedia.org/wiki/Platform_as_a_service) que usam a virtualização no nível do Sistema Operacional para fornecer software em pacotes chamados contêineres. Os contêineres são isolados um do outro e agrupam seu próprio software, bibliotecas e arquivos de configuração; eles podem também se comunicar através de canais bem definidos. Todos os contêineres são executados por um único kernel do sistema operacional e, portanto, são mais leves que as máquinas virtuais, o software que hospeda os contêineres é chamado **Docker Engine**:

![img](https://docs.docker.com/engine/images/engine-components-flow.png)

### Docker Container

Um contêiner é uma unidade padrão de software que empacota o código e todas as suas dependências para que o aplicativo seja executado de maneira rápida e confiável de um ambiente de computação para outro. Uma imagem de contêiner do Docker é um pacote de software leve, independente e executável que inclui tudo que é necessário para executar um aplicativo: código, tempo de execução, ferramentas do sistema, bibliotecas e configurações do sistema.

As imagens de contêineres se tornam contêineres em tempo de execução, e no caso de contêineres do Docker - as imagens se tornam contêineres quando executadas no **Docker Engine**. Disponível para aplicativos baseados em Linux e Windows, o software em contêiner sempre será executado da mesma forma, independentemente da infraestrutura. Os contêineres isolam o software de seu ambiente e garantem que ele funcione de maneira uniforme, apesar das diferenças, por exemplo, entre desenvolvimento e preparação.

Contêineres do Docker que são executados no Docker Engine:

- **Padrão:** Docker criou o padrão do setor para contêineres, para que eles pudessem ser portáteis em qualquer lugar
     
- **Leve:** os contêineres compartilham o kernel do sistema operacional da máquina e, portanto, não exigem um sistema operacional por aplicativo, aumentando a eficiência do servidor e reduzindo os custos com servidor e licenciamento
     
- **Seguro:** os aplicativos são mais seguros em contêineres e o Docker fornece os recursos de isolamento padrão mais fortes do setor

Para maiores detalhes técnicos profundos sobre Docker você poder ler sua **[completa documentação](https://docs.docker.com/engine/docker-overview/)**. Para instalar Docker em seu computador visite: **[Instalação](https://docs.docker.com/install/)** e selecione sua plataforma, uma vez que Docker esteja instalado em sua máquina podemos seguir para a próxima etapa de nosso tutorial.

É importante também citarmos o **[Docker Hub](https://hub.docker.com/)**, que é a maior biblioteca e comunidade para imagens de contêiner. Procure mais de 100.000 imagens de contêineres de fornecedores de software, projetos de código aberto e da comunidade. Para o nosso tutorial específico, não vamos buscar uma imagem pronta, mas sim construir a nossa própria.

### Criando a Aplicação

Partindo do princípio, vamos iniciar criando o diretório principal de nossa aplicação, para esta tarefa irei utilizar o comando **mkdir**, sinta-se a vontade para criar da forma que lhe for mais conveniente.

```
mkdir flask_docker
```

Criamos agora o script Python que chamaremos de `app.py` ele irá conter nosso aplicativo. A diferença da aplicação usual do Flask está na linha 5, onde obtemos a porta do Heroku. Sem essa linha, o Heroku não conseguiria vincular a porta e, em vez disso, você receberia uma mensagem de erro dizendo *“Web process failed to bind $PORT within 60 seconds of launch”*.

```python
from flask import Flask
import os

app = Flask(__name__)
port = int(os.environ.get("PORT", 5000))

@app.route('/')
def home():
	return 'Aplicação Flask Dockerizada'

if __name__ == '__main__':
	app.run(host='0.0.0.0',port=port)
```

Uma vez que construímos o arquivo `app.py`, agora é necessário criarmos o arquivo `requirements.txt` que irá guardar todas as dependências de nossa aplicação, em nosso caso específico, precisaremos apenas do Flask.

```
Flask==1.1.1
```

Agora devemos criar o importante arquivo **Dockerfile**, importante: é necessário criá-lo com o exato nome **Dockerfile** para que possamos construir a imagem e executar o aplicativo Flask. Você pode usar a imagem base do Ubuntu ou Python. Eu usei o Ubuntu como minha imagem base e instalei o Python nele.

```
FROM ubuntu:16.04

RUN apt-get update -y && \
    apt-get install -y python-pip python-dev

COPY ./requirements.txt /app/requirements.txt

WORKDIR /app

RUN pip install -r requirements.txt

COPY . /app

ENTRYPOINT [ "python" ]

CMD [ "app.py" ]
```

Compreendendo as instruções Docker utilizadas:

1. **&& \** não é um comando Docker específico, ele serve para dizer ao Linux que ele deve rodar o próximo comando como parte da linha existente (para assim evitarmos usar múltiplas directivas *RUN*)

2. **COPY** irá copiar os arquivos do primeira parâmetro (da origem .) para o parâmetro destino (nesse caso: **/app**)

3. **WORKDIR** vai setar o diretório de trabalho (todas as seguintes instruções vão operar dentro desse diretório).

4. **ENTRYPOINT** configurará o container para rodar como um executável: apenas a última instrução **ENTRYPOINT** irá executar.

5. **pip** irá instalar através do arquivo `requirements.txt` normalmente, uma vez que temos apenas o Flask nele, ele será a nossa única dependência.

Nosso **Dockerfile** está estabelecido corretamente, agora vamos construir a imagem Docker e rodá-la para termos certeza de que o serviço está funcionando localmente.

```
docker build -t flask-docker:latest .
```

O processo de construção pode levar alguns minutos, depois de pronto, podemos rodar nossa imagem como um container através do seguinte comando:

```
docker run -d -p 5000:5000 flask-docker
```

Para vermos nossa aplicação funcionando, podemos visitar o seguinte endereço: `http://0.0.0.0:5000/`. Importante: Verifique se você está usando as portas corretas. Flask, por padrão, é executado na porta 5000.

### Deploy da Aplicação no Heroku

Uma vez que nossa aplicação está funcionando com sucesso em nossa máquina local, podemos agora começar o processo de Deployment na plataforma Heroku.

Nosso primeiro passo é fazer login no Heroku, caso você não o tenha instalado ainda, Visite este **[Link](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)** para fazer o download, lembrando que é necessário ter o **[git](https://git-scm.com/)** instalado em sua máquina!

Tendo o Heroku instalado com sucesso, vamos abrir nossa interface de linha de comandos e digitar o seguinte comando:

```
heroku login -i
```

Digite suas credenciais e você estará autenticado com sucesso na plataforma. 

Precisamos também fazer login no **Container Registry**, sendo assim, vamos executar:

```
heroku container:login
```

Agora é hora de criarmos um novo aplicativo Heroku. Você pode escolher um nome ou deixar o Heroku criar um nome mágico para o seu aplicativo, em nosso caso, chamaremos nossa aplicação de **dockerflaskpy**.

```
heroku create dockerflaskpy
```

**Importante:** Certifique-se de que você está dentro do diretório principal de nossa aplicação (este que contém 3 arquivos: app.py, requirements.txt e Dockerfile) e execute o seguinte comando para criarmos o container no Heroku:

```
heroku container:push web --app dockerflaskpy
```

Veja que o procedimento pode levar alguns minutos para ser concluído, ao finalizar vamos receber a seguinte mensagem: *"Your image has been successfully pushed. You can now release it with the 'container:release' command."* nos indicando que nossa imagem foi enviada com sucesso.

Estamos quase concluindo nossa implantação. O contêiner foi enviado, mas ainda não foi liberado. Não sei exatamente qual poderia ser o motivo para colocá-lo no estágio inicial antes de liberá-lo. De qualquer forma, o comando abaixo irá liberar o contêiner.

```
heroku container:release web --app dockerflaskpy
```

Feito! Funcionou! Nossa aplicação já está **live** no seguinte endereço: `https://dockerflaskpy.herokuapp.com/`

### Conclusão

Neste pequeno tutorial fomos capazes de aprender um pouco sobre a tecnologia Docker, bem como a transformação de uma aplicação Flask em um Docker Container no qual enviamos para o Heroku.

Para aprender mais sobre Docker você pode visitar o curso gratuito: **[Docker Tutorial for Beginners - A Full DevOps Course on How to Run Applications in Containers](https://www.youtube.com/watch?v=fqMOX6JJhGo)**, um excelente curso para iniciantes com Docker!

Namaskar.
