---
title: Introdução ao ReactJS
date: "2020-02-27T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/introducao-react/"
category: "Web"
tags:
  - "Web"
  - "Programação"
  - "Fundamentos"
description: "Neste artigo irei introduzir o famoso ReactJS, que é uma poderosa biblioteca Javascript, feita especialmente para a construção de Interfaces de Usuários."
---

![img](https://d2eip9sf3oo6c2.cloudfront.net/tags/images/000/000/026/thumb/react.png)

## Conteúdo

- [Introdução](#introdução)
- [Hello World e Instalação](#instalação)
- [Elementos e JSX](#elementos-e-jsx)
- [Components e Props](#components-e-props)
- [Conclusão](#conclusão)
- [Referências](#referências)

## Introdução

React é uma das mais populares bibliotecas JavaScript para criar interfaces de usuário(**UI**). É mantida pelo Facebook e por uma comunidade de desenvolvedores e empresas individuais, originalmente criada por Jordan Walke, sendo lançada em 2013.

Um dos aspectos mais importantes do **React** é o fato de que você pode criar **components**, como elementos HTML reutilizáveis personalizados, para criar interfaces de usuário de maneira rápida e eficiente. O React também simplifica a maneira como os dados são armazenados e manipulados, usando **state** e **props**.

**Components** são como funções JavaScript. Eles aceitam entradas arbitrárias de dados(chamadas de **props**) e retornam elementos React descrevendo o que deve aparecer na tela.

Os **componentes** são bits de **código independentes** e **reutilizáveis** que ajudam a tornar nossa aplicação modular e organizada, fazendo com que seja mais fácil mantê-la.

### Conceitos Principais

- Elementos e JSX
- Components e Props
- State e Lifecycle
- Listas e Keys
- Eventos

## Instalação

Para podermos trabalhar com a biblioteca ReactJS, primeiramente é necessário termos o gerenciador de pacotes Javascript **[NPM](https://www.npmjs.com/)** em nossa máquina, sendo assim faremos o download de [NodeJS](https://nodejs.org/en/) que nos fornecerá acesso ao NPM.

Uma vez instalados, você pode verificar suas respectivas versões através dos comandos

```
node -v
npm -v
```

### Hello World

O mais simples Hello World em React pode ser obtido através do seguinte código:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Hello World React</title>
    <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
  </head>
  <body>
    <div id="root"></div>

    <script type="text/babel">
      const App = ({ nome }) => {
          return <h1>Hello {nome}</h1>
      }
      ReactDOM.render(<App nome="World" />, document.getElementById('root'))
    </script>
  </body>
</html>
```

Porém essa não é a maneira mais otimizada de trabalharmos em projetos React, tendo em vista que estamos utlizando **CDN's** e as configurações foram feitas manualmente, por este motivo vamos utilizar uma ferramenta especial chamada **Create React App**.

### Create React App

Para facilitar a vida dos desenvolvedores o Facebook criou o **[Create React App](https://github.com/facebook/create-react-app)**, um ambiente pré-configurado com tudo o que você precisa para criar um aplicativo React. Ele criará um servidor de desenvolvimento ativo, usará o Webpack para compilar automaticamente os arquivos React, JSX e ES6, e usará o ESLint para testar e avisar sobre erros no código.

Para instalar o **create-react-app** em sua máquina, você pode digitar o seguinte comando:

```
npm install -g create-react-app 
```

Agora já estamos aptos a criar aplicações React, vamos criar uma chamada de **projeto**, para esta tarefa executaremos o seguinte comando:

```
npx create-react-app projeto 
```

**create-react-app** irá preparar nosso projeto instalando os pacotes necessários para o funcionamento de nossa aplicação. Agora que instalação foi finalizada, vamos navegar até o diretório principal de nosso projeto:

```
cd projeto
```

E finalmente vamos executar nossa aplicação:

```
npm start
```

Podemos navegar até o endereço `http://localhost:3000` para ver nossa aplicação funcionando! 

Se você examinar a estrutura do projeto, verá um diretório `/public` e `/src`, juntamente com os `node_modules` regulares `.gitignore`, `README.md` e `package.json`.

Em `/public`, nosso arquivo importante é `index.html`, muito semelhante ao arquivo `index.html` estático que criamos anteriormente para demonstrar o **Hello World** - apenas uma div principal com o **id root**, é nela que injetaremos todo nosso código React. O diretório `/src` conterá todo o nosso código React e é nele que vamos focar.

Agora que já temos um ambiente de desenvolvimento configurado, podemos então iniciar nossos estudos sobre os conceitos fundamentais de React, porém antes iremos remover os respectivos arquivos de nosso **projeto**:

- ~~src/App.test.js~~
- ~~src/index.css~~
- ~~src/logo.svg~~
- ~~src/serviceWorker.js~~
- ~~src/setupTests.js~~

O diretório `src` contará apenas com os arquivos que vamos editar:

**App.js**

```jsx
import React from 'react';
import './App.css';

function App() {
  return (
    <div className="App">
      <p>Hello World</p>
    </div>
  );
}

export default App;
```

**App.css**

```css
.App {
  text-align: center;
}
```

**index.js**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```

Nossa aplicação já se encontra configurada, agora podemos estudar os conceitos fundamentais de React!

## Elementos e JSX

### Introdução

JSX é uma sintaxe baseada em XML que nos permite escrever **elementos HTML** em JavaScript e inserí-los no DOM sem nenhum método como **createElement()** ou **appendChild()**.

É importante lembrar que você não precisa obrigatoriamente usar o JSX, mas o JSX facilita e agiliza muito a escrita de aplicativos React.

### Criando Elementos

Para compreendermos melhor a ideia de criação de elementos, vamos fazer algumas alterações em nosso arquivo `App.js`:

```javascript
import React from 'react';
import './App.css';

function App() {
  const h1 = <h1>Hello World</h1>
  return (
    <div className="App">
      { h1 }
    </div>
  );
}

export default App;
```

Perceba a seguinte síntaxe:

```javascript
const h1 = <h1>Hello World</h1>
```

Observe que estamos definindo um elemento `h1` com o texto Hello World, essa síntaxe específica não se trata de uma string ou HTML, mas sim **JSX**, que é uma extensão de sintaxe do JavaScript. Recomendamos usá-lo com o React para descrever a aparência da Interface de Usuário. O JSX pode lembrá-lo de uma linguagem de template, mas com ele vem com todo o poder do JavaScript. **JSX** produz "elementos" React.

No exemplo anterior, nós declaramos uma variável `h1` que cria um elemento React. 

Considere agora a seguinte síntaxe:

```javascript
<div className="App">
  { h1 }
  <h2>{ 2 + 2 }</h2>
</div>
``` 

Veja que estamos utilizando **chaves**(**{}**), dentro delas nós podemos colocar qualquer expressão JavaScript válida. Por exemplo, nosso elemento `h1` será apresentado em nossa Interface de Usuário e a expressão `2 + 2` será avaliada para `4`. Podemos também utilizar funções, para entendermos melhor, vamos novamente editar nosso arquivo `App.js`:

```javascript
import React from 'react';
import './App.css';

function App() {
  const livro = {
    titulo: '1984',
    autor: 'George Orwell'
  }
  function formatarLivro(livro){
    return `Título: ${livro.titulo} Autor: ${livro.autor}`
  }
  const elemento = (
    <h1>{ formatarLivro(livro) }</h1>
  )
  return (
    <div className="App">
      { elemento }
    </div>
  );
}

export default App;
```

Definimos um objeto chamado `livro`, uma função `formatarLivro` que nos retorna o título e o autor do livro e por fim criamos um `elemento h1`, dentro dele chamamos a função `formatarLivro()` passando o objeto **livro** como argumento. Por fim, injetamos o elemento criado por nós na Interface de Usuário através da expressão `{ elemento }`.

### JSX como Expressão

JSX também é uma expressão, após a compilação, as expressões JSX se tornam chamadas regulares de função JavaScript e avaliam como objetos JavaScript, novamente vamos alterar nosso arquivo `App.js` para ilustrarmos essa ideia:

```javascript
import React from 'react';
import './App.css';

function App() {
  function usuarioLogado(usuario){
    if(usuario){
      return <h1>{ usuario } logado</h1>
    }
    return <h1>Necessário fazer login com um usuário</h1>
  }
  return (
    <div className="App">
      { usuarioLogado('Alan Turing') }
      { usuarioLogado() }
    </div>
  );
}

export default App;
```

JSX nos permite utilizá-lo dentro das instruções `if` e `for loops`, atribuí-lo a variáveis, aceitá-lo como argumentos e retorná-lo das funções. Perceba que definimos uma função chamada de `usuarioLogado()` que recebe um **usuario** como argumento. O bloco `if` somente será executado se houver um usuario fornecido via argumento, caso não haja um usuário fornecido, o último **return** da função será executado. Posteriormente, dentro de nossa `div` estamos utilizando as expressões para invocar nossa função `usuarioLogado()`.

### Especificando Atributos com JSX

Podemos usar **chaves**(**{}**) para incorporar uma expressão JavaScript em um atributo, vamos alterar novamente o nosso arquivo `App.js` para vermos um exemplo:

```javascript
import React from 'react';
import './App.css';

function App() {
  const imagem = 'https://i.imgur.com/jgNrYhw.png'
  return (
    <div className="App">
      <img src={imagem} />
    </div>
  );
}

export default App;
```

Declaramos uma string chamada de `imagem`, posteriormente em nossa Interface de Usuário utilizamos a tag `img` com a nossa variável `imagem` sendo utilizada em uma **expressão** no atributo **src** que por sua vez nos apresentará a imagem em nossa tela.

### Inspecionando Elementos JSX

De forma a compreendermos os elementos React vamos utilizar o `console.log()` para imprimirmos no **console** de nosso navegador o objeto **Symbol(react.element)**, vamos então editar nosso arquivo `App.js` e criar um elemento parágrafo.

```javascript
import React from 'react';
import './App.css';

function App() {
  const elemento = <p>React</p>
  console.log(elemento)
  return (
    <div className="App">
      { elemento }
    </div>
  );
}

export default App;
```

Ao abrir nosso **console**, vamos nos deparar com o seguinte **objeto**:

```javascript
"$$typeof": Symbol(react.element)
​_owner: Object { tag: 0, key: null, index: 0, … }
​_self: null
​_source: Object { fileName: "/Documentos/Javascript/React/projeto/src/App.js", lineNumber: 5 }
​_store: Object { … }
​  key: null
​props: {…}
​​  children: "React"
​​  <prototype>: Object { … }
​ref: null
​type: "p"
​​<prototype>: Object { … }
```

Perceba que estamos lidando com um **objeto** especial do tipo **Symbol(react.element)** com diversas propriedades.

### JSX Representa Objetos

O **[Babel](https://babeljs.io/)** é um transcompilador JavaScript de código aberto e gratuito usado principalmente para converter o código ECMAScript 2015+(ES6 +) em uma versão compatível com versões anteriores do JavaScript que pode ser executada por mecanismos JavaScript mais antigos. Babel é uma ferramenta popular para usar os recursos mais recentes da linguagem de programação JavaScript.

Babel compila **JSX** para chamadas **React.createElement()**.

Vamos considerar o seguinte exemplo, onde criamos um elementos através de duas maneiras diferentes, editamos então o arquivo `App.js`:

```javascript
import React from 'react';
import './App.css';

function App() {
  const h1 = (
    <h1 className="classe">
      React!
    </h1>
  );
  const h2 = React.createElement(
    'h2',
    {className: 'classe'},
    'JS!'
  );
  return (
    <div className="App">
      { h1 }
      { h2 }
    </div>
  );
}

export default App;
```

Perceba que de ambas as formas é possível criar um elemento, porém é muito mais confortável utilizarmos a primeira opção. 

**Sobre os elementos React:** Você pode pensar neles como descrições do que deseja ver na tela. O React lê esses objetos e os utiliza para construir o DOM e mantê-lo atualizado.

## Components e Props

Os **Components** permitem que possamos dividir nossa Interface do Usuário em partes independentes e reutilizáveis, fazendo com que possamos tratá-las isoladamente. **Componentes** são como funções que recebem dados(**props**) e retornam elementos HTML, eles também pode possuir **state**, que veremos com maiores detalhes em breve.

Outra definição seria que os **Components** são microentidades independentes e auto-sustentáveis que descrevem uma parte da sua Interface do Usuário. A interface do usuário de uma aplicação pode ser dividida em components menores, onde cada component tem seu próprio código, estrutura e API.

![img](https://i.imgur.com/8ewnLfR.png)

Existem dois tipos de Components: **Function components** e **Class components**

### Function components

- **Function components** são funções básicas do JavaScript. Normalmente, são *arrow functions*, mas também podem ser criadas com a palavra-chave `function` regular.
- Chamados no passado de componentes "limitados" ou "*stateless*", pois eles simplesmente aceitavam dados e os exibiam de alguma forma, essa realidade mudou quando introduziram a *feature* **Hooks** no React, que hoje permite Functional Components terem **state**.
- Eles são os principais responsáveis pela renderização da **Interface do Usuário**.
- Não há a presença do método `render()` em Functional Components.
- Functional Components podem aceitar e usar **props**.

Lembrando que ao criar um component React, o nome do component deve começar com uma letra maiúscula. Para entendermos melhor, vejamos alguns exemplos de Functional Components:

#### Quote Component

Definimos um component de nome **Quote** utilizando a tradicional palavra-chave `function`, veja que estamos recebendo **props**, espeficiamente duas: **texto** e **autor** e estamos retornando ambos em nosso **JSX**.

```javascript
function Quote(props) {
  const texto = props.texto
  const autor = props.autor
  return ( 
    <div>
      <p><b>Texto:</b> { texto }</p>
      <small><b>Autor:</b> { autor }</small>
    </div>
  )
}
```

#### Operacoes Component

Definimos um component de nome **Operacoes** utilizando uma *arrow function*, observe que também estamos recebendo especificamente duas funções: **multiplicacao()** e **adicao()** e um objeto de nome **valores** como **props** e estamos utilizando eles em nossas expressões **{}**.

```javascript
const Operacoes = (props) => {
  return (
    <div>
      <p>{ props.multiplicacao(props.valores.x, props.valores.y) }</p>
      <p>{ props.adicao(props.valores.x, props.valores.y) }</p>
    </div>
  )
}
```

#### Quadrado Component

Definimos um component de nome **Quadrado** utilizando uma *arrow function*, este que recebe um array de números como **props** e os eleva ao quadrado.

```javascript
const Quadrado = (props) => {
  return props.valores.map(valor => <p>{ valor ** 2 }</p>)
}
```

Logo vamos utilizá-los em nossa aplicação, porém antes vamos fazer um estudo sobre os **Class components**.

### Class components

- Os **Class components** usam a classe ES6 e estendem a classe **Component** no React.
- Conhecidos também como componentes "inteligentes" ou "*stateful*", pois tendem a implementar lógica e **state**.
- Os métodos de *Lifecyle* de React podem ser implementados dentro dos Class components(por exemplo: `componentDidMount`).
- Podemos passar **props** para os Class components e conseguimos acessá-los com `this.props`

Vejamos então um exemplo de Class components:

#### Linguagens Component

Através do uso da palavra-chave `class`, característica especial da especificação ES6, declaramos uma classe chamada **Linguagens** e estamos estendendo `Component`, que nos fornece propriedades especiais. O único método que você deve definir obrigatoriamente em uma subclasse `Component` é chamado `render()`. Nesse component estamos basicamente definindo um array de linguages de programação e apresentando-as em nossa Interface de Usuário através do método **map()** que nos permite mapear cada elemento do array em um `<li>`.

```javascript
import React, { Component } from 'react'

class Linguagens extends Component {
  render () {
    const linguagens = ['Javascript', 'Python', 'C++']
    return (
      <nav>
        <h1>Linguagens de Programação</h1>
        <ul>
           { linguagens.map(linguagem => <li>{ linguagem }</li>) }
        </ul>
      </nav>
    )
  }
}
```

### Renderizando nossos Components

Desde então estamos trabalhando com o arquivo `App.js`, que por sua vez possui um component `App`, ele é o component root, o principal de nossa aplicação, e é através dele que vamos renderizar nossos components. Porém, antes de tudo, vamos criar um arquivo `.js` para cada **component** que possuímos, dentro da pasta `src`, vamos criar então os arquivos:

##### Linguagens.js

```javascript
import React, { Component } from 'react'

export default class Linguagens extends Component {
  render () {
    return (
      <nav>
        <h1>Linguagens de Programação</h1>
        <ul>
           { this.props.linguagens.map(linguagem => <li>{ linguagem }</li>) }
        </ul>
      </nav>
    )
  }
}
```

##### Quadrado.js

```javascript
import React from 'react'

const Quadrado = (props) => {
  return props.valores.map(valor => <p>{ valor ** 2 }</p>)
}

export default Quadrado
```

##### Quote.js

```javascript
import React from 'react'

export function Quote(props) {
  const texto = props.texto
  const autor = props.autor
  return ( 
    <div>
      <p><b>Texto:</b> { texto }</p>
      <small><b>Autor:</b> { autor }</small>
    </div>
  )
}
```

##### Operacoes.js

```javascript
import React from 'react'

export const Operacoes = (props) => {
  return (
    <div>
      <p>{ props.multiplicacao(props.valores.x, props.valores.y) }</p>
      <p>{ props.adicao(props.valores.x, props.valores.y) }</p>
    </div>
  )
}
```

Agora vamos importar nossos components no arquivo `App.js` para que possamos renderizá-los, vamos então editá-lo da seguinte maneira:

```javascript
import React from 'react'
import Linguagens from './Linguagens'
import Quadrado from './Quadrado'
import { Quote } from './Quote'
import { Operacoes } from './Operacoes'
import './App.css'

const App = () => {
  function multiplicacao(x, y) {
    return x * y
  }
  const adicao = (x, y) => {
    return x + y
  }
  return (
    <div className="App">
        <Linguagens linguagens={['Javascript', 'Python', 'C++']} />
        <hr />
        <Quote 
        texto="Tell me and I forget. Teach me and I remember. Involve me and I learn"
        autor="Benjamin Franklin" 
        />
        <Quote 
        texto="Getting information off the Internet is like taking a drink from a fire hydrant"
        autor="Mitchell Kapor" 
        />
        <hr />
        <Operacoes 
        multiplicacao={multiplicacao}
        adicao={adicao} 
        valores={{x: 3, y: 99}}
        />
        <hr />
        <Quadrado valores={[5,10,15,20]} />
        <hr />
    </div>
  );
};

export default App;
```

No topo de nosso arquivo, estamos importando os components, veja que importamos de duas maneiras diferentes, **Linguagens** e **Quadrado** não envolvemos em **{}**, isso porque exportamos eles no modo **default**, o que nos permite importá-los com qualquer nome desejado, porém optei por não alterar seus nomes, embora pudesse. Já os components **Quote** e **Operacoes** que não foram exportados no modo **default**, foi necessário o uso de **{}** com o nome exato do component.

Feitas as importações, vamos para nosso component root `App`, nele estamos definindo duas funções `multiplicacao()` e `adicao()` que serão passamos como **props** para o component `Operacoes`.

Finalmente em nosso **JSX**, estamos retornando nossos components para serem renderizados e apresentados na Interface do Usuário:

##### Linguagens Component

No component Linguagens estamos passando o array linguagens como **props**

```javascript
<Linguagens linguagens={['Javascript', 'Python', 'C++']} />
```

##### Quote Component

Estamos renderizando dois components Quote, cada um recebendo seus **props** exclusivos

```javascript
<Quote 
texto="Tell me and I forget. Teach me and I remember. Involve me and I learn"
autor="Benjamin Franklin" 
/>
<Quote 
texto="Getting information off the Internet is like taking a drink from a fire hydrant"
autor="Mitchell Kapor" 
/>
``` 

##### Operacoes Component

O component Operacoes está recebendo nossas duas funções como **props** e também um objeto de nome **valores**, que estamos utilizando como argumento em nossas funções.

```javascript
<Operacoes 
multiplicacao={multiplicacao}
adicao={adicao} 
valores={{x: 3, y: 99}}
/>
```

##### Quadrado Component

Por fim, nosso component Quadrado recebe um array de números que serão elevados ao quadrado e apresentados na Interface de Usuário.

```javascript
<Quadrado valores={[5,10,15,20]} />
```

## Conclusão

Através dessa breve introdução ao React fomos capazes de aprender os fundamentos da biblioteca através do estudos de **elementos**, **JSX** e **components e props** no qual constituem a estrutura principal para construirmos Interfaces de Usuários. Conceitos como **state** e **eventos** não foram abordados para mantermos a simplicidade do tutorial, porém considere a elevada importância deles para seus próximos estudos.

## Referências

- [React Docs](https://reactjs.org/docs/getting-started.html)
- [React Source](https://github.com/facebook/react)
- [React Wiki](https://en.wikipedia.org/wiki/React_(web_framework))
- [Reactjs 101](https://github.com/kdchang/reactjs101)
- [Awesome-React](https://github.com/enaqx/awesome-react)
- [React W3Schools](https://www.w3schools.com/REACT/default.asp)
- [React Cheatsheet](https://devhints.io/react)
- [Robin Wieruch Blog](https://www.robinwieruch.de/blog)
- [Overreacted](https://overreacted.io/)
- [FullStack Open](https://fullstackopen.com/en/)
- [Getting Started with React](https://www.taniarascia.com/getting-started-with-react/)
- [React Training Blog](https://reacttraining.com/blog/)
- [Learn React By Itself](https://reactarmory.com/guides/learn-react-by-itself)
- [The Beginner's Guide to React](https://egghead.io/courses/the-beginner-s-guide-to-react)
- [React Fundamentals](https://www.freecodecamp.org/news/all-the-fundamental-react-js-concepts-jammed-into-this-single-medium-article-c83f9b53eac2/)
- [React Tutorialspoint](https://www.tutorialspoint.com/reactjs/index.htm)
- [How to Learn React](https://skillcrush.com/blog/how-to-learn-react-js/)
- [Fetching API Data with React](https://blog.hellojs.org/fetching-api-data-with-react-js-460fe8bbf8f2)
- [ReactJS Tutorial](https://www.javatpoint.com/reactjs-tutorial)
- [A way to learn React](https://itnext.io/a-way-to-learn-react-b95056eafebb)
- [React for Beginners](https://www.kirupa.com/react/)
- [Getting Started with React](https://sabe.io/tutorials/getting-started-with-react)
- [A Complete Beginner's Guide to React](https://dev.to/aspittel/a-complete-beginners-guide-to-react-2cl6)
- [The React Cheatsheet for 2020](https://dev.to/codeartistryio/the-react-cheatsheet-for-2020-real-world-examples-4hgg)
- [10 Component Commandments](https://dev.to/selbekk/the-10-component-commandments-2a7f)
- [Manual de React](https://desarrolloweb.com/manuales/manual-de-react.html)
