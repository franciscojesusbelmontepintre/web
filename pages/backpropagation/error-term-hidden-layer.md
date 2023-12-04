---
layout: post
title: Error term en la capa oculta/intermedia
permalink: /notes/backpropagation/error-term-hidden-layer/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Supónganse 2 capas intermedias: la capa $$ k $$ y la capa $$ k+1 $$, inmediatamente siguiente. 

El error term en los nodos de la capa intermedia $$ k $$ cuantifica la variación del error cometido en la salida final de la red neuronal con respecto a la variación de la entrada a ese nodo intermedio de la capa $$ k $$. Se modela mediante el término $$ \delta_{j}^{k} $$. 

$$ \delta_j^{k} = \frac{\partial C}{\partial a_j^{k}} \\ $$ 

Dicho de otra manera, la cuantificación del impacto que tiene dicho nodo de la capa $$ k $$, en el error final cometido $$ C $$ . 

Una pequeña variación en la entrada del nodo de la capa intermedia $$ k $$, tiene un impacto en el error final cometido $$ C $$ por la red neuronal, pero no de manera directa, sino que se propaga directamente a la capa inmediatamente siguiente $$ k+1 $$. 

Por tanto, la variación que sufre el error cometido $$ C $$ en la salida final de la red neuronal es con respecto a la variación sufrida en la entrada del nodo de la capa inmediatamente siguiente $$ k+1 $$, y a su vez, dicha variación sufrida en la entrada
de dicho nodo de capa $$ k+1 $$ es con respecto a la variación de la entrada del nodo intermedio de capa $$ k $$, que es quien lo alimenta y propaga el error hacia delante en primera instancia. Esto se modela por medio de la regla de la cadena, como sigue: 

$$ \frac{\partial C}{\partial a_i^{k+1}} \frac{\partial a_i^{k+1}}{\partial a_j^{k}} \\ $$ 

Dado que dicho nodo intermedio de capa $$ k $$ alimenta a todos los nodos de la capa inmediatamente siguiente $$ k+1 $$, la variación se propaga a todos y cada uno de los nodos de la capa inmediatamente siguiente $$ k+1 $$, luego se trata de una suma de derivadas parciales: 

$$ \sum_{i=1}^{r_{k+1}} \frac{\partial C}{\partial a_i^{k+1}} \frac{\partial a_i^{k+1}}{\partial a_j^{k}} \\ $$ 

En consecuencia, el impacto que tiene una pequeña variación de un nodo intermedio de la capa $$ k $$ sobre el error final $$ C $$ de la salida de la red neuronal, se obtiene sumando las variaciones del error final $$ C $$ de la red debidas a la variación en la entrada de los nodos de la capa $$ k+1 $$, y que a su vez dicha variación sufrida en la entrada de los nodos de la capa $$ k+1 $$ son el resultado de la propagación hacia delante de la variación que sufre en la entrada un nodo de la capa inmediatamente anterior $$ k $$. 

$$ \delta_j^{k} = \frac{\partial C}{\partial a_j^{k}} = \sum_{i=1}^{r_{k+1}} \frac{\partial C}{\partial a_i^{k+1}} \frac{\partial a_i^{k+1}}{\partial a_j^{k}} \\ $$

Resolución del sumatorio: 

$$ \left. \sum_{i=1}^{r_{k+1}} \frac{\partial C}{\partial a_i^{k+1}} \frac{\partial a_i^{k+1}}{\partial a_j^{k}} \right\rvert_{\frac{\partial C}{\partial a_i^{k+1}} = \delta_i^{k+1}} = \\ $$

$$ \left. \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \frac{\partial a_i^{k+1}}{\partial a_j^{k}} \right\rvert_{a_i^{k+1} = b_i^{k} + \sum_{l=1}^{r_{k}} w_{il}^{k} o_{l}^{k}} = \\ $$

$$ \left. \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \frac{\partial (b_i^{k} + \sum_{l=1}^{r_{k}} w_{il}^{k} o_{l}^{k})}{\partial a_j^{k}} \right\rvert_{o_{l}^{k} = g(a_{l}^{k})} = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \frac{\partial (b_i^{k} + \sum_{l=1}^{r_{k}} w_{il}^{k} g(a_{l}^{k}))}{\partial a_j^{k}} = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \left( \frac{\partial b_{i}^{k}}{\partial a_j^{k}} + \frac{\partial (\sum_{l=1}^{r_{k}} w_{il}^{k} g(a_{l}^{k}))}{\partial a_j^{k}} \right) = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \left( 0 + \frac{\partial (\sum_{l=1}^{r_{k}} w_{il}^{k} g(a_{l}^{k}))}{\partial a_j^{k}} \right) = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \left(\frac{ \partial (\sum_{l=1}^{r_{k}} w_{il}^{k} g(a_{l}^{k}))}{\partial a_j^{k}} \right) = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \left( \frac{\partial (w_{ij}^{k}g(a_{j}^{k}))}{\partial a_j^{k}} + \frac{\partial \sum_{\substack{l=1 \\ l\neq j}}^{r_{k}} w_{il}^{k} g(a_{l}^{k})}{\partial a_j^{k}} \right) = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \left( \frac{\partial (w_{ij}^{k}g(a_{j}^{k}))}{\partial a_j^{k}} + 0 \right) = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \frac{\partial (w_{ij}^{k}g(a_{j}^{k}))}{\partial a_j^{k}} = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} \frac{w_{ij}^{k} \partial g(a_{j}^{k})}{\partial a_j^{k}} = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k} \frac{\partial g(a_{j}^{k})}{\partial a_j^{k}} = \\ $$

$$ \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k} {g}'(a_{j}^{k}) = \\ $$

$$ {g}'(a_{j}^{k}) \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k} $$
