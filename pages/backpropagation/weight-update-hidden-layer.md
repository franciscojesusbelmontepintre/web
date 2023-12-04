---
layout: post
title: Actualización de pesos en la capa oculta/intermedia
permalink: /notes/backpropagation/weight-update-hidden-layer/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Recuérdese como queda el error term para nodos de capas intermedias:

$$ \delta_{j}^{k} = {g}'(a_{j}^{k}) \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k}\\$$

Una vez se conoce la expresión del error term para nodos de capas intermedias, $$\delta_{j}^{k}$$, sustituimos en la derivada parcial como sigue:

$$ \frac{\partial C}{\partial W_{ij}^{k}} = \\$$

$$ \delta_{j}^{k}o_{i}^{k-1} = \\$$

$$ ({g}'(a_{j}^{k}) \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k})o_{i}^{k-1} = \\$$

$$ {g}'(a_{j}^{k})o_{i}^{k-1} \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k} \\$$

Una vez resuelta la derivada parcial, ya podemos sustituir en la expresión de la actualización del error:

$$ W_{ij} := W_{ij} - \alpha\frac{\partial C}{\partial W_{ij}} = W_{ij} - \alpha{g}'(a_{j}^{k})o_{i}^{k-1} \sum_{i=1}^{r_{k+1}} \delta_i^{k+1} w_{ij}^{k}\\$$