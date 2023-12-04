---
layout: post
title: ejemplo 1
permalink: /notes/recurrence-complexity/ej1/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---

Sea el siguiente algoritmo en código java, definimos la complejidad en tiempo como el número de operaciones que lleva a cabo el algoritmo.

```java

public class Main {
	
  public void f(int n) {
	
	for(int i=0; i<n; i++){
		for(int j=i; j<i*i; j++){
			System.out.println("*");
		}
	}
	    
  }

}
```


El primer bucle itera $$ n $$ veces, donde $$ n $$ es el tamaño del problema.
El segundo bucle depende directamente del valor de la variable del bucle anterior, e itera $$ i^{2} - i \\ $$ veces.
La operación $$ System.out.println("*") $$ tiene complejidad $$ \theta(1) $$

Ahora bien, se puede apreciar que el bucle interno no itera para los valores de $$ i = 0, 1 $$, y sólo lo hace para $$ i >= 2 $$.

Por tanto, la ecuación en recurrencias del algoritmo es la siguiente:

$$
T(n) =
\begin{cases}
T(n-1) + (n^{2} - n)1, n >= 2\\
0, n = 0, 1\\
\end{cases}
$$

Algunos ejemplos de la complejidad en tiempo del algoritmo para ciertos tamaños del problema son los siguientes:

$$ T(0) = 0 \\$$

$$ T(1) = 0 \\$$

$$ T(2) = T(1) + (4 - 2) = 0 + 2 = 2 \\$$

$$ T(3) = T(2) + (9 - 3) = 2 + 6 = 8 \\$$

$$ T(4) = T(3) + (16 - 4) = 8 + 12 = 20 \\$$

La resolución de la ecuación en recurrencias es la siguiente:

$$ T(n) = T(n-1) + (n^{2} - n) = \\$$

$$ T(n) = T(n-2) + ((n-1)^{2} - (n - 1)) + (n^{2} - n) = \\$$

$$ T(n) = T(n-3) + ((n-2)^{2} - (n - 2)) + ((n-1)^{2} - (n - 1)) + (n^{2} - n) = \\$$

$$ ... \\$$

$$ T(n) = ((n - n)^{2} - (n - n)) + ((n - (n - 1))^{2} - (n - (n - 1))) + ... + ((n-2)^{2} - (n - 2)) + ((n-1)^{2} - (n - 1)) + ((n-0)^{2} - (n-0)) \\$$

En este punto, nos tenemos que dar cuenta de 3 patrones:

- El primero de ellos, hay un sumatorio del tipo $$ 0 + 1 + 2 + ... + n = \sum_{i=0}^{n} i \\ $$
- El segundo de ellos, $$ -n \\ $$  aparece un total de $$ n+1 \\ $$  veces, luego se tiene $$ (n+1)(-n) \\ $$ 
- El tercero, hay un sumatorio del tipo $$ (n - n)^{2} + (n - (n - 1))^{2} + ... + (n-2)^{2} + (n-1)^{2} + (n-0)^{2} = (0)^{2} + (1)^{2} + (2)^{2} + ... + (n-(n-1))^{2} + (n-n)^{2} = \sum_{i=0}^{n} (i)^{2} \\ $$ 

Por tanto reescribimos la expresión como sigue:

$$ T(n) = (\sum_{i=0}^{n} i) + (n+1)(-n) + (\sum_{i=0}^{n} (i)^{2}) \\$$

El primer sumatorio equivale a:

$$ (\sum_{i=0}^{n} i) =  \frac{n(n+1)}{2} \\$$

Se demuestra como sigue:

$$ (\sum_{i=0}^{n} i) = T(N) = 1 + 2 + 3 + … + N \\$$

$$ T(N) = N + T(N - 1) \\$$

$$ T(N) = N + (0 + 1 + 2 + … + (N - 1)) \\$$

$$ T(N) = N + (N - 1) + (0 + 1 + 2 + … + (N - 2)) \\$$

$$ T(N) = N + (N - 1) + T(N - 2) \\$$

$$ T(N) = N + (N - 1) + (N - 2) + T(N - 3) \\$$

$$ ... \\$$

$$ T(N) = N + (N - 1) + (N - 2) + … + (N - N) \\$$

$$ T(N) = N + N - 1 + N - 2 + … + N - N \\$$

$$ T(N) = N * N - (1 + 2 + ... + (N - 1)) \\$$

$$ T(N) = N * N - (T(N - 1)) \\$$

Sabemos que $$ T(N) = N + T(N - 1) \\$$. Restamos en ambos lados $$ N \\$$

$$ T(N) - N = N * N - T(N - 1) - N \\$$

$$ T(N) - N = N * N - (T(N - 1) + N) \\$$

$$ T(N) - N = N * N - (N + T(N - 1)) \\$$

Reemplazamos $$ N + T(N - 1) \\$$ por $$ T(N) \\$$

$$ T(N) - N = N * N - (T(N)) \\$$

$$ T(N) - N = N * N - T(N) \\$$

Sumamos $$ T(N) \\$$ en ambos lados

$$ T(N) - N + T(N) = N * N - T(N) + T(N) \\$$

$$ T(N) + T(N) - N = N * N \\$$

$$ 2 * T(N) - N = N * N \\$$

Sumamos $$ N \\$$ en ambos lados

$$ 2 * T(N) = N * N + N \\$$

Dividimos por $$ 2 \\$$ en ambos lados

$$ T(N) = \frac{(N * N + N)}{2} \\$$

$$ T(N) = \frac{(N^2 + N)}{2} \\$$

$$ T(N) = \frac{(N * (N + 1))}{2} \\$$

El segundo sumatorio equivale a:

$$ (\sum_{i=0}^{n} (i)^{2}) = \frac{n(n+1)(2n+1)}{6} \\$$

Se demuestra como sigue:

Sea la expresión:

$$ n^{3} - (n-1)^{3} = 3n^{2} - 3n + 1 \\$$

Para $$ n=1 \\$$ se tiene:

$$ 1^{3} - (0)^{3} = 3*1^{2} - 3*1 + 1 \\$$

Para $$ n=2 \\$$ se tiene: 

$$ 2^{3} - (1)^{3} = 3*2^{2} - 3*2 + 1 \\$$

Para $$ n=3 \\$$ se tiene: 

$$ 3^{3} - (2)^{3} = 3*3^{2} - 3*3 + 1 \\$$

$$ ... \\$$

Para $$ n=n \\$$ se tiene: 

$$ n^{3} - (n-1)^{3} = 3*n^{2} - 3*n + 1 \\$$

Calculamos $$ \sum_{i=1}^{n} (n^{3} - (n-1)^{3}) \\$$ y nos queda:

$$ n^{3} - 0^{3} = 3(1^{2} + 2^{2} + ... + n^{2}) - 3(1 + 2 + ... + n) + n\\$$

Despejamos el sumatorio de cuadrados y nos queda

$$ 1^{2} + 2^{2} + ... + n^{2} = \frac{n^{3} + 3(1 + 2 + ... + n) - n}{3}\\$$

que equivale a:

$$ \sum_{i=0}^{n} (i)^{2} = 1^{2} + 2^{2} + ... + n^{2} = \frac{n^{3} + 3\frac{n(n+1)}{2} - n}{3} = \\$$

$$ \frac{2n^{3}+3n(n+1)-2n}{2} : \frac{3}{1} = \\$$

$$ \frac{2n^{3}+3n^{2}+n}{6} = \\$$

$$ \frac{n(n+1)(2n+1)}{6} \\$$

Volvemos a reescribir la expresión como sigue:

$$ T(n) = (\frac{n(n+1)}{2}) + (n+1)(-n) + (\frac{n(n+1)(2n+1)}{6}) \\$$

Por tanto, la complejidad exacta en tiempo del algoritmo es:

$$ \theta(\frac{-2n + 2n^{3}}{6}) \\$$