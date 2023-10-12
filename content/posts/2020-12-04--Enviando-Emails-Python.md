---
title: Enviando Emails com Python
date: "2020-04-12T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/enviando-emails-python/"
category: "Fundamentos"
tags:
  - "Web Development"
  - "Python"
  - "Fundamentos"
  - "Programação"
description: "Neste breve guia vamos explorar técnicas de envio de Emails com Python."
---

## Introdução

*Electronic mail* (**email** ou **e-mail**) é um método de troca de mensagens ("correio") entre pessoas que usam dispositivos eletrônicos.

**[Ray Tomlinson](https://en.wikipedia.org/wiki/Ray_Tomlinson)** é creditado como o inventor do email, pois em 1971 ele desenvolveu o primeiro sistema capaz de enviar email entre usuários em diferentes hosts na [ARPANET](https://developer.mozilla.org/en-US/docs/Glossary/Arpanet), usando o sinal **@** para vincular o nome do usuário a um servidor de destino. Em meados da década de 1970, esse formulário já era reconhecido como **email**.

O **email** opera através de redes de computadores, que hoje são principalmente a Internet. Os sistemas de email de hoje são baseados em um modelo de armazenamento e encaminhamento. Os servidores de email aceitam, encaminham, entregam e armazenam mensagens. Nem os usuários e nem seus computadores precisam estar online simultaneamente; eles precisam se conectar apenas brevemente, geralmente a um servidor de email ou a uma interface de webmail pelo tempo necessário para enviar ou receber mensagens ou fazer o download.

A linguagem [Python](https://www.python.org/) conta com o módulo [smtplib](https://docs.python.org/3/library/smtplib.html) embutido, para enviar emails usando o **[SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)**(Simple Mail Transfer Protocol) de forma simplificada e eficaz.

### A Biblioteca smtplib

O módulo **[smtplib](https://docs.python.org/3/library/smtplib.html)** define um objeto de sessão do cliente SMTP que pode ser usado para enviar email para qualquer máquina da Internet com um daemon ouvinte **SMTP** ou **ESMTP**.

Para obter detalhes sobre a operação SMTP e ESMTP, consulte o [RFC 821](https://tools.ietf.org/html/rfc821.html) (Simple Mail Transfer Protocol) e o [RFC 1869](https://tools.ietf.org/html/rfc1869.html) (SMTP Service Extensions).

Nos exemplos usados nesse guia, vamos utilizar o **SMTP Server** do Gmail para o envio dos Emails, porém, esteja ciente que o mesmo princípio se aplicará para outros serviços de email:

- Caso você ainda não possua uma conta no Gmail, você pode criá-la no seguinte endereço: **[Crie uma conta no Gmail](https://accounts.google.com/signup)**
- Lembre de ativar a opção: **[Permitir aplicativos menos seguros](https://myaccount.google.com/lesssecureapps)**, ela é um requisito necessário, para que possamos enviar emails com Python.

## Enviando Email Plain-Text

Primeiramente, vejamos como enviar um simples email em [Plain-Text](https://en.wikipedia.org/wiki/Plain_text):

```python
from email.message import EmailMessage
import smtplib

EMAIL = 'gabrielfelippe90@gmail.com'
SENHA = input('Digite a senha do seu Email: ')

msg = EmailMessage()
msg['Subject'] = 'Linguagem Python'
msg['From'] = EMAIL
msg['To'] = 'gabrielfelippe@outlook.com'
msg.set_content('Contéudo do Email enviado com Python!')

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
    smtp.login(EMAIL, SENHA)
    smtp.send_message(msg)
```

O código acima cria uma conexão segura com o servidor SMTP do [Gmail](https://www.google.com/gmail/), usando o `SMTP_SSL()` do smtplib para iniciar uma conexão criptografada por [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security).

Estamos definindo nosso endereço de `EMAIL` de envio, e obtendo a `SENHA` do Email através do método `input()`, por questões de segurança, uma outra opção seria utilizar uma [variável de ambiente](https://en.wikipedia.org/wiki/Environment_variable) ou até mesmo a função [getpass()](https://docs.python.org/2/library/getpass.html) do Python.

Através do método `EmailMessage()` construímos o objeto que irá definir nosso Email:

- `msg['Subject']` é o assunto do email que estamos enviando.
- `msg['From']` é o email que está enviando a mensagem.
- `msg['To']` é o email que receberá nossa mensagem.
- `msg.set_content()` define o conteúdo da mensagem que enviaremos.

Finalmente criamos a conexão segura, efetuamos o **login** e enviamos a mensagem.

Ao executarmos nosso script, o email será enviado com sucesso para o endereço `gabrielfelippe@outlook.com`

## Enviando Email com Imagens

Imagine que desejamos enviar um email com as seguintes imagens:

![img](https://arquivos.netlify.com/imagem2.jpg)

![img](https://arquivos.netlify.com/imagem.jpg)

Salvaremos ambas no mesmo diretório de nosso script Python com os respectivos nomes: `imagem.jpg` e `imagem2.jpg`.

Contaremos com o auxílio do módulo `imghdr()` para trabalharmos com as imagens, ele determinará o tipo de imagem contida em um arquivo ou *byte stream*.

```python
from email.message import EmailMessage
import smtplib
import imghdr

EMAIL = 'gabrielfelippe90@gmail.com'
SENHA = input('Digite a senha do seu Email: ')

msg = EmailMessage()
msg['Subject'] = 'Teste'
msg['From'] = EMAIL
msg['To'] = 'gabrielfelippe@outlook.com'
msg.set_content('Imagem anexada')

files = ['imagem.jpg','imagem2.jpg']

for file in files:
    with open(file, 'rb') as f:
        file_data = f.read()
        file_type = imghdr.what(f.name)
        file_name = f.name

    msg.add_attachment(file_data, 
        maintype='image', 
        subtype=file_type,
        filename=file_name
    )

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
    smtp.login(EMAIL, SENHA)
    smtp.send_message(msg)
```

Observe que na lista `files` estamos guardando o nome das imagens que desejamos enviar, em seguida estamos abrindo ambas as imagens no modo *read-binary*. Guardamos os dados das imagens na variável `file_data`, o tipo de arquivo na variável `file_type` e o nome do arquivo na variável `file_name`.

Com o método `add_attachment()` anexamos nossas imagens à mensagem.

Finalmente enviamos o email, seguindo o mesmo procedimento do script anterior.

## Enviando Email com Arquivos PDF

O envio de documentos PDF's através de emails é uma prática muito comum na Internet.

Imagine, por exemplo, que desejamos enviar os livros: [Neuromancer](https://arquivos.netlify.com/Neuromancer.pdf) e [Simulacra and Simulation](https://arquivos.netlify.com/Simulacra_and_Simulation.pdf) para **dois** de nossos contatos, sendo assim, vamos salvar ambos os livros no mesmo diretório de nosso script Python.

```python
from email.message import EmailMessage
import smtplib

EMAIL = 'gabrielfelippe90@gmail.com'
SENHA = input('Digite a senha do seu Email: ')

contacts = ['gabrielfelippe@outlook.com','akirascientist@gmail.com']

msg = EmailMessage()
msg['Subject'] = 'Teste'
msg['From'] = EMAIL
msg['To'] = ', '.join(contacts)
msg.set_content('Livro em Anexo')

files = ['Simulacra_and_Simulation.pdf','Neuromancer.pdf']

for file in files:
    with open(file, 'rb') as f:
        file_data = f.read()
        file_name = f.name

    msg.add_attachment(file_data, 
        maintype='application', 
        subtype='octet-stream',
        filename=file_name
    )

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
    smtp.login(EMAIL, SENHA)
    smtp.send_message(msg)
```

Observe que estamos armazenando os contatos que receberão os livros na variável `contacts`, e estamos usando a função `join()` em `msg['To']` para criar a lista de contatos separada por vírgula.

Novamente guardamos os nomes dos livros que enviaremos na lista `files`.

Seguimos o procedimento similar ao de envio de imagens, porém veja que nesse caso específico estamos utilizando dois parâmetros diferentes no método `add_attachment()`:

- Definimos o `maintype` como **application**
- Definimos o `subtype` como **octet-stream**

Finalmente enviamos o email com os livros em anexo.

## Enviando Email com Conteúdo HTML

Também é possível adicionarmos conteúdo HTML em nossa mensagem, considere o seguinte exemplo:

```python
from email.message import EmailMessage
import smtplib

EMAIL = 'gabrielfelippe90@gmail.com'
SENHA = input('Digite a senha do seu Email: ')

contacts = ['gabrielfelippe@outlook.com','akirascientist@gmail.com']

msg = EmailMessage()
msg['Subject'] = 'Teste'
msg['From'] = EMAIL
msg['To'] = ', '.join(contacts)

msg.set_content('Conteúdo em texto puro')
msg.add_alternative("""
<!DOCTYPE html>
<html>
    <body>
        <h1 style="color:MidnightBlue;">Email escrito em HTML!</h1>
    </body>
</html>
""", subtype='html')

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
    smtp.login(EMAIL, SENHA)
    smtp.send_message(msg)
```

Perceba que utilizamos o método `add_alternative()`, passando como argumento o conteúdo HTML que enviaremos e o **subtype** respectivo, que nesse caso é justamente HTML.

Em seguida enviamos o email para nossos contatos!

## Conclusão

Através desse breve guia fomos capazes de aprender técnicas de envio de emails com a linguagem Python, bem como anexar arquivos e até mesmo enviar conteúdo HTML.

Para maiores detalhes você pode consultar a documentação da biblioteca smtplib em nossas referências.

Bons estudos!

## Referências

- [Email Wikipedia](https://en.wikipedia.org/wiki/Email)
- [smtplib](https://docs.python.org/3/library/smtplib.html)
- [Python Send Email](https://realpython.com/python-send-email/)
- [Email Examples](https://docs.python.org/3/library/email.examples.html)
- [Send Emails Using Python](https://www.youtube.com/watch?v=JRCJ6RtE3xU)
