---
title: Os Três Pilares de Javascript
date: "2020-02-12T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/pilares-javascript/"
category: "Javascript"
tags:
  - "Javascript"
  - "Programação"
  - "Fundamentos"
description: "Neste artigo estudaremos os três pilares ao qual a linguagem Javascript é organizada: Escopos/Closures, Prototypes/Objetos e Tipos/Coerção."
---

![img](https://i.imgur.com/r2RAWrU.png)

# Conteúdo

1. [Introdução](#introdução)
2. [Pilar I: Escopo e Closure](#pilar-i-escopo-e-closure)
    - [Escopo Global](#escopo-global)
    - [Escopo Local](#escopo-local)
    - [Instruções de Bloco](#instruções-de-bloco)
    - [O Conceito de Escopo Léxico](#o-conceito-de-escopo-léxico)
    - [Closure](#closure)
3. [Pilar II: Prototypes e Objetos](#pilar-ii-prototypes-e-objetos)
    - [Criando Objetos](#criando-objetos)
    - [Métodos](#métodos)
    - [Modificando Propriedades dos Objetos](#modificando-propriedades-dos-objetos)
    - [Prototypes em Javascript](#prototypes-em-javascript)
    - [Herança e a Cadeia de Prototypes](#herança-e-a-cadeia-de-prototypes)
4. [Pilar III: Tipos e Coerção](#pilar-iii-tipos-e-coerção)
    - [Tipos de Dados em Javascript](#tipos-de-dados-em-javascript)
    - [Valores como Tipos](#valores-como-tipos)
    - [Coerção de Tipos](#coerção-de-tipos)
5. [Referências](#referências)

# Introdução

Inspirado pelo livro [You Don't Know JS Yet](https://github.com/getify/You-Dont-Know-JS), no qual o autor Kyle Simpson cita **Escopos/Closures**, **Prototypes/Objetos** e **Tipos/Coerção** como os três pilares fundamentais em que a linguagem Javascript é organizada, gostaria de estabelecer um breve estudo sobre esses importantes conceitos de forma a maximizar nosso conhecimento. Para informações mais detalhadas você pode consultar o livro ou as referências utilizadas.

## Pilar I: Escopo e Closure

O escopo no JavaScript refere-se ao contexto atual do código, que determina a acessibilidade das variáveis ao JavaScript.

JavaScript possui dois níveis de escopo: **global** e **local**. Qualquer variável declarada fora de uma função pertence ao escopo **global** e, portanto, é acessível a partir de qualquer lugar no seu código. Cada função tem seu próprio escopo e qualquer variável declarada nessa função é acessível apenas a partir dessa função e de quaisquer funções aninhadas. Como o escopo local em JavaScript é criado por funções, também é chamado de escopo de função. Quando colocamos uma função dentro de outra função, criamos um escopo aninhado.

Sumarizando então temos:

- JavaScript tem escopo de função: cada função cria um novo escopo.
- O escopo determina a acessibilidade (visibilidade) das variáveis.
- Variáveis definidas dentro de uma função não são acessíveis (visíveis) de fora da função.

### Escopo Global

Uma variável que estiver declarada fora de todas as **funções** ou **chaves** (`{}`) pertencerá ao **escopo global**.

O JavaScript possui três **palavras-chave** diferentes para declarar uma variável, o que adiciona uma camada extra de complexidade à linguagem. As diferenças entre os três são baseadas no escopo, *hoisting* e reatribuição.

| Palavra-chave                                                                                 | Escopo           | *Hoisting* | Pode ser Reatribuído | Pode ser redeclarado |
| --------------------------------------------------------------------------------------------- | --------------   | --------   | -----------------    | -----------------    |
| [`var`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)     | Escopo de função | Sim        | Sim                  | Sim                  |
| [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)     | Escopo de Bloco  | Não        | Sim                  | Não                  |
| [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) | Escopo de Bloco  | Não        | Não                  | Não                  |


Uma vez que declaramos uma variável **global**, podemos utilizar ela em qualquer lugar do nosso código, até mesmo em funções, vejamos um exemplo:

```javascript
let autor = 'H.P Lovecraft'

function imprimir(){
	console.log(autor)
}

console.log(autor) // H.P Lovecraft
imprimir() // H.P Lovecraft
```

É aconselhável tomarmos cuidado ao declararmos variáveis no escopo **global**, isso porque existe a possibilidade de ocorrerem **colisões de nomes**, quando duas ou mais variáveis recebem o mesmo nome.

Se declararmos nossas variáveis utilizando as palavras-chave `const` ou `let` receberemos um erro(*SyntaxError*) caso ocorra uma colisão:

```javascript
let nome = 'felippe'
const nome = 'gabriel'
// SyntaxError: Identifier 'nome' has already been declared
```

O mesmo não ocorre com `var`, nesse caso específico a segunda variável sobrescreve a primeira:

```javascript
var nome = 'gabriel'
var nome = 'felippe'

console.log(nome) // felippe
```

### Escopo Local

As variáveis com escopo local são apenas visíveis e acessíveis em seus escopos locais (onde estão definidas). Você pode pensar no escopo local como um novo escopo criado dentro do escopo global.

Um exemplo simples é quando trabalhamos com funções. Cada função escrita em JavaScript cria um novo escopo **local**. Essas variáveis com escopo local podem ser acessadas apenas dentro da função que elas estão definidas. Vejamos um exemplo, em que vamos definir uma função com uma variável dentro dela e tentaremos acessar essa variável de fora da função.

```javascript
function escopoLocal() {
	var numero = 13
	console.log(numero)
}
```

Vamos agora invocar essa função:

```javacript
escopoLocal() // 13
```

Vejamos o que acontece caso tentarmos imprimir a variável `numero` fora do escopo da função:

```javascript
console.log(numero) // ReferenceError: numero is not defined
```

Como imaginávamos, nos foi retornado um erro(*ReferenceError*), uma vez que a variável `numero` pertence exclusivamente ao escopo local da função `escopoLocal()`.

### Instruções de Bloco

Instruções de bloco como as condições `if` e `switch` ou loops `for` e `while`, diferente das funções, eles não criam um novo escopo. Variáveis definidas dentro da instrução de bloco irão permanecer no escopo em que já estavam. Vejamos exemplos:

```javascript
if (true) {
    var estado = 'Rio Grande do Sul'
}

console.log(estado) // Rio Grande do Sul
```

Observe que o bloco da condição `if` não cria um novo escopo, sendo assim, a variável `estado` pode ser acessada de fora do bloco `if`.

As palavras-chaves `let`e `const` que foram introduzidas na especificação **ECMAScript 6** são capazes de suportar a declaração de um **escopo local** dentro de **instruções de bloco**. Vamos então declarar três variáveis dentro de um bloco `if`:

```javascript
if(true) {
	var banda = 'Pink Floyd'
	let livro = 'Brave New World'
	const jogo = 'Super Castlevania IV'
}
```

Vamos agora imprimir as variáveis: 

**var**

```javascript
console.log(banda) // Pink Floyd
```

**let**

```javascript
console.log(livro) // ReferenceError: livro is not defined
```

**const**

```javascript
console.log(jogo) // ReferenceError: jogo is not defined
```

Perceba que não foi possível acessarmos as variáveis `livro` e `jogo`, uma vez que elas estão restritas ao **escopo do bloco**.

### O Conceito de Escopo Léxico

O escopo de JavaScript é determinado em tempo de compilação. O termo para essa forma de escopo é "escopo léxico". Esta palavra "léxico" está relacionada ao estágio "*lexing*" de compilação, a idéia principal do "escopo léxico" é que ele é totalmente controlado pelo posicionamento de funções, blocos e declarações de variáveis, um em relação ao outro.

Se você colocarmos uma declaração de variável dentro de uma função, o compilador irá lidar com essa declaração enquanto faz o *[parsing](https://www.techopedia.com/definition/3853/parse)* da função e associa essa declaração ao escopo da função. 

Se uma variável é declarada no **escopo do bloco** (`let` / `const`), então ela estará associada com o bloco `{}` mais próximo, ao invés de sua função envolvente(como com `var`).

Vamos agora considerar o seguinte exemplo:

```javascript
function inicializar() {
	var bibliotecas = ['react','angular','vue'] 
	function imprimir() { 
		for(biblioteca of bibliotecas) { 
			console.log(biblioteca)
		}
	}
	imprimir()
}

inicializar()
// react
// angular
// vue
```

- `bibliotecas` é uma variável local criada pela função `inicializar()`
- `imprimir()` é uma função interna, também conhecida como *[closure](https://en.wikipedia.org/wiki/Closure_(computer_programming))*
- No **for** loop podemos utilizar a variável declarada na função paterna

O que ocorreu?

A função `inicializar()` cria uma variável local chamada `bibliotecas` e uma função chamada `imprimir()`. A função `imprimir()` é uma função interna que está definida dentro da função `inicializar()` e está disponível apenas dentro do corpo da função `inicializar()`. Perceba que a função `imprimir()` não possível nenhuma variável própria, porém, uma vez que funções internas possuem acesso a variáveis de funções externas, a função `imprimir()` pode acessar a variável `bibliotecas` declarada na função paterna.

Este é um exemplo de escopo léxico, que descreve como um *[parser](https://www.techopedia.com/definition/3853/parse)* resolve nomes de variáveis quando funções são aninhadas. A palavra "léxico" refere-se ao fato de o escopo léxico usar o local em que uma variável é declarada no código-fonte para determinar onde essa variável está disponível. Funções aninhadas têm acesso a variáveis declaradas em seu escopo externo, por isso foi possível acessarmos os dados do [array](https://www.w3schools.com/js/js_arrays.asp) `bibliotecas`.

### Closure

*[Closure](https://en.wikipedia.org/wiki/Closure_(computer_programming))* é a combinação de uma função agrupada (envolvida) com referências ao seu ambiente léxico. Em outras palavras, closure nos fornece acesso ao escopo de uma função externa a partir de uma função interna. Em JavaScript, closures são criados toda vez que uma função é criada, no momento da criação da função.

*Closure* pode não apenas acessar as variáveis definidas em sua função externa, mas também os argumentos da função externa.

Vamos agora considerar o seguinte exemplo:

```javascript
function inicializar(nome) {
	let bibliotecas = ['react','angular','vue']
	function imprimir() {
		for(biblioteca of bibliotecas) {
			console.log(biblioteca)
		}
		console.log(nome)
	}
	return imprimir
}

var init = inicializar('svelte')
init()
// react
// angular
// vue
// svelte
```

Perceba que esse código é bastante similar ao anterior, exceto que estamos recebendo **nome** como argumento e agora nossa funçao externa `inicializar()` está retornando a função interna `imprimir()` antes dela ser executada.

Uma vez que `inicializar('svelte')` teve sua execução finalizada, você pode esperar que a variável `nome` e `bibliotecas` não sejam mais acessíveis. No entanto, como o código ainda funciona conforme o esperado, não é o que ocorre com JavaScript.

O motivo de conseguirmos acessar as variáveis é porque em Javascript as funções formam *Closures*. Podemos dizer então que uma *Closure* é uma combinação de uma função e o ambiente léxico no qual essa função foi declarada. O ambiente consiste de qualquer variável local que estava no escopo no momento em que a *Closure* foi criada.

As situações em que você pode querer aplicar *Closures* são particularmente comuns na web. Grande parte do código que escrevemos no JavaScript front-end é baseado em eventos - definimos algum comportamento e o anexamos a um evento que é disparado pelo usuário (como um clique ou um pressionamento de tecla). Nosso código geralmente é anexado como um *Callback*: uma única função que é executada em resposta ao evento.

Vejamos então mais um exemplo, considere o seguinte arquivo `index.html`:

```html
<!DOCTYPE html>
<html>
<body>

<h2>Exemplo de JavaScript Closures</h2>

<p>Contador com uma variável local</p>

<button type="button" onclick="contar()">Clique em mim!</button>

<p id="demo">0</p>

<script>
function adicao() {
  var counter = 0;
  function contador(){
  	counter += 1
    return counter
  }
  return contador
}

var conta = adicao()
function contar(){
  document.getElementById("demo").innerHTML = conta();
}
</script>

</body>
</html>
```

Nesse simples documento **HTML** temos uma botão que ativa a função `contar()`, que por sua vez modifica o elemento parágrafo (`<p id="demo">0</p>`) através de uma *Closure*. Dentro da função externa `adicao()` definimos a variável `counter = 0` que é acessada e modificada pela função interna `contador()`, que incrementa seu valor em **1**, posteriormente retornamos a função `contador` sem executá-la. Por fim a variável `conta` receberá o valor de retorno da função `adicao()`.

Em outras palavras, cada vez o que usuário clicar no botão, `conta()` receberá o valor de retorno da função `adicao()`, guardando sempre o estado da variável `counter`, fazendo possível com que consigamos incrementá-la sem perder o seu "estado" e assim modificando o valor do parágrafo em cada clique que ocorre.

Finalizamos então nosso breve estudo sobre **Escopos e Closures** reconhecendo a importância desses aspectos dentro da linguagem Javascript, para mais detalhes você pode visitar as referências!

## Pilar II: Prototypes e Objetos

JavaScript é uma linguagem projetada em um paradigma simples baseado em objetos. 

Um **objeto** é uma coleção de propriedades e uma propriedade é uma **associação** entre um **nome (ou chave)** e um **valor**. O valor de uma propriedade pode ser uma função; nesse caso, a propriedade é conhecida como método. Além dos objetos predefinidos no navegador, podemos definir nossos próprios objetos. 

### Criando Objetos

Existem diversas maneiras de criarmos um objeto em JavaScript. A maneira mais simples e popular é usar a **sintaxe literal do objeto**:

```javascript
const pessoa = {
    nome: 'Rafael',
    sobrenome: 'Silva',
    idade: 67 
}

console.log(pessoa)
// { nome: 'Rafael', sobrenome: 'Silva', idade: 67 }
```
Os valores das propriedades são acessíveis usando a notação de ponto (**.**) e a notação de colchete (**[]**):

```javascript
console.log(pessoa.nome) // Rafael
```

```javascript
console.log(pessoa['sobrenome']) // Silva
```

Objetos também podem ser criados usando o operador `new`. A palavra-chave `new` pode ser usada com a função construtora `Object()` incorporada no JavaScript ou com a função construtora definida pelo usuário:

**Função Construtora**:

```javascript
const livro = new Object()
livro.titulo = 'Lord of the Rings'
livro.autor = 'J.R.R Tolkien'

console.log(livro)
// { titulo: 'Lord of the Rings', autor: 'J.R.R Tolkien' }
```

**Função Construtora definida por nós**:

```javascript
function Livro(titulo, autor) {
    this.titulo = titulo
    this.autor = autor
}

const livro = new Livro(
    'The Art of Computer Programming', 
    'Donald Knuth'
)

console.log(livro)
// Livro {
// titulo: 'The Art of Computer Programming',
// autor: 'Donald Knuth' }
```

Como você pode percerber, utilizamos a palavra-chave `this`, que refere-se ao objeto ao qual pertence.

A palavra-chave `this` tem valores diferentes, dependendo do contexto em que é usado:

- Em um método, `this` se refere ao objeto proprietário.
- Sozinho, `this` se refere ao objeto global.
- Em uma função, `this` se refere ao objeto global.
- Em uma função, no [*strict mode*](https://www.w3schools.com/js/js_strict.asp), `this` é indefinido.
- Em um evento, `this` se refere ao elemento que recebeu o evento.
- Métodos como `call()` e `apply()` podem se referir a qualquer objeto. 

### Métodos

Como vimos, uma propriedade é a associação entre um **nome(chave)** e um **valor** dentro de um objeto, e nela pode estar contigo qualquer tipo de dados. Uma propriedade geralmente se refere à característica de um objeto.

Um **método** é uma função que é o valor de uma propriedade d objeto e, portanto, uma tarefa que um objeto pode executar, vejamos um exemplo:

```javascript
const pessoa = {
    nome: 'Rafael',
    sobrenome: 'da Silva',
    idade: 67,
    nomeCompleto: function() {
    	return `${this.nome} ${this.sobrenome}`
    } 
}

console.log(pessoa.nomeCompleto())
// Rafael da Silva
```

No exemplo acima, vemos que o valor da string do método do objeto `nomeCompleto()` é retornado.

### Modificando Propriedades dos Objetos

#### Adicionando Propriedades

Para adicionar uma nova propriedade a um **objeto**, você atribui um novo valor a uma propriedade com o operador de atribuição (**=**). Por exemplo, nós podemos modificar o **nome**, **sobrenome** e **idade** do objeto **pessoa**:

```javascript
pessoa.nome = 'Bruce'
pessoa['sobrenome'] = 'Lee'

console.log(pessoa.nomeCompleto())
// Bruce Lee
```

Dessa maneira, podemos até mesmo adicionar um novo método para nosso objeto. Vamos então criar o método `info()`:

```javascript
pessoa.info = function(){
	return `${this.nome} possui ${this.idade} anos`
}

console.log(pessoa.info())
// Bruce possui 67 anos
```

Como podemos ver através da operação de atribuição, podemos modificar as propriedades e métodos de um objeto JavaScript.

#### Removendo Propriedades

Você pode remover uma propriedade - não herdada - usando o operador `delete`. No código a seguir vamos remover a propriedade **idade** de nossa **pessoa**.

```javascript
console.log(delete pessoa.idade)
// true 
```

A operação `delete` é avaliada como `true` se a propriedade foi removida com sucesso, se utilizarmos `delete` em uma propriedade que não existe no objeto, também nos será retornado `true`.

Vamos ver se realmente foi possível deletar a propriedade idade:

```javascript
console.log(pessoa.idade)
// undefined
```

Observe que nos é retornado `undefined`, demonstrando assim que a propriedade `idade` e seu valor associado não estão mais disponíveis, mostrando assim que a exclusão ocorreu com sucesso.

#### Enumerando as Propriedades

O JavaScript possui um tipo interno de loop `for` que se destina especificamente à iteração sob as propriedades de um **objeto**. Ele é conhecido como loop `for...in`.

Vamos considerar a versão simplificada do objeto `pessoa`:

```javascript
const pessoa = {
    nome: 'Rafael',
    sobrenome: 'da Silva',
    idade: 67
}
```

Vamos agora utilizar o loop `for...in` para iterar sob todas as propriedades do objeto e posteriormente imprimir elas no console. Através da notação de colchetes (**[]**) nós podemos obter o valor da propriedade como uma variável, nesse caso vamos chamá-la de `chave`:

```javascript
for(chave in pessoa){
	console.log(pessoa[chave])
}
// Rafael
// da Silva
// 67
```

Também podemos obter o próprio nome da **propriedade** usando apenas a primeira variável no loop `for...in`. Vamos também utilizar um método de string para converter os valores das chaves em maiúsculas.

```javascript
for(chave in pessoa){
	console.log(`${chave.toUpperCase()}: ${pessoa[chave]}`)
}
// NOME: Rafael
// SOBRENOME: da Silva
// IDADE: 67
```

Existem outros dois métodos muito importantes, são eles, respectivamente:

- **[Object.keys(x)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)**: Esse método retorna um **array** com todos os **nomes("chaves")** de propriedades enumeráveis **próprias**(não da cadeia de protótipos) de um objeto **x**.
- **[Object.getOwnPropertyNames(x)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames)**: Este método retorna um **array** contendo todos os nomes de propriedades próprias(enumeráveis ou não) de um objeto **x**.

Por exemplo:

```javascript
console.log(Object.keys(pessoa)) 
console.log(Object.getOwnPropertyNames(pessoa))
// [ 'nome', 'sobrenome', 'idade' ]
// [ 'nome', 'sobrenome', 'idade' ]
```

### Prototypes em Javascript

Todos os objetos JavaScript herdam propriedades e métodos de um **prototype**. 

A propriedade **prototype** nos permite adicionar novas propriedades e métodos aos construtores de objetos existentes. As novas propriedades são compartilhadas entre todas as instâncias do tipo especificado, em vez de apenas uma instância do objeto.

Sendo assim, todo objeto em JavaScript possui uma propriedade interna chamada [[Prototype]]. Podemos demonstrar isso criando um novo objeto vazio:

```javascript
let objeto = {}
```

Observe que optamos por criar um objeto utilizando a **sintaxe literal do objeto**, poderíamos também utilizar o construtor: `let objeto = new Object()`.

**Importante**: Os colchetes duplos que incluem [[Prototype]] significam que é uma propriedade interna e não pode ser acessada diretamente no código.

Por exemplo, se tentarmos acessar diretamente:

```javascript
objeto.Prototype
// undefined
```

Nos será retornado `undefined`. Para que possamos encontrar o [[Prototype]] de nosso **objeto**, vamos utilizar o método `getPrototypeOf()`:

```javascript
Object.getPrototypeOf(objeto)
```

Nos será retornado diversas propriedades e métodos internos construídos.

```javascript
Object { 
    __defineGetter__: function __defineGetter__()
    __defineSetter__: function __defineSetter__()
    __lookupGetter__: function __lookupGetter__()
    __lookupSetter__: function __lookupSetter__()
    __proto__: 
    constructor: function Object()
    hasOwnProperty: function hasOwnProperty()
    isPrototypeOf: function isPrototypeOf()
    propertyIsEnumerable: function propertyIsEnumerable()
    toLocaleString: function toLocaleString()
    toSource: function toSource()
    toString: function toString()
    valueOf: function valueOf()
    <get __proto__()>: function __proto__()
    <set __proto__()>: __proto__()
}
```

É importante que todo objeto em JavaScript tenha um [[Prototype]], pois cria uma maneira de vincular dois ou mais objetos.

### Herança e a cadeia de Prototypes

Quando se trata de herança, o JavaScript possui apenas um constructo: **objetos**. Cada objeto possui uma propriedade privada que mantém um link para outro objeto chamado **prototype**. Esse objeto de **prototype** possui um prototype próprio e assim por diante até que um objeto seja alcançado com `null` como seu prototype. Por definição, `null` não possui protótipo e atua como o link final nessa cadeia de protótipos.

Quase todos os objetos em JavaScript são instâncias de **Object**, que reside no topo de uma **cadeia de prototypes**.

Quando tentamos acessar uma propriedade ou método de um objeto, o JavaScript primeiro pesquisa no próprio objeto e, se não for encontrado, pesquisará no **[[Prototype]]** do objeto. Se, após consultar o objeto e seu **[[Prototype]]**, ainda não houver correspondência, o JavaScript verificará o prototype do objeto vinculado e continuará a pesquisa até o final da cadeia de prototypes.

No final da cadeia de prototypes está o `Object.prototype`. Todos os objetos herdam as propriedades e métodos de `Object`. Qualquer tentativa de procurar além do final da cadeia resulta em `null`.

Em nosso exemplo, `objeto` é um objeto vazio herdeiro de `Object`. Por isso `objeto` pode utilizar qualquer propriedade ou método de `Object`, como por exemplo:

```javascript
objeto.toLocaleString()
```

Que nos irá retornar `"[object Object]"`. Essa cadeia de prototypes tem apenas um link. objeto -> Objeto. Sabemos disso, porque se tentarmos encadear duas propriedades [[Prototype]], elas serão `null`:

```javascript
objeto.__proto__.__proto__
// null
```

Podemos perceber que o mecanismo de herança de Javascript é diferente de linguagens tradicionais como Java e C++, uma vez ele que é baseado em **prototypes**. Ao usar a herança, é recomendável que você não tenha muitos níveis de herança e mantenha um controle cuidadoso de onde define seus métodos e propriedades.

## Pilar III: Tipos e Coerção

### Tipos de Dados em Javascript

JavaScript define sete tipos internos:

- null
- undefined
- boolean
- number
- string
- object
- symbol

**Importante:** Todos esses tipos, exceto o objeto, são chamados de "primitivos".

O operador `typeof` inspeciona o tipo do valor fornecido e sempre retorna um dos sete valores da sequência. Por exemplo:

```javascript
console.log(typeof undefined) // undefined
console.log(typeof true) // boolean
console.log(typeof 100) // number
console.log(typeof "javascript") // string
console.log(typeof {}) // object
console.log(typeof Symbol()) // symbol
console.log(typeof null) // object
```

Perceba que `null` nos retorna um **object**, isso é um bug que persistiu por quase duas décadas e provavelmente nunca será corrigido porque há muito conteúdo da Web existente que se baseia no seu comportamento.

### Valores como Tipos

No JavaScript, as variáveis não têm tipos - os valores possuem tipos. As variáveis podem conter qualquer valor, a qualquer momento.

Outra forma de pensarmos sobre os tipos em Javascript é que a linguagem não tem "imposição de tipo", pois o mecanismo não insiste que uma variável sempre mantenha valores do mesmo tipo inicial com o qual começa. Uma variável pode, em uma instrução de atribuição, conter uma string e, na próxima, um número, e assim por diante.

O valor 100 tem um tipo intrínseco de número e seu tipo não pode ser alterado. Outro valor, como "88" com o tipo de string, pode ser criado a partir do valor numérico 88 por meio de um processo chamado **coerção**.

Portando, quando utilizamos `typeof` em uma variável, nós não estamos perguntando "qual o tipo da variável?", mas sim estamos perguntando "qual o tipo do valor contido na variável?".

```javascript
var n = 50
var x = false
console.log(typeof n) // number
console.log(typeof x) // boolean
console.log(typeof typeof x) // string
```

Uma vez que `typeof` retorna uma string, se utilizarmos por exemplo ele duas vezes em um número, receberemos uma string como retorno.

### Coerção de Tipos

Coerção de tipo é o processo de conversão de valor de um tipo para outro (como string para número, objeto para booleano e assim por diante). Qualquer tipo, seja primitivo ou um objeto, é um válido para coerção de tipo. A coerção de tipos pode ser explícita e implícita.

#### Coerção Explícita

Quando um desenvolvedor expressa a intenção de converter entre tipos escrevendo o código apropriado, como `Number(valor)`, ele é chamado de **coerção explícita de tipo**(ou conversão de tipo), por exemplo:

```javascript
console.log(Number('8')) // 8
console.log(String(8)) // 8 
console.log(Boolean(1)) // true
```

#### Coerção Implícita

Uma vez que JavaScript é uma [linguagem de tipagem fraca](https://en.wikipedia.org/wiki/Strong_and_weak_typing), os valores também podem ser convertidos entre diferentes tipos automaticamente, e isso é chamado de **coerção implícita** de tipo, por exemplo:

```javascript
const n1 = '10'
const n2 = 7
let soma = n1 + n2
console.log(soma) // 107
console.log(typeof soma) // string
```

No exemplo acima, ocorreu uma coerção do número 7 de um **number** para uma **string** e concatenou os dois valores juntos, resultando em uma string com o valor **107**. O JavaScript teve de fazer uma escolha entre uma string ou um number e decidiu usar uma **string**.

Para que houvesse a coerção para um número, seria necessário utilizar uma coerção explícita em **n1**:

```javascript
const n1 = '10'
const n2 = 7
let soma = Number(n1) + n2
console.log(soma) // 17
console.log(typeof soma) // number
```

A **coerção implícita** geralmente acontece quando você aplica operadores a valores de tipos diferentes, como
`1 == '1'`, `7 / '2'`, `1 + new Date()` ou pode ser acionado pelo contexto envolvente, como por exemplo `if(valor){…}`, em que o valor é coagido em um **boolean**. Por exemplo:

```javascript
console.log(1 == '1') // true
console.log(1 + new Date()) // string
console.log(7 / '2') // number

if(1){
	console.log('Coerção para true')
}

if(0){
	console.log('Coerção para false, não será executado')
}
```

Um operador que não aciona a coerção implícita de tipo é o `===`, chamado de operador de igualdade estrita. O operador de igualdade `==`, por outro lado, faz comparação e coerção de tipos, se necessário.

```javascript
console.log(1 === '1') // false
```

O conceito de coerção é algo que encontramos constantemente como desenvolvedores JavaScript, portanto, entendê-lo é essencial. As informações que expomos nesse artigo são suficientes para obtermos uma visão geral, porém existem muitos detalhes que você pode estudar para se aprofundar.

## Referências

- [Understanding Scope in Javascript](https://scotch.io/tutorials/understanding-scope-in-javascript)
- [Javascript: A Basic Guide to Scope](https://codeburst.io/javascript-a-basic-guide-to-scope-9682d57be6fc)
- [Scope and Closures in Javascript](https://8thlight.com/blog/jarkyn-soltobaeva/2017/06/13/scope-and-closures-in-javascript.html)
- [JS Function Closures](https://www.w3schools.com/js/js_function_closures.asp)
- [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [Javascript Scope Closures](https://css-tricks.com/javascript-scope-closures/)
- [YDKJY: Scope-Closures](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/scope-closures/README.md)
- [Understanding Objects in Javascript](https://www.taniarascia.com/understanding-objects-in-javascript/)
- [Understanding Prototypes and Inheritance in JS](https://www.taniarascia.com/understanding-prototypes-and-inheritance-in-javascript/)
- [Javascript Objects](https://javascript.info/object)
- [Objects Prototypes and Classes in JS](https://dev.to/attacomsian/objects-prototypes-and-classes-in-javascript-3i9b)
- [Working with Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)
- [Objects Inheritance](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance)
- [JS Objects Prototypes](https://www.w3schools.com/js/js_object_prototypes.asp)
- [Understanding JS: Coercion](https://hackernoon.com/understanding-js-coercion-ff5684475bfc)
- [Type Coercion](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion)
- [Type Coercion in JavaScript](https://2ality.com/2019/10/type-coercion.html#what-is-type-coercion%3F)
- [JavaScript Type Coercion Explained](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/)
- [JS Type Conversion](https://www.w3schools.com/js/js_type_conversion.asp)
- [You Don't Know JS Yet: Types & Grammar](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/types-grammar/ch1.md)
