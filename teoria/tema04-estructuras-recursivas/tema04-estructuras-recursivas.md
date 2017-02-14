
## Tema 3: Procedimientos y estructuras recursivas

### Contenidos

- [1. Procedimientos recursivos](#1)
    - [1.1. Pensando recursivamente](#1-1)
	- [1.2. El coste de la recursión](#1-2)
	- [1.3. Soluciones al coste de la recursión: procesos iterativos](#1-3)
	- [1.4. Soluciones al coste de la recursión: memoization](#1-4)
    - [1.5 Recursión y gráficos de tortuga](#1-5)
- [2. Listas estructuradas](#2)
    - [2.1. Definición y ejemplos](#2-1)
    - [2.2. Funciones recursivas sobre listas estructuradas](#2-2)
- [3. Árboles](#3)
    - [3.1. Definición de árboles en Scheme](#3-1)
    - [3.2. Funciones recursivas sobre árboles](#3-2)

### Bibliografía - SICP

En este tema explicamos conceptos de los siguientes capítulos del libro *Structure and Intepretation of Computer Programs*:

- [1.2 - Procedures and the Processes They Generate](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-11.html#%_sec_1.2)
- [1.2.1 - Linear Recursion and Iteration](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-11.html#%_sec_1.2.1)
- [1.2.2 - Tree Recursion](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-11.html#%_sec_1.2.2)
- [2.2.2 - Hierarchical Structures](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-15.html#%_sec_2.2.2)

### <a name="1"></a> 1. Procedimientos recursivos

Ya hemos visto algunos muchos ejemplos de funciones recursivas. Una
función es recursiva cuando se llama a si misma. Una vez que uno se
acostumbra a su uso, se comprueba que la recursión es una forma mucho
más natural que la iteración de expresar un gran número de funciones y
procedimientos.

La formulación matemática de la recursión es sencilla de entender,
pero su implementación en un lenguaje de programación no lo es
tanto. El primer lenguaje de programación que permitió el uso de
expresiones recursivas fue el Lisp. En el momento de su creación
existía ya el Fortran, que no permitía que una función se llamase a si
misma.

Ya hemos visto la utilidad de la recursión en muchos ejemplos para
recorrer listas, para filtrarlas, etc. En este tema veremos algunos
aspectos negativos de la recursión: su coste espacial y
temporal. Veremos que hay soluciones a estos problemas, cambiando el
estilo de la recursión y generando *procesos iterativos* o usando un
enfoque automático llamado *memoization* en el que se guardan los
resultados de cada llamada recursiva. Por último, veremos un último
ejemplo curioso e interesante de la recursión para realizar figuras
fractales con gráficos de tortuga.

En los siguientes apartados del tema veremos que la recursión no sólo
se utiliza para definir funciones y procedimientos sino que existen
estructuras de datos cuya definición es recursiva, como las listas o
los árboles. Estudiaremos estas estructuras, su implementación en
Scheme y algoritmos recursivos que trabajan sobre ellas.

#### <a name="1-1"></a> 1.1. Pensando recursivamente

Para diseñar procedimientos recursivos no vale intentarlo resolver por
prueba y error. Hay que diseñar la solución recursiva desde el
principio. Debemos fijarnos en *lo que devuelve la función* y debemos
preguntarnos cómo sería posible descomponer el problema de forma que
podamos lanzar la recursión sobre una versión más sencilla del
mismo. Supondremos que la llamada recursiva funciona correctamente y
devuelve el resultado correcto. Y después debemos transformar este
resultado correcto de la versión más pequeña en el resultado de la
solución completa.

Es muy importante escribir y pensar en las funciones de forma
declarativa, teniendo en cuenta lo que hacen y no cómo lo hacen.

Debes **confiar en que la llamada recursiva va a hacer su trabajo y
devolver el resultado correcto**, sin preocuparte de cómo lo va a
hacer. Después tendrás que utilizar lo que la llamada recursiva ha
devuelto para componer la solución definitiva al problema.

Para diseñar un algoritmo recursivo es útil no ponerse a programar
directamente, sino reflexionar sobre la solución recursiva con algún
ejemplo. El objetivo es obtener una formulación abstracta del caso
general de la recursión antes de programarlo. Una vez que encontramos
esta formulación, pasarlo a un lenguaje de programación es muy
sencillo.

Por último, deberemos reflexionar en el caso base. Debe ser el caso
más sencillo que puede recibir como parámetro la recursión. Debe
devolver un valor compatible con la definición de la función. Por
ejemplo, si la función debe construir una lista, el caso base debe
devolver también una lista. Si la función construye una pareja, el
caso base también devolverá una pareja. No debemos olvidar que el caso
base es también un ejemplo de invocación de la función.

Ya hemos usado todos estos consejos en el tema anterior y en las
prácticas realizadas hasta ahora.

Veamos ejemplo más. ¿cómo definimos una lista palíndroma de forma
recursiva?. Por ejemplo, las siguientes listas son palíndromas:

```
{1 2 3 3 2 1}
{1 2 1}
{1}
'()
```

Comenzamos con una definición **no recursiva**:

> Una lista es palíndroma cuando es igual a su inversa. 

Esta definición no es recursiva porque no llamamos a la recursión con
un caso más sencillo.

La definición **recursiva** del caso general es la siguiente:

> Una lista es palíndroma cuando su primer elemento es igual que el
> último y la lista resultante de quitar el primer y el último
> elemento también es palíndroma

En el caso base debemos buscar el caso más pequeño no contemplado por
la definición anterior. En este caso, una lista de un elemento y una
lista vacía también las consideraremos palíndromas.

> palindroma(lista) <=> (primer-elemento(lista) == ultimo-elemento(lista))  y palindroma(quitar-primero-ultimo(lista))   
> palindroma(lista) <=> un-elemento(lista) o vacía(lista) 


Vamos a escribirlo en Scheme:

```scheme
(define (palindromo? lista)
   (or (null? lista)
       (null? (cdr lista))
       (and (equal? (car lista) (ultimo lista))
            (palindromo? (quitar-primero-ultimo lista)))))
```

La función auxiliar `quitar-primero-ultimo` la podemos definir así:

```scheme
(define (quitar-ultimo lista)
   (if (null? (cdr lista))
      '()
      (cons (car lista)
            (quitar-ultimo (cdr lista)))))

(define (quitar-primero-ultimo lista)
   (cdr (quitar-ultimo lista)))
```

#### <a name="1-2"></a> 1.2. El coste de la recursión

##### 1.2.1. La pila de la recursión

Vamos a estudiar el comportamiento del proceso generado por una
llamada a un procedimiento recursivo. Supongamos la función
`mi-length`:

```scheme
(define (mi-length items)
   (if (null? items)
      0
      (+ 1 (mi-length (cdr items)))))
```

Examinamos cómo se evalúan las llamadas recursivas:

```
(mi-length '(a b c d))
(+ 1 (mi-length '(b c d)))
(+ 1 (+ 1 (mi-length '(c d))))
(+ 1 (+ 1 (+ 1 (mi-length '(d)))))
(+ 1 (+ 1 (+ 1 (+ 1 (mi-length '())))))
(+ 1 (+ 1 (+ 1 (+ 1 0))))
(+ 1 (+ 1 (+ 1 1)))
(+ 1 (+ 1 2))
(+ 1 3)
4
```

Cada llamada a la recursión deja una función en espera de ser evaluada
cuando la recursión devuelva un valor (en el caso anterior el +). Esta
función, junto con sus argumentos, se almacenan en la *pila de la
recursión*.

Cuando la recursión devuelve un valor, los valores se recuperan de la
pila, se realiza la llamada y se devuelve el valor a la anterior
llamada en espera. Si la recursión está mal hecha y nunca termina se
genera un *stack overflow* porque la memoria que se almacena en la
pila sobrepasa la memoria reservada para el intérprete DrRacket.

##### 1.2.2. Coste espacial de la recursión

El coste espacial de un programa es una función que relaciona la
memoria consumida por una llamada para resolver un problema con alguna
variable que determina el tamaño del problema a resolver.

En el caso de la función `mi-length` el tamaño del problema viene dado
por la longitud de la lista. El coste espacial de `mi-lenght` es
*O(n)*, siendo *n* la longitud de la lista.

##### 1.2.3. El coste depende del número de llamadas a la recursión

Veamos con un ejemplo que el coste de las llamadas recursivas puede
dispararse. Supongamos la famosa [secuencia de Fibonacci]:
0,1,1,2,3,5,8,13,...

[secuencia de Fibonacci]:http://en.wikipedia.org/wiki/Fibonacci_number

Formulación matemática de la secuencia de Fibonacci:

```
>Fibonacci(n) = Fibonacci(n-1) + Fibonacci(n-2)  
>Fibonacci(0) = 0  
>Fibonacci(1) = 1
```

Formulación recursiva en Scheme:

```
(define (fib n)
   (cond ((= n 0) 0)
      ((= n 1) 1)
      (else (+ (fib (- n 1))
               (fib (- n 2))))))
```

Evaluación de una llamada a Fibonacci:

<img src="imagenes/fibonacci.png"/>

Cada llamada a la recursión produce otras dos llamadas, por lo que el
número de llamadas finales es 2^n siendo n el número que se pasa a la
función.

El coste espacial y temporal es exponencial, O(2^n). ¿Qué pasa si
intentamos evaluar `(fibonaci 35)`?

#### <a name="1-3"></a> 1.3. Soluciones al coste de la recursión: procesos iterativos

Diferenciamos entre procedimientos y procesos: un procedimiento es un
algoritmo y un proceso es la ejecución de ese algoritmo.

Es posible definir procedimientos recursivos que generen procesos
iterativos (como los bucles en programación imperativa) en los que no
se dejen llamadas recursivas en espera ni se incremente la pila de la
recursión. Para ello construimos la recursión de forma que en cada
llamada se haga un cálculo parcial y en el caso base se pueda devolver
directamente el resultado obtenido.

Este estilo de recursión se denomina *recursión por la cola*
([tail recursion](http://en.wikipedia.org/wiki/Tail_call), en inglés).

Se puede realizar una implementación eficiente de la ejecución del
proceso, eliminando la pila de la recursión.

##### 1.3.1. Factorial iterativo

Es posible modificar la formulación de la recursión para se eviten las
llamadas en espera:

* Definimos la función `(fact-iter-aux product n)` que es la que
  define el proceso iterativo
* Tiene un parámetro adicional (`product`) que es el parámetro en el
  que se irán guardando los cálculos intermedios
* Al final de la recursión el factorial debe estar calculado en
  `product` y se devuelve

```
(define (factorial-iter n)
   (fact-iter-aux n n))

(define (fact-iter-aux product n)
   (if (= n 1)
      product
      (fact-iter-aux (* product (- n 1))  (- n 1))))
```

Secuencia de llamadas:

```
(factorial-iter 4)
(factorial-iter-aux 4 4)
(factorial-iter-aux 12 3)
(factorial-iter-aux 24 2)
(factorial-iter-aux 24 1)
24
```

##### 1.3.2. Versión iterativa de mi-length

¿Cómo sería la versión iterativa de mi-length?

Solución:

```scheme
(define (mi-length-iter lista)
   (mi-length-iter-aux lista 0))

(define (mi-length-iter-aux lista result)
   (if (null? lista)
      result
      (mi-length-iter-aux (cdr lista) (+ result 1))))
```

##### 1.3.3. Procesos iterativos

* La recursión resultante es menos elegante
* Se necesita una parámetro adicional en el que se van acumulando los
  resultados parciales
* La última llamada a la recursión devuelve el valor acumulado
* El proceso resultante de la recursión es iterativo en el sentido de
  que no deja llamadas en espera ni incurre en coste espacial

##### 1.3.4 Fibonacci iterativo

Cualquier programa recursivo se puede transformar en otro que genera
un proceso iterativo.

En general, las versiones iterativas son menos intuitivas y más
difíciles de entender y depurar.

Ejemplo: Fibonacci iterativo

```scheme
(define (fib-iter n)
   (fib-iter-aux 1 0 n))

(define (fib-iter-aux a b count)
   (if (= count 0)
      b
      (fib-iter-aux (+ a b) a (- count 1))))
```

##### 1.3.5. Triángulo de Pascal

```
1
1   1
1   2   1
1   3   3   1
1   4   6   4   1
1   5  10   10  5   1
1   6  15  20   15  6   1
1   7  21  35   35  21  7   1
          ...
```

Formulación matemática:

```
> Pascal (0,0) = Pascal (1,0) = Pascal (1,1) = 1  
> Pascal (fila, columna) = Pascal (fila-1,columna-1) + Pascal (fila-1, columna)
```

La versión recursiva pura:

```scheme
(define (pascal row col)
   (cond ((= col 0) 1)
         ((= col row) 1)
         (else (+ (pascal (- row 1) (- col 1))
               (pascal (- row 1) col) ))))
(pascal 4 2)
⇒ 6
(pascal 8 4)
⇒ 70
(pascal 27 13)
⇒ 20058300
```

La versión iterativa:

```scheme
(define (pascal-iter fila col)
   (list-ref (pascal-iter-aux '(1 1) fila) col))

(define (pascal-iter-aux fila n)
   (if (= n (length fila))
      fila
      (pascal-iter-aux (pascal-sig-fila fila) n)))

(define (pascal-sig-fila fila)
   (append '(1)
	       (pascal-sig-fila-central fila)
           '(1)))

(define (pascal-sig-fila-central fila)
   (if (= 1 (length fila))
      '()
      (append (list (+ (car fila) (car (cdr fila))))
	          (pascal-sig-fila-central (cdr fila)))))
```

#### <a name="1-4"></a> 1.4. Soluciones al coste de la recursión: memoization

Una alternativa que mantiene la elegancia de los procesos recursivos y
la eficiencia de los iterativos es la
[memoization](http://en.wikipedia.org/wiki/Memoization). Si miramos la
traza de `(fibonacci 4)` podemos ver que el coste está producido por
la repetición de llamadas; por ejemplo `(fibonacci 3)` se evalúa 2
veces.

En programación funcional la llamada a `(fibonacci 3)` siempre va a
devolver el mismo valor.

Podemos guardar el valor devuelto por la primera llamada en alguna
estructura (una lista de asociación, por ejemplo) y no volver a
realizar la llamada a la recursión las siguientes veces.

##### 1.4.1. Fibonacci con memoization

Usamos los métodos procedurales `put` y `get` que implementan un
diccionario *clave-valor* (para probarlos hay que importar la librería
de Scheme que permite mutar las parejas):

```scheme
(import (rnrs)
      (rnrs mutable-pairs))

(define lista (list '*table*))

(define (get key lista)
   (let ((record (assq key (cdr lista))))
      (if (not record)
         '()
         (cdr record))))

(define (put key value lista)
   (let ((record (assq key (cdr lista))))
      (if (not record)
	      (set-cdr! lista
	         (cons (cons key value)
                (cdr lista)))
	      (set-cdr! record value)))
	'ok)
```

La función `(put key value lista)` asocia un valor a una clave y la
guarda en la lista (con mutación).

La función `(get key lista)` devuelve el valor de la lista asociado a
una clave.

Ejemplos:

```
(define mi-lista (list '*table*))
(put 1 10 mi-lista)
(get 1 mi-lista) ; ⇒ 10
(get 2 mi-lista) ; ⇒ '()
```

La función `fib-memo` realiza el cálculo de la serie de Fibonacci
utilizando el proceso recursivo visto anteriormente y la técnica de
memoización, en la que se consulta el valor de Fibonacci de la lista
antes de realizar la llamada recursiva:

```scheme
(define (fib-memo n lista)
   (cond ((= n 0) 0)
         ((= n 1) 1)
         ((not (null? (get n lista)))
          (get n lista))
         (else (let ((result (+ (fib-memo (- n 1) lista)
                                (fib-memo (- n 2) lista))))
                   (begin
                      (put n result lista)
                      result)))))
```

Podemos comprobar la diferencia de tiempos de ejecución entre esta
versión y la anterior. El coste de la función *memoizada* es
O(n). Frente al coste O(2^n) de la versión inicial que la hacía
imposible de utilizar.

```scheme
(define lista (list '*table*))
(fib-memo 200 lista)
⇒ 280571172992510140037611932413038677189525
```

#### <a name="1-5"></a> 1.5. Recursión y gráficos de tortuga

Vamos a terminar el apartado sobre procedimientos recursivos con un
último ejemplo algo distinto de los vistos hasta ahora. Usaremos la
recursión para dibujar figuras fractale usando los denominados
*gráficos de tortuga*. Para dibujar las figuras tendremos que utilizar
un estilo de programación no funcional, dibujando los distintos trazos
de las figuras con pasos de ejecución secuenciales. Para ello usaremos
una primitiva imperativa de Scheme: la forma especial `begin` que
permite realizar un grupo de pasos de ejecución de forma secuencial.

Ten cuidado con la forma especial `begin`, es una forma especial
imperativa. No debes usarla en la implementación de ninguna función
cuando estemos usando el paradigma funcional.

##### 1.5.1. Gráficos de tortuga en Racket

Se pueden utilizar los
[gráficos de tortuga](http://en.wikipedia.org/wiki/Turtle_graphics) en
Racket cargando la librería `(graphics turtles)`:

```
#lang r6rs
(import (rnrs)
      (graphics turtles))
```

Los comandos más importantes de esta librería son:

* `(turtles #t)`: abre una ventana y coloca la tortuga en el centro,
  mirando hacia el eje X (derecha)
* `(clear)`: borra la ventana y coloca la tortuga en el centro
* `(draw d)`: avanza la tortuga dibujando *d* píxeles 
* `(move d)`: mueve la tortuga *d* píxeles hacia adelante (sin dibujar)
* `(turn g)`: gira la tortuga *g* grados (positivos: en el sentido
  contrario a las agujas del reloj)

Prueba a realizar algunas figuras con los comandos de tortuga, antes
de escribir el algoritmo en Scheme del triángulo de Sierpinski.

Por ejemplo, podemos definir una función que dibuja un triángulo
rectángulo con catetos de longitud `x`:

```scheme
(define (hipot x)
	(* x (sqrt 2)))

(define (triangulo-rectangulo x)
   (begin
      (draw x)
      (turn 90)
      (draw x)
      (turn 135)
      (draw (hipot x))
      (turn 135)))

(triangulo-rectangulo 100)
```

La función `(hipot x)` devuelve la longitud de la hipotenusa de un
triángulo rectángulo con dos lados de longitud `x`. O sea, la
expresión:

$$hipot(x) = \sqrt{x^2+x^2} = x \sqrt{2}$$

Como puedes comprobar, el código es imperativo. La forma especial
`begin` permite realizar una serie de pasos de ejecución que modifican
el estado (posición y orientación) de la *tortuga* y dibujan los
trazos de la figura.

El siguiente código es una variante del anterior que dibuja un
triángulo rectángulo de base `w` y lados `w/2`. Va a ser la figura
base del triángulo de Sierpinski.

```scheme
(define (triangle w)
   (begin
      (draw w)
      (turn 135)
      (draw (hipot (/ w 2)))
      (turn 90)
      (draw (hipot (/ w 2)))
      (turn 135)))
```

##### 1.5.2. Triángulo de Sierpinski

<img src="imagenes/sierpinski.png" width="400px"/>

*Triángulo de Sierpinski*

- ¿Ves alguna recursión en la figura?
- ¿Cuál podría ser el parámetro de la función que la dibujara? 
- ¿Se te ocurre un algoritmo recursivo que la dibuje?

La figura es *autosimilar* (una característica de las figuras
fractales). Una parte de la figura es idéntica a la figura total, pero
reducida de escala. Esto nos da una pista de que es posible dibujar la
figura con un algoritmo recursivo.

Para intentar encontrar una forma de enfocar el problema, vamos a
pensarlo de la siguiente forma: supongamos que tenemos un triángulo de
Sierpinski de anchura *h* y altura *h/2* con su esquina inferior
izquierda en la posición 0,0. ¿Cómo podríamos construir **el
siguiente** triángulo de Sierpinski?.

Podríamos construir un triángulo de Sierpinski más grande dibujando 3
veces el mismo triángulo, pero en distintas posiciones:

1. Triángulo 1 en la posición (0,0)
2. Triángulo 2 en la posición (h/2,h/2)
3. Triángulo 3 en la posición (h,0)

El algoritmo recursivo se basa en la misma idea, pero *hacia
atrás*. Debemos intentar dibujar un triángulo de altura *h* situado en
la posición *x*, *y* basándonos en 3 llamadas recursivas a triángulos
más pequeños. En el caso base, cuando *h* sea menor que un umbral,
dibujaremos un triángulo de lado *h* y altura *h/2*:

O sea, que para dibujar un triángulo de Sierpinski de base *h* y
altura *h/2* debemos:

* Dibujar tres triángulos de Sierpinsky de la mitad del tamaño del
  original (*h/2*) situadas en las posiciones *(x,y)*, *(x+h/4,
  y+h/4)* y *(x+h/2,y)*
* En el caso base de la recursión, en el que *h* es menor que una
  constante, se dibuja un triángulo de base *h* y altura *h/2*.

Una versión del algoritmo en *pseudocódigo*:

```
Sierpinsky (x, y, h):
   if (h > MIN) {
      Sierpinsky (x, y, h/2)
      Sierpinsky (x+h/4, y+h/4, h/2)
      Sierpinsky (x+h/2, y, h/2)
   } else dibujaTriangulo (x, y, h)
```		


##### 1.5.3. Sierpinski en Racket

La siguiente es una versión imperativa del algoritmo que dibuja el
triángulo de Sierpinski. No es funcional porque se realizan *pasos de
ejecución*, usando la forma especial `begin` o múltiples instrucciones
en una misma función (por ejemplo la función `triangle`).

```
#lang r6rs
(import (rnrs)
        (graphics turtles))

(turtles #t)

(define (hipot x)
	(* x (sqrt 2)))

(define (triangle w)
   (begin
      (draw w)
      (turn 135)
      (draw (hipot (/ w 2)))
      (turn 90)
      (draw (hipot (/ w 2)))
      (turn 135)))
      
(define (sierpinski w)
   (if (> w 20)
      (begin
         (sierpinski (/ w 2))
         (move (/ w 4)) (turn 90) (move (/ w 4)) (turn -90)
         (sierpinski (/ w 2))
         (turn -90) (move (/ w 4)) (turn 90) (move (/ w 4))
         (sierpinski (/ w 2))
         (turn 180) (move (/ w 2)) (turn -180)) ;; volvemos a la posición original
      (triangle w)))
```

La llamada a

```scheme
(sierpinski 40)
```

produce la siguiente figura:

<img src="imagenes/sierpinski-40.png"/>

La llamada a

```scheme
(sierpinski 700)
```

Produce la figura que vimos al principio del apartado:

<img src="imagenes/sierpinski.png" width="400px"/>

Para ocupar la venta completa debemos desplazar la tortuga hacia atrás
antes de invocar a `sierpinski`:

```scheme
(clear)
(move -350)
(sierpinski 700)
```

##### 1.5.4. Recursión mutua

En la recursión mutua definimos una función en base a una segunda, que
a su vez se define en base a la primera.

También debe haber un caso base que termine la recursión

Por ejemplo:

- x es par si x-1 es impar
- x es impar si x-1 es par
- 0 es par

Programas en Scheme:

```scheme
(define (par? x)
   (if (= 0 x)
      #t
      (impar? (- x 1))))

(define (impar? x)
   (if (= 0 x)
      #f
      (par? (- x 1))))
```

##### 1.5.5. Ejemplo avanzado: curvas de Hilbert

La curva de Hilbert es una curva fractal que tiene la propiedad de
rellenar completamente el espacio

Su dibujo tiene una formulación recursiva:

<img src="imagenes/hilbert.png" width="600px"/>

La curva H3 se puede construir a partir de la curva H2. El algoritmo
recursivo se formula dibujando la curva i-ésima a partir de la curva
i-1.

Para dibujar una curva de Hilbert de orden i a la *derecha* de la tortuga:

	1. Gira la tortuga -90
	2. Dibuja una curva de orden i-1 a la izquierda
	3. Avanza w dibujando
	4. Gira 90
	5. Dibuja una curva de orden i-1 a la derecha
	6. Avanza w dibujando
	7. Dibuja una curva de orden i-1 a la derecha
	8. Gira 90
	9. Avanza w dibujando
	10. Dibuja una curva de orden i-1 a la izquierda
	11. Gira -90

El algoritmo para dibujar a la izquierda es simétrico.

Como en la curva de Sierpinsky, utilizamos la librería
`graphics/turtles`, que permite usar la tortuga de Logo con los
comandos de Logo `draw` y `turn`. Definimos dos funciones simétricas,
la función `(h-der i long)` que dibuja una curva de Hilbert de orden
`i` con una longitud de trazo `long` a la *derecha* de la tortuga y la
función `(h-izq i w)` que dibuja una curva de Hilbert de orden `i` con
una longitud de trazo `long` a la *izquierda* de la tortuga.

El algoritmo en Scheme:

```scheme
#lang r6rs
(import (rnrs)
      (graphics turtles))

(define (h-der i long)
   (if (> i 0)
      (begin
        (turn -90)
        (h-izq (- i 1) long)
        (draw  long)
        (turn 90)
        (h-der (- i 1) long)
        (draw long)
        (h-der (- i 1) long)
        (turn 90)
        (draw long)
        (h-izq (- i 1) long)
        (turn -90))))

(define (h-izq i long)
   (if (> i 0)
     (begin
      (turn 90)
      (h-der (- i 1) long)
      (draw  long)
      (turn -90)
      (h-izq (- i 1) long)
      (draw long)
      (h-izq (- i 1) long)
      (turn -90)
      (draw long)
      (h-der (- i 1) long)
      (turn 90))))
```

Podemos probarlo con distintos parámetros de grado de curva y longitud
de trazo.

Curva de Hilbert de nivel 3 con trazo de longitud 20:

```scheme
(clear)
(move -350)
(turn -90)
(move 350)
(turn 90)
(h-izq 3 20)
```

Curva de Hilbert de nivel 6 con trazo de longitud 10:

```scheme
(clear)
(move -350)
(turn -90)
(move 350)
(turn 90)
(h-izq 6 10)
```

Curva de Hilbert de nivel 7 con trazo de longitud 5:

```scheme
(clear)
(move -350)
(turn -90)
(move 350)
(turn 90)
(h-izq 7 5))
```

<img src="imagenes/hilbert-scheme.png"/>

### <a name="2"></a> 2. Listas estructuradas

Hemos visto que las listas en Scheme se implementan como un estructura
de datos recursiva, formada por una pareja que enlaza en su parte
derecha el resto de la lista y que termina con una parte derecha en la
que hay una lista vacía.

En este apartado vamos a volver a estudiar las listas desde un nivel
de abstracción alto, usando las funciones:

- `(car lista)` para obtener el primer elemento de una lista
- `(cdr lista)` para obtener el resto de la lista
- `(cons dato lista)` para construir una nueva lista con el dato como
  primer elemento

En la mayoría de funciones y ejemplos que hemos visto hasta ahora las
listas están formadas por datos y el recorrido por la lista es un
recorrido lineal, iterando por sus elementos.

En este apartado vamos a ampliar este concepto y estudiar cómo
trabajar con *listas que contienen otras listas*.

#### <a name="2-1"></a> 2.1. Definición y ejemplos

Las listas en Scheme pueden tener cualquier tipo de elementos,
incluido otras listas.

Llamaremos **lista estructurada** a una lista que contiene otras
sublistas. Lo contrario de lista estructurada es una **lista plana**,
una lista formada por elementos que no son listas. Llamaremos
**hojas** a los elementos de una lista que no son sublistas.

A las listas estructuradas cuyas hojas son símbolos se les denomina en
el contexto de la programación funcional _expresiones-S_
([S-expression](http://en.wikipedia.org/wiki/S-expression)).

Por ejemplo, la lista estructurada:

```
{a b {c d e} {f {g h}}}
```

es una lista estructurada con 4 elementos:

- El elemento `'a`, una hoja
- El elemento `'b`, otra hoja
- La lista plana `{c d e}`
- La lista estructurada `{f {g h}}`

Se puede construir con cualquiera de las siguientes expresiones:

```
(define lista (list 'a 'b (list 'c 'd 'e) (list 'f (list 'g 'h))))
(define lista '(a b (c d e) (f (g h))))
```

Una lista formada por parejas la consideraremos una lista plana, ya
que no contiene ninguna sublista. Por ejemplo, la lista

```
{{a . 3} {b . 5} {c . 12}}
```

es una lista plana de tres elementos (hojas) que son parejas.

##### 2.1.1. Definiciones en Scheme

Vamos a escribir las definiciones anteriores de `hoja`, `plana` y
`estructurada` usando código de Scheme.

###### Función `(hoja? dato)`

Un dato es una hoja si no es una lista:

```
(define (hoja? dato)
   (not (list? dato)))
```

Utilizaremos esta función para comprobar si un determinado elemento de
una lista es o no una hoja. Por ejemplo, supongamos la siguiente
lista:

```
{{1 2} 3 4 {5 6}}
```

Es una lista de 4 elementos, siendo el primero y el último otras
sublistas y el segundo y el tercero hojas. Podemos comprobar si son o
no hojas sus elementos:

```
(define lista '((1 2) 3 4 (5 6)))
(hoja? (car lista)) ; ⇒ #f
(hoja? (cadr lista)) ; ⇒ #t
(hoja? (caddr lista)) ; ⇒ #t
(hoja? (cadddr lista)) ; ⇒ #f
```

La lista vacía no es una hoja

```
(hoja? '()) ; ⇒ #f
```

###### Función `(plana? lista)`

Una definición recursiva de lista plana:

>Una lista es plana si y solo si el primer elemento es una hoja y el
>resto es plana.

Y el caso base:

>Una lista vacía es plana.

Usando esta definición recursiva, podemos implementar en Scheme la
función `(plana? lista)` que comprueba si una lista es plana:

```
(define (plana? lista)
   (or (null? lista)
       (and (hoja? (car lista))
            (plana? (cdr lista)))))
```

Ejemplos:

```scheme
(plana? '(a b c d e f)) ; ⇒ #t
(plana? (list (cons 'a 1) "Hola" #f)) ; ⇒ #t
(plana? '(a (b c) d)) ; ⇒ #f
(plana? '(a () b)) ; ⇒ #f
```


###### Función `(estructurada? lista)`

Una lista es estructurada cuando alguno de sus elementos es otra lista:

```
(define (estructurada? lista)
   (if (null? lista)
      #f
      (or (list? (car lista))
          (estructurada? (cdr lista)))))
```

Ejemplos:

```scheme
(estructurada? '(1 2 3 4)) ; ⇒ #f
(estructurada? (list (cons 'a 1) (cons 'b 2) (cons 'c 3))) ; ⇒ #f
(estructurada? '(a () b)) ; ⇒ #t
(estructurada? '(a (b c) d)) ; ⇒ #t
```

Realmente bastaría con haber hecho una de las dos definiciones y
escribir la otra como la negación de la primera:

```
(define (estructurada? lista)
   (not (plana? lista)))
```

##### 2.1.2. Ejemplos de listas estructuradas

Las listas estructuradas son muy útiles para representar información
jerárquica en donde queremos representar elementos que contienen otros
elementos.

Por ejemplo, las expresiones de Scheme son listas estructuradas:

	(= 4 (+ 2 2))
	(if (= x y) (* x y) (+ (/ x y) 45))
	(define (factorial x) (if (= x 0) 1 (* x (factorial (- x 1)))))

El análisis sintáctico de una oración puede generar una lista
estructurada de símbolos, en donde se agrupan los distintos elementos
de la oración:

	{{Juan} {compró} {la entrada {de los Miserables}} {el viernes por la tarde}}

Una página HTML, con sus distintos elementos, unos dentro de otros,
también se puede representar con una lista estructurada:

	{{<h1> Mi lista de la compra </h1>}
	  {<ul> {<li> naranjas </li>}
            {<li> tomates </li>}
            {<li> huevos </li>} </ul>}}


##### 2.1.3. *Pseudo árboles* con niveles

Las listas estructuradas definen una estructura de niveles, donde la
lista inicial representa el primer nivel, y cada sublista representa
un nivel inferior. Los datos de las listas representan las hojas.

Por ejemplo, la representación en forma de niveles de la lista `{{a b
c} d e}` es la siguiente:

<img src="imagenes/expresion-e-1.png" width="350px"/>

Las hojas `d` y `e` están en el nivel 1 y en las posiciones 2 y 3 de
la lista y las hojas `a`, `b` y `c` en el nivel 2 y en la posición 1
de la lista.

> UNA LISTA ESTRUCTURADA NO ES UN ÁRBOL  
> Una lista estructurada no es un árbol propiamente dicho, porque
> todos los datos están en las hojas.

Otro ejemplo. ¿Cuál sería la representación en niveles de la siguiente
lista estructurada?:

	{let {{x 12}
	      {y 5}}
	   {+ x y}}}

<img src="imagenes/expresion-e-2.png" width="300px"/>

#### <a name="2-2"></a> 2.2. Funciones recursivas sobre listas estructuradas

##### 2.2.1. Número de hojas

Veamos como primer ejemplo la función `(num-hojas lista)` que cuenta
el número de hojas de una lista estructurada.

Por ejemplo:

```
(num-hojas '((1 2) (3 4 (5) 6) (7))) ⇒ 7
```

Podemos definir la función obteniendo el primer elemento y el resto de
la lista, y contando recursivamente el número de hojas del primer
elemento y del resto. Al ser una lista estructurada, el primer
elemento puede ser a su vez otra lista, por lo que llamamos a la
recursión para contar sus hojas.

La definición de este caso general usando _pseudocódigo_ es:

> El número de hojas de una lista estructurada es la suma del número
> de hojas de su primer elemento (que puede ser otra lista) y del
> número de hojas del resto.

<img src="imagenes/num-hojas-estructurada.png" width="400px"/>

Como casos base, podemos considerar cuando la lista es vacía (el
número de hojas es 0) y cuando la lista no es tal, sino que es un dato
(una hoja), en cuyo caso es 1. La implementación en Scheme es:


```
(define (num-hojas lista)
   (cond
      ((null? lista) 0)
      ((hoja? lista) 1)
      (else (+ (num-hojas (car lista))
               (num-hojas (cdr lista))))))
```

Hay que hacer notar que el parámetro `lista` puede ser tanto una lista
como un dato atómico. Estamos aprovechándonos de la característica de
Scheme de ser débilmente tipeado para hacer un código bastante
conciso.


###### Versión con funciones de orden superior

Podemos usar también las funciones de orden superior `map` y
`fold-right` para obtener una versión más concisa.

Una lista estructurada tiene como elementos hojas o listas. Podemos
entonces mapear una expresión lambda con _la propia función que
estamos definiendo_ sobre sus elementos, poniendo como caso especial
el hecho de que la lista sea una hoja. El resultado será una lista de
números (el número de hojas de cada componente), que podemos sumar
haciendo un `fold-right` con la función `+`:

```
(define (num-hojas-fos lista)
    (if (hoja? lista) 
        1
        (fold-right + 0 (map num-hojas-fos lista))))
```

Una versión alternativa de la función anterior es la siguiente (también correcta):

```scheme
(define (num-hojas-fos lista)
    (fold-right + 0 (map (lambda (sublista)
                           (if (hoja? sublista)
                               1
                               (num-hojas-fos sublista))) lista)))
```


##### 2.2.2. Altura de una lista estructurada

La *altura* de una lista estructurada viene dada por su número de
niveles: una lista plana tiene una altura de 1, la lista `'((1 2 3) 4
5)` tiene una altura de 2.

Para calcular la altura de una lista estructurada tenemos que obtener
(de forma recursiva) la altura de su primer elemento, y la altura del
resto de la lista, sumarle 1 a la altura del primer elemento y
devolver el máximo de los dos números.

<img src="imagenes/altura-estructurada.png" width="300px"/>

Como casos base, la altura de una lista vacía o de una hoja (dato) es 0.


En Scheme:

```
(define (altura lista)
   (cond 
      ((null? lista) 0)
      ((hoja? lista) 0)
      (else (max (+ 1 (altura (car lista)))
                 (altura (cdr lista))))))

```

Por ejemplo:

```
(altura '(1 (2 3) 4)) ⇒ 2
(altura '(1 (2 (3)) 3)) ⇒ 3
```

###### Versión con funciones de orden superior

Y la segunda versión, usando las funciones de orden superior `map`
para obtener la altura de las sublistas y `fold-right` para quedarse
con el máximo.

```
(define (altura-fos lista)
    (if (hoja? lista)
        0
        (+ 1 (fold-right max 0 (map altura-fos lista)))))
```

Otra versión de esta función, también correcta:

```scheme
(define (altura-fos lista)
   (+ 1 (fold-right max 0 (map (lambda (sublista)
                                 (if (hoja? sublista)
                                     0
                                     (altura-fos sublista))) lista))))
```

##### 2.2.3. Otras funciones recursivas

Vamos a diseñar otras funciones recursivas que trabajan con la
estructura jerárquica de las listas estructuradas.

* `(pertenece-lista? dato lista)`: busca una hoja en una lista
  estructurada
* `(nivel-lista dato lista)`: devuelve el nivel en el que se encuentra
  un dato en una lista
* `(cuadrado-lista lista)`: eleva todas las hojas al cuadrado
  (suponemos que la lista estructurada contiene números)
* `(map-lista f lista)`: similar a map, aplica una función a todas las
  hojas de la lista estructurada y devuelve el resultado (otra lista
  estructurada)

###### `(pertenece-lista? dato lista)`

Comprueba si el dato x aparece en la lista estructurada. 

```scheme
(define (pertenece? x lista)
  (cond 
    ((null? lista) #f)
    ((hoja? lista) (equal? x lista))
    (else (or (pertenece? x (car lista))
              (pertenece? x (cdr lista))))))
```

Ejemplos:

```scheme
(pertenece? 'a '(b c (d (a)))) ⇒ #t
(pertenece? 'a '(b c (d e (f)) g)) ⇒ #f
```

###### `(nivel-lista dato lista)`

Veamos por último la función `(nivel-lista dato lista)` que recorre la
lista buscando el dato y devuelve el nivel en que se encuentra. Si el
dato no se encuentra en la lista, se devolverá 0. Si el dato se
encuentra en más de un lugar de la lista se devolverá el nivel del
primero que se encuentre.

Ejemplos:

```scheme
(nivel-hoja 'b '(a b (c))) ; ⇒ 1
(nivel-hoja 'b '(a (b) c)) ; ⇒ 2
(nivel-hoja 'b '(a c d ((b))) ; ⇒ 4
(nivel-hoja 'b '(a c d ((e)))) ; ⇒ 0
```

Vamos a implementar la función con una recursión por la cola en la que
pasamos como parámetro el nivel en el que se encuentra la recursión.

En el parámetro `lista` se pasa o bien una lista o un elemento y se
comprueba si el elemento es igual que el dato. En el caso en que lo
sea se devuelve el `nivel` actual. Y en el caso en sea una lista, se
llama a la recursión con su primer elemento (aumentando el nivel) y se
guarda el resultado en la variable local `nivel-dato-car` usando un
`let`. Se comprueba si se ha encontrado el dato comprobando si el
valor es mayor de 0. En el caso en que no se encuentre el dato, se
continua buscando por el resto de la lista (sin aumentar el nivel).

```
(define (nivel-lista dato lista)
    (nivel-lista-iter dato 0 lista))

(define (nivel-lista-iter dato nivel lista)
    (cond
        ((null? lista) 0)
        ((hoja? lista) (if (equal? lista dato)
                           nivel
                           0))
        (else (let ((nivel-dato-car (nivel-lista-iter dato (+ nivel 1) (car lista))))
                 (if (> nivel-dato-car 0)
                     nivel-dato-car
                     (nivel-lista-iter dato nivel (cdr lista)))))))
```

###### `(cuadrado-lista lista)`

Devuelve una lista estructurada con la misma estructura y sus números
elevados al cuadrado.

```scheme
(define (cuadrado-lista lista)
  (cond ((null? lista) '())
        ((hoja? lista) (* lista lista))
        (else (cons (cuadrado-lista (car lista))
                    (cuadrado-lista (cdr lista))))))
```

Por ejemplo:

```scheme
(cuadrado-lista '(2 3 (4 (5)))) ⇒ (4 9 (16 (25))
```

Es muy interesante la versión de esta función con funciones de orden
superior:

```scheme
(define (cuadrado-lista-fos lista)
    (map (lambda (sublista)
           (if (hoja? sublista)
               (* sublista sublista)
               (cuadrado-lista-fos sublista))) lista))
```

Como una lista estructurada está compuesta de datos o de otras
sublistas podemos aplicar `map` para que devuelva la lista resultante
de transformar la original con la función que le pasamos como
parámetro.

###### `(map-lista f lista)`

Devuelve una lista estructurada igual que la original con el resultado
de aplicar a cada uno de sus hojas la función f
 
```
(define (map-lista f lista)
  (cond ((null? lista) '())
        ((hoja? lista) (f lista))
        (else (cons (map-lista f (car lista))
                    (map-lista f (cdr lista))))))
```
	
Por ejemplo:

```
(map-lista (lambda (x) (* x x)) '(2 3 (4 (5)))) ⇒ (4 9 (16 (25))
```

### <a name="3"></a>3. Árboles

#### <a name="3-1"></a>3.1. Definición de árboles en Scheme

##### 3.1.1. Definición de árbol

Un **árbol** es una estructura de datos definida por un valor raíz,
que es el padre de toda la estructura, del que salen otros subárboles
hijos
([Wikipedia](https://en.wikipedia.org/wiki/Tree_(data_structure))).

Un **árbol** se puede definir recursivamente de la siguiente forma:

- Una colección de un **dato** (el valor de la raíz del árbol) y una
  **lista de hijos** que también son árboles.
- Una **hoja** será un árbol sin hijos (un dato con una lista de hijos
  vacía).

Un ejemplo de árbol:

<img src="imagenes/arbol-sencillo.png" width="250px"/>

El árbol anterior tiene como dato de la raíz es el símbolo `+` y tiene
3 árboles hijos:

<img src="imagenes/arboles-hijos.png" width="300px"/>

- El primer hijo es un árbol hoja, con valor 5 y sin hijos
- El segundo hijo es un árbol con valor `*` y dos hijos hoja, el 2 y
  el 3
- El tercer hijo es otro árbol hoja, con valor 10

##### 3.1.2. Representación de árboles con listas

En Scheme tenemos como estructura de datos principal la lista. ¿Cómo
construimos un árbol usando listas?

La forma de hacerlo será usar **una lista de _n+1_ elementos** para
representar un árbol con n hijos:

- el primer elemento la lista será el dato de la raíz
- el resto serán los árboles hijos

> Árbol: '(dato hijo-1 hijo-2 ... hijo-n)

Los nodos hoja serán por tanto listas de un elemento, el propio dato
(no tiene más elementos porque no tiene hijos)

> Nodo hoja: '(dato)

Por ejemplo, el árbol anterior lo representaremos en Scheme con la
siguiente lista:

```scheme
{+ {5} {* {2} {3}} {10}}
```

Los elementos de esta lista son:

- El primer elemento es el símbolo `+`, el dato valor de la raíz del
  árbol
- El segundo elemento es la lista `{5}`, que representa el árbol hoja
  formado por un 5
- El tercer elemento es la lista `{* {2} {3}}`, que representa el
  árbol con un dato `*` y dos hijos
- El cuarto elemento es la lista `{10}`, que representa el árbol hoja
  formado por un 10

Podríamos definir el árbol con la siguiente sentencia:

```scheme
(define arbol1 '(+ (5) (* (2) (3)) (10)))
```

Otro ejemplo más. ¿Cómo se implementa en Scheme el árbol de la siguiente figura?

<img src="imagenes/binario-2.png" width="300px"/>

Se haría con la lista de la siguiente sentencia:

```scheme
(define arbol2 '(40 (18 (3) (23 (29))) (52 (47))))
```

##### 3.1.2. Barrera de abstracción

Una vez definida la forma de representar árboles, vamos a definir las
funciones para manejarlos. Veremos las funciones para obtener el dato
y los hijos y la función para construir un árbol nuevo. Estas
funciones proporcionarán lo que se denomina _barrera de abstracción_
del tipo datos *árbol*.

Una _barrera de abstracción_ es un conjunto de funciones que permiten
trabajar con un tipo de datos escondiendo su implementación. Por
convenio, en todas las funciones ponemos el mismo sufijo, el nombre
del tipo de dato, en este caso **tree** (vamos a hacer una mezcla un
poco extraña, escribiendo el nombre del tipo de dato en inglés, y el
nombre de las funciones en castellano).

Definimos dos conjuntos de funciones: **constructores** para construir
un nuevo árbol y **selectores** para obtener los elementos del
árbol. Vamos a empezar por los selectores.

**Selectores**

Funciones que obtienen los elementos de un árbol:

```scheme
(define (dato-tree arbol) 
    (car arbol))

(define (hijos-tree arbol) 
    (cdr arbol))

(define (hoja-tree? arbol) 
   (null? (hijos-tree arbol)))
```

Es importante tener claro los tipos devueltos por las dos primeras
funciones:

- `(dato-tree arbol)`: devuelve el dato de la raíz del árbol.
- `(hijos-tree arbol)`: devuelve una lista de árboles hijos. En
  algunas ocasiones llamaremos *bosque* a una lista de árboles.

Por ejemplo, en el árbol `arbol1` las funciones anteriores devuelven
los siguientes valores:

```scheme
(dato-tree arbol1) ; ⇒ +
(hijos-tree arbol1) ; ⇒ {{5} {* {2} {3}} {10}}
(hoja-tree? (car (hijos-tree arbol1))) ; ⇒ #t
```

- La llamada `(dato-tree arbol1)` devuelve el dato que hay en la raíz
  del árbol, el símbolo `+`
- La invocación `(hijos-tree arbol1)` devuelve una lista de tres
  elementos, los árboles hijos: `{{5} {* {2} {3}} {10}}`:
    - El primer elemento es la lista `{5}`, que representa el árbol
      hoja formado por el `5`
    - El segundo es la lista `{* {2} {3}}`, que representa el árbol
      formado por el `*` en su raíz y las hojas `2` y `3`
    - El tercero es la lista `{10}`, que representa el árbol hoja
      `10`.
- El primer elemento de la lista de hijos es un árbol hoja:
  `(hoja-tree? (car (hijos-tree arbol1))) ⇒ #t`
				   
Es muy importante considerar en cada caso con qué tipo de dato estamos
trabajando y usar la barrera de abstracción adecuada en cada caso:

- La función `hijos-tree` siempre devuelve una lista de árboles, que
  podemos recorrer usando `car` y `cdr`.
- El `car` de una lista de árboles (devuelta por `hijos-tree`) siempre
  es un árbol y debemos de usar las funciones de su barrera de
  abstracción: `dato-tree` e `hijos-tree`.
- La función `dato-tree` devuelve un dato de árbol, del tipo que
  guardemos en el árbol.

Por ejemplo, para obtener el número 2 en el árbol anterior tendríamos
que hacer lo siguiente: acceder al segundo elemento de la lista de
hijos, después al primer hijo de éste y por último acceder a su
dato. Recordemos que `hijos-tree` devuelve la lista de árboles hijos,
por lo que utilizaremos las funciones `car` y `cdr` para recorrerlas y
obtener los elementos que nos interesen:

```scheme
(dato-tree (car (hijos-tree (cadr (hijos-tree arbol1))))) ; ⇒ 2
```

**Constructores**

Funciones que permiten construir un nuevo árbol:

```scheme
(define (make-tree dato lista-arboles)  
   (cons dato lista-arboles))

(define (make-hoja-tree dato) 
    (make-tree dato '()))
```

- La función `(make-tree dato lista-arboles)` recibe un dato y una
  lista de árboles y devuelve un árbol formado por el dato en su raíz
  y la lista de hijos.
- La función `(make-hoja-tree dato)` recibe un dato y devuelve un
  árbol hoja (un árbol sin hijos).

El árbol anterior se puede construir con las siguientes llamadas a los
constructores:

```scheme
(make-tree '+ (list (make-hoja-tree 5)
                             (make-tree '* 
                                        (list (make-hoja-tree 2) 
                                              (make-hoja-tree 3)))
                             (make-hoja-tree 10)))
```

##### 3.1.3. Diferencia entre árboles y listas estructuradas

Es importante diferenciar la barrera de abstracción de los árboles de
la de las listas estructuradas. Aunque un árbol se implementa en
Scheme con una lista estructurada, a la hora de definir funciones
sobre árboles hay que trabajar con las funciones definidas arriba.

El siguiente esquema resumen las características de la barrera de
abstracción de listas y árboles:

<img src="imagenes/barrera-abstraccion.png" width="550px">

#### <a name="3-2"></a>3.2. Funciones recursivas sobre árboles

Vamos a diseñar las siguientes funciones recursivas:

* `(suma-datos-tree tree)`: devuelve la suma de todos los nodos
* `(to-list-tree tree)`: devuelve una lista con los datos del árbol
* `(cuadrado-tree tree)`: eleva al cuadrado todos los datos de un
  árbol manteniendo la estructura del árbol original
* `(map-tree f tree)`: devuelve un árbol con la estructura del árbol
  original aplicando la función f a subdatos.
* `(altura-tree tree)`: devuelve la altura de un árbol

Todas comparten un patrón similar de recursión mutua.

##### 3.2.1. `(suma-datos-tree tree)`

Vamos a implementar una función recursiva que sume todos los datos de
un árbol.

Un árbol siempre va a tener un dato y una lista de hijos (que puede
ser vacía) que obtenemos con las funciones `dato-tree` e
`hijos-tree`. Podemos plantear entonces el problema de sumar los datos
de un árbol como la suma del dato de su raíz y lo que devuelva la
llamada a una función auxiliar que sume los datos de su lista de hijos
(un bosque):

```scheme
(define (suma-datos-tree tree)
    (+ (dato-tree tree)
       (suma-datos-bosque (hijos-tree tree))))
```

Esta función suma los datos de **UN** árbol. La podemos utilizar
entonces para construir la siguiente función que suma una lista de
árboles:

```scheme
(define (suma-datos-bosque bosque)
   (if (null? bosque)
       0
       (+ (suma-datos-tree (car bosque)) (suma-datos-bosque (cdr bosque)))))
```

Tenemos una recursión mutua: para sumar los datos de una lista de
árboles llamamos a la suma de un árbol individual que a su vez llama a
la suma de sus hijos, etc. La recursión termina cuando calculamos la
suma de un árbol hoja. Entonces se pasa a `suma-datos-bosque` una
lista vacía y ésta devolverá 0.


```scheme
(suma-datos-tree arbol2) ; 212⇒
```

**Versión alternativa con funciones de orden superior**

Al igual que hacíamos con las listas estructuradas, es posible
conseguir una versión más concisa y elegante utilizando funciones de
orden superior:

```scheme
(define (suma-datos-tree-fos tree)
    (+ (dato-tree tree)
       (fold-right + 0 (map suma-datos-tree-fos (hijos-tree tree)))))
```	

La función `map` aplica la propia función que estamos definiendo a
cada uno de los árboles de `(hijos-tree tree)`, devolviendo una lista
de números. Esta lista de número la sumamos haciendo un `fold-right +
0`. Una traza de su funcionamiento sería la siguiente:

```scheme
(suma-datos-tree-fos '(1 (2 (3) (4)) (5) (6 (7)))) ⇒
(+ 1 (fold-right + 0 (map suma-datos-tree-fos '((2 (3) (4)) 
                                                (5)
                                                (6 (7)))))) ⇒
(+ 1 (fold-right + '(9 5 13))) ⇒
(+ 1 27) ⇒
28
```

##### 3.2.2. `(to-list-tree tree)`

Queremos diseñar una función `(to-list-tree tree)` que devuelva una
lista con los datos del árbol en un recorrido *preorden*.

```scheme
(define (to-list-tree tree)
   (cons (dato-tree tree)
         (to-list-bosque (hijos-tree tree))))

(define (to-list-bosque bosque)
   (if (null? bosque)
       '()
       (append (to-list-tree (car bosque))
               (to-list-bosque (cdr bosque)))))
```

La función utiliza una *recursión mutua*: para listar todos los nodos,
añadimos el dato a la lista de nodos que nos devuelve la función
`to-list-bosque`. Esta función coge una lista de árboles (un *bosque*)
y devuelve la lista *inorden* de sus nodos. Para ello, concatena la
lista de los nodos de su primer elemento (el primer árbol) a la lista
de nodos del resto de árboles (que devuelve la llamada recursiva).

Ejemplo:

```scheme
(to-list-tree '(* (+ (5) (* (2) (3)) (10)) (- (12)))) 
; ⇒ (* + 5 * 2 3 10 - 12)
```

Una definición alternativa usando funciones de orden superior:

```scheme
(define (to-list-tree-fos tree)
    (cons (dato-tree tree)
          (fold-right append '() (map to-list-tree-fos (hijos-tree tree)))))
```

Esta versión es muy elegante y concisa. Usa la función `map` que
aplica una función a los elementos de una lista y devuelve la lista
resultante. Como lo que devuelve `(hijos-tree tree)` es precisamente
una lista de árboles podemos aplicar a sus elementos cualquier función
definida sobre árboles. Incluso la propia función que estamos
definiendo (¡confía en la recursión!).

##### 3.2.3. `(cuadrado-tree tree)`

Veamos ahora la función `(cuadrado-tree tree)` que toma un árbol de
números y devuelve un árbol con la misma estructura y sus datos
elevados al cuadrado:

```scheme
(define (cuadrado-tree tree)
   (make-tree (cuadrado (dato-tree tree))
                   (cuadrado-bosque (hijos-tree tree))))  

(define (cuadrado-bosque bosque)
   (if (null? bosque)
       '()
       (cons (cuadrado-tree (car bosque))
               (cuadrado-bosque (cdr bosque)))))
```

Ejemplo:

```scheme
(cuadrado-tree '(2 (3 (4) (5)) (6))) 
; ⇒ (4 (9 (16) (25)) (36))
```

Versión 2, con `map`:

```scheme
(define (cuadrado-tree tree)
   (make-tree (cuadrado (dato-tree tree))
   	          (map cuadrado-tree (hijos-tree tree))))
```

##### 3.2.4. `map-tree`

La función `map-tree` es una función de orden superior que generaliza
la función anterior. Definimos un parámetro adicional en el que se
pasa la función a aplicar a los elementos del árbol.

```scheme
(define (map-tree f tree)
   (make-tree (f (dato-tree tree))
              (map-bosque f (hijos-tree tree))))  

(define (map-bosque f bosque)
   (if (null? bosque)
       '()
       (cons (map-tree f (car bosque))
             (map-bosque f (cdr bosque)))))
```

Ejemplos:

```scheme
(map-tree cuadrado '(2 (3 (4) (5)) (6)))
; ⇒ (4 (9 (16) (25)) (36))
(map-tree (lambda (x) (+ x 1)) '(2 (3 (4) (5)) (6)))
; ⇒ (3 (4 (5) (6)) (7))
```

Con `map`:

```scheme
(define (map-tree f tree)
  (make-tree (f (dato-tree tree))
             (map (lambda (x)
                    (map-tree f x)) (hijos-tree tree))))
```


##### 3.2.5. `altura-tree`

Vamos por último a definir una función que devuelve la altura de un
árbol (el nivel del nodo de mayor nivel). Un nodo hoja tiene de altura
0.

Solución 1:

```scheme
(define (altura-tree tree)
   (if (hoja-tree? tree)
       0
       (+ 1 (max-altura-bosque (hijos-tree tree)))))  

(define (max-altura-bosque bosque)
    (if (null? bosque)
        0
        (max (altura-tree (car bosque))
             (max-altura-bosque (cdr bosque)))))
```

Ejemplos:


```scheme
(altura-tree '(2)) ;  ⇒ 0
(altura-tree '(4 (9 (16) (25)) (36))) ; ⇒ 2
```

Solución con funciones de orden superior:

La función `max-altura-bosque` puede implementarse de una forma más
concisa todavía usando las funciones con funciones de orden superior:

```scheme
(define (max-altura-bosque-fos bosque)
   (fold-right max 0 (map altura-tree bosque)))
```
	
La función `map` mapea la función `altura-tree` a todos los elementos
del *bosque* (lista de árboles) devolviendo una lista de números, de
la que obtenemos el máximo plegando la lista con la función `max`.

----

Lenguajes y Paradigmas de Programación, curso 2015-16  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Domingo Gallardo, Cristina Pomares
