---
title: Compreendendo Detalhes Fundamentais de Javascript
date: "2020-02-07T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/detalhes-fundamentais-javascript/"
category: "Javascript"
tags:
  - "Javascript"
  - "Programação"
  - "Fundamentos"
  - "Web"
description: "Neste artigo vamos estudar detalhes fundamentais sobre Javascript e os mecanismos principais que atuam por trás da linguagem."
---

## Conteúdo

1. [Introdução](#introducao)
2. [O Poder de Javascript](#o-poder-de-javascript)
3. [Javascript Engine](#javascript-engine)
4. [JavaScript Runtime Environment](#javascript-runtime-environment)
5. [Single-Threaded vs Assíncrono](#single-threaded-vs-assíncrono)
6. [Conclusão](#conclusão)
7. [Referências](#referências)

## Introdução

JavaScript é uma linguagem de programação que está em conformidade com a especificação **[ECMAScript](https://www.ecma-international.org/publications/standards/Ecma-262.htm)**. 

JavaScript é de alto nível, **[interpretada](https://en.wikipedia.org/wiki/Interpreted_language)** / **[compilada just-in-time](https://en.wikipedia.org/wiki/Just-in-time_compilation)** com **[funções de primeira classe](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)**.

Embora seja mais conhecida como linguagem de script para páginas da Web, muitos ambientes que não são de navegador também a usam, como por exemplo **[Nodejs](https://nodejs.org/en/)**. 

JavaScript é uma linguagem dinâmica **[baseada em protótipos](https://www.w3schools.com/js/js_object_prototypes.asp)**, com vários paradigmas, single-threaded e dinâmica, suportando paradigmas orientado a objetos, imperativos e declarativos (por exemplo: programação funcional).

JavaScript pode ser executada em qualquer dispositivo que possua a **[JavaScript Engine](https://en.wikipedia.org/wiki/JavaScript_engine)**. 

Normalmente cada Browser/Navegador incorpora uma **JavaScript Engine** específica, por exemplo:

- **Chrome V8** - Utilizado no Google Chrome. É um projeto de código aberto escrito em C++. O V8 também é usado no Opera, NodeJS e Couchbase.
- **SpiderMonkey** - O mecanismo de código aberto implementado em C++. É mantido pela Mozilla Foundation. Você pode encontrá-lo no Firefox.
- **Nitro** - O mecanismo desenvolvido pela Apple. É usado no Safari.
- **Chakra** - Desenvolvido pela Microsoft como o mecanismo JavaScript do navegador Edge.

### O Poder de Javascript

O JavaScript moderno é uma linguagem de programação "segura". Ele não fornece acesso de baixo nível à memória ou ao CPU, porque foi criada inicialmente para navegadores que não precisam deste acesso.

As capacidades do JavaScript dependem muito do ambiente em que está sendo executado. 

Por exemplo, o **ambiente Nodejs** suporta funções que permitem ao JavaScript: 

- Ler e Gravar arquivos arbitrários
- Adicionar, excluir e modificar dados em seu banco de dados
- Coletar dados de formulários
- Executar solicitações de rede

O **ambiente do JavaScript no navegador** pode fazer tudo relacionado à manipulação de páginas da web, interação com o usuário e o servidor da web:

- Adicionar novos elementos HTML à página web, mudar o conteúdo existente, modificar estilos.
- Reagir às ações do usuário, cliques de mouse, movimentos de ponteiro, pressionamento de teclas.
- Obtenha e defina cookies, faça perguntas ao visitante, mostre mensagens.
- Persistência dos dados no lado do cliente ("armazenamento local").
- Envie solicitações pela rede para servidores remotos, faça o download e faça o upload de arquivos (AJAX).

#### Application Programming Interfaces (API's)

Uma característica muito importante do JavaScript são as **[Application Programming Interfaces (APIs)](https://en.wikipedia.org/wiki/Application_programming_interface)** que nos fornecem poderes extras para utilizarmos em nosso código Javascript.

As **API's** normalmente pertencem a duas categorias:

As **API's do navegador** são incorporadas ao seu navegador da Web e podem expor dados do ambiente de computador ao redor. Por exemplo:

- A **API** do **[DOM(Document Object Model)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)** permite manipular **[HTML](https://www.w3schools.com/html/default.asp)** e **[CSS](https://www.w3schools.com/css/default.asp)**, criando, removendo e alterando HTML, aplicando dinamicamente novos estilos à sua página etc. 
- A **[API de geolocalização](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)** recupera informações geográficas. É assim que o Google Maps consegue encontrar sua localização e plotá-la em um mapa.
- As **[APIs Canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)** e **WebGL** permitem criar gráficos animados em 2D e 3D.

**API's de terceiros** não são incorporados no navegador por padrão, e você geralmente precisa pegar o código e as informações de algum lugar da Web. Por exemplo:

- A **[API do Twitter](https://developer.twitter.com/en/docs)** permite que você faça coisas como exibir seus últimos tweets em seu site.
- A **[API do Google Maps](https://cloud.google.com/maps-platform/)** e permite incorporar mapas personalizados no seu site e outras funcionalidades desse tipo.

### Javascript Engine

Uma JavaScript Engine é um programa de computador que executa o código JavaScript.

A primeira JavaScript Engine foi criada por **Brendan Eich** em 1995 para o browser Netscape Navigator. Era um interpretador rudimentar da língua nascente que Eich inventou. (Isso evoluiu para a Engine SpiderMonkey, ainda usado pelo navegador Firefox).

A primeira JavaScript Engine moderna foi a V8, criada pelo Google para seu navegador Chrome. O V8 estreou como parte do Chrome em 2008 e seu desempenho foi muito melhor do que qualquer Engine anterior. A principal inovação foi a **compilação just-in-time**, que pode melhorar significativamente os tempos de execução.

As Engines convertem o código de alto nível em código legível por máquina, que permite ao computador executar algumas tarefas específicas. Vamos entender a **Google Chrome’s JavaScript V8 Engine** usando uma imagem:

![img](https://raw.githubusercontent.com/the-akira/artigosReact/master/Imagens/JSEngine.png)

- **Primeiramente**: Um arquivo Javascript é alimentado ao **Analisador Léxico ou Parser**.
- **Analisador Léxico ou Parser**: Verifica a síntaxe e a semântica. O **Parser** nada mais é do que uma análise léxica que resulta na quebra do código em tokens de forma a entender seu significado e esses tokens são convertidos em uma **Árvore de Sintaxe Abstrata (AST)**.
- **Abstract Syntax Tree**: É uma árvore hierárquica que serve como a estrutura de representação do programa, que permite ao interpretador entender o programa. A Abstract Syntax Tree é inicialmente direcionado ao interpretador. Você pode utilizar o [ASTExplorer](https://astexplorer.net/) para explorar em detalhes como o **Parser** funciona.
- **Interpretador**: Permite que a **AST** seja convertida em **[Bytecode](https://en.wikipedia.org/wiki/Bytecode)**. No mecanismo V8, esse processo é conhecido como Ignição. Porém quando um código é repetido várias vezes:

```js
function adicionar(x, y) {
	return x + y;
}
  
for(let i=0; i < 1000; i++) { 
    console.log(adicionar(9, 9)); 
} 
```

No código acima nós estamos chamando a função **adicionar()** 1000 vezes. Quando esse código chega até o interpretador, a perfomance do interpretador é diminuída, uma vez que o interpretador deve repetir o código diversas vezes. É o momento que o **Profiler** marcará esse código como otimizável e então ele entrará em ação.

- **Profiler**: Ele irá verificar o código de repetição que pode ser otimizado. Assim que obtém o código de repetição, basicamente move o código para o compilador.
- **Compilador**: Ele nos fornecerá os **Bytecodes** mais otimizados. No caso acima, ele verá o código de repetição e otimizará o código substituindo a função **adicionar(9, 9)** por **18**, uma vez que é repetida várias vezes e produzirá o Bytecode otimizado que é substituído pelo Bytecode mais lento produzido pelo interpretador. No V8 Engine, esse compilador é chamado de TurboFan. Esse processo é repetido várias vezes, o que significa que a velocidade do JavaScript Engine melhora.

É importante recordarmos também que desde 2017 os browsers adicionaram suporte ao **WebAssembly**. Isso permite o uso de executáveis pré-compilados para partes críticas de desempenho dos scripts da página. Os mecanismos JavaScript executam o código do WebAssembly na mesma sandbox que o código JavaScript comum.

### JavaScript Runtime Environment

Quando visitamos um website, utilizamos normalmente um web browser como Google Chrome ou Mozilla Firefox para esta tarefa. Cada browser possui um **Javascript Runtime Environment**, nele estão contidas as **Web API's** que um desenvolvedor pode acessar para construir um programa.

O **[AJAX](https://www.w3schools.com/js/js_ajax_intro.asp)**, a **[árvore DOM](https://www.w3schools.com/js/js_htmldom.asp)** e outras APIs não fazem parte do Javascript, são apenas objetos com propriedades e métodos, fornecidos pelo navegador e disponibilizados no JavaScript Runtime Environment do navegador.

Também existe dentro do JavaScript Runtime Environment a Javascript Engine(vista anteriormente por nós) que analisa o código.

Podemos imaginar o JavaScript Runtime Environment como um grande container. Dentro desse container existem outros containers menores. Conforme a Javascript Engine executa a análise do código, esta começa a colocar diferentes pedaços de código em diferentes containers.

![img](https://raw.githubusercontent.com/the-akira/artigosReact/master/Imagens/Java-Script-Engine.png)

- **Heap**: Este é o espaço de memória física usado para armazenar variáveis, funções e objetos. Como tudo em Javascript é um objeto, tudo o que é alocado na memória usando a palavra-chave `new` é armazenado no **heap**. O Javascript também possui um coletor de lixo que libera a memória alocada para que não precise ser liberada manualmente, como em C/C++
- **Stack**: É aqui que as chamadas de função e API (API da Web em browsers e API C/C ++ em máquinas locais via Nodejs) são armazenadas. Essa parte se comporta exatamente como uma estrutura de dados de pilha típica com uma estrutura LIFO (Last In, First Out). As chamadas de função são adicionadas ao topo da pilha e salvas do topo após a conclusão da execução.
- **API's (Web ou C/C++)**: É aqui que a funcionalidade real das funções internas, como `setTimeout()` e `fetch()`, está localizada. De certa forma, funções como `setTimeout()` podem ser consideradas como uma ativação da função API e sendo retiradas da **Stack de chamadas** imediatamente enquanto a função API continua sendo executada em segundo plano (nesse caso, um temporizador é executado no fundo)
- **Fila de Callbacks**: Algumas funções, como `setTimeout()` que entram em contato com a API, exigem que uma **[função callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)** seja fornecida para que ela saiba o que fazer após a execução da função da API. Nesse caso, as funções callback são colocadas na fila de callback. A própria fila se comporta como uma estrutura de dados fila, uma estrutura FIFO (primeiro a entrar, primeiro a sair)
- **Event Loop (Loop de Eventos)**: O loop de eventos é um algoritmo que verifica constantemente a pilha de chamadas para verificar se existem chamadas de função que precisam ser executadas. Quando a pilha de chamadas está vazia, a primeira entrada na fila de callback é enviada para a pilha de chamadas para concluir a execução. Isso acontece até que a fila esteja vazia.

Para compreendermos melhor o **Event Loop** é importante termos conhecimento de um detalhe importante. Todos os scripts escritos em JavaScript podem ser divididos em dois grupos:

O **primeiro grupo** é composto por scripts imediatamente chamados. Depois de carregados, o ambiente os transmite para execução na JavaScript Engine. No desenvolvimento da web, esses geralmente são scripts iniciais chamados logo após o carregamento de uma página da web pelo navegador.

O **segundo grupo** contém os chamados **Event Callbacks**. Um Event Callback é um pedaço de código executado quando um evento específico ocorre. No contexto de uma página da web, um exemplo de evento é um clique do mouse ou o retorno de uma solicitação de rede.

O Event Loop, que faz parte do JavaScript Runtime Environment, é um mecanismo responsável pelo tratamento de callbacks.

Ao criar uma função **Callback**, você sempre o associa a um evento específico. Se esse evento ocorrer, o ambiente coloca seu Callback na chamada Fila de Callbacks. O Event Loop monitora constantemente a fila e executa seus elementos na ordem em que eles chegam. Callbacks são sempre executados completamente. O Event Loop executa um Callback por vez. Nenhuma alternância de contexto. Todos os retornos de chamada na fila precisam aguardar até que o atual seja concluído.

Se um script é muito longo, ele bloqueia outros. É por isso que os Callbacks devem ser relativamente curtos e simples.

### Single-Threaded vs Assíncrono

Javascript é uma linguagem de Single-Thread (Na ciência da computação, uma Thread é como uma entidade capaz de executar linhas de código independentemente) isso significa que ele possui uma stack de chamadas e um heap(memória). Como esperado, ele executa o código em ordem e deve concluir a execução de uma peça de código antes de passar para o próximo. É síncrono, mas às vezes isso pode ser prejudicial. Por exemplo, se uma função demorar para executar ou tiver de esperar por algo, isso causará um "congelamento" durante esse período, um bom exemplo que podemos considerar é:

```javascript
alert("Hello World");
```

Não poderemos interagir com a página web até que cliquemos em **OK** para que o alerta possa desaparecer.

É então que podemos nos perguntar, como obter código assíncrono com Javascript?

A stack de chamadas é capaz de reconhecer funções da **Web API** e possibilita elas serem manuseadas pelo browser. Uma vez que essas tarefas sejam finalizadas pelo browser, elas retornam e são colocadas na stack como uma Callback.

Podemos abrir o **console** de nosso navegador e digitar `window` e então apertar **enter**. Podemos então visualizar tudo que a **Web API** tem a nos oferecer. Isso inclui chamadas AJAX, Event Listeners, fetch API e setTimeout, o Javascript usa linguagens de programação de baixo nível como C++ para executá-las nos bastidores.

Vamos agora executar o seguinte código em nosso console para vermos um exemplo:

```javascript
console.log("1")
setTimeout(() => {
    console.log("2")
}, 1000)
console.log("3")
```

Veja que vamos obter:

```
undefined
1
3
2
```

Vamos compreender o que aconteceu:

- Nos é retornado *undefined*, isso porque `setTimeout` não foi finalizado, ele retorna *undefined* como padrão, uma vez que ainda não foi atribuído um valor a ele.
- `console.log("1")` é o primeiro na stack, sendo assim, ele é impresso. Em seguida, a JavaScript Engine percebe o `setTimeout`, ao qual não é manuseado pelo Javascript, sendo assim ele é enviado para a **Web API** para ser tratado de forma assíncrona. A stack de chamadas segue então sem se preocupar com o código que foi enviado para a Web API e o `console.log("3")` é impresso.
- Uma vez que a função Callback finalmente é executada, `console.log("2")` é impresso.

Para mais detalhes, **[Loupe](https://latentflip.com/loupe/)** é uma visualização para ajudar você a entender como a **stack de chamadas** / **loop de eventos** / **fila de callbacks** do JavaScript interagem entre si.

A partir da especificação **ES6**, o JavaScript introduziu vários recursos que nos ajudam com código assíncrono que não envolve o uso de Callbacks, são eles:

- **[Promises](https://flaviocopes.com/javascript-promises/)**
- **[Async/Await](https://flaviocopes.com/javascript-async-await/)**

### Conclusão

Javascript é uma linguagem moderna com diversas particularidades, tornando assim essencial que entendamos os mecanismos que operam por trás da linguagem, para que assim seja possível escrevermos códigos mais eficientes e livre de *bugs*! 

Este artigo trouxe um estudo sobre essas peculiaridades e apresentou a sua importância, demonstrando que é essencial entendermos como uma linguagem é construída, dessa forma a sua caminhada perante o aprendizado de Javascript será otimizada. 

Para masterizar seu conhecimento existem recursos excelentes na Web, destaco especialmente:

- **[W3Schools Javascript Tutorial](https://www.w3schools.com/js/)**
- **[The Modern JavaScript Tutorial](https://javascript.info/)**
- **[TypeOfNaN JavaScript Quizzes](https://quiz.typeofnan.dev/)**
- **[MDN - JavaScript - Dynamic client-side scripting](https://developer.mozilla.org/en-US/docs/Learn/JavaScript)**
- **[You Don't Know JS Yet (book series)](https://github.com/getify/You-Dont-Know-JS)**

Bons estudos!

### Referências

- [V8 Engine](https://v8.dev/)
- [What is the difference between JavaScript Engine and JavaScript Runtime Environment](https://stackoverflow.com/questions/29027845/what-is-the-difference-between-javascript-engine-and-javascript-runtime-environm/29027933)
- [What happens inside JavaScript Engine?](https://www.geeksforgeeks.org/what-happens-inside-javascript-engine/)
- [What is JavaScript Engine?](http://dolszewski.com/javascript/javascript-runtime-environment/)
- [If Javascript Is Single Threaded, How Is It Asynchronous?](https://dev.to/steelvoltage/if-javascript-is-single-threaded-how-is-it-asynchronous-56gd)
- [Javascript and Asynchronous Magic - Explaining the JS Engine and Event Loop](https://levelup.gitconnected.com/javascript-and-asynchronous-magic-bee537edc2da)
- [The Javascript Runtime Environment](https://medium.com/@olinations/the-javascript-runtime-environment-d58fa2e60dd0)
- [JavaScript Visualized: the JavaScript Engine](https://dev.to/lydiahallie/javascript-visualized-the-javascript-engine-4cdf)
- [So you think you know JavaScript?](https://www.amanexplains.com/so-you-think-you-know-JavaScript/)