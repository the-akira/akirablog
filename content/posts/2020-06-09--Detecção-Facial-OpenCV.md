---
title: Detecção Facial com a Biblioteca OpenCV
date: "2020-09-06T22:40:32.169Z"
template: "post"
draft: false
slug: "/posts/face-detection/"
category: "Científico"
tags:
  - "Programação"
  - "Científico"
  - "Machine Learning"
description: "Neste breve guia vamos explorar o conceito de Computer Vision com a biblioteca OpenCV, assim nos possibilitando, detectar faces em uma fotografia."
---

## Computer Vision

**Computer Vision**, também chamado de [Visão Computacional](https://en.wikipedia.org/wiki/Computer_vision) é um campo científico interdisciplinar que trata de como os computadores podem obter um entendimento de alto nível de imagens ou vídeos digitais. Do ponto de vista da engenharia, busca entender e automatizar tarefas que o sistema visual humano pode realizar.

A visão computacional envolve ver ou sentir um estímulo visual, dar sentido ao que foi visto e também extrair informações complexas que podem ser usadas para outras atividades de *Machine Learning*.

### Aplicações de Computer Vision

Existem diversas aplicações práticas da Visão Computacional, podemos citar por exemplo:

- **Veículos Autônomos**: Esta é uma das aplicações mais importantes da visão computacional, na qual os carros autônomos precisam reunir informações sobre o ambiente ao redor para decidir como se comportar.
- **Reconhecimento Facial**: Esta também é uma aplicação muito importante da visão computacional, onde a eletrônica usa tecnologia de reconhecimento facial para basicamente validar a identidade de um usuário.
- **Pesquisa de Imagens e Reconhecimento de Objetos**: Atualmente podemos pesquisar objetos em uma imagem usando a pesquisa de imagens. Um bom exemplo é o [Google Lens](https://lens.google.com/), onde podemos pesquisar um objeto específico dentro da imagem clicando na foto da imagem e o algoritmo de visão computacional pesquisará no catálogo de imagens e extrairá informações da imagem.
- **Robótica**: A maioria das máquinas robóticas, geralmente na fabricação, precisa ver os arredores para realizar uma tarefa em questão. Na fabricação, as máquinas podem ser usadas para inspecionar as tolerâncias de montagem “olhando para elas”.

Agora que sabemos o significado de *Computer Vision* e algumas de suas aplicações, veremos um exemplo de sua implementação com o auxílio da [biblioteca OpenCV](https://opencv-python-tutroals.readthedocs.io/en/latest/index.html).

### OpenCV

**OpenCV** (Open Source Computer Vision Library: https://opencv.org) é uma biblioteca licenciada-BSD de código aberto que inclui diversos algoritmos de visão computacional.

Neste exemplo vamos usar o **Haar Cascade Classifiers**, que é uma abordagem eficaz de detecção de objetos que foi proposta por Paul Viola e Michael Jones no artigo, *“Rapid Object Detection using a Boosted Cascade of Simple Features”* em 2001.

Esta é basicamente uma abordagem baseada em *Machine Learning* em que uma função em cascata é treinada a partir de várias imagens positivas e negativas. Com base no treinamento, ele é usado para detectar os objetos nas outras imagens.

Então, como isso funciona? Eles são enormes arquivos `.xml` individuais com muitos conjuntos de *features* e cada xml corresponde a um tipo muito específico de caso de uso.

Por exemplo, se formos até a página do [GitHub](https://github.com/opencv/opencv/tree/master/data/haarcascades) do Haarcascade, veremos que existe um arquivo **xml** contendo o conjunto de *features* para detectar: [Corpo Inteiro](https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_fullbody.xml), [Olho](https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_eye.xml), [Face Frontal](https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_frontalface_alt.xml) e diversos outros.

Para este exemplo específico, vamos trabalhar com a detecção da face frontal.

#### Detecção da Face Frontal

Para que possamos fazer a detecção de faces frontais, precisamos do arquivo **xml** respectivo, você pode fazer o download dele neste **[Repositório](https://github.com/the-akira/python-experimentos/blob/master/OpenCV/haarcascade_frontalface_alt.xml)**.

Uma maneira muito simples de obter este arquivo é com a ferramenta **wget**:

```
wget https://raw.githubusercontent.com/the-akira/python-experimentos/master/OpenCV/haarcascade_frontalface_alt.xml
```

Agora precisamos de uma imagem para trabalhar, neste exemplo irei usar a famosa imagem da [Quinta Solvay Conference](https://en.wikipedia.org/wiki/Solvay_Conference) sobre elétrons e fótons, realizada de 24 a 29 de outubro de 1927, onde os físicos mais notáveis do mundo se reuniram para discutir a recém-formulada teoria quântica.

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/OpenCV/f%C3%ADsicos.jpg)

Uma vez que temos nossa imagem selecionada, devemos então instalar a biblioteca **[OpenCV-Python](https://pypi.org/project/opencv-python/)** com o comando:

```
pip install opencv-python
```

Podemos agora importar ela em nosso código e também usar a função **CascadeClassifier()** do OpenCV para apontar a localização do arquivo **xml** que fizemos download, no meu caso ele está nomeado como `haarcascade_frontalface_alt.xml`.

```python
import cv2

GREEN = (0,255,0)

faces = cv2.CascadeClassifier("haarcascade_frontalface_alt.xml")
```

Observe também que eu defini uma variável chamada de **GREEN**, que representará a cor do retângulo de detecção de faces em BGR, sinta-se livre para escolher a cor que quiser.

Devemos agora carregar a imagem que vamos usar e converter ela em [escala de cinza](https://en.wikipedia.org/wiki/Grayscale).

Geralmente as imagens que vemos estão na forma de canal RGB (Vermelho, Verde, Azul). Portanto, quando o OpenCV lê a imagem RGB, ele geralmente armazena a imagem no canal BGR (Azul, Verde, Vermelho). Para fins de reconhecimento de imagem, precisamos converter este canal BGR para canal cinza. A razão para isso é que o canal cinza é fácil de processar e é computacionalmente menos intensivo, pois contém apenas 1 canal de preto e branco.

```python
image = cv2.imread("físicos.jpg")
image_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

Neste caso os argumentos para a função **cvtColor()** serão o nome da variável da imagem e **COLOR_BGR2GRAY**.

Agora, depois de converter a imagem de RGB para cinza, vamos tentar localizar as características exatas dos rostos.

```python
detections = faces.detectMultiScale(image_gray, scaleFactor=1.1, minNeighbors=6)
```

Nesta linha de código, o que estamos tentando fazer é: usar o objeto **faces** carregado com `haarcascade_frontalface_default.xml`, usando um método embutido nele chamado de **detectMultiScale()**.

Este método nos ajudará a encontrar as características/localizações da nova imagem. Ele usará todos os recursos do objeto **faces** para detectar os recursos da nova imagem.

Os argumentos que estamos passando para este método são respectivamente:

- **image_gray**: Nossa imagem em escala de cinza.
- **scaleFactor**: Especifica quanto o tamanho da imagem é reduzido em cada escala de imagem. Basicamente, o fator de escala é usado para criar sua pirâmide de escala. O modelo possui um tamanho fixo definido durante o treinamento, que é visível no XML. Isso significa que o tamanho do rosto é detectado na imagem, se houver, no entanto, redimensionamento da imagem de *input*, é possível redimensionar uma face maior para uma menor, tornando-a detectável pelo algoritmo.
- **minNeighbors**: Especifica quantos vizinhos cada retângulo candidato deve ter para retê-lo. Este parâmetro afetará a qualidade dos rostos detectados. Valores mais altos resultam em menos detecções, mas com qualidade superior. Normalmente `3 ~ 6` é um bom valor.

O método **detectMultiScale** retornará 4 valores: 

- coordenada x 
- coordenada y
- largura (w)
- altura (h) 

Respectivos do recurso detectado do rosto. Com base nesses 4 valores, desenharemos um retângulo ao redor da face.

```python
for (x, y, w, h) in detections:
	cv2.rectangle(image, (x,y), (x+w,y+h), GREEN, 2)
```

Nosso código final ficará então da seguinte forma:

```python
import cv2

GREEN = (0,255,0)

faces = cv2.CascadeClassifier("haarcascade_frontalface_alt.xml")
image = cv2.imread("físicos.jpg")
image_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
detections = faces.detectMultiScale(image_gray, scaleFactor=1.1, minNeighbors=6)

for (x, y, w, h) in detections:
	cv2.rectangle(image, (x,y), (x+w,y+h), GREEN, 2)

cv2.imshow("output", image)
cv2.waitKey(0)
```

Ele nos apresentará a seguinte imagem como resultado:

![img](https://raw.githubusercontent.com/the-akira/PythonExperimentos/master/OpenCV/f%C3%ADsicos_detec%C3%A7%C3%A3o.png)

Como podemos observar, fomos capazes de detectar todas as faces da imagem. Com este conhecimento você pode agora experimentar a detecção de outros objetos e até mesmo utilizar outros algoritmos.

Bons estudos!

## Referências

- [Computer Vision](https://en.wikipedia.org/wiki/Computer_vision)
- [Computer-Vision-Tutorial](https://github.com/krishnaik06/Computer-Vision-Tutorial)
- [Introduction to OpenCV-Python Tutorials](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_setup/py_intro/py_intro.html#intro)
- [Detecting objects using Haar Cascade Classifier](https://towardsdatascience.com/computer-vision-detecting-objects-using-haar-cascade-classifier-4585472829a9)
