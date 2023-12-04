---
layout: post
title: Problema del desvanecimiento del gradiente para funciones sigmoide en capas tempranas
permalink: /notes/backpropagation/vanishing-gradient-problem/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Habíamos visto la expresión del error term cometido por la neurona j-esima de la capa $$k$$, en función de si se trata de la capa de salida, o de una capa intermedia:

$$
\delta_{j}^{k} =
\begin{cases}
(g(a_{j}^{k})-y_{j}^{k}) {g}'(a_{j}^{k}), if-output-layer-node\\
{g}'(a_{j}^{k}) \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k}, if-hidden-layer-node\\
\end{cases}
$$

Vamos a suponer que todas las funciones de activación empleadas para las neuronas de la red neuronal son de tipo sigmoide:

$$ g = \sigma : \mathbb{R} \rightarrow [0, 1] \\$$

Definida como sigue:

$$ g(x) = \sigma (x) = \frac{1}{1+e^{-x}}\\$$

La derivada de dicha función es:

$$ g' = \sigma ' : \mathbb{R} \rightarrow [0, 0.25]$$

Definida como sigue:

$$ \frac{dg}{dx} = \frac{d\sigma}{dx} = \href{https://math.stackexchange.com/questions/78575/derivative-of-sigmoid-function-sigma-x-frac11e-x/1225116#1225116}{\sigma (x)(1 - \sigma (x))}\\$$

Como proposición establecemos que las capas más tempranas de una red neuronal profunda, p.e. k = 1ª capa, no aprende. Esto es, la actualización de pesos que sufre es una perturbación infima que tiende a 0:

$$ \Delta W_{ij} = \delta_{j}^{k}o_{i}^{k-1} = {g}'(a_{j}^{k}) o_{i}^{k-1} \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k} \rightarrow 0 \\$$

Para una red neuronal de $$ m = 4 $$ capas, $$ r_{k} = 1 $$ nodos por capa, $$ {g}'(a_{j}^{k}) = 0.25 $$ como activación de la neurona suponiendo el mejor caso. Veamos que ocurre a la capa $$ k = 1 $$:

$$ \left. {g}'(a_{j}^{k}) o_{i}^{k-1} \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k} \right\rvert_{\delta_i^{k+1} = {g}'(a_{j}^{k+1}) \sum_{i=1}^{r_{k+2}} \delta_i^{k+2} w_{ij}^{k+1}} = \\$$

$$ \left. {g}'(a_{j}^{k}) o_{i}^{k-1} \sum_{i=1}^{r_{k+1}} ({g}'(a_{j}^{k+1}) \sum_{i=1}^{r_{k+2}} \delta_i^{k+2} w_{ij}^{k+1}) w_{ij}^{k} \right\rvert_{\delta_i^{k+2} = {g}'(a_{j}^{k+2}) \sum_{i=1}^{r_{k+3}} \delta_i^{k+3} w_{ij}^{k+2}} = \\$$ 

$$ \left. {g}'(a_{j}^{k}) o_{i}^{k-1} \sum_{i=1}^{r_{k+1}} ({g}'(a_{j}^{k+1}) \sum_{i=1}^{r_{k+2}} ({g}'(a_{j}^{k+2}) \sum_{i=1}^{r_{k+3}} \delta_i^{k+3} w_{ij}^{k+2}) w_{ij}^{k+1}) w_{ij}^{k} \right\rvert_{\delta_i^{k+3} = (o_{j}^{k+3}-y_{j}^{k+3}) {g}'(a_{j}^{k+3})} = \\$$

$$ {g}'(a_{j}^{k}) o_{i}^{k-1} \sum_{i=1}^{r_{k+1}} ({g}'(a_{j}^{k+1}) \sum_{i=1}^{r_{k+2}} ({g}'(a_{j}^{k+2}) \sum_{i=1}^{r_{k+3}} ((o_{j}^{k+3}-y_{j}^{k+3}) {g}'(a_{j}^{k+3})) w_{ij}^{k+2}) w_{ij}^{k+1}) w_{ij}^{k} \\$$

Dado que se ha establecido que todas las capas tienen 1 sola neurona, perdemos los sumatorios:

$$ {g}'(a_{j}^{k}) o_{i}^{k-1} ({g}'(a_{j}^{k+1}) ({g}'(a_{j}^{k+2}) ((o_{j}^{k+3}-y_{j}^{k+3}) {g}'(a_{j}^{k+3})) w_{ij}^{k+2}) w_{ij}^{k+1}) w_{ij}^{k} \\$$

Sustituimos las derivadas de las funciones de activación sigmoide por su valor máximo, 0.25:

$$ 0.25 o_{i}^{k-1} (0.25 (0.25 ((o_{j}^{k+3}-y_{j}^{k+3}) 0.25) w_{ij}^{k+2}) w_{ij}^{k+1}) w_{ij}^{k} \\$$

Movemos a la izquierda todos los 0.25:

$$ 0.25^{4} o_{i}^{k-1} ( ( ((o_{j}^{k+3}-y_{j}^{k+3}) ) w_{ij}^{k+2}) w_{ij}^{k+1}) w_{ij}^{k} \\$$

$$ 0.0039 o_{i}^{k-1} ( ( ((o_{j}^{k+3}-y_{j}^{k+3}) ) w_{ij}^{k+2}) w_{ij}^{k+1}) w_{ij}^{k} \rightarrow 0\\$$

El problema viene de un número muy pequeño, $$0.25$$, elevado a una potencia, $$m$$, que termina multiplicando al resto de la expresión

Generalizando se tiene que para $$ K = 1ª$$ capa:

$$ \frac{\partial C}{\partial W_{ij}} = 0.25^{m}o_{i}^{k-1}(o_{j}^{m} - y_{j}^{m})\prod_{k}^{m-1}w^{k} \\$$

Para $$ K = 2ª$$ capa:

$$ \frac{\partial C}{\partial W_{ij}} = 0.25^{m-1}o_{i}^{k-1}(o_{j}^{m} - y_{j}^{m})\prod_{k}^{(m-1)-1}w^{k} \\$$

En general, para cualquier capa $$k$$ intermedia:

$$ \frac{\partial C}{\partial W_{ij}} = 0.25^{(m+1)-k}o_{i}^{k-1}(o_{j}^{m} - y_{j}^{m})\prod_{k}^{m-k}w^{k} \\$$