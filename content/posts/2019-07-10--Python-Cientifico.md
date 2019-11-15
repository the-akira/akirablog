---
title: Python Científico - Ferramentas e Bibliotecas
date: "2019-07-10T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/python-cientifico-bibliotecas/"
category: "Fundamentos"
tags:
  - "Fundamentos"
  - "Científico"
  - "Python"
description: "A alta popularidade da linguagem resultou em uma grande quantidade de bibliotecas e pacotes produzidos em campos como visualização
de dados, machine learning, processamento de linguagem natural, análise complexa de dados, cálculos matemáticos, web scraping e muito
mais."
---

![img](https://i.imgur.com/gMzk3ti.png)

---------------------------------------

Python é uma linguagem capaz de rodar em múltiplas plataformas, de propósito-geral e alto nível. De tal forma, ela possui
inúmeras aplicações e vem sendo amplamente adotada em diversas comunidades, da ciência de dados até mesmo desenvolvimento web.
Python é muito valorizada por sua eficiente e precisa sintáxe e sua capacidade de integração com outras linguagens (**C**/**C++**/**FORTRAN**).

A alta popularidade da linguagem resultou em uma grande quantidade de bibliotecas e pacotes produzidos em campos como visualização
de dados, machine learning, processamento de linguagem natural, análise complexa de dados, cálculos matemáticos, web scraping e muito
mais. Nesse **post** específico, vamos focar nas mais poderosas bibliotecas e ferramentas para computação científica com Python.

---------------------------------------

* **[The Jupyter Notebook](https://jupyter.org/)**

O **Jupyter Notebook** é uma aplicação web open-source que permite você criar e compartilhar documentos que contém **código ao vivo**,
equações, visualizações e texto narrativo. Usos incluem: Limpeza e transformações de dados, simulação numérica, modelagens estatísticas,
visualização de dados, machine learning e muito mais. O notebook tem suporte para mais de 40 linguagens de programação, incluindo Python,
R, Julia e Scala. Notebooks podem ser compartilhados com outras pessoas utilizando email, [Dropbox](https://www.dropbox.com/), [GitHub](https://github.com/) ou [Jupyter Notebook Viewer](https://nbviewer.jupyter.org/). Também possui output interactivo rico, produzindo: HTML,
imagens, videos, LaTeX e tipos MIME customizados, além de capacidade de integração com Big Data.

* **[Spyder](https://www.spyder-ide.org/)**

**Spyder** é um ambiente científico poderoso escrito em Python, para Python, e projetado por e para cientistas, engenheiros e analistas de 
dados. Oferece uma combinação única de edição avançada, análise, debugging e funcionalidade profiling de uma ferramenta de desenvolvimento
compreensiva com exploração de dados, execução interactiva, inspeção profunda e belas capacidades de visualizações. Além de suas capacidades
pré-construídas, suas habilidades podem ser extendidas muito além, através de plugins e API.

* **[NumPy](https://www.numpy.org/)**

**NumPy** é o pacote fundamental para Computação Científica com Python. Ele contém:

- Um poderoso objeto array N-dimensional
- Funções sofisticadas (broadcasting)
- Ferramentas para integrar código C/C++ e Fortran
- Capacidades como Álgebra Linear, Transformadas de Fourier e números aleatórios

Além de seu uso científico óbvio, NumPy também pode ser utilizada como um container eficiente de múltiplas-dimensões para dados
genéricos. Tipos de dados arbitrários podem ser definidos, isso permite NumPy perfeitamente e com velocidade integrar com uma grande
variedade de bancos de dados.

* **[SciPy](https://www.scipy.org/)**

**SciPy** é um ecossistema baseado em Python de softwares open-source para Matemática, Ciência e Engenharia. Em particular, esses são
os principais pacotes: NumPy, Biblioteca SciPy, Matplotlib, IPython, SymPy, Pandas. Por sua importância, existem diversas conferências dedicadas ao SciPy, sendo elas Python - SciPy, EuroSciPy e SciPy.in.

* **[Numba](https://numba.pydata.org/)**

**Numba** torna o código Python mais rápido. Numba é um compilador JIT open source que traduz um subconjunto de código Python e NumPy em
código de máquina rápido. Numba acelera as funções Python, traduzindo elas para um código de máquina otimizado em tempo de execução, 
utilizando a biblioteca compiladora LLVM que é padrão da indústria. Algoritmos numéricos compilados em Numba podem ter a aproximação
de eficiência de C e Fortran.

* **[Pandas](https://pandas.pydata.org/)**

**Pandas** é uma biblioteca open source licenciada-BSD que provê estruturas de dados de utilização fácil e alta perfomance e ferramentas de 
análises de dados para a linguagem de programação Python, seu objetivo é ser um bloco de construção de alto nível fundamental para análises
de dados do mundo real em Python, adicionalmente ela tem o objetivo maior de ser a mais poderosa e flexível ferramenta open source para 
análise e manipulação de dados. 

* **[Dask](https://dask.org/)**

**Dask** escala Python nativamente, provendo paralelismo avançado para *analytics*, possibilitando desempenho em escala para as ferramentas
que você adora. Dask é composta de duas partes: *Agendamento de Tarefas Dinâmicas* (otimizado para computação) e *coleções "Big Data"* (como arrays paralelos, dataframes e listas que extendem interfaces comuns).

* **[Bokeh](https://bokeh.pydata.org/en/latest/)**

**Bokeh** é uma biblioteca de visualização interativa que visa *web browsers* modernos para apresentação. Seu objetivo é prover uma elegante e concisa construção de gráficos versáteis, e para extender essa capacidade com alta perfomance interactiva sob conjuntos de dados muito grandes. Bokeh pode ajudar qualquer um que deseja rápido e facilmente criar **plots** interactivos, dashboards e aplicações de dados.

* **[HoloViews](http://holoviews.org/)**

**HoloViews** é uma biblioteca Python open source desenvolvida com o objetivo de tornar análises e visualizações de dados muito mais simples. Com HoloView você pode usualmente expressar o que você desejar em poucas linhas de código, deixando assim você focar naquilo que você está explorando, não no processo de *plotting*.

* **[Datashader](http://datashader.org/)**

**Datashader** é um sistema de pipeline de gráficos para criar significantes representações de grandes conjuntos de dados, de maneira rápida e flexível. Datashader quebra a criação de imagens em uma série de passos explícitos que permitem computações serem feitas em representações intermediárias. Essa abordagem permite visualizações precisas e efetivas a serem produzidas automaticamente sem tunning de parâmetros tentativa-e-erro. Ela também torna simples para cientistas de dados focarem em dados particulares e relações de interesse.

* **[Matplotlib](https://matplotlib.org/)**

**Matplotlib** é uma biblioteca Python de 2D *plotting* que produz a publicação de figuras de qualidade em uma variedade de formatos e ambientes interactivos em diversas plataformas. Matplotlib pode ser utilizada em scripts Python, IPython Shells, Jupyter Notebook, servidores de aplicações web e quatro kits de ferramentas de interface de usuário. Matplotlib tenta tornar as coisas fáceis mais fáceis e as difíceis possíveis. Você pode gerar com ela: *plots*, histogramas, *power spectra*, gráficos de barra, gráficos de erro, gráficos de dispersão, etc, com poucas linhas código.

* **[Scikit-Learn](https://scikit-learn.org/stable/)**

**Scikit-Learn** traz as revolucionárias capacidades do Machine Learning para a linguagem Python. Possui ferramentas eficientes e simples para *data mining* e análise de dados, acessível para todos e reusável em vários contextos. Construída em NumPy, SciPy e matplotlib. Open source, usável comercialmente com licença-BSD. Dentre algumas tarefas que a biblioteca conta: classificação, regressão, clusterização, redução de dimensionalidade, seleção de modelos e pré-processamento.

* **[H2O](https://www.h2o.ai/products/h2o/)**

A plataforma de Machine Learning open source para empresas. **H2O** é plataforma de machine learning completamente open source distribuída em memória com escalabilidade linear. H2O suporta os algoritmos mais utilizados em estatística e machine learning, incluindo *gradient boosted machines*, *generalized linear models*, *deep learning* e mais. **H2O** também tem uma funcionalidade de *AutoML* líder na indústria, que é capaz de rodar automaticamente através de todos os algoritmos e seus hiperparâmetros para produzir uma tabela de liderança dos melhores modelos. A plataforma H2O é usada por mais de 18.000 organizações globalmente e é extremamente popular em ambas as comunidades de R e Python.

* **[Tensorflow](https://www.tensorflow.org/)**

**Tensorflow** é uma plataforma open source end-to-end para machine learning. Possui um ecossistema compreensivo e flexível de ferramentas, bibliotecas e recursos de comunidade que permite os pesquisadores empurrarem o *state-of-art* em machine learning e para desenvolvedores construirem aplicações com machine learning.

* **[Conda](https://docs.conda.io/en/latest/)**

**Conda** é um sistema gerenciador de pacotes open source e gerenciador de ambientes que roda em Windows, macOS e Linux. Conda rapidamente instala, roda e atualiza pacotes e suas dependências. Conda facilmente cria, salva, carrega e troca entre ambientes em seu computador local. Foi criado para programas Python, porém pode empacotar e distribuir software para qualquer linguagem. Conda como gerenciador de pacotes nos ajuda a encontrar e instalar pacotes. Conda também pode ser combinado com sistemas de integração contínua como Travis CI e AppVeyor, para prover frequentes testes automatizados para seu código.

* **[Seaborn](https://seaborn.pydata.org/)**

**Seaborn**: Visualização de dados estatísticos, é uma biblioteca de visualização de dados baseada em matplotlib. Ela provê uma interface de alto-nível para desenhos de gráficos estatísticos informativos e atrativos. Ela é construída em cima de matplotlib e pode ser integrada com as estruturas de dados de **Pandas**. Dentre suas funcionalidades estão: Uma API orientada a conjunto de dados para examinar relações entre múltiplas variáveis. Suporte especializado para uso de variáveis categóricas para mostrar observações ou agregar estatísticas. Opções para visualizar distribuições bivariadas ou univariadas e para comparar elas entre subconjuntos de dados. Estimativa automática e *plotting* de modelos de *linear regression* para diferentes tipos de variáveis dependentes, dentre muitas outras funcionalidades.

* **[Graph-tool](https://graph-tool.skewed.de/)**

**Graph-tool** é um módulo eficiente de Python para manipulação e análise estatística de grafos(também conhecidos como [*networks*](http://en.wikipedia.org/wiki/Network_theory)). Ao contrário da maioria dos outros módulos com a mesma funcionalidade, as estruturas de dados e algoritmos principais estão implementados em C++, fazendo extensivo uso de *template metaprogramming* baseado diretamente na [Biblioteca Boost Graph](http://www.boost.org/doc/libs/release/libs/graph). Isso confere a ela um nível de perfomance que é comparável(ambos em uso de memória e tempo computacional) a uma biblioteca de C/C++ pura.

* **[mlpy - Machine Learning Python](http://mlpy.sourceforge.net/)**

**mlpy** é um módulo Python para Machine Learning construída em cima de NumPy/SciPy e das [Bibliotecas Científicas GNU](http://www.gnu.org/s/gsl/). mlpy fornece uma larga variedade de métodos *state-of-art* de machine learning para problemas *supervised* e *unsupervised* e tem como objetivo a busca de um compromisso razoável entre modularidade, manutenção, reprodutibilidade, usabilidade e eficiência. mlpy é multiplataforma, funciona com Python 2 e 3 e é open source, distribuída sob a licença geral pública GNU versão 3.

* **[PyTorch](https://pytorch.org/)**

**PyTorch** é uma plataforma open source de *Deep Learning* que fornece um caminho rápido de protótipos de pesquisa para implantação de produção. Características chaves e capacidades: Front-End Híbrido, Treinamento Distribuído, Python-Primeiro, ferramentas e bibliotecas.

* **[Sage Math](https://www.sagemath.org/)**

**Sage Math** é um sistema de sofware matemático livre e open source licenciado sob GPL. É construída em cima de muitos pacotes open source existentes: NumPy, SciPy, matplotlib, SymPy, GAP, FLINT, R e muitos outros. Acesse seu poder combinado através de uma linguagem comum baseado em Python. Sua missão é criar uma alternativa open source e livre viável em relação a Magma, Maple, Mathematica e Matlab.

* **[Scikit-image](https://scikit-image.org/)**

Processamento de imagens em Python. **Scikit-image** é uma coleção de algoritmos para processamento de imagens. Ele é disponível livre de qualquer custo e restrição, ele extende o módulo **scipy.ndimage** para fornecer um conjunto versátil de rotinas para processamento de imagens, ele é escrito na linguagem Python.

* **[GEKKO Dynamic Optimization](https://gekko.readthedocs.io/en/latest/)**

**GEKKO** é um pacote Python para machine learning e otimização de inteiros-mistos e equações diferenciais algébricas. Está acoplado com ele resolvedores de grande escala para linear, quadrático, não-linear e programação inteira mista (LP, QP, NLP, MILP, MINLP). Modos de operações incluem regressão de parâmetros, reconciliação de dados, otimização em tempo real, simulação dinâmica e controle preditivo não-linear. GEKKO é uma biblioteca orientada a objetos para facilitar a execução local de APMonitor.

* **[SymPy](https://www.sympy.org/en/index.html)**

**SymPy** é uma biblioteca Python para matemática simbólica. Seu objetivo é tornar-se um Sistema Computacional Algébrico cheio de recursos e ao mesmo tempo manter o código o mais simples possível de forma a ser mais compreensível e facilmente extensível. SymPy é escrito inteiramente em Python e é livre sob a licença BSD.

* **[StatsModels](http://www.statsmodels.org/devel/)**

**Statsmodel** é um módulo Python que fornece classes e funções para estimativas de diferentes modelos estatísticos, assim como para conduzir testes estatísticos e exploração de dados estatísticos. Uma lista extensiva de resultados estatísticos está disponível para cada estimador. Os resultados são testados contra pacotes estatísticos existentes para haver certeza de que estão corretos. O pacote foi lançado sob licença open source modificada BSD (cláusula 3).

* **[Plotly](https://plot.ly/python/)**

**Ploty's** é uma biblioteca gráfica em Python que torna possível a publicação de gráficos online de alta qualidade e interactivos. 

**[Keras](https://keras.io/)**

* **Keras**: A Biblioteca Python para *Deep Learning*. Keras é API de alto nível para redes neurais, escrita em Python e capaz de rodar em cima de TensorFlow, CNTK ou Theano. Foi desenvolvida com o foco de possiblitar rápida experimentação. Estar aptp a ir da ideia para o resultado com o menor atraso possível é a chave para um boa pesquisa.

* **[CNTK](https://github.com/Microsoft/cntk)**

O *Microsoft Cognitive Toolkit* é um kit de ferramentas unificado para *Deep Learning* que descreve redes neurais como uma série de passos computacionais através de grafos dirigidos. Nesses grafos dirigidos, nós de folha representam valores de *input* ou parâmetros de redes, enquanto que outros nós representam operações de matrizes sobre seus *inputs*. CNTK permite usuários realizarem com facilidade e combinar modelos populares como DNN's, CNN's e RNN's/LSTM's.

* **[Theano](http://deeplearning.net/software/theano/)**

**Theano** é uma biblioteca Python que permite você definir, otimizar e avaliar expressões matemáticas envolvendo arrays multi-dimensionais de forma eficaz. Theano consiste de: integração com NumPy, uso transparente de GPU, diferenciação simbólica eficiente, estabilidade e velocidade de otimizações, geração de código C dinãmico, extensivos testes unitários e auto-verificação. Theano empodera computação científica de grande-escala investigativa e intensiva desde 2007.

* **[Google's Colab](https://colab.research.google.com)**

**Google's Colab** é um ambiente livre de Jupyter Notebook que não requer instalação e roda inteiramente na nuvem. Com ele você pode escrever e executar código, salvar e compartilhar análises e acessar recursos computacionais poderosos, incluindo GPU, tudo isso gratuitamente em seu web browser.

* **[NLTK](https://www.nltk.org/)**

**Natural Language Toolkit** é a plataforma líder para construção de programas Python para trabalhar com dados de linguagem humana. Ela fornece uma interface de uso simples com mais de 50 rescursos corpora e léxicos como WordNet, junto com um conjunto de bibiliotecas de processamento de texto para classificação, tokenização, stemming, taggging, parsing e raciocínio semântico, além disso traz wrappers para biblioteca NLP poderosas e uma comunidade ativa.

* **[spaCy](https://spacy.io/)**

**spaCy** fornece processamento de linguagem natural de força industrial, tendo como principais características: tokenização não-destrutiva, reconhecimento de entidades nomeadas, suporte para mais de 49 linguagens, 16 modelos estatísticos para 9 linguagens, vetores de palavras pré-treinados, velocidade, fácil integração com *Deep Learning* e diversas funcionalidades essencais para NLP.

* **[PyBrain](http://pybrain.org/)**

**PyBrain** é uma biblioteca modular de Machine Learning para Python. Seu objetivo é oferecer algortimos flexíveis, fáceis-de-usar, porém poderosos, para tarefas de Machine Learning e também uma variedade de ambientes pré-definidos para testar e comparar seus algoritmos. PyBrain é a abreviação para Python-Based Reinforcement Learning Artificial Intelligence and Neural Network Library.

---------------------------------------