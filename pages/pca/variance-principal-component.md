---
layout: post
title: Varianza de la componente principal
permalink: /notes/pca/variance-principal-component/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Sea el escalar que mide la proyección de un punto $$x$$ sobre un vector unitario $$a$$, como sigue:

$$z = \left. \frac{a^{t}x}{a^{t}a} \right\rvert_{a^{t}a = 1} = a^{t}x = x^{t}a $$

Si en vez de un punto, tenemos un conjunto de puntos representados a traves de una matriz $$X$$ de $$n$$ puntos y $$p$$ características, el vector de escalares que mide cada una de las proyecciones de cada punto sobre el vector sería:

$$ X = \begin{pmatrix}
  x_{11} & \cdots & x_{1p} \\
  \vdots & \ddots & \vdots  \\
  x_{n1} & \cdots & x_{np} 
 \end{pmatrix} $$

$$ a = \left[\begin{array}{@{}c@{}}
    a_{11} \\
    \vdots \\
    a_{p1}
    \end{array} \right]$$
	
$$ Z = Xa = \begin{pmatrix}
  x_{11} & \cdots & x_{1p} \\
  \vdots & \ddots & \vdots  \\
  x_{n1} & \cdots & x_{np} 
 \end{pmatrix} \left[\begin{array}{@{}c@{}}
    a_{11} \\
    \vdots \\
    a_{p1}
    \end{array} \right] = \left[\begin{array}{@{}c@{}}
    z_{11} \\
    \vdots \\
    z_{n1}
    \end{array} \right] = \left[\begin{array}{@{}c@{}}
    x_{1}^{t}a \\
    \vdots \\
    x_{n}^{t}a 
	\end{array} \right] = \left[\begin{array}{@{}c@{}}
    \lambda_{1} \\
    \vdots \\
    \lambda_{n}
	\end{array} \right]$$
	
Donde $$z_{i1} = \lambda_{i} $$ es el escalar que mide la proyección del punto $$i$$ sobre el vector unitario $$a$$

Una vez tenemos todas las varianzas proyectadas en el vector $$a$$, esto es $$ Z $$, vamos a proceder a calcular la varianza total de la componente principal.

Para ello emplearemos cada una de las varianzas proyectadas calculadas previamente:

$$ \frac{1}{n}\sum_{i=1}^{n}(\lambda_{i} - \mu)^{2} \\$$

Nótese que suponemos como situación de partida, que los datos de $$X$$ son datos centrados, esto es, al valor de la característica para un determinado punto, se le ha restado la media de dicha característica, por tanto:

$$ \left. \frac{1}{n}\sum_{i=1}^{n}(\lambda_{i} - \mu)^{2} \right\rvert_{\mu = 0} = \frac{1}{n}\sum_{i=1}^{n}(\lambda_{i})^{2} \\$$

Los sumandos son las diferentes varianzas proyectadas. Cada punto del conjunto hace su contribución mediante la varianza de su proyección al cuadrado.

Dicha expresión la podemos reescribir como sigue:

$$ \frac{1}{n}\sum_{i=1}^{n}(\lambda_{i})^{2} = \frac{1}{n} [\lambda_{1}, ..., \lambda_{n}] \left[\begin{array}{@{}c@{}}
    \lambda_{1} \\
    \vdots \\
    \lambda_{n}
    \end{array} \right] = \left. \frac{1}{n}Z^{t}Z \right\rvert_{Z = Xa} = \left. \frac{1}{n}(Xa)^{t}Xa \right\rvert_{(Xa)^{t} = a^{t}X^{t}} = \frac{1}{n}a^{t}X^{t}Xa = a^{t}(\frac{1}{n}X^{t}X)a \\$$
	
Antes de continuar, veamos en detenimiento la expresión $$ \frac{1}{n}X^{t}X $$:

$$ X = \begin{pmatrix}
  x_{11} & \cdots & x_{1p} \\
  \vdots & \ddots & \vdots  \\
  x_{n1} & \cdots & x_{np} 
 \end{pmatrix} \\$$

$$ X^{t} = \begin{pmatrix}
  x_{11} & x_{21} & \cdots & x_{n1} \\
  x_{12} & x_{22} & \cdots & x_{n2} \\
  \vdots & \vdots & \ddots & \vdots  \\
  x_{1p} & x_{2p} & \cdots & x_{np} 
 \end{pmatrix} \\\\$$

$$ \frac{1}{n}X^{t}X = \frac{1}{n}\begin{pmatrix}
  x_{11} & x_{21} & \cdots & x_{n1} \\
  x_{12} & x_{22} & \cdots & x_{n2} \\
  \vdots & \vdots & \ddots & \vdots  \\
  x_{1p} & x_{2p} & \cdots & x_{np} 
 \end{pmatrix} \begin{pmatrix}
  x_{11} & \cdots & x_{1p} \\
  \vdots & \ddots & \vdots  \\
  x_{n1} & \cdots & x_{np} 
 \end{pmatrix}\\$$
 
La celda de la primera fila y primera columna, quedaría de la siguiente manera:

$$ \frac{1}{n}(x_{11}x_{11} + x_{21}x_{21} + ... + x_{n1}x_{n1}) = \frac{1}{n}\sum_{i=1}^{n}(x_{i1})^{2} = \left. \frac{1}{n}\sum_{i=1}^{n}(x_{i1} - \mu_{1})(x_{i1} - \mu_{1}) \right\rvert_{\mu_{i} = 0} \\$$

Lo cual corresponde a la covarianza de la característica 1 consigo misma.

La celda de la última fila y última columna, quedaría de la siguiente manera:

$$ \frac{1}{n}(x_{1p}x_{1p} + x_{2p}x_{2p} + ... + x_{np}x_{np}) = \frac{1}{n}\sum_{i=1}^{n}(x_{ip})^{2} = \left. \frac{1}{n}\sum_{i=1}^{n}(x_{ip} - \mu_{p})(x_{ip} - \mu_{p}) \right\rvert_{\mu_{p} = 0} \\$$

Lo cual corresponde a la covarianza de la última característica consigo misma.

Por tanto, para la diagonal se tiene que:

$$ \vee_{i=1,...,n} \vee_{j=1,...,p} (i = j => \frac{1}{n}\sum_{i=1}^{n}(x_{ij})^{2} = \sigma_{ii}^{2}) \\$$

La celda de la primera fila y última columna, quedaría de la siguente manera:

$$ \frac{1}{n}(x_{11}x_{1p} + x_{21}x_{2p} + ... + x_{n1}x_{np}) = \frac{1}{n}\sum_{i=1}^{n}x_{i1}x_{ip} = \left. \frac{1}{n}\sum_{i=1}^{n}(x_{i1} - \mu_{1})(x_{ip} - \mu_{p}) \right\rvert_{\mu_{1} = 0, \mu_{p} = 0} \\$$

Lo cual corresponde a la covarianza de la característica 1 con la característica $$p$$

La celda de la última fila y primera columna, quedaría de la siguiente manera:

$$ \frac{1}{n}(x_{1p}x_{11} + x_{2p}x_{21} + ... + x_{np}x_{n1}) = \frac{1}{n}\sum_{i=1}^{n}x_{ip}x_{i1} = \left. \frac{1}{n}\sum_{i=1}^{n}(x_{ip} - \mu_{p})(x_{i1} - \mu_{1}) \right\rvert_{\mu_{p} = 0, \mu_{1} = 0} \\$$

Lo cual corresponde a la covarianza de la característica $$p$$ con la característica 1, que es igual a la covarianza de la característica 1 con la característica $$p$$. Lo cual hace denotar la simetría de la matriz resultante:

$$ \frac{1}{n}X^{t}X = (\frac{1}{n}X^{t}X)^{t} \\$$ 

En efecto, la matriz resultante resulta ser la matriz de varianzas-covarianzas, esto es varianzas en la diagonal, y covarianzas entre características para celdas que no son de la diagonal

$$ \frac{1}{n}X^{t}X = \Sigma = \frac{1}{n}\begin{pmatrix}
  x_{11} & x_{21} & \cdots & x_{n1} \\
  x_{12} & x_{22} & \cdots & x_{n2} \\
  \vdots & \vdots & \ddots & \vdots  \\
  x_{1p} & x_{2p} & \cdots & x_{np} 
 \end{pmatrix} \begin{pmatrix}
  x_{11} & \cdots & x_{1p} \\
  \vdots & \ddots & \vdots  \\
  x_{n1} & \cdots & x_{np} 
 \end{pmatrix} = \frac{1}{n}\begin{pmatrix}
  \sum_{i=1}^{n}(x_{i1})^{2} & \cdots & \sum_{i=1}^{n}x_{i1}x_{ip} \\
  \vdots & \ddots & \vdots  \\
  \sum_{i=1}^{n}x_{ip}x_{i1} & \cdots & \sum_{i=1}^{n}(x_{ip})^{2} 
 \end{pmatrix} = \\$$
 
 $$ \begin{pmatrix}
  \frac{1}{n}\sum_{i=1}^{n}(x_{i1})^{2} & \cdots & \frac{1}{n}\sum_{i=1}^{n}x_{i1}x_{ip} \\
  \vdots & \ddots & \vdots  \\
  \frac{1}{n}\sum_{i=1}^{n}x_{ip}x_{i1} & \cdots & \frac{1}{n}\sum_{i=1}^{n}(x_{ip})^{2} 
 \end{pmatrix} = \begin{pmatrix}
  \sigma_{11}^{2} = VAR(1) & \cdots & \sigma_{1p}^{2} = COV(1, p) \\
  \vdots & \ddots & \vdots  \\
  \sigma_{p1}^{2} = COV(p, 1) & \cdots & \sigma_{pp}^{2} = VAR(p)
 \end{pmatrix} \\$$
 
Retomando la expresión de la varianza de la componente principal, nos queda por tanto que:

$$ \frac{1}{n}\sum_{i=1}^{n}(\lambda_{i})^{2} = \left. a^{t}(\frac{1}{n}X^{t}X)a \right\rvert_{\frac{1}{n}X^{t}X = \Sigma} = a^{t}\Sigma a\\$$