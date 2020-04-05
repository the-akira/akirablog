---
title: Download de Arquivos com Python
date: "2020-04-05T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/download-arquivos-python/"
category: "Fundamentos"
tags:
  - "Web"
  - "Python"
description: "Neste guia vamos explorar diversas técnicas de download de arquivos da Web. Para estas tarefas contaremos com o auxílio de diferentes bibliotecas Python."
---

<img src="https://i.ibb.co/jMv819W/Downloading.png"> </br>

## Introdução

Em redes de computadores, o termo download significa receber dados de um sistema remoto, normalmente um servidor: como um servidor Web, um servidor FTP, um servidor de email ou outros sistemas similares. Isso contrasta com o upload, onde os dados são enviados para um servidor remoto. 

Um download é um arquivo oferecido para download ou que foi baixado ou o processo de recebimento desse arquivo.

### Download com Python

Existem diversas bibliotecas Python que nos auxiliam na tarefa de fazer download de arquivos, neste guia vamos explorar as respectivas:

- [wget](https://pypi.org/project/wget/)
- [PycURL](http://pycurl.io/)
- [urllib](https://docs.python.org/3/library/urllib.html)
- [HTTPX](https://www.python-httpx.org/)
- [Requests](https://requests.readthedocs.io/en/master/)

Veremos exemplos de diferentes scripts e analisaremos os seus funcionamentos. Durante este processo iremos fazer o download de diversos tipos de arquivos.

## A Biblioteca wget

No seguinte exemplo vamos fazer o download da arte "A Última Ceia" de Leonardo da Vinci

```python
import wget

url = 'https://arquivos.netlify.com/SantaCeia.jpg'
wget.download(url,'santaceia.jpg')
```

Observe que não foi necessário muitas linhas de código, apenas importamos a biblioteca, guardamos a URL de nossa imagem em uma variável e por sua vez executamos a função `download()`, salvando assim a imagem como `santaceia.jpg` no mesmo diretório que o script foi executado.

## A Biblioteca PycURL

No seguinte exemplo iremos fazer o download do `index.html` da página web https://pythonwebscraping.netlify.com

```python
import pycurl

with open('index.html', 'wb') as f:
    curl = pycurl.Curl()
    curl.setopt(curl.URL, 'https://pythonwebscraping.netlify.com/')
    curl.setopt(curl.WRITEDATA, f)
    curl.perform()
    curl.close()
```

Estamos utilizando o statement `with` capaz de simplificar o gerenciamento de recursos comuns, como arquivos. Neste caso específico vamos abrir o arquivo `index.html` para escrita.

Iniciamos com a criação do objeto `curl`, utilizamos a função `setopt()` para setarmos a opção da session, e finalizamos com as funções `perform()` para execução das ações e `close()` para fecharmos a conexão.

Ao executarmos o script teremos o arquivo `index.html` salvo no mesmo diretório de nosso script.

## A Biblioteca urllib

O [urllib](https://github.com/python/cpython/tree/3.8/Lib/urllib/) é um pacote que contém vários módulos para trabalharmos com URLs. Um deles é o **request**, que nos permite abrir e ler URLs.

Neste exemplo vamos fazer o download de um arquivo MP3.

```python
import urllib.request

file_url = 'https://arquivos.netlify.com/AnotherBrickInTheWall.mp3'
mp3 = urllib.request.urlopen(file_url)
file_size = int(mp3.length)

print(f'o arquivo possui o seguinte tamanho: {file_size}')

with open('anotherbrickinthewall.mp3', 'wb') as output:
	output.write(mp3.read())
```

Observe que através da função `urlopen()` estamos abrindo a URL do arquivo. Na variável `file_size` estamos guardando o tamanho do arquivo em bytes e em seguida imprimindo seu valor. Por fim salvamos o arquivo MP3 no mesmo diretório de nosso script.

## A Biblioteca HTTPX

O HTTPX é um cliente HTTP completo para Python 3, que fornece APIs síncrona e assíncrona e suporte para HTTP/1.1 e HTTP/2.

No seguinte exemplo vamos utilizar o concurrency model **[async](https://www.python-httpx.org/async/)** capaz de nos prover significantes benefícios de perfomance.

Iremos nos conectar com dois endpoints diferentes da API do Github e salvaremos os recursos como arquivos **json**.

```python
import asyncio
import httpx
import json

async def main():
    async with httpx.AsyncClient() as client:
        response = await client.get('https://api.github.com/gists/public')
        public_gists = json.dumps(response.json(), indent=4, sort_keys=True)
        with open('public_gists.json', 'w') as f:
        	f.write(public_gists)
    async with httpx.AsyncClient() as client:
        response = await client.get('https://api.github.com/emojis')
        emojis = json.dumps(response.json(), indent=4, sort_keys=True)
        with open('emojis.json', 'w') as f:
        	f.write(emojis)

asyncio.run(main())
``` 

Observe que estamos utilizando as palavra-chaves `async/await` para declararmos nossas [Coroutines](https://docs.python.org/3/library/asyncio-task.html), também estamos utilizando o `AsyncClient` para que consigamos executar requisições asíncronas.

## A Biblioteca Requests

Requests é uma biblioteca HTTP elegante e simples para Python, com ela é muito fácil trabalharmos com requisições HTTP.

### Exemplo 1: Arquivo Zip

Neste primeiro exemplo faremos o download de um repositório github

```python
from tqdm import tqdm
import requests

url = "https://github.com/donnemartin/data-science-ipython-notebooks/archive/master.zip"
req = requests.get(url, stream=True, headers={"Accept-Encoding": None})

total_size = int(req.headers.get("Content-Length", 0))
block_size = 1024

filename = url.split("/")[-1]
print(f"Downloading {filename}")

t = tqdm(total=total_size, unit="iB", unit_scale=True)
with open("data-science-notebooks.zip", "wb") as f:
    for data in req.iter_content(block_size):
    	t.update(len(data))
    	f.write(data)
t.close()
```

Observe que estamos importando a biblioteca **requests** e também a biblioteca **tqdm** que nos fornece uma barra de progresso para acompanharmos a evolução de nosso download.

Em seguida estamos utilizando o método `get()` para solicitar o arquivo na respectiva URL, veja que estamos usando os parâmetros `stream=True` e setando o header `"Accept-Enconding": None`.

Obtemos o tamanho do arquivo caso o header `Content-Length` esteja disponível e também definimos o tamanho de cada bloco para `1024`. Utilizamos a função `split()` para obtermos o nome do arquivo que estamos fazendo download.

Por fim executamos o download através da execução das iterações e salvamos o arquivo no mesmo diretório de nosso script.

### Exemplo 2: Arquivo PDF

Neste segundo exemplo faremos o download de um arquivo pdf

```python
from halo import Halo
import requests
import os

spinner = Halo(text='Downloading file...', spinner='dots')
spinner.start()

cwd = os.path.dirname(os.path.realpath(__file__))
url = 'https://arquivos.netlify.com/TheArtOfComputerProgramming.pdf'
file = requests.get(url, allow_redirects=True).content

with open(f'{cwd}/livro.pdf', 'wb') as pdf:
	pdf.write(file)
	
spinner.stop()
```

Perceba que além da biblioteca requests, também estamos importando **halo** e **os**.

A biblioteca halo nos fornecerá uma animação de loading. Iniciamos instanciando o objeto spinner e startamos ele. 

Utilizamos a biblioteca **os** para obtermos o caminho de nosso script para assim salvarmos o arquivo no mesmo diretório do script.

Em seguida fazemos uma requisição através da função `get()`, com o parâmetro `allow_redirects=True`, permitindo assim o redirecionamento.

Por fim salvamos o arquivo com o nome `livro.pdf` e paramos o spinner.

### Exemplo 3: Múltiplas Imagens com Paralelismo

Neste exemplo faremos o download de múltiplas imagens. Para esta tarefa contaremos com o auxílio da biblioteca [multiprocessing](https://docs.python.org/3.4/library/multiprocessing.html) 

```python
from multiprocessing.pool import ThreadPool
import requests
import os

cwd = os.path.dirname(os.path.realpath(__file__))

urls = [
    (f"{cwd}/1.jpg","https://res.cloudinary.com/theakira/image/upload/v1577882990/books/yibdrffecx9eouoa03wt.jpg"),
    (f"{cwd}/2.jpg","https://res.cloudinary.com/theakira/image/upload/v1577796198/books/pshjs5haugk7zjc4cp5v.jpg"),
    (f"{cwd}/3.jpg","https://res.cloudinary.com/theakira/image/upload/v1577788502/books/ug85fm5yfmnnvmatexgh.jpg"),
    (f"{cwd}/4.jpg","https://res.cloudinary.com/theakira/image/upload/v1577754647/books/sdfsasw1nk3omkyvr5bn.jpg"),
    (f"{cwd}/5.jpg","https://res.cloudinary.com/theakira/image/upload/v1577496602/books/upnvtmunzzqp6xtbfjrz.jpg"),
    (f"{cwd}/6.jpg","https://res.cloudinary.com/theakira/image/upload/v1577496704/books/v41rhsi8llqunvrplwog.jpg"),
    (f"{cwd}/7.jpg","https://res.cloudinary.com/theakira/image/upload/v1575248626/books/akc2bqsllxyblmhertjg.jpg"),
    (f"{cwd}/8.jpg","https://res.cloudinary.com/theakira/image/upload/v1577491761/books/nrbjfwvmw2zeik07j4ra.jpg"),
    (f"{cwd}/9.jpg","https://res.cloudinary.com/theakira/image/upload/v1571363356/books/aranrpgoypm3prjq6xe2.jpg"),
    (f"{cwd}/10.jpg","https://arquivos.netlify.com/SantaCeia.jpg"),
    (f"{cwd}/11.jpg","https://res.cloudinary.com/theakira/image/upload/v1577497190/books/ibibnw4najtmnz1gsna1.jpg"),
    (f"{cwd}/12.jpg","https://res.cloudinary.com/theakira/image/upload/v1575248527/books/qcamggmgqazt8dbvunac.jpg")
]

def fetch_url(entry):
    path, uri = entry
    if not os.path.exists(path):
        r = requests.get(uri, stream=True)
        if r.status_code == 200:
            with open(path, 'wb') as f:
                for chunk in r:
                    f.write(chunk)
    return path

threads = len(urls)
results = ThreadPool(threads).imap_unordered(fetch_url, urls)
for path in results:
    print(path)
```

O `multiprocessing.pool.ThreadPool` se comporta da mesma forma que o `multiprocessing.pool.Pool` com a única diferença que usa [threads](https://en.wikipedia.org/wiki/Thread_(computing)) em vez de processos para executar a lógica dos workers.

Novamente estamos definindo o diretório em que os arquivos serão salvos, nesse caso, o mesmo em que o script se encontra.

Guardamos as URLs em uma lista de tuplas: Contendo respectivamente o local que o arquivo será salvo e seu nome, bem como sua URL na web.

Definimos a função `fetch_url()` responsável por fazer o download dos arquivos e nos retornar seu respectivo caminho que será salvo.

Por fim, definimos o número de threads baseado na quantidade de urls em nossa lista e iniciamos a execução das threads, imprimindo cada arquivo que baixamos em nosso prompt de comandos.

## Conclusão

Neste breve guia apresentamos a você os métodos e bibliotecas mais comuns para fazermos download de arquivos com Python.

Também fomos capazes de explorar técnicas como **async/await** e **threads** de forma a maximizarmos a eficiência de nossos scripts.

Você pode utilizar a documentação de cada biblioteca como referência para estudos mais aprofundados.
