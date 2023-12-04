---
layout: post
title: Distancia de edición/Levenshtein
permalink: /notes/recurrence-complexity/levenshtein-edition-distance/
feature-img: # "assets/img/pexels/circuit.jpeg"
hide: true  # Prevent the page title to appear in the navbar
tags: [Markdown]
---


Sea una cadena $$ x = <x_{1},...,x_{n}> \\$$ que se quiere transformar en la cadena $$ y = <y_{1},...,y_{m}> \\$$. 

Para poder transformarla, se dispone de 3 operacions:

## Insertar:

Si tengo la subcadena $$ x_{0}x_{1}...x_{i} = <x_{i}>, i\in \{0,\ldots\,i-1,i,\ldots\,n-1,n\} \\$$ 
y quiero la subcadena $$ y_{0}y_{1}...y_{j} = <y_{j}>, j\in \{0,\ldots\,j-1,j,\ldots\,m-1,m\} \\$$, 
suponiendo que la última posición de ambas subcadenas no sean coincidentes, esto es $$ x_{i} \neq y_{j} \\$$, entonces
opto por insertar $$ y_{j} \\$$ en $$ <x_{i}> \\$$, quedando pues $$ <x_{i}> \cup \{y_{j}\} \\$$. De esta manera, ya puedo olvidarme de $$ y_{j} \\$$ ya que ya tengo coincidencia, luego el subproblema a resolver pasa a ser 
el transformar la subcadena  $$ <x_{i}>, i\in \{0,\ldots\,i-1,i,\ldots\,n-1,n\} \\$$ en la subcadena $$ <y_{j-1}>, j-1\in \{0,\ldots\,j-1,j,\ldots\,m-1,m\} \\$$

### Ejemplo real:

tengo "abc" y quiero "def". Para la última posición, como tengo "c", pero quiero "f", inserto "f", quedándome "abcf" y me olvido de la "f" ya que ya tengo coincidencia, luego el subproblema a resolver es pasar de "abc" a "de". 
Operación de suma 1.

## Borrar:

Si tengo la subcadena $$ x_{0}x_{1}...x_{i} = <x_{i}>, i\in \{0,\ldots\,i-1,i,\ldots\,n-1,n\} \\$$
y quiero la subcadena $$ y_{0}y_{1}...y_{j} = <y_{j}>, j\in \{0,\ldots\,j-1,j,\ldots\,m-1,m\} \\$$, 
suponiendo que la última posición de ambas subcadenas no sean coincidentes, esto es $$ x_{i} \neq y_{j} \\$$, entonces
opto por borrar $$ x_{i} \\$$ de $$ <x_{i}> \\$$, quedando pues $$ <x_{i-1}>, i-1\in \{0,\ldots\,i-1,i,\ldots\,n-1,n\} \\$$. Por tanto, el subproblema a resolver pasa a ser el 
transformar la subcadena  $$ <x_{i-1}>, i-1\in \{0,\ldots\,i-1,i,\ldots\,n-1,n\} \\$$ en la subcadena $$ <y_{j}>, j\in \{0,\ldots\,j-1,j,\ldots\,m-1,m\} \\$$

### Ejemplo real:

tengo "abc" y quiero "def". Para la última posición, como tengo "c", pero necesito "f", elimino "c" quedándome "ab", luego el subproblema a resolver es pasar de "ab" a "def". 
Operación de suma 1.
Nótese que si seguimos por este camino, acabaremos con " " queriendo pasar a "def", lo cual supone 3 borrados + 3 inserciones.

## Reemplazar:

Si tengo la subcadena $$ x_{0}x_{1}...x_{i} = <x_{i}>, i\in \{0,\ldots\,i-1,i,\ldots\,n-1,n\} \\$$ 
y quiero la subcadena $$ y_{0}y_{1}...y_{j} = <y_{j}>, j\in \{0,\ldots\,j-1,j,\ldots\,m-1,m\} \\$$, 
suponiendo que la última posición de ambas subcadenas no sean coincidentes, esto es $$ x_{i} \neq y_{j} \\$$, entonces
opto por reemplazar $$ x_{i} \\$$ por $$ y_{j} \\$$ en $$ <x_{i}> \\$$, quedando pues $$ <x_{i-1}> \cup \{y_{j}\} \\$$. De esta manera, ya puedo olvidarme de $$ y_{j} \\$$ ya que ya tengo coincidencia, luego el subproblema a resolver pasa a ser 
el transformar la subcadena  $$ <x_{i-1}>, i-1\in \{0,\ldots\,i-1,i,\ldots\,n-1,n\} \\$$ en la subcadena $$ <y_{j-1}>, j-1\in \{0,\ldots\,j-1,j,\ldots\,m-1,m\} \\$$

### Ejemplo real:

tengo "abc" y quiero "def". Para la última posición, como tengo "c", pero necesito "f", reemplazo "c" por "f", quedándome "abf" y me olvido de la "f" ya que ya tengo coincidencia, luego el subproblema a resolver es pasar de "ab" a "de". 
Operación de suma 1.

## Solución por BackTracking recursivo:

### Ecuación recursiva:

Una primera forma de resolverlo sería por BackTracking recursivo, como sigue:

$$
T(x,y,i,j) =
\begin{cases}
i, j == 0 (C.B.)\\
j, i == 0 (C.B.)\\
0 + T(x,y,i-1,j-1), x_{i} == y_{j} (C.R.)\\
1 + min(T(x,y,i,j-1),T(x,y,i-1,j),T(x,y,i-1,j-1)), (E.O.C.)\\
\end{cases}
$$

$$ T(x,y,n,m), n = |\{x_{1},x_{2},\ldots\,x_{n}\}|-1, m = |\{y_{1},y_{2},\ldots\,y_{m}\}|-1 \\$$

### Complejidad en tiempo:

Debido a que la ejecución de un algoritmo que implemente la ecucación recursiva será diferente dependiendo del par de cadenas que se pasen como entrada, se tiene que acotar la complejidad.

El peor caso posible es que no haya ninguna coincidencia de caracteres para el par de cadenas, lo cual implica que para cada llamada al algoritmo se ejecutará el mínimo de las 3 operaciones, así hasta llegar a los casos base.

Suponiendo esta situación, cada nodo ramifica en 3 nodos hijos, y este proceso se repite, al menos, $$ n \\$$ veces, siendo $$ n \\$$ el tamaño de la cadena de origen.

Cuando se llega a los casos base, se ejecutan instrucciones de retorno $$ \theta(1)\\$$.

Por tanto, la cota mínima de complejidad en tiempo para esta situación es: $$ 3_{n-1}3_{n-2}...3_{0}\theta(1) = 3_{n-1}3_{n-2}...3_{0}1 = 3_{n-1}3_{n-2}...3_{0} = \theta(3^{n})\\$$.

Se trata de una cota mínima, que no exacta, para la situación descrita, y es que los nodos del nivel $$ n \\$$, pueden ser o no casos base. Si son casos base, dejan de ramificar, pero si no lo son, siguen ramificando.

En caso de ser todos los nodos hijo, en el nivel $$ n \\$$, casos base para una ejecución dada, entonces sí podríamos afirmar que para esa ejecución, la complejidad exacta es $$ \theta(3^{n})\\$$.

No se sabe si existe algún caso como el descrito para un par de cadenas, pero en la mayoría de los casos existirán esos nodos de nivel $$ n \\$$ que seguirán ramificando, luego la complejidad temporal será algo mayor que $$ \theta(3^{n})\\$$, de ahí que digamos que es una cota mínima.

Cuando las cadenas son exactamente iguales, cada nodo ramifica en 1, luego la complejidad es exacta: $$ \theta(1^{n}) = \theta(1)\\$$.

Volviendo a la situación en la que para un par de cadenas no hay ningún carácter coincidente, el camino más rápido y que supone el menor número de operaciones siempre será el de reemplazo de todos los caracteres de x en y, sumado a la inserción
de los caracteres que faltan en x, y que están en y.

{% comment %} 

### Complejidad en espacio:

Los algoritmos recursivos hacen uso de la pila de llamadas, donde las llamadas se van amontonando una encima de otra hasta llegar a los casos base, que se almacenan en la parte superior de la pila, momento en el cual el proceso comienza a deshacerse. 

Cada llamada del algoritmo que se encuentra almacenada en la pila mantiene almacenado punteros a las 4 variables (x, y, n, m), junto a la dirección de retorno.

Ahora bien, aquí es importante recuperar lo que se dijo anteriormente sobre los nodos del nivel $$ n \\$$ que se siguen ramificando, ya que esos nodos son los que determinarán el espacio máximo necesario para la ejecución recursiva del algoritmo.

La cantidad de espacio exacto requerido vendría determinado por la rama del árbol que mayor altura tenga de entre todas las ramas, y dicha altura multiplicaría a la suma de espacios en memoria que necesitan los 4 punteros más la dirección de retorno.

Las instrucciones de retorno a la dirección de memoria requieren de una unidad de almacenamiento, $$ \theta(1)\\$$.
Los índices $$ n, m \\$$ tienen complejidad en su almacenamiento de $$ \theta(1)\\$$ cada uno.
Los punteros a cada cadena $$ x, y \\$$ tienen complejidad en su almacenamiento de $$ \theta(1)\\$$ cada uno.

Por tanto, la cota mínima de complejidad espacial, no teniendo en cuenta la rama de máxima profundidad por la razón de que no es fácil saber cuál es en función de los parámetros de entrada, es $$ \theta(n(1+2+2)) = \theta(5n) \\$$.

### Otros:

Las instrucciones de los casos base tienen complejidad $$ \theta(1) \\$$. En el peor de los casos, en cada llamada recursiva hay que calcular el mínimo de las 3 posibles operaciones (E.O.C), 
lo que implica $$ 3_{n-1}3_{n-2}...3_{0}O(1) = 3_{n-1}3_{n-2}...3_{0}1 = 3_{n-1}3_{n-2}...3_{0} = \theta(3^{n})\\$$. Por tanto, la cota de complejidad temporal es $$ \theta(3^{n})\\$$

cuando no hay ninguna coincidencia entre caracteres entre x, e y, lo más rápido es siempre reemplazo + insercción de lo que falta, y ello implica una cota mínima de complejidad temporal de O(3^m) donde m es el tamaño de la palabra de origen.
Dar una cota máxima es más complicado, más si se busca algo preciso y no una aproximación. Si buscamos una aproximación, hay que tener en cuenta que a partir de la altura "m" del arbol, hay ciertos nodos que no son hojas(C.B.) y que siguen ramificando,
por tanto hay que tenerlos en cuenta. El problema es que es difícil saber, en función de las entradas, cuántos nodos con ese carácter excepcional son, y cuantos niveles extra de altura siguen ramificando.
Consideraremos para una cota máxima, que todos los nodos a los que les presuponemos el carácter de hoja, tanto si lo son como si no lo son, ramifican hasta 3 niveles extra.
{% endcomment %}

## Solución por Programación dinámica:
En caso de usar la programación dinámica, podemos pasar de una complejidad en tiempo $$ \theta(3^{n})\\$$ a una complejidad $$ \theta(n*m + n+m)\\$$, donde $$ \theta(n*m)\\$$ viene a representar el coste de poblar la matriz de la cual 
hace uso el algoritmo de programación dinámica, donde cada celda de la matriz, $$ C_{ij}\\$$, viene a representar el mínimo número de operaciones necesarias para transformar la subcadena $$ <x_{0},...,x_{i}> \\$$, contenida en $$ x \\$$,
en la subcadena $$ <y_{0},...,y_{j}> \\$$, contenida en $$ y \\$$. 
En definitiva, cada celda representa la solución de un subproblema (recursión), como el mínimo entre la celda de su izquierda (+1), de su superior (+1) y de su diagonal (+1) (en caso de coincidencia) ó diagonal (+0) (en caso de divergencia). 
Se efectua la operación (+1) cuando precisa de alterarse (divergencia), y se suma (+0) cuando no precisa alterarse (coincidencia).

El caso de la primera fila y de la primera columna de la matriz, corresponden a los casos bases.
Para la primera fila, la celda $$ C_{0k}\\$$ representa el mínimo número de operaciones necesarias para transformar la cadena vacía en la cadena $$ <x_{0},...,x_{k}> \\$$, con $$ 0<=k<=n-1 \\$$
Para la primera columna, la celda $$ C_{z0}\\$$ representa el mínimo número de operaciones necesarias para transformar la cadena vacía en la cadena $$ <y_{0},...,y_{z}> \\$$, con $$ 0<=z<=m-1 \\$$

El coste de poblar la matriz es $$ \theta(n*m)\\$$, empleando 2 bucles anidados, uno de tamaño $$ n\\$$ equivalente a la longitud de $$ x\\$$, y otro de tamaño $$ n\\$$ equivalente a la longitud de $$ y\\$$, de ahí su producto.
La programación dinámica sacrifica la complejidad espacial en pos de mejorar la complejidad temporal. Gracias a ello, hay una mejora al pasar de $$ \theta(3^{n})\\$$ a $$ \theta(n*m + n+m)\\$$. 
Por otra parte, $$ \theta(n+m)\\$$ representa el coste en tiempo de reconstruir la solución. Una vez se ha poblado la matriz $$ n*m\\$$, se tiene que la última celda, $$ C_{n-1,m-1}\\$$, es un número que representa el mínimo número de operaciones
que se necesitan para transformar a la subcadena $$ x\\$$ en la subcadena $$ y\\$$, pero aún no sabemos qué operaciones son esas que necesitamos para llevar a cabo la transformación. Ahí es donde entre el algoritmo de reconstrucción de la solución,
con un coste de complejidad en tiempo de $$ \theta(n+m)\\$$

