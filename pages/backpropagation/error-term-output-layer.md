---
layout: post
title: Error term en la capa de salida/final
permalink: /notes/backpropagation/error-term-output-layer/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

El error term en nodos de la capa de salida, $$ \delta_{j}^{k} $$, mide el impacto que tiene un nodo de la capa final en el error cometido, cuando hay una pequeña variación en su entrada:

$$ \delta_{j}^{k} = \frac{\partial C}{\partial a_j^{k}} = \\ $$

En la capa final conocemos el error cometido, como la diferencia cuadrática entre lo esperado y lo obtenido por la salida de la red neuronal, ergo es posible resolver la derivada parcial con respecto a la entrada que alimenta el nodo, como sigue:

$$ \left. \frac{\partial C}{\partial a_j^{k}} \right\rvert_{C_{j}^{k} = \frac{1}{2}(o_{j}^{k}-y_{j}^{k})^{2}} = \\ $$

$$ \frac{\partial (\frac{1}{2}(o_{j}^{k}-y_{j}^{k})^{2})}{\partial a_j^{k}} = \\ $$

$$ \frac{1}{2}2(o_{j}^{k}-y_{j}^{k})\frac{\partial (o_{j}^{k}-y_{j}^{k})}{\partial a_j^{k}} = \\ $$

$$ \left. (o_{j}^{k}-y_{j}^{k})\frac{\partial (o_{j}^{k}-y_{j}^{k})}{\partial a_j^{k}} \right\rvert_{o_{j}^{k} = g(a_{j}^{k})} = \\ $$

$$ (g(a_{j}^{k})-y_{j}^{k})\frac{\partial (g(a_{j}^{k})-y_{j}^{k})}{\partial a_j^{k}} = \\ $$

$$ (g(a_{j}^{k})-y_{j}^{k}) \left( \frac{\partial g(a_{j}^{k})}{\partial a_j^{k}} - \frac{\partial y_{j}^{k}}{\partial a_j^{k}} \right) = \\ $$

$$ (g(a_{j}^{k})-y_{j}^{k}) \left( \frac{\partial g(a_{j}^{k})}{\partial a_j^{k}} - 0 \right) = \\ $$

$$ (g(a_{j}^{k})-y_{j}^{k}) \left( \frac{\partial g(a_{j}^{k})}{\partial a_j^{k}} \right) = \\ $$

$$ (g(a_{j}^{k})-y_{j}^{k}) {g}'(a_{j}^{k}) \\ $$

Lo que acabamos de ver es el error term para el nodo j-esimo de la capa de salida $$k$$. Si queremos el error term de todos los nodos de la capa de salida $$k$$, se trataría de un vector:

$$ \delta^{k} = \left[\begin{array}{@{}c@{}}
    \delta_{1}^{k} \\
    \vdots \\
    \delta_{n}^{k}
    \end{array} \right] = \left[\begin{array}{@{}c@{}}
    \frac{\partial C}{\partial a_{1}{k}} \\
    \vdots \\
    \frac{\partial C}{\partial a_{n}{k}}
    \end{array} \right] = \left[\begin{array}{@{}c@{}}
    (g(a_{1}^{k})-y_{1}^{k}) {g}'(a_{1}^{k}) \\
    \vdots \\
    (g(a_{n}^{k})-y_{n}^{k}) {g}'(a_{n}^{k})
    \end{array} \right]$$