---
title: Carregando e Manipulando Imagens em Python
date: "2020-04-16T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/manipulando-imagens-python/"
category: "Fundamentos"
tags:
  - "Programação"
  - "Python"
description: "Neste tutorial vamos aprender como carregar e manipular imagens em Python, com o auxílio das poderosas bibliotecas: NumPy, Matplotlib, Pillow e OpenCV."
---

## Introdução

Manipulação de imagem refere-se ao processo de trazer alterações a uma imagem digitalizada, de forma a transformá-la em uma imagem desejada.

A linguagem Python conta com diversas bibliotecas que nos permitem trabalhar com imagens de forma intuitiva e eficiente.

## Conhecendo e Instalando as Bibliotecas

Nesta etapa vamos conhecer e instalar as bibliotecas que utilizaremos em nosso tutorial.

### A Biblioteca Pillow

A biblioteca padrão mais popular em Python para carregar e trabalhar com dados de imagem é o [Pillow](https://pillow.readthedocs.io/en/stable/). 

Pillow é uma versão atualizada da [Python Image Library](https://www.pythonware.com/products/pil/), também conhecida como PIL, e suporta uma variedade de funcionalidades simples e sofisticadas de manipulação de imagens.

#### Instalação

Utilizaremos **[pip](https://pypi.org/project/pip/)** para instalar a biblioteca Pillow, para isso podemos executar o seguinte comando:

```
pip install Pillow
```

O seguinte código nos confirma se a instalação ocorreu com sucesso e nos informa a versão atual da biblioteca Pillow

```python
import PIL
print(f'Versão Pillow: {PIL.__version__}')
```

Ao executá-lo ele irá me reportar: `Versão Pillow: 6.2.0`.

### A Biblioteca OpenCV-Python

O OpenCV foi iniciado na Intel em 1999 por **Gary Bradsky** e o primeiro lançamento ocorreu em 2000. **Vadim Pisarevsky** se uniu a Gary Bradsky para gerenciar a equipe OpenCV de software russa da Intel. 

Em 2005, o OpenCV foi usado no Stanley, o veículo que venceu o 2005 [DARPA Grand Challenge](https://en.wikipedia.org/wiki/DARPA_Grand_Challenge_(2005)). Mais tarde, seu desenvolvimento ativo continuou com o apoio da Willow Garage, com Gary Bradsky e Vadim Pisarevsky liderando o projeto. No momento, o OpenCV suporta muitos algoritmos relacionados a Computer Vision e Machine Learning e está se expandindo dia a dia.

Atualmente, o OpenCV suporta uma ampla variedade de linguagens de programação como C++, Python, Java, entre outras.

**[OpenCV-Python](https://opencv-python-tutroals.readthedocs.io/)** é a API Python do OpenCV. Ela combina as melhores qualidades da API OpenCV C++ e da linguagem Python.

A Biblioteca OpenCV-Python conta com operações que vão nos auxiliar na manipulação e processamento de imagens.

#### Instalação

Novamente vamos instalar a biblioteca Opencv-Python utilizando o **pip**:

```
pip install opencv-python
```

Executamos o seguinte comando em nosso terminal para checar se a instalação ocorreu com sucesso:

```
python -c 'import cv2; print(cv2.__version__);'
```

Ele nos irá reportar a versão `4.2.0`. Caso você esteja utilizando uma máquina Windows e ocorra algum problema, você pode visitar **[este guia](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_setup/py_setup_in_windows/py_setup_in_windows.html)**.

### A Biblioteca Matplotlib

[Matplotlib](https://matplotlib.org/3.1.1/index.html) é uma biblioteca Python de plotagem 2D capaz de produzir figuras de qualidade para publicação em uma variedade de formatos de cópia impressa e ambientes interativos entre plataformas.

Matplotlib nos permite carregar, manipular e plotar imagens.

#### Instalação

O Matplotlib e suas dependências estão disponíveis como pacotes para distribuições do macOS, Windows e Linux, vamos instalá-los com o seguinte comando:

```
pip install matplotlib
```

Para testarmos se a instalação ocorreu com sucesso, podemos executar:

```
python -c 'import matplotlib; print(help(matplotlib));'
```

Como output ele nos trará o manual de instruções de matplotlib.

### A Biblioteca NumPy

NumPy é o pacote fundamental para a computação científica em Python e conta com um poderoso **objeto array de N-dimensões**.

Frequentemente, em Machine Learning, desejamos trabalhar com imagens como arrays NumPy de dados em pixels.

#### Instalação

Também é possível instalar NumPy através do **pip**:

```
pip install numpy
```

Verificando se a instalação ocorreu com sucesso:

```
python -c 'import numpy as np; print(np.__version__);'
```

O código acima nos trará como output a seguinte versão: `1.17.2`.

## Tutorial

Uma vez que todas as bibliotecas necessárias foram instaladas e já conhecemos um pouco sobre elas, é o momento de selecionarmos imagens para manipularmos: 

![img](https://arquivos.netlify.app/images/alchemy.jpg)

---

![img](https://arquivos.netlify.app/images/buddhism.jpg)

---

![img](https://arquivos.netlify.app/images/creation.jpg)

---

![img](https://arquivos.netlify.app/images/japanese.png)

---

![img](https://arquivos.netlify.app/images/hal9000.jpg)

Salvarei todas as imagens em um diretório chamado `images` que ficará dentro do mesmo diretório de nossos scripts experimentais. Sinta-se livre para escolher suas imagens favoritas.

### Carregando e Apresentando Imagens com Pillow

As imagens geralmente estão no formato **PNG** ou **JPEG** e podem ser carregadas diretamente usando a função `open()` da classe `Image`. 

Essa operação nos retorna um objeto de imagem que contém os dados de pixel da imagem, além de detalhes sobre a imagem.

```python
from PIL import Image

# Carregando a imagem
imagem = Image.open('images/creation.jpg')

# Sumarizando detalhes sobre a imagem
# Modo da imagem
print(imagem.mode)
# Formato da imagem
print(imagem.format)
# Tamanho da imagem (largura, altura)
print(imagem.size)

# Apresentando a imagem
imagem.show()
```

O código irá exibir a seguinte imagem:

![img](https://arquivos.netlify.app/images/creation.jpg)

E nos trará como output em nosso console:

```
RGB
JPEG
(932, 310)
```

De forma que:

- **RGB** é o modo, em outras palavras, o formato de [canal de pixel](https://en.wikipedia.org/wiki/Channel_(digital_image)).
- **JPEG** é o formato da imagem que carregamos.
- **(932, 310)** são as dimensões da imagem.

### Carregando, Apresentando e Manipulando Imagens com Matplotlib

A função `mpimg` do módulo `matplotlib.image` nos permite carregar uma imagem como um **numpy.ndarray** **3D**, sendo cada dimensão um canal de cor, **RED**, **GREEN**, **BLUE** e cada lista interna representa um pixel.

```python
from matplotlib import pyplot as plt
import matplotlib.image as mpimg

# Carrega a imagem como um array de pixels
data = mpimg.imread('images/japanese.png')
# Imprime os arrays de dados
print(data)
# Imprime o tipo 
print(type(data))

# Dados sobre o array de pixels
# Tipo de dados
print(data.dtype)
# Dimensão de dados
print(data.shape)

# Apresenta o array de pixels como uma imagem
lum_img = data[:, :, 0]
imgplot = plt.imshow(lum_img, cmap='twilight')
plt.axis('off')
plt.show()
``` 

O código retornará em nosso console o seguinte output:

```
[[[0.9254902  0.9490196  0.89411765]
  [0.9254902  0.9529412  0.8901961 ]
  [0.93333334 0.9490196  0.8901961 ]
  ...
  [0.9372549  0.9490196  0.9137255 ]
  [0.9372549  0.9490196  0.9137255 ]
  [0.93333334 0.94509804 0.91764706]]
  ...
 [[0.9254902  0.9490196  0.9019608 ]
  [0.9254902  0.9490196  0.9019608 ]
  [0.9254902  0.9490196  0.9019608 ]
  ...
  [0.9372549  0.9607843  0.9137255 ]
  [0.92156863 0.94509804 0.90588236]
  [0.91764706 0.9411765  0.9019608 ]]]
<class 'numpy.ndarray'>
float32
(691, 470, 3)
```

E nos apresentará a imagem modificada, uma vez que alteramos o `cmap='twilight'`.

![img](https://arquivos.netlify.app/images/japanesetwilight.png)

Para que possamos fazer essa alteração, foi necessário a seleção de apenas um canal de nossos dados `lum_img = data[:, :, 0]`.

### Carregando, Apresentando e Manipulando Imagens com Matplotlib, NumPy e Pillow

Neste exemplo vamos carregar uma imagem com a função `open()` e imediatamente utilizaremos a função `convert('L')` para convertermos a imagem em Preto e Branco, em seguida usamos a função `np.array()` para carregarmos a imagem como um **numpy.ndarray**.

```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image

imagem = np.array(Image.open('images/hal9000.jpg').convert("L"))
imagem[100:120] = 188
imagem[500:520] = 188

lx, ly = imagem.shape
print(lx,ly)

X, Y = np.ogrid[0:lx, 0:ly]
mask = (X - lx / 2) ** 2 + (Y - ly / 2) ** 2 > lx * ly / 4
imagem[mask] = 188

imagem[range(lx), range(ly)] = 255

plt.imshow(imagem, cmap='gray')
plt.show()
```

Observe que estamos capturando as dimensões da imagem através das variáveis `lx` e `ly`, que nos trará os valores `630 630`, dimensão de nossa imagem carregada.

Nas linhas a seguir estamos aplicando um valor de cinza arbitrário **188** nos valores selecionados do array imagem, criando assim duas linhas horizontais em nossa imagem, uma superior e outra inferior.

```python
imagem[100:120] = 188
imagem[500:520] = 188
```

Em seguida calculamos uma mascará circular e à aplicamos em nosso array imagem, também estamos aplicando uma linha diagonal branca(**255**) em nossa imagem.

```python
X, Y = np.ogrid[0:lx, 0:ly]
mask = (X - lx / 2) ** 2 + (Y - ly / 2) ** 2 > lx * ly / 4
imagem[mask] = 188

imagem[range(lx), range(ly)] = 255
```

Nos será então apresentada a seguinte imagem:

![img](https://arquivos.netlify.app/images/halgrayscale.png)

Uma vez que não desabilitamos os eixos **X** e **Y**, é possível vermos a imagem como um gráfico de pixels.

### Dividindo os Canais da Imagem (RED, GREEN, BLUE)

Neste script vamos carregar nossa imagem com a função `imread()` da biblioteca OpenCV.

Em seguida imprimimos o **array de dados**, **seu tipo** e **número de dimensões**.

Para dividir a imagem em seus três canais, basta chamar a função `split()` do módulo **cv2**, passando como entrada nossa imagem original, esta função retornará uma lista com os três canais. Cada canal é representado como um **ndarray** de **2** Dimensões

Em seguida criamos um array único para cada cor, com a ajuda de um “canal vazio” com a chamada da função `zeros()` do módulo **numpy**. Isso retornará um novo **ndarray** com a forma especificada e preenchida com zeros.

Para criar uma imagem RGB a partir de três canais separados, basta chamarmos a função de `merge()`, passando como entrada uma tupla com os canais.

Portanto, para criar a imagem RGB a partir do canal Blue, por exemplo, o primeiro elemento da tupla deve ser o canal Blue e os demais devem ser o "canal vazio". Consideramos então o script:

```python
import numpy as np
import cv2

img = cv2.imread('images/alchemy.jpg')
print(img)
print(img.dtype)
print(img.ndim)

blue, green, red = cv2.split(img)
zeros = np.zeros(blue.shape, np.uint8)
blueBGR = cv2.merge((blue,zeros,zeros))
greenBGR = cv2.merge((zeros,green,zeros))
redBGR = cv2.merge((zeros,zeros,red))

cv2.imshow('imagem',img)
cv2.imshow('blue BGR',blueBGR)
cv2.imshow('green BGR',greenBGR)
cv2.imshow('red BGR',redBGR)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

Além da imagem original, nos será apresentado uma versão de cada cor

**RED**

![img](https://arquivos.netlify.app/images/red.jpg)

**GREEN**

![img](https://arquivos.netlify.app/images/green.jpg)

**BLUE**

![img](https://arquivos.netlify.app/images/blue.jpg)

### Histograma de uma Imagem

Você pode considerar o histograma como um gráfico que fornece uma idéia geral sobre a distribuição de intensidade de uma imagem. 

É um gráfico com valores de pixel (variando de **0** a **255**, nem sempre) no **eixo X** e número correspondente de pixels na imagem no **eixo Y**.

É apenas outra maneira de entender a imagem. Ao olhar para o histograma de uma imagem, obtemos intuição sobre contraste, brilho, distribuição de intensidade dessa imagem.

Neste exemplo, usamos a função `cv.calcHist()` para encontrar o histograma, esta que recebe os seguintes parâmatros:

1. **images**: é a imagem de origem do tipo *uint8* ou *float32*. deve ser fornecido entre colchetes, ou seja, "[img]".
2. **channels**: também é fornecido entre colchetes. É o índice do canal para o qual calculamos o histograma. Por exemplo, se a entrada for uma imagem em escala de cinza, seu valor será [0]. Para imagens coloridas, você pode passar [0], [1] ou [2] para calcular o histograma dos canais azul, verde ou vermelho, respectivamente.
3. **mask**: mascarar imagem. Para encontrar o histograma da imagem completa, é fornecido como "None". Mas se você deseja encontrar o histograma de determinada região da imagem, é necessário criar uma imagem de máscara para isso e fornecê-la como máscara. 
4. **histSize**: representa nossa contagem de BIN. Precisa ser fornecido entre colchetes. Para escala completa, passamos [256].
5. **ranges**: essa é o RANGE. Normalmente é [0,256].

A seguir temos o código que vamos executar

```python
from matplotlib import pyplot as plt
import cv2

plt.style.use('classic')

img = cv2.imread('images/buddhism.jpg')
color = ('b','g','r')

for i,col in enumerate(color):
    histr = cv2.calcHist([img],[i],None,[256],[0,256])
    plt.plot(histr,color=col,lw=2)
    plt.xlim([0,256])
plt.grid()
plt.show()
```

O script nos apresentará o seguinte gráfico para a imagem selecionada:

![img](https://arquivos.netlify.app/images/grafico.png)

Com o gráfico dessa imagem específica podemos observar que o AZUL tem valores elevados em algumas regiões da imagem.

### Transformando a Perspectiva com OpenCV

Para transformação de perspectiva, vamos precisar de uma matriz de transformação 3x3. As linhas retas permanecerão retas, mesmo após a transformação. 

Para encontrar essa matriz de transformação, precisamos de 4 pontos na imagem de entrada e pontos correspondentes na imagem de saída. Entre esses 4 pontos, 3 deles não devem ser colineares. 

Em seguida, a matriz de transformação pode ser encontrada pela função `cv2.getPerspectiveTransform()`. Por fim, aplicamos `cv2.warpPerspective()` com essa matriz de transformação 3x3.

Vejamos o script de exemplo

```python
import matplotlib.pyplot as plt
import numpy as np
import cv2

img = cv2.imread('images/creation.jpg')
rows,cols,ch = img.shape

pts1 = np.float32([[390,5],[750,5],[505,265],[885,265]])
pts2 = np.float32([[0,0],[310,0],[0,310],[350,310]])

M = cv2.getPerspectiveTransform(pts1,pts2)
dst = cv2.warpPerspective(img,M,(350,310))

plt.subplot(121),plt.imshow(img),plt.title('Input')
plt.subplot(122),plt.imshow(dst),plt.title('Output')
plt.show()
```

O resultado será

![img](https://arquivos.netlify.app/images/creationperspective.png)

### Redimensionando Imagens

Redimensionamento é uma tarefa muito comum ao trabalharmos com imagens.

Às vezes, é desejável que todas as imagens em miniatura tenham a mesma largura ou altura. Isso pode ser alcançado com Pillow usando a função `thumbnail()`. 

A função usa uma tupla com a **largura** e a **altura**, e a imagem será redimensionada para que a largura e a altura da imagem sejam iguais ou menores que a forma especificada.

Por exemplo, a imagem de teste com a qual vamos trabalhar no script a seguir, tem largura e altura de **(932, 310)**. Podemos redimensioná-lo para **(400, 200)**, a maior dimensão, neste caso, a largura, será reduzida para **400** e a altura será redimensionada para manter a proporção da imagem, por este motivo nossa altura resultante será **133**.

```python
from PIL import Image

# Carrega a imagem
image = Image.open('images/creation.jpg')
# cria um thumbnail e preserva o aspect ratio
image.thumbnail((400,200))
# reporta o tamanho do thumbnail
print(image.size)

image.save('images/creation.png',format='PNG')
```

Nos será apresentado o seguinte resultado.

![img](https://arquivos.netlify.app/images/creation.png)

Observe também que optamos por salvar a imagem no formato **PNG**.

É possível que não desejemos preservar a proporção e, em vez disso, forçamos os pixels a uma nova forma.

Isso pode ser obtido usando a função `resize()` que permite especificar a largura e a altura em pixels e a imagem será reduzida ou esticada para se ajustar à nova forma.

O exemplo abaixo demonstra como redimensionar uma nova imagem e ignorar a proporção original

```python
from PIL import Image

# Carrega a imagem
image = Image.open('images/creation.jpg')
print(image.size)
# altera o tamanho da imagem e ignora o aspect ratio original
img_resized = image.resize((400,200))
# reporta o tamanho do thumbnail
print(img_resized.size)

img_resized.save('images/creation_resized.jpg')
```

Nos será trazido o seguinte resultado

![img](https://arquivos.netlify.app/images/creationresized.jpg)

Observe que agora a altura de nossa imagem será exatamente **200** pixels.

### Girando uma Imagem

Uma imagem pode ser girada chamando a função `transpose()` e passando um método como `FLIP_LEFT_RIGHT` para um giro horizontal ou `FLIP_TOP_BOTTOM` para um giro vertical.

O exemplo a seguir cria versões de giros horizontais e verticais da imagem carregada

```python
from matplotlib import pyplot
from PIL import Image

# Carregando a imagem
image = Image.open('images/creation.png')
# Giro horizontal
hoz_flip = image.transpose(Image.FLIP_LEFT_RIGHT)
# Giro vertical
ver_flip = image.transpose(Image.FLIP_TOP_BOTTOM)

# Projetando imagens
pyplot.subplot(311)
pyplot.imshow(image)
pyplot.subplot(312)
pyplot.imshow(hoz_flip)
pyplot.subplot(313)
pyplot.imshow(ver_flip)
pyplot.show()
```

Nos será apresentado o seguinte gráfico

![img](https://arquivos.netlify.app/images/creationflip.png)

### Rotacionando uma Imagem

Podemos utilizar a função `rotate()` para rotacionar imagens, passando o ângulo de rotação desejada como argumento.

Vejamos um simples exemplo

```python
from PIL import Image

# Carregando a imagem
imagem = Image.open('images/creation.jpg')

# Rotacionando a imagem em 13 graus
img_rotacao = imagem.rotate(13)

# Apresentando a imagem
img_rotacao.show()
```

Será aplicada uma rotação de 13 graus, nos trazendo o seguinte resultado

![img](https://arquivos.netlify.app/images/creationrotation.jpg)

### Cortando uma Imagem e Aplicando Gaussian Blur

Uma imagem pode ser cortada: em outras palavras, um pedaço pode ser cortado para criar uma nova imagem, usando a função `crop()`. A função `crop()` recebe como argumento uma tupla que define as duas coordenadas **x**/**y** da caixa para cortar a imagem.

Por exemplo, se nossa imagem é de 1000 por 1000 pixels, podemos recortar uma caixa de 100 por 100 no meio da imagem, definindo uma tupla com os pontos superior esquerdo e inferior direito de (450, 450, 550, 550)

Vejamos um exemplo para ilustrar a ideia

```python
from PIL import Image, ImageFilter

# Carregando a imagem
image = Image.open('images/japanese.png')
# Cria uma imagem cortada
cropped = image.crop((55, 60, 320, 400))
# Aplica o GaussianBlur
img_filter = cropped.filter(ImageFilter.GaussianBlur(1.3))
# Mostra a imagem
img_filter.show()
```

Obteremos o seguinte output

![img](https://arquivos.netlify.app/images/japanesecropblur.png)

Observe também que estamos aplicando um filtro **[Gaussian blur](https://en.wikipedia.org/wiki/Gaussian_blur)** que é o resultado de desfocar uma imagem por uma função Gaussiana(em homenagem ao matemático e cientista [Carl Friedrich Gauss](https://en.wikipedia.org/wiki/Carl_Friedrich_Gauss)). Este é um efeito amplamente usado em software gráfico, geralmente usado para reduzir o ruído da imagem e os seus detalhes.

### Detectação de Edge

A [detecção de edges](https://en.wikipedia.org/wiki/Edge_detection) é uma técnica de processamento de imagem para encontrar limites de objetos na imagem. Os pontos nos quais o brilho da imagem muda muito são tipicamente organizados em um conjunto de segmentos de linhas curvas denominados **edges**.

O Canny Edge Detection é um algoritmo popular de detecção de edges. Foi desenvolvido por John F. Canny em 1986. Ele é um algoritmo de vários estágios, você poder ler mais detalhes sobre ele nesse [link](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_canny/py_canny.html)

A biblioteca OpenCV nos fornece uma função `Canny()`, utilizada para detecção de edges.

Iremos aplicá-la na imagem que carregaremos e em seguida vamos redimensioná-la, vejamos o script de exemplo

```python
import cv2
 
# Carregamos a imagem
img = cv2.imread('images/hal9000.jpg')

# Utilizamos a função Canny() para detectar edges
edges = cv2.Canny(img,100,200)

COLOR = [102,102,153]

scale_percent = 40 # percentagem do tamanho original
width = int(edges.shape[1] * scale_percent / 100)
height = int(edges.shape[0] * scale_percent / 100)
dim = (width, height)

# redimensiona a imagem
resized = cv2.resize(edges, dim, interpolation=cv2.INTER_AREA)

# adiciona borda a imagem
image = cv2.copyMakeBorder(resized, 10, 10, 10, 10,cv2.BORDER_CONSTANT,value=COLOR) 
 
# salva a imagem modificada
cv2.imwrite('images/haledge.jpg', image) 
```

Teremos como output a seguinte imagem modificada

![img](https://arquivos.netlify.app/images/haledge.jpg)

Observe que também aplicamos uma borda na imagem através da função `copyMakeBorder()`.

## Conclusão

Através desse tutorial fomos capazes de experimentar diversas funcionalidades das bibliotecas Pillow, OpenCV-Python, Matplotlib e NumPy, que tornam a manipulação de imagens com Python muito mais simples e intuitiva. 

Especificamente, aprendemos:

- Instalação das bibliotecas necessárias
- Carregamento de imagens
- Manipulações como: redimensionamento, rotações, giros, corte, gaussian blur, conversões
- Salvar as imagens em formatos diferentes

Para maiores detalhes e guias você pode consultar as referências do tutorial.

Tenha um excelente dia, ou noite!

## Referências

- [Python Imaging Library](https://docs.python-guide.org/scenarios/imaging/)
- [Image manipulation and processing using Numpy and Scipy](https://scipy-lectures.org/advanced/image_processing/)
- [How to Load and Manipulate Images for Deep Learning](https://machinelearningmastery.com/how-to-load-and-manipulate-images-for-deep-learning-in-python-with-pil-pillow/)
- [Chapter 17 – Manipulating Images](https://automatetheboringstuff.com/chapter17/)
- [Image Manipulation in Python](https://www.codementor.io/@isaib.cicourel/image-manipulation-in-python-du1089j1u)
- [Python OpenCV: Splitting image channels](https://techtutorialsx.com/2020/03/02/python-opencv-splitting-image-channels/)
- [Tutorial - Pillow](https://pillow.readthedocs.io/en/stable/handbook/tutorial.html)
- [Image Processing in Python with Pillow](https://auth0.com/blog/image-processing-in-python-with-pillow)
- [Image Edge Detection](https://www.tutorialkart.com/opencv/python/image-edge-detection/)
- [Histograms - 1 : Find, Plot, Analyze !!!](https://docs.opencv.org/master/d1/db7/tutorial_py_histogram_begins.html)
- [OpenCV-Python Tutorials](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_tutorials.html)
