---
layout: post
title: Componente principal de vector unitario
permalink: /notes/pca/unitary-vector-principal-component/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Sea $$ Z $$ la componente principal obtenida de la tranformación lineal de una matriz $$ X $$, de $$nxp$$, y un vector $$a$$. 

$$ Z = \begin{pmatrix}
  x_{11} & \cdots & x_{1p} \\
  \vdots & \ddots & \vdots  \\
  x_{n1} & \cdots & x_{np} 
 \end{pmatrix} \left[\begin{array}{@{}c@{}}
    a_{11} \\
    \vdots \\
    a_{p1}
    \end{array} \right] = \left[\begin{array}{@{}c@{}}
    \lambda_{1} \\
    \vdots \\
    \lambda_{n}
	\end{array} \right]\\$$

La varianza de la componente quedaría:

$$VAR(Z) =  \frac{1}{n}\sum_{i=1}^{n}(\lambda_{i})^{2} = \frac{1}{n} [\lambda_{1}, ..., \lambda_{n}] \left[\begin{array}{@{}c@{}}
    \lambda_{1} \\
    \vdots \\
    \lambda_{n}
    \end{array} \right] = \left. \frac{1}{n}Z^{t}Z \right\rvert_{Z = Xa} = \left. \frac{1}{n}(Xa)^{t}Xa \right\rvert_{(Xa)^{t} = a^{t}X^{t}} = \frac{1}{n}a^{t}X^{t}Xa = a^{t}(\frac{1}{n}X^{t}X)a = a^{t}\Sigma a\\$$

Si no se exige expresamente que el módulo de $$a$$ sea 1, entonces podemos maximizar la varianza de la componente principal multiplicando por un escalar, $$\alpha $$:

$$ a' = \alpha a\\$$

$$ Z' = Xa' = X\alpha a = \left[\begin{array}{@{}c@{}}
    \alpha\lambda_{1} \\
    \vdots \\
    \alpha\lambda_{n}
    \end{array} \right]\\$$

$$ VAR(Z') = \frac{1}{n}\sum_{i=1}^{n}(\alpha\lambda_{i})^{2} = \frac{1}{n}(\alpha)^{2}\sum_{i=1}^{n}(\lambda_{i})^{2} = \left. (\alpha)^{2}\frac{1}{n}\sum_{i=1}^{n}(\lambda_{i})^{2} \right\rvert_{\frac{1}{n}\sum_{i=1}^{n}(\lambda_{i})^{2} = VAR(Z)}= \alpha^{2}VAR(Z)\\$$