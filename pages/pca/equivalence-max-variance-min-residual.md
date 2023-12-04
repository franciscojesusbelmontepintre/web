---
layout: post
title: Equivalencia entre maximizar la varianza y minimizar los residuos
permalink: /notes/pca/equivalence-max-variance-min-residual/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

En una situación inicial, donde tuviesemos una nube de puntos previamente centrada en el origen con media $$\mu = 0$$, el calculo de la varianza quedaría:

$$ \frac{\sum_{i=0}^{n} (||\vec{x_{i}}|| - \mu)^2}{n} = \frac{\sum_{i=0}^{n} (||\vec{x_{i}}|| - 0)^{2}}{n} = \frac{\sum_{i=0}^{n} (||\vec{x_{i}}||)^{2}}{n} = \frac{\sum_{i=0}^{n} x_{i}^{t}x_{i}}{n}\\ $$

Por tanto, la contribución a la varianza que llevase a cabo cada punto de la nube, vendría determinado por el cuadrado de su módulo, esto es: 

$$ ||\vec{x_{i}}||^{2} \\$$

{% include aligner.html images="pca/variance-single-point.png" column=2 %}

El problema viene cuando llevamos a cabo la proyección de dichos puntos de la nube, a nuestra componente principal, y es que la contribución a la varianza que lleva a cabo cada punto, se ve descompuesta en una suma de 2 sumandos, a modo y manera del teorema de pitágoras:

$$ ||\vec{x_{i}}||^{2} = \left. ||\vec{proj_{CP}(x_{i})}||^{2} + ||\vec{x_{i}} - \vec{proj_{CP}(x_{i})}||^{2} \right\rvert_{proj_{CP}(x_{i}) = a^{t}x_{i}} = \\$$

$$ ||a^{t}x_{i}||^{2} + ||x_{i} - a^{t}x_{i}||^{2} \\$$

{% include aligner.html images="pca/variance-decomposition.png" column=2 %}

Cuando se genera una componente principal, se busca que buena parte de la varianza original se retenga en ésta, pero es indudable que va a existir algo de perdida. Lo proyectado en la componente principal es la varianza que se retiene para dicho punto, mientras que el residuo es la que se pierde:

$$RETAIN_{VAR}(x_{i}) = \\$$

$$LOSS_{VAR}(x_{i}) = \\$$

Dado que partimos del supuesto de que los puntos están centrados en el origen con media $$\mu = 0$$, entonces la contribución a la varianza de la CP por parte del punto proyectado es exactamente el primer sumando:

$$||\vec{proj_{CP}(x_{i})} - (0, ..., 0)||^{2} = ||\vec{proj_{CP}(x_{i})}||^{2}\\$$

Una vez todos los datos originales están proyectados en la componente principal, el cálculo de la varianza de la componente principal suma el cuadrado de las diferencias entre proyección y media (Para nuestro caso, $$\mu = 0$$), como sigue:

Durante el proceso de búsqueda de la componente principal (Problema de optimización de Lagrange), conforme giramos la componente principal en busca de la máxima varianza, puede apreciarse que un incremento en la varianza proyectada es un decremento en el residuo cometido:

{% include aligner.html images="pca/equivalence-max-variance-min-residual.png" column=3 %}

$$ RETAIN_{VAR}(x_{i}) + \Delta RETAIN_{VAR}(x_{i}) <=> LOSS_{VAR}(x_{i}) - \Delta LOSS_{VAR}(x_{i}) \\$$

$$ RETAIN_{VAR}(x_{i}) - \Delta RETAIN_{VAR}(x_{i}) <=> LOSS_{VAR}(x_{i}) + \Delta LOSS_{VAR}(x_{i}) \\$$

{% include aligner.html images="pca/increment-var-decrement-residual.png" column=3 %}
