---
title: Fundamentos de Análise de Dados com Pandas
date: "2019-10-25T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/fundamentos-pandas/"
category: "Data Science"
tags:
  - "Python"
  - "Programação"
  - "Fundamentos"
description: "Pandas é uma biblioteca Python que fornece estruturas de dados rápidas, flexíveis e expressivas."
---

![img](https://i.ibb.co/r3TXjDf/Pandas-Capa.png)

# Conteúdo

1. [Introdução](#introducao)
2. [Instalação](#instalacao)
3. [Objetos Pandas](#objetos-pandas)
4. [Lendo Arquivos](#lendo-arquivos)
5. [Referências](#referencias)

# Introducao

**Pandas** é uma biblioteca **Python** que fornece estruturas de dados rápidas, flexíveis e expressivas. Desenvolvida para tornar o trabalho com dados 'relacionais' e 'rotulados' fácil e intuitivo. Seu objetivo é ser o bloco de construção fundamental de alto nível para fazer análises de dados práticas, com dados reais, em Python. Adicionalmente possui o objetivo maior de ser a mais poderosa e flexível ferramenta open source para análise de dados e manipulação. 

Pandas é adequado para muitos diferentes tipos de dados:

- Dados tabulares com tipos de colunas heterogêneas, como uma tabela SQL ou uma planilha Excel
- Ordenados e Não-Ordenados (não necessariamente com frequência fixa) dados de séries temporais
- Matriz de dados arbitrários (tipo homegêneo ou heterogêneo) com rótulos de linhas e colunas
- Qualquer outra forma de conjuntos de dados estatísticos / observacional.
- Os dados realmente não precisam ser rotulados para serem colocados em uma estrutura de dados Pandas

As duas estruturas de dados primárias de pandas são: **Series** (1-dimensão) e **DataFrame** (2-dimensões), ambos são capazes de lidar com a vasta maioria de casos de uso em finanças, estatísticas, ciências sociais e muitas áreas da engenharia. Para usuários **R**, **DataFrame** fornece tudo que o **data.frame de R** fornece e muito mais. Pandas é construída em cima de NumPy e a intenção é que possa ser bem integrado com ambientes de computação científica junto de outras bibliotecas.

Aqui temos uma lista de tarefas que Pandas pode executar com maestria:

- Fácil de lidar com **dados faltantes** (representados como NaN) em pontos flutuantes assim como dados não-ponto flutuantes
- Mutabilidade de tamanho: colunas podem ser **inseridas e deletadas** do DataFrame e de objetos de dimensão elevada
- Automático e explícito **alinhamento de dados**: objetos podem ser explicitamente alinhados a um conjunto de rótulos, ou o usuário pode simplesmente ignorar os rótulos e deixar *Series*, *DataFrame*, etc. Automaticamente alinhando os dados para você em computações
- Poderosa, flexível, a funcionalidade **group by** executa operações *split-apply-combine* em conjuntos de dados, para ambos agregar e transformar dados
- Torna fácil de converter dados com índices diferentes e irregulares em outras estruturas de dados Python e NumPy para objetos DataFrame
- Inteligente **slicing**, **fancy indexing** e **subsetting** de grandes conjuntos de dados
- Intuitivos **merging** e **joining**
- Flexível **reshaping** e **pivoting** de conjuntos de dados
- Rotulagem de eixos **Hierárquica**
- Ferramentas robutas de Input/Output para carregar dados de **flat files** (CSV e delimitados), arquivos Excel, bancos de dados, e salvar/carregar dados do ultra-rápido **HDF5** formato
- Funcionalidade específica para **Séries Temporais**: geração de datas e conversão de frequência, movendo estatísticas de janelas, movendo janelas de regressão linear, alteração de datas, etc.

Muitos desses princípios estão aqui para abordar as deficiências freqüentemente experimentadas em outros linguagens / ambientes de pesquisa científica. Para os cientistas de dados, o trabalho com dados é tipicamente dividido em vários estágios: filtrar e limpar os dados, analisá-los / modificá-los e organizar os resultados da análise em uma forma adequada para plotagem ou exibição tabular. O pandas é a ferramenta ideal para todas essas tarefas.

Algumas observações:

- pandas é rápido. Muitos dos bits algorítmicos de baixo nível foram extensivamente ajustados no código Cython. No entanto, como com qualquer outra coisa, a generalização geralmente sacrifica o desempenho. Portanto, se você se concentrar em um recurso para seu aplicativo, poderá criar uma ferramenta especializada mais rápida.
- pandas é uma dependência de statsmodels, tornando-se uma parte importante do ecossistema de computação estatística em Python.
- pandas tem sido amplamente utilizado na produção em aplicações financeiras.

### Estruturas de Dados

| Dimensões | Nomes     | Descrição                                                                                       |
|-----------|-----------|-------------------------------------------------------------------------------------------------|
| 1         | Séries    | Array 1D rotulado homegêneo                                                                     |
| 2         | DataFrame | Estrutura tabular geral 2D, rotulada, de tamanho mutável com colunas potencialmente heterogêneo |

Vejamos uma ilustração representativa:

![img](https://i.ibb.co/4Jd6ncF/PandasDS.png)

A melhor maneira de pensar sobre as estruturas de dados dos pandas é imaginando recipientes flexíveis para dados dimensionais menores. Por exemplo, DataFrame é um contêiner para Series e Series é um contêiner para escalares. Gostaríamos de poder inserir e remover objetos desses contêineres de maneira semelhante a um dicionário.

# Instalacao

Existem algumas maneiras de obtermos a biblioteca Pandas, vamos citar aqui duas fáceis opções. 

### Instalação com o gerenciador de pacotes pip

É muito comum **pip** já vir instalado junto com a linguagem Python, então uma vez que já tenhamos Python instalado, vamos abrir nosso **terminal** e digitar os seguintes comandos:

```python
pip install pandas
```

Vamos instalar também a biblioteca NumPy, pois precisaremos de algumas de suas funcionalidades.

```python
pip install numpy
```

Caso você não esteja com o pip instalado, você pode obter ele [nesse link](https://pip.pypa.io/en/stable/installing/).
Agora vamos executar o Python interactivo com o comando:

```python
python
```

Para termos certeza que nossa instalação foi bem sucedida, vamos tentar importar as bibliotecas:

```python
>>> import pandas as pd
```

```python
>>> import numpy as np
```

Caso não nos tenha retornado nenhum erro, significa que já temos Pandas instalado em nosso computador.

### Instalação com a Distribuição Anaconda

Esta opção de instalação é muito interessante, pois além de obtermos a biblioteca **Pandas** e **NumPy**, ganharemos de presente uma plataforma científica completa, que inclui diversas ferramentas e bibliotecas poderosas para manipulação e exibição de dados, assim como cálculos complexos de alta perfomance.

Podemos fazer o download da Distribuição Anaconda [nesse link](https://www.anaconda.com/distribution/#download-section), siga os passos intuitivos da instalação e você estará pronto para utilizar Pandas.

# Objetos Pandas

No nível mais básico, os objetos Pandas podem ser considerados versões aprimoradas de Matrizes estruturadas NumPy nas quais as linhas e colunas são identificadas com rótulos em vez de simples índices inteiros. O Pandas fornece uma série de ferramentas, métodos e funcionalidades úteis, em cima de estruturas de dados básicas, mas quase tudo o que se segue exigirá uma compreensão de quais são essas estruturas. 

### Series

Series é um array rotulado unidimensional capaz de conter qualquer tipo de dados (inteiros, strings, números de ponto flutuante, objetos Python, etc.). Os rótulos dos eixos são referidos coletivamente como o índice. O método básico para criar uma série é:

```python
dados = [1,2,3,4,5]
indices = ['a','b','c','d','e']

s = pd.Series(dados, index=indices)
s
```

Output:
```
a    1
b    2
c    3
d    4
e    5
dtype: int64
```

Aqui, os **dados** podem ser diferentes tipos:

- Um dicionário Python
- Um *ndarray*
- Um valor escalar (como 6 por exemplo)

Nesse caso utilizamos uma lista Python de 5 números como nossos dados, os indices passados são uma lista de rótulos do eixo.

#### Criando uma Series através de um ndarray

Caso os dados sejam um ndarray, os indices devem ser do mesmo tamanho dos dados. Caso nenhum índice seja passado, será criado um com valores entre 0 e o tamanho dos dados - 1.

```python
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
s
```

Output:
```
a    0.756326
b    0.287549
c   -0.513211
d   -0.592722
e    1.381578
dtype: float64
```

Para acessarmos os índices:

```python
s.index
```

Output:
```
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
```

Caso queiramos que os rótulos sejam gerados automaticamente:

```python
pd.Series(np.random.randn(5))
```

Output:
```
0   -0.628466
1   -1.652754
2   -1.387233
3   -0.266279
4   -0.313893
dtype: float64
```

#### Criando uma Series através de um dicionário Python

Podemos também instanciar as Series através de dicionários

```python
dicionario = {'a': 10, 'b': 12, 'c': '13'}
pd.Series(dicionario)
```

Output:
```
a    10
b    12
c    13
dtype: object
```

Perceba o que ocorre ao adicionarmos mais um índice

```python
pd.Series(dicionario, index=['b', 'c', 'd', 'a'])
```

Output:
```
b     12
c     13
d    NaN
a     10
dtype: object
```

Veja que o índice **d** aparece como NaN (not a number), que é o marcador padrão de dados ausentes usado em Pandas.

#### Criando uma Series através de um valor escalar

Caso os dados sejam um valor escalar, um índice deve ser fornecido. O valor será repetido de forma a corresponder com o comprimento do **index**.

```python
pd.Series(5.0, index=['a', 'b', 'c', 'd', 'e'])
```

Output:
```
a    5.0
b    5.0
c    5.0
d    5.0
e    5.0
dtype: float64
```

#### Series são similares ao ndarray

A Series age de maneira muito semelhante a um ndarray e é um argumento válido para a maioria das funções do NumPy. No entanto, operações como *slicing* também dividirão o índice.

```python
s[0]
```

Output:
```
0.756326006943376
```

Seleciona os três primeiros:

```python
s[:3]
```

Output:
```
a    0.756326
b    0.287549
c   -0.513211
dtype: float64
```

Valores de **s** maiores que a mediana:

```python
s[s > s.median()]
```

Output:
```
a    0.756326
e    1.381578
dtype: float64
```

Selecionando valores em uma ordem diferente:

```python
s[[0, 3, 1]]
```

Output:
```
a    0.756326
d   -0.592722
b    0.287549
dtype: float64
```

Calculando o valor exponencial de cada valor em **s**:

```python
np.exp(s)
```

Output:
```
a    2.130435
b    1.333157
c    0.598570
d    0.552821
e    3.981179
dtype: float64
```

Assim como os arrays NumPy, as Series pandas possuem um atributo **dtype**.

```python
s.dtype
```

Output:
```
dtype('float64')
```

#### Series sao similares aos dicionários

A Series é como um dicionário de tamanho-fixo, de forma que podemos obter e setar os valores pelo rótulo de índice:

```python
s['a']
```

Output:
```
0.756326006943376
```

Alterando o valor referente ao índice **e**:

```python
s['e'] = 12
```

Vejamos novamente os valores em **s**:

```python
s
```

Output:
```
a     0.756326
b     0.287549
c    -0.513211
d    -0.592722
e    12.000000
dtype: float64
```

Verificando se o índice **e** está presente em **s**:

```python
'e' in s
```

Output:
```
True
```

Verificando se o índice **f** está presente em **s**:

```python
'f' in s
```

Output:
```
False
```

#### Operações Vetorizadas e alinhamento de rótulos com Series

Ao trabalhar com arrays NumPy brutos, o loop por valor-a-valor geralmente não é necessário. O mesmo acontece quando se trabalha com Series em pandas. A Series também pode ser passada para a maioria dos métodos NumPy que esperam um ndarray. Vejamos alguns exemplos:

```python
s + s
```

Output:
```
a     1.512652
b     0.575099
c    -1.026423
d    -1.185444
e    24.000000
dtype: float64
```

Multiplicando todos os valores por **2**:

```python
s * 2
```

Output:
```
a     1.512652
b     0.575099
c    -1.026423
d    -1.185444
e    24.000000
dtype: float64
```

Novamente, calculando o exponencial de cada valor em **s**:

```python
np.exp(s)
```

Output:
```
a    2.130435
b    1.333157
c    0.598570
d    0.552821
e    162754.791419
dtype: float64
```

### DataFrame

DataFrame é uma estrutura de dados rotulada bidimensional com colunas de tipos potencialmente diferentes. Você pode pensar nele como uma planilha ou tabela SQL, ou um dicionário de objetos **Series**. Geralmente é o objeto de pandas mais comumente usado. Como a **Series**, o **DataFrame** aceita muitos tipos diferentes de entrada:

- Dicionário de ndarrays de 1 dimensão, listas, dicionários ou Series
- numpy.ndarray de 2 dimensões
- [Arrays Estruturados](https://docs.scipy.org/doc/numpy/user/basics.rec.html)
- Uma Series
- Outro DataFrame

Juntamente com os dados, você pode, opcionalmente, passar argumentos de índice (rótulos de linha) e colunas (rótulos de coluna). Se você passar um índice e / ou colunas, estará garantindo o índice e / ou colunas do DataFrame resultante. Assim, um dicionário de Series mais um índice específico descartará todos os dados que não correspondam ao índice passado.

#### Criando nosso primeiro DataFrame

Vamos iniciar criando dois dicionários que representarão respectivamente a área e a população de cidades brasileiras

```python
cidades_area = {'Florianopolis': 675409, 'Maceió': 511000 , 'Joinville': 1130878, 'Santa Maria': 18231, 'Porto Velho': 3408237}
cidades_populacao = {'Florianopolis': 477798, 'Maceió': 996733, 'Joinville': 577077, 'Santa Maria': 277309, 'Porto Velho': 519531}
```

A partir dos dicionários criaremos duas Series pandas

```python
area = pd.Series(cidades_area)
populacao = pd.Series(cidades_populacao)
```

Agora finalmente poderemos construir nosso **DataFrame**

```python
cidades = pd.DataFrame({'populacao': populacao, 'area': area})
```

Para confirmarmos o tipo

```python
type(cidades)
```

Output:
```
pandas.core.frame.DataFrame
```

Assim como o Objeto Series, o DataFrame tem um atributo chamado **index** que nos dá acesso aos rótulos de índice

```python
cidades.index
```

Output:
```
Index(['Florianopolis', 'Maceió', 'Joinville', 'Santa Maria', 'Porto Velho'], dtype='object')
```

Adicionalmente, o DataFrame possui um atributo **columns** no qual é um objeto Index que guarda os rótulos das colunas

```python
cidades.columns
```

Output:
```
Index(['populacao', 'area'], dtype='object')
```

#### DataFrame como um Dicionário Especial

Similarmente nós também podemos imaginar o DataFrame como um dicionário especial. O dicionário mapeia uma chave para um valor, um DataFrame mapeia o nome de uma coluna para uma Series de dados que representam esta coluna. Por exemplo, se pedirmos pelo atributo 'area', nos será retorna um objeto Series contendo os valores das areas.

```python
cidades['area']
```

Output:
```
Florianopolis     675409
Maceió            511000
Joinville        1130878
Santa Maria        18231
Porto Velho      3408237
Name: area, dtype: int64
```

#### Construindo Objetos DataFrame

Um DataFrame Pandas pode ser construído de diversas maneiras, vejamos alguns exemplos interessantes

##### Através de uma lista de dicionários

Qualquer lista de dicionários pode ser feita em um DataFrame. Vamos usar compreensões de lista para criarmos dados

```python
dados = [{'a': i, 'b': 5 * i} for i in range(5)]
pd.DataFrame(dados)
```

Output:

|   | a | b  |
|---|---|----|
| 0 | 0 | 0  |
| 1 | 1 | 5  |
| 2 | 2 | 10 |
| 3 | 3 | 15 |
| 4 | 4 | 20 |

##### Através de um dicionário de ndarrays / listas

Os ndarrays devem todos ter o mesmo comprimento. Se um índice for passado, ele deve claramente ter o mesmo tamanho que os arrays. Se nenhum índice for passado, o resultado será um intervalo(n), onde **n** é o comprimento do array.

```python
d = {'um': [1, 3, 5, 7], 'dois': [6, 9, 11, 12]}
pd.DataFrame(d)
```

Output:

|   | um | dois |
|---|----|------|
| 0 | 1  | 6    |
| 1 | 3  | 9    |
| 2 | 5  | 11   |
| 3 | 7  | 12   |

##### Através de um Array Estruturado

```python
estruturado = np.zeros((2, ), dtype=[('A', 'i4'), ('B', 'f4'), ('C', 'a10')])
```

```python
estruturado[:] = [(1, 2., 'Hello'), (2, 3., "World")]
pd.DataFrame(estruturado)
```

Output:

|   | A | B   | C        |
|---|---|-----|----------|
| 0 | 1 | 2.0 | b'Hello' |
| 1 | 2 | 3.0 | b'World' |

Podemos alterar os índices de nosso **DataFrame**

```python
pd.DataFrame(estruturado, index=['Primeiro', 'Segundo'])
```

Output:

|          | A | B   | C        |
|----------|---|-----|----------|
| Primeiro | 1 | 2.0 | b'Hello' |
| Segundo  | 2 | 3.0 | b'World' |

Podemos alterar a ordem das colunas em nosso **DataFrame**

```python
pd.DataFrame(estruturado, columns=['C', 'A', 'B'])
```

Output:

|   | C        | A | B   |
|---|----------|---|-----|
| 0 | b'Hello' | 1 | 2.0 |
| 1 | b'World' | 2 | 3.0 |

##### Através de uma lista de Dicionários

```python
dados_2 = [{'a': 11, 'b': 22}, {'a': 53, 'b': 100, 'c': 23}]
```

Vejamos os valores impressos

```python
pd.DataFrame(dados_2)
```

Output:

|   | a  | b   | c    |
|---|----|-----|------|
| 0 | 11 | 22  | NaN  |
| 1 | 53 | 100 | 23.0 |

Alterando os índices:

```python
pd.DataFrame(dados_2, index=['primeiro', 'segundo'])
```

Output:

|          | a  | b   | c    |
|----------|----|-----|------|
| primeiro | 11 | 22  | NaN  |
| segundo  | 53 | 100 | 23.0 |

Selecionando apenas as colunas **a** e **b**

```python
pd.DataFrame(dados_2, columns=['a', 'b'])
```

Output:

|   | a  | b   |
|---|----|-----|
| 0 | 11 | 22  |
| 1 | 53 | 100 |

##### Através de uma lista de Tuplas

```python
pd.DataFrame({('a', 'b'): {('A', 'B'): 1, ('A', 'C'): 2},
               	('a', 'a'): {('A', 'C'): 3, ('A', 'B'): 4},
               	('a', 'c'): {('A', 'B'): 5, ('A', 'C'): 6},
               	('b', 'a'): {('A', 'C'): 7, ('A', 'B'): 8},
               	('b', 'b'): {('A', 'D'): 9, ('A', 'B'): 10}})
```

#### Indexando e Selecionando Dados em Series

Como já havíamos visto, um objeto Series atua de muitas maneiras como um array unidimensional NumPy e de muitas maneiras como um dicionário Python padrão. Se nós mantermos essas duas analogias sobrepostas em mente, fará com que seja mais fácil para entendermos os padrões para indexarmos e selecionarmos nesses arrays.

##### Series como um dicionário

Como um dicionário, o objeto Series fornece um mapeamento através de uma coleção de chaves para uma coleção de valores:

```python
dados_3 = pd.Series([0.33, 0.66, 0.55, 1.1], index=['a','b','c','d'])
dados_3
dados_3['b']
```

Output:
```
a    0.33
b    0.66
c    0.55
d    1.10
dtype: float64

0.66
```

Nós também podemos utilizar expressões Python do tipo dicionário e métodos para examinarmos as chaves/índices e valores:

```python
'a' in dados_3
dados_3.keys()
list(dados_3.items())
```

Output:
```
True

Index(['a', 'b', 'c', 'd'], dtype='object')

[('a', 0.33), ('b', 0.66), ('c', 0.55), ('d', 1.1)]
```

Objetos Series podem até mesmo serem modificados com sintáxe do tipo dicionário. Assim como você consegue extender um dicionário ao atribuir uma nova chave, você pode extender a Series ao atribuir para um novo valor de índice:

```python
dados_3['e'] = 1.77
dados_3
```

Output:
```
a    0.33
b    0.66
c    0.55
d    1.10
e    1.77
dtype: float64
```

##### Series como um array unidimensional

Uma série baseia-se nessa interface semelhante a um dicionário e fornece seleções de itens em estilo array através dos mesmos mecanismos básicos que os arrays NumPy - isto é, *slicing*, *masking* e indexação extravagante. Exemplos destes são os seguintes:

Slicing por índice explícito

```python
dados_3['a':'c'] 
```

Output:
```
a    0.33
b    0.66
c    0.55
dtype: float64
```

Slicing pelo índice inteiro implícito

```python
dados_3[0:2] 
```

Output:
```
a    0.33
b    0.66
dtype: float64
```

Utilizando **masking** para seleções específicas

```python
dados_3[(dados_3 > 0.3) & (dados_3 <= 0.8)] 
```

Output:
```
a    0.33
b    0.66
c    0.55
dtype: float64
```

Indexação 'Extravagante'

```python
dados_3[['a','e']] 
```

Output:
```
a    0.33
e    1.77
dtype: float64
```

##### Indexadores: loc e iloc

As convenções de divisão e indexação podem ser uma fonte de confusão. Por exemplo, se sua série tem um índice inteiro explícito, uma operação de indexação como dados[1] usa os índices explícitos, enquanto uma operação de *slicing* como dados [1:3] usará a Índice no estilo Python.

```python
dados_4 = pd.Series(['a','b','c'], index=[1,3,5])
dados_4
```

Output:
```
1    a
3    b
5    c
dtype: object
```

Índice explícito ao indexar

```python
dados_4[1] # 
```

Output:
```
'a'
```

Índice implícito ao slicing

```python
dados_4[1:3]
```

Output:
```
3    b
5    c
dtype: object
```

Devido a essa confusão potencial no caso de índices inteiros, o Pandas fornece alguns atributos especiais do indexador que explicitamente expõem certos esquemas de indexação. Estes não são métodos funcionais, mas atributos que expõem uma interface de *slicing* específica para os dados da Series.

Primeiro, o atributo **loc** permite a indexação e o *slicing* que sempre fazem referência ao índice:

```python
dados_4.loc[1]
dados_4.loc[1:3]
```

Output:
```
'a'

1    a
3    b
dtype: object
```

O atributo **iloc** permite a indexação e fatiamento que sempre faz referência ao implícito Índice de estilo Python:

```python
dados_4.iloc[1]
dados_4.iloc[1:3]
```

Output:
```
'b'

3    b
5    c
dtype: object
```

#### Seleção de Dados em um DataFrame

Lembre-se de que um DataFrame age de várias maneiras, como um array bidimensional ou estruturado, e de outras maneiras, como um dicionário de estruturas da Series que compartilha o mesmo índice. Essas analogias podem ser úteis para manter em mente à medida que exploramos a seleção de dados dentro esta estrutura.

##### DataFrame como um dicionário

A primeira analogia que consideraremos é o DataFrame como um dicionário de objetos Series relacionadas. Vamos voltar ao nosso exemplo de áreas e populações de cidades:

```python
area = pd.Series({'Florianopolis': 675409, 'Maceió': 511000 , 'Joinville': 1130878, 'Santa Maria': 277309, 'Porto Velho': 519531})
pop = pd.Series({'Florianopolis': 477798, 'Maceió': 996733, 'Joinville': 577077, 'Santa Maria': 277309, 'Porto Velho': 519531})
dados_cidades = pd.DataFrame({'area': area, 'pop': pop})
dados_cidades
```

Output:

|               | area    | pop    |
|---------------|---------|--------|
| Florianopolis | 675409  | 477798 |
| Maceió        | 511000  | 996733 |
| Joinville     | 1130878 | 577077 |
| Santa Maria   | 18231   | 277309 |
| Porto Velho   | 3408237 | 519531 |

As séries individuais que compõem as colunas do DataFrame podem ser acessadas via indexação em estilo de dicionário do nome da coluna:

```python
dados_cidades['area']
```

Output:
```
Florianopolis     675409
Maceió            511000
Joinville        1130878
Santa Maria        18231
Porto Velho      3408237
Name: area, dtype: int64
```

De forma equivalente, podemos utilizar o acesso estilo-atributo com nomes de colunas que são strings:

```python
dados_cidades.area
```

Output:
```
Florianopolis     675409
Maceió            511000
Joinville        1130878
Santa Maria        18231
Porto Velho      3408237
Name: area, dtype: int64
```

Como com os objetos Series discutidos anteriormente, essa sintaxe no estilo de dicionário também pode ser usado para modificar o objeto, neste caso, para adicionar uma nova coluna:

```python
dados_cidades['densidade'] = dados_cidades['pop'] / dados_cidades['area']
dados_cidades
```

Output:

|               | area    | pop    | densidade |
|---------------|---------|--------|-----------|
| Florianopolis | 675409  | 477798 | 0.707420  |
| Maceió        | 511000  | 996733 | 1.950554  |
| Joinville     | 1130878 | 577077 | 0.510291  |
| Santa Maria   | 18231   | 277309 | 15.210850 |
| Porto Velho   | 3408237 | 519531 | 0.152434  |

##### DataFrame como um array bidimensional

Como mencionado anteriormente, também podemos ver o DataFrame como um array bidimensional aperfeiçoado. Podemos examinar o array de dados subjacente bruto usando os valores atributo:

```python
dados_cidades.values
```

Output:
```
array([[6.75409000e+05, 4.77798000e+05, 7.07420245e-01],
       [5.11000000e+05, 9.96733000e+05, 1.95055382e+00],
       [1.13087800e+06, 5.77077000e+05, 5.10291119e-01],
       [1.82310000e+04, 2.77309000e+05, 1.52108497e+01],
       [3.40823700e+06, 5.19531000e+05, 1.52433942e-01]])
```

Com essa ideia em mente, nós podemos utilizar observações familiares de tipo-array no próprio DataFrame. Por exemplo, nós podemos aplicar *transpose* no DataFrame para trocarmos linhas e colunas

```python
dados_cidades.T
```

Output:

|           | Florianopolis | Maceió        | Joinville    | Santa Maria  | Porto Velho  |
|-----------|---------------|---------------|--------------|--------------|--------------|
| area      | 675409.00000  | 511000.000000 | 1.130878e+06 | 18231.00000  | 3.408237e+06 |
| pop       | 477798.00000  | 996733.000000 | 5.770770e+05 | 277309.00000 | 5.195310e+05 |
| densidade | 0.70742       | 1.950554      | 5.102911e-01 | 15.21085     | 1.524339e-01 |

Quando se trata de indexar um objeto DataFrame, entretanto, fica claro que a indexação estilo-dicionário de colunas impede nossa habilidade de simplesmente tratar ele como um array NumPy, em particular, passando um único índice para um array acessa a linha:

```python
dados_cidades.values[0]
```

Output:
```
array([6.75409000e+05, 4.77798000e+05, 7.07420245e-01])
```

Passando um único índice para o DataFrame, acessa a coluna:

```python
dados_cidades['area']
```

Output:
```
Florianopolis     675409
Maceió            511000
Joinville        1130878
Santa Maria        18231
Porto Velho      3408237
Name: area, dtype: int64
```

Sendo assim, para indexação no estilo array, precisamos de outra convenção. Aqui o Pandas usa novamente indexadores **loc** e **iloc** mencionados anteriormente. Usando o indexador **iloc**, podemos indexar o array subjacente como se fosse um simples array NumPy (usando o
Índice de estilo Python), mas os rótulos de índice e coluna DataFrame são mantidos no resultado:

```python
dados_cidades.iloc[:3, :2]
```

Output:

|               | area    | pop    |
|---------------|---------|--------|
| Florianopolis | 675409  | 477798 |
| Maceió        | 511000  | 996733 |
| Joinville     | 1130878 | 577077 |

```python
dados_cidades.loc[:'Florianopolis', :'pop']
```

Output:

|               | area   | pop    |
|---------------|--------|--------|
| Florianopolis | 675409 | 477798 |

##### Convenções Adicionais de Indexação

Existem algumas outras convenções de indexação que talvez pareçam estranhas, porém podem ser muito úteis na prática. Primeiro de tudo, enquanto que indexação se refere às colunas, *slicing* se refere às linhas:

```python
dados_cidades['Florianopolis':'Joinville']
```

Output:

|               | area    | pop    | densidade |
|---------------|---------|--------|-----------|
| Florianopolis | 675409  | 477798 | 0.707420  |
| Maceió        | 511000  | 996733 | 1.950554  |
| Joinville     | 1130878 | 577077 | 0.510291  |

```python
dados_cidades[1:5]
```

Output:

|             | area    | pop    | densidade |
|-------------|---------|--------|-----------|
| Maceió      | 511000  | 996733 | 1.950554  |
| Joinville   | 1130878 | 577077 | 0.510291  |
| Santa Maria | 18231   | 277309 | 15.210850 |
| Porto Velho | 3408237 | 519531 | 0.152434  |

```python
dados_cidades[dados_cidades.densidade > 1]
```

Output:

|             | area   | pop    | densidade |
|-------------|--------|--------|-----------|
| Maceió      | 511000 | 996733 | 1.950554  |
| Santa Maria | 18231  | 277309 | 15.210850 |

# Lendo Arquivos

Pandas nos oferece uma API de Input/Output que é um conjunto de funções *reader*, podemos citar por exemplo a muito utilizada função **pandas.read_csv()** que é capaz de ler arquivos **.csv**, normalmente essas funções nos retornaum um Objeto pandas. As correspondentes funções *writer* são objetos métodos que são acessados como **DataFrame.to_csv()**. Abaixo você pode ver a tabela com a lista de *readers* e *writers*. 

| Tipo de Formato | Descrição dos Dados         | Leitor         | Escritor     |
|-----------------|-----------------------------|----------------|--------------|
| texto           | CSV                         | read_csv       | to_csv       |
| texto           | JSON                        | read_json      | to_json      |
| texto           | HTML                        | read_html      | to_html      |
| texto           | Área de transferência local | read_clipboard | to_clipboard |
| binário         | Microsoft Excel             | read_excel     | to_excel     |
| binário         | Formato HDF5                | read_hdf       | to_hdf       |
| binário         | Formato Feather             | read_feather   | to_feather   |
| binário         | Formato Parquet             | read_parquet   | to_parquet   |
| binário         | Msgpack                     | read_msgpack   | to_msgpack   |
| binário         | Stata                       | read_stata     | to_stata     |
| binário         | SAS                         | read_sas       |              |
| binário         | Formato Python Pickle       | read_pickle    | to_pickle    |
| SQL             | SQL                         | read_sql       | to_sql       |
| SQL             | Google Big Query            | read_gbq       | to_gbq       |

Vejamos um exemplo de leitura utilizando um arquivo **.json** contendo uma lista *objetos países* com **nome** e **expectativa de vida**, vamos começar lendo nosso arquivo em uma DataFrame e vamos armazená-lo na variável **paises**

```python
paises = pd.read_json('https://raw.githubusercontent.com/samayo/country-json/master/src/country-by-life-expectancy.json')
type(paises)
```

Output:
```
pandas.core.frame.DataFrame
```

Vejamos as colunas do nosso DataFrame:

```python
paises.columns
```

Output:
```python
Index(['country', 'expectancy'], dtype='object')
```

Agora vamos armazenar em uma variável a coluna **expectancy**, esta que representa a expectativa de vida de cada país

```python
expectativa_de_vida = paises['expectancy']
type(paises['expectancy'])
```

Output:
```
pandas.core.series.Series
```

Podemos aplicar métodos como **mean()**, que retornará a **média** de expectativa de vida entre os países

```python
expectativa_de_vida.mean()
```

Output:
```
66.4023148148148
```

Vejamos agora um exemplo com um arquivo **.html**

```python
html = pd.read_html('https://www.contextures.com/xlSampleData01.html')
type(html)
type(html[0])
```

Observe que ele nos retornou uma lista contendo um **DataFrame**

Output:
```
list

pandas.core.frame.DataFrame
```

Podemos agora usar o método **head()** por exemplo

```python
html[0].head()
```

Output:

|   | 0         | 1       | 2       | 3      | 4     | 5        | 6      |
|---|-----------|---------|---------|--------|-------|----------|--------|
| 0 | OrderDate | Region  | Rep     | Item   | Units | UnitCost | Total  |
| 1 | 1/6/2018  | East    | Jones   | Pencil | 95    | 1.99     | 189.05 |
| 2 | 1/23/2018 | Central | Kivell  | Binder | 50    | 19.99    | 999.50 |
| 3 | 2/9/2018  | Central | Jardine | Pencil | 36    | 4.99     | 179.64 |
| 4 | 2/26/2018 | Central | Gill    | Pen    | 27    | 19.99    | 539.73 |

É possível percebermos que Pandas é muito flexível em relação à leitura de arquivos, tornando muito confortável para trabalharmos com diversos formatos e nos possibilitando até mesmo salvar arquivos em outros formatos. Veja:

```python
html[0].to_csv()
```

Output:
```
',0,1,2,3,4,5,6\n0,OrderDate,Region,Rep,Item,Units,UnitCost,Total\n1,1/6/2018,East,Jones,Pencil,95,1.99,189.05\n2,1/23/2018,Central,Kivell,Binder,50,19.99,999.50\n3,2/9/2018,Central,Jardine,Pencil,36,4.99,179.64\n4,2/26/2018,Central,Gill,Pen,27,19.99,539.73\n5,3/15/2018,West,Sorvino,Pencil,56,2.99,167.44\n6,4/1/2018,East,Jones,Binder,60,4.99,299.40\n7,4/18/2018,Central,Andrews,Pencil,75,1.99,149.25\n8,5/5/2018,Central,Jardine,Pencil,90,4.99,449.10\n9,5/22/2018,West,Thompson,Pencil,32,1.99,63.68\n10,6/8/2018,East,Jones,Binder,60,8.99,539.40\n11,6/25/2018,Central,Morgan,Pencil,90,4.99,449.10\n12,7/12/2018,East,Howard,Binder,29,1.99,57.71\n13,7/29/2018,East,Parent,Binder,81,19.99,1619.19\n14,8/15/2018,East,Jones,Pencil,35,4.99,174.65\n15,9/1/2018,Central,Smith,Desk,2,125.00,250.00\n16,9/18/2018,East,Jones,Pen Set,16,15.99,255.84\n17,10/5/2018,Central,Morgan,Binder,28,8.99,251.72\n18,10/22/2018,East,Jones,Pen,64,8.99,575.36\n19,11/8/2018,East,Parent,Pen,15,19.99,299.85\n20,11/25/2018,Central,Kivell,Pen Set,96,4.99,479.04\n21,12/12/2018,Central,Smith,Pencil,67,1.29,86.43\n22,12/29/2018,East,Parent,Pen Set,74,15.99,1183.26\n23,1/15/2019,Central,Gill,Binder,46,8.99,413.54\n24,2/1/2019,Central,Smith,Binder,87,15.00,1305.00\n25,2/18/2019,East,Jones,Binder,4,4.99,19.96\n26,3/7/2019,West,Sorvino,Binder,7,19.99,139.93\n27,3/24/2019,Central,Jardine,Pen Set,50,4.99,249.50\n28,4/10/2019,Central,Andrews,Pencil,66,1.99,131.34\n29,4/27/2019,East,Howard,Pen,96,4.99,479.04\n30,5/14/2019,Central,Gill,Pencil,53,1.29,68.37\n31,5/31/2019,Central,Gill,Binder,80,8.99,719.20\n32,6/17/2019,Central,Kivell,Desk,5,125.00,625.00\n33,7/4/2019,East,Jones,Pen Set,62,4.99,309.38\n34,7/21/2019,Central,Morgan,Pen Set,55,12.49,686.95\n35,8/7/2019,Central,Kivell,Pen Set,42,23.95,1005.90\n36,8/24/2019,West,Sorvino,Desk,3,275.00,825.00\n37,9/10/2019,Central,Gill,Pencil,7,1.29,9.03\n38,9/27/2019,West,Sorvino,Pen,76,1.99,151.24\n39,10/14/2019,West,Thompson,Binder,57,19.99,1139.43\n40,10/31/2019,Central,Andrews,Pencil,14,1.29,18.06\n41,11/17/2019,Central,Jardine,Binder,11,4.99,54.89\n42,12/4/2019,Central,Jardine,Binder,94,19.99,1879.06\n43,12/21/2019,Central,Andrews,Binder,28,4.99,139.72\n'
```

# Referencias

- [Pandas](https://pandas.pydata.org/)
- [Python Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/)
- [Beginner's Tutorial on the Pandas Python Library](https://stackabuse.com/beginners-tutorial-on-the-pandas-python-library/)
- [Python Pandas Tutorial: A Complete Introduction for Beginners](https://www.learndatasci.com/tutorials/python-pandas-tutorial-complete-introduction-for-beginners/)
- [Python Pandas Tutorial](https://www.tutorialspoint.com/python_pandas/)
- [Pandas Tutorial](https://data36.com/pandas-tutorial-1-basics-reading-data-files-dataframes-data-selection/)
- [Pandas Tutorial: DataFrames in Python](https://www.datacamp.com/community/tutorials/pandas-tutorial-dataframe-python)

---------------------------------------
