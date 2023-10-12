---
title: Testes Unitários com unittest - Testando uma Aplicação Flask
date: "2020-05-01T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/testes-unitarios/"
category: "Programação"
tags:
  - "Fundamentos"
  - "Python"
  - "Programação"
description: "Neste guia vamos explorar o framework unittest do Python para testes automatizados através da execução de testes em uma Aplicação Flask."
---

## Introdução

No universo da programação de computadores, o [teste de unidade](https://pt.wikipedia.org/wiki/Teste_de_unidade) é um método de teste de software pelo qual unidades individuais do código-fonte, conjuntos de um ou mais módulos de um programa de computador, juntamente com dados de controle associados, procedimentos de uso e procedimentos operacionais, são testados para determinar se estes são adequados para uso.

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/Unit%20Testing/1.png)

A figura acima ilustra uma visão geral sobre os diferentes tipos de testes, nesse guia focaremos apenas nos testes unitários.

Os testes de unidade geralmente são testes automatizados escritos e executados por desenvolvedores de software para garantir que uma seção de um aplicativo (conhecida como "unidade") atenda ao seu design e se comporte conforme o pretendido. 

Na programação procedural, uma unidade pode ser um módulo inteiro, mas geralmente é uma função ou procedimento individual. Na programação orientada a objetos, uma unidade geralmente é uma interface inteira, como uma classe, mas pode ser um método individual. 

## Importância

O objetivo do teste de unidade é isolar cada parte do programa e mostrar que as partes individuais estão corretas. Um teste de unidade fornece um contrato estrito e por escrito que o código deve cumprir. Como resultado, oferece vários benefícios.

O teste de unidade encontra problemas no início do ciclo de desenvolvimento. Isso inclui erros na implementação do programador e falhas ou partes ausentes da especificação da unidade. O processo de escrever um conjunto completo de testes força o autor a refletir sobre *inputs*, *outputs* e condições de erro e, assim, definir com mais clareza o comportamento desejado da unidade. 

O custo de encontrar um bug antes do início da codificação ou quando o código é escrito pela primeira vez é consideravelmente menor que o custo de detectar, identificar e corrigir o bug posteriormente. Erros no código liberado também podem causar problemas para os usuários finais do software.

No **[test-driven development (TDD)](https://en.wikipedia.org/wiki/Test-driven_development)** que é freqüentemente usado em [Extreme Programming](https://en.wikipedia.org/wiki/Extreme_programming) e [Scrum](https://en.wikipedia.org/wiki/Scrum_(software_development)), os testes de unidade são criados antes mesmo do próprio código ser escrito. Quando os testes passam, esse código é considerado completo.

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/Unit%20Testing/2.png)

O teste de unidade permite ao programador [refatorar](https://en.wikipedia.org/wiki/Code_refactoring) o código ou atualizar as bibliotecas do sistema posteriormente e garantir que o módulo ainda funcione corretamente. O procedimento é escrever casos de teste para todas as funções e métodos, para que sempre que uma alteração cause uma falha, ela possa ser rapidamente identificada. Os testes de unidade detectam alterações que podem quebrar um [design contract](https://en.wikipedia.org/wiki/Design_by_contract).

## Unit testing framework - unittest

O **[unittest](https://docs.python.org/3/library/unittest.html)** unit testing framework foi originalmente inspirado no [JUnit](https://junit.org/junit5/) e tem um aspecto semelhante às principais estruturas de teste de unidade de outros idiomas. Ele suporta automação de testes, compartilhamento de códigos de configuração e desligamento de testes, agregação de testes em coleções e independência dos testes do framework de relatórios.

Para alcançar esse potencial, **unittest** suporta conceitos importantes em uma maneira orientada a objetos:

- **test fixture**: Um test fixture representa a preparação necessária para executar um ou mais testes e quaisquer ações de limpeza associadas. Isso pode envolver, por exemplo, a criação de bancos de dados temporários ou proxy, diretórios ou o início de um processo do servidor.

- **test case**: Um **test case** é a unidade individual de teste. Ele verifica uma resposta específica para um conjunto específico de entradas. O unittest fornece uma classe base, **[TestCase](https://docs.python.org/3/library/unittest.html#unittest.TestCase)**, que pode ser usada para criar novos casos de teste.

- **test suite**: Um **test suite** é uma coleção de casos de teste, conjuntos de testes ou ambos. É usado para agregar testes que devem ser executados juntos.

- **test runner**: Um **test runner** é um componente que orquestra a execução dos testes e fornece o resultado ao usuário. Ele pode usar uma interface gráfica, uma interface textual ou retornar um valor especial para indicar os resultados da execução dos testes.

## Executando Testes

Vejamos um exemplo de um simples teste para compreendermos melhor o funcionamento do framework unittest, para isso vamos criar o arquivo `calculadora.py` e vamos adicionar duas funções nele.

```python
def adicao(x, y):
	"""
	Função de adição
	"""
	return x + y

def subtracao(x, y):
	"""
	Função de subtração
	"""
	return x - y
```

Agora que temos duas funções, vamos criar o arquivo `test_calculadora.py`, onde escreveremos nossos testes.

```python
import calculadora
import unittest

class TestCalculadora(unittest.TestCase):

	def test_adicao(self):
		self.assertEqual(calculadora.adicao(13,13), 26)
		self.assertEqual(calculadora.adicao(-3,-6), -9)
		self.assertEqual(calculadora.adicao(0,0), 0)

	def test_subtracao(self):
		self.assertEqual(calculadora.subtracao(12,6), 6)
		self.assertEqual(calculadora.subtracao(-2,2), -4)
		self.assertEqual(calculadora.subtracao(-3,-3), 0)

if __name__ == '__main__':
	unittest.main()
```

Iniciamos importando o módulo **calculadora** onde estão as funções que desejamos testar, em seguida importamos o módulo **unittest**, logo então definimos a classe **TestCalculadora**, herdando de **unittest.Testcase**, que nos fornecerá diversas funcionalidades em nossa classe.

Em nossa classe definimos dois métodos:

- **test_adicao**: Responsável por testar a função **adicao()**, utilizamos o método **assertEqual()** para checar pelos resultados esperados.
- **test_subtracao**: Responsável por testar a função **subtracao()**, utilizamos o método **assertEqual()** para checar pelos resultados esperados.

Por fim, **unittest.main()** irá executar todos os nossos testes quando executarmos o arquivo `test_calculadora.py`, sendo assim, vamos então executá-lo:

```
python test_calculadora.py
```

Receberemos o seguinte output:

```
..
-------------------------------------------------------------------
Ran 2 tests in 0.000s

OK
```

Nos indicando **OK**, que todos os testes passaram com sucesso. Com os fundamentos em mente, agora já podemos iniciar a construção de nossa pequena aplicação Flask para explorarmos mais casos de testes.

## Criando a Aplicação Flask

Você pode obter esta Aplicação diretamente pelo GitHub: [Flask-Testing](https://github.com/the-akira/Flask-Testing).

Iniciaremos criando o diretório da Aplicação com o comando **mkdir**

```
mkdir Flask-Testing
```

Acessamos o diretório com o comando **cd**

```
cd Flask-Testing/
```

Criamos um [ambiente virtual](https://github.com/the-akira/Python-Iluminado/blob/master/Capitulos/29.PIP.md) Python

```
python -m venv myvenv
```

Ativamos o ambiente virtual

```
source myvenv/bin/activate
```

Vamos agora instalar as bibliotecas necessárias para nossa aplicação

```
pip install flask
pip install flask-sqlalchemy
```

Criaremos agora o arquivo principal de nossa aplicação, que chamarei de `app.py`, para esta tarefa utilizarei o comando **touch**

```
touch app.py
```

O arquivo `app.py` contará com o seguinte conteúdo

```python
from flask import Flask, request, render_template, url_for, redirect
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///site.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['SECRET_KEY'] = 'flask'
app.config['USERNAME'] = 'admin'
app.config['PASSWORD'] = 'admin'
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def __repr__(self):
        return f'<User {self.username}>' 

@app.route('/')
def index():
    return 'Hello, World!'

@app.route("/api")
def api():
    return {
        "username": 'admin',
        "email": 'admin@example.com'
    }

@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != app.config['USERNAME']:
            error = 'Usuário inválido'
        elif request.form['password'] != app.config['PASSWORD']:
            error = 'Senha inválida'
        else:
            return redirect(url_for('index'))
    return render_template('login.html', error=error)

if __name__ == '__main__':
    app.run()
```

Estamos importando algumas funções da biblioteca Flask e também a biblioteca Flask-SQLAlchemy. Observe que utilizaremos o [SQLite](https://www.sqlite.org/index.html) como nosso banco de dados, devido a sua praticidade, também criamos um **USERNAME** e **PASSWORD** que serão utilizados para validar uma rota de login que vamos simular.

A classe **User** representará um usuário do banco de dados que vamos testar.

Nossa aplicação contará com 3 **rotas**:

- **"/"** retornará apenas "Hello World"
- **"/api"** retornará um JSON
- **"/login"** retornará um template HTML com um formulário de login, se usuário e senha digitados forem válidos, o usuário será redirecionado para a rota "/"

Para podermos executar nossa aplicação, precisamos agora criar o diretório templates

```
mkdir templates
```

Dentro dele vamos criar um arquivo HTML chamado de `login.html`

```html
<!DOCTYPE html>
<html>
<head>
	<title>Login Flask Test</title>
</head>
<body>
	<h1>Login</h1>
	<form action="{{ url_for('login') }}" method="POST">
		<input type="text" name="username">
		<input type="password" name="password">
		<input type="submit" value="Login">
	</form>
	{% if error %}
		<p><b>Error:</b> {{ error }}</p>
	{% endif %}
</body>
</html>
```

Finalmente podemos retornar ao diretório principal de nossa aplicação e executá-la

```
python app.py
```

Podemos então visitar as rotas: `http://127.0.0.1:5000/`, `http://127.0.0.1:5000/api` e `http://127.0.0.1:5000/login` e ver que ela está executando com sucesso. Precisamos agora escrever os testes unitários para validá-la, de forma que tenhamos mais confiança em suas funcionalidades.

## Testando a Aplicação Flask

No diretório principal de nossa aplicação criaremos um arquivo chamado `app.test.py`, nele vamos definir nossos testes

```python
from app import app, User, db
import unittest
import json
import os

class FlaskTestCase(unittest.TestCase):
	def setUp(self):
		app.config['TESTING'] = True
		app.config['WTF_CSRF_ENABLED'] = False
		app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///site.db'
		self.app = app.test_client()
		db.create_all()

	def tearDown(self):
		db.session.remove()
		db.drop_all()

	def test_database(self):
		test = os.path.exists('site.db')
		self.assertTrue(test)

if __name__ == '__main__':
    unittest.main()
```

Observe, como padrão, que iniciamos importando os módulos necessários para trabalharmos, em seguida definimos o nosso TestCase que chamamos de **FlaskTestCase**, nele possuímos dois métodos muito importantes:

- **[setUp()](https://docs.python.org/3/library/unittest.html#unittest.TestCase.setUp)**: Método chamado para preparar o test fixture. Ele é chamado imediatamente antes de chamar o método de teste.
- **[tearDown()](https://docs.python.org/3/library/unittest.html#unittest.TestCase.tearDown)**: Método chamado imediatamente após o método de teste ter sido chamado e o resultado registrado. Ele é chamado mesmo se o método de teste gerou uma exceção.

Por fim temos o nosso primeiro método de teste chamado de **test_database**, que irá testar pela existência de nosso arquivo de banco de dados **site.db**, se ele existir nosso teste passará, vamos então executá-lo

```
python app.test.py
```

Imediatamente iremos obter

```
.
-------------------------------------------------------------------
Ran 1 test in 0.343s

OK
```

Nosso primeiro teste foi aprovado, vamos agora definir um teste que seja capaz de inserir um usuário no banco de dados e verificar a presença dele

```python
	def test_user(self):
		u = User(username='admin', email='admin@example.com')
		db.session.add(u)
		db.session.commit()
		user = User.query.filter_by(username='admin').first()
		email = User.query.filter_by(email='admin@example.com').first()
		assert user.username == 'admin'
		assert user.email == 'admin@example.com'
```

Estamos inserindo um usuário de nome **admin** e email **admin@example.com**, em seguida executamos duas [queries](https://www.qhmit.com/database/tutorial/querying_a_database.cfm) no banco de dados e comparamos com o resultado esperado, novamente executamos nossos testes

```
python app.test.py
```

Que nos fornecem o resultado

```
..
-------------------------------------------------------------------
Ran 2 tests in 0.401s

OK
```

Nosso banco de dados parece estar funcionando corretamente, vamos agora testar as rotas "/" e "/api"

```python
	def test_index(self):
		test = app.test_client(self)
		response = test.get('/', content_type='html/text')
		self.assertEqual(response.status_code, 200)
		self.assertEqual(response.data, b'Hello, World!')

	def test_api(self):
		response = self.app.get('/api')
		data = json.loads(response.get_data(as_text=True))
		self.assertEqual(data['username'], 'admin')
		self.assertEqual(data['email'], 'admin@example.com')
```

Em **test_index()** estamos esperando uma resposta de status_code 200, indicando que nossa requisição ocorreu com sucesso, também estamos esperando a string 'Hello World' como dados da resposta.

Já em **test_api()** estamos carregando os dados como **JSON** e utilizando a função assertEqual() para checarmos pelo resultado esperado.

Novamente executamos os testes

```
python app.test.py
```

Que nos traz como output

```
....
-------------------------------------------------------------------
Ran 4 tests in 0.761s

OK
```

Precisamos agora definir um teste para nossa rota de "/login" e também para o [erro 404](https://pt.wikipedia.org/wiki/HTTP_404)

```python
	def test_404(self):
		response = self.app.get('/forum')
		self.assertEqual(response.status, '404 NOT FOUND')

	def login(self, username, password):
	    return self.app.post('/login', data=dict(
	        username=username,
	        password=password
	    ), follow_redirects=True)

	def test_login(self):
		response = self.login(app.config['USERNAME'], app.config['PASSWORD'])
		self.assertIn(b'Hello, World!', response.data)
```

No método **test_404()** estamos fazendo uma requisição para uma rota inexistente em nossa aplicação, o que nos faz esperar um erro 404, sendo assim, estamos esperando a string '404 NOT FOUND' nesse teste.

Observe também que definimos um método auxiliar chamado de **login()**, ele será responsável por fazer uma [requisição POST](https://en.wikipedia.org/wiki/POST_(HTTP)) para a nossa rota "/login" com o **username** e **password** sendo passados como parâmetro.

Por fim o método **test_login()** irá testar a rota "/login", enviando as variáveis de configurações(que definimos no início de nossa aplicação) através do método **POST**. Nesse caso estamos esperando a string 'Hello World' como retorno, uma vez que estamos utilizando o usuário e senha corretos, o que deve nos redirecionar para a rota "/" que retorna 'Hello World'.

Podemos agora finalmente executar nossos testes

```
python app.test.py
```

Onde vamos obter

```
......
-------------------------------------------------------------------
Ran 6 tests in 1.026s

OK
```

Nos indicando que todos os nossos testes estão passando e a aplicação está atendendo as demandas esperadas até então.

Caso tenha alguma dúvida, consulte o repositório da aplicação no [GitHub](https://github.com/the-akira/Flask-Testing)

## Conclusão

Através desse estudo fomos capazes de compreender a importância dos Testes Unitários na [Engenharia de Software](https://en.wikipedia.org/wiki/Software_engineering), eles fornecem uma espécie de documentação viva do sistema. Os desenvolvedores que desejam aprender qual funcionalidade é fornecida por uma unidade e como usá-la, podem examinar os testes de unidade para obter um entendimento básico da interface da unidade(API).

Considere também estudar a biblioteca [pytest](https://docs.pytest.org/en/latest/) que é capaz de oferecer funcionalidades adicionais para nossos testes, ela é é uma ferramenta madura de teste Python, com todos os recursos que ajuda a escrever melhores programas.

Você pode também adicionar TestCases para cenários dessa aplicação que eu não considerei.

Tenha bons estudos!

## Referências

- [Unit testing](https://en.wikipedia.org/wiki/Unit_testing)
- [unittest — Unit testing framework](https://docs.python.org/3/library/unittest.html)
- [Unit Testing Your Code with the unittest](https://www.youtube.com/watch?v=6tNS--WetLI)
- [Flaskr - Intro to Flask, Test-Driven Development](https://github.com/mjhea0/flaskr-tdd)
