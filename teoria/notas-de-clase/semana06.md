
----

### Tema 3: Procedimientos y estructuras recursivas

Notas de clase Semana 6   
[Enlace a los apuntes](http://domingogallardo.github.io/lpp/teoria/Tema03-ProcedimientosEstructurasRecursivas.html#1-5)

----

### Contenidos

- 1 Procedimientos recursivos
    - 1.1 Pensando recursivamente
	- 1.2 El coste de la recursión
	- 1.3 Soluciones al coste de la recursión: procesos iterativos
	- 1.4. Soluciones al coste de la recursión: memoization
    - **1.5 Recursión y gráficos de tortuga**
- **2 Listas estructuradas**
    - **2.1. Definición y ejemplos**
    - **2.2. Funciones recursivas sobre listas estructuradas**
- 3 Árboles

----


### 1.5. Recursión y gráficos de tortuga

- Último ejemplo algo distinto de los vistos hasta ahora
- Usaremos la recursión para dibujar figuras fractales usando los denominados *gráficos de tortuga*
- Estilo de programación **no funcional**, dibujando los distintos trazos de las figuras con pasos de ejecución secuenciales con la primitiva imperativa `begin` 

> Ten cuidado con la forma especial `begin`, es una forma especial imperativa. No debes usarla en la implementación de ninguna función cuando estemos usando el paradigma funcional.

----

### 1.5.1. Gráficos de tortuga en Racket

- Los [gráficos de tortuga](http://en.wikipedia.org/wiki/Turtle_graphics) se crearon en el lenguaje de programación Logo, para enseñar a programar a niños y jóvenes, en los años 80.
- Se pueden utilizar los  en Racket cargando la librería `(graphics turtles)`: 

```scheme
#lang r6rs
(import (rnrs)
      (graphics turtles))
```

- Los comandos más importantes de esta librería son:
    - `(turtles #t)`: abre una ventana y coloca la tortuga en el centro, mirando hacia el eje X (derecha)
    - `(clear)`: borra la ventana y coloca la tortuga en el centro
    - `(draw d)`: avanza la tortuga dibujando *d* píxeles 
    - `(move d)`: mueve la tortuga *d* píxeles hacia adelante (sin dibujar)
    - `(turn g)`: gira la tortuga *g* grados (positivos: en el sentido contrario a las agujas del reloj)

-----

### Ejemplo `(triangulo-rectangulo)`


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

La función `(hipot x)` devuelve la longitud de la hipotenusa de un triángulo rectángulo con dos lados de longitud `x`. O sea, la expresión:

$$hipot(x) = \sqrt{x^2+x^2} = x \sqrt{2}$$

----

### Triángulo de base `w` y lados `w/2`

- El siguiente código es una variante del anterior que dibuja un triángulo rectángulo de base `w` y lados `w/2`. 
- va a ser la figura base del triángulo de Sierpinski.

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

----

### 1.5.2. Triángulo de Sierpinski

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/sierpinski.png" width="400px"/>

*Triángulo de Sierpinski*

- ¿Ves alguna recursión en la figura? 
- ¿Cuál podría ser el parámetro de la función que la dibujara? 
- ¿Se te ocurre un algoritmo recursivo que la dibuje?

----

### Primera idea del algoritmo recursivo

- Podríamos construir un triángulo de Sierpinski más grande dibujando 3 veces el mismo triángulo, pero en distintas posiciones:
    - Triángulo 1 en la posición (0,0)
    - Triángulo 2 en la posición (h/2,h/2)
    - Triángulo 3 en la posición (h,0)
- El algoritmo recursivo se basa en la misma idea, pero *hacia atrás*. Debemos intentar dibujar un triángulo de altura *h* situado en la posición *x*, *y* basándonos en 3 llamadas recursivas a triángulos más pequeños. En el caso base, cuando *h* sea menor que un umbral, dibujaremos un triángulo de lado *h* y altura *h/2*:

----

### Algoritmo recursivo 

- Para dibujar un triángulo de Sierpinski de base *h* y altura *h/2* debemos:
    - Dibujar tres triángulos de Sierpinsky de la mitad del tamaño del original (*h/2*) situadas en las posiciones *(x,y)*, *(x+h/4, y+h/4)* y *(x+h/2,y)*
    - En el caso base de la recursión, en el que *h* es menor que una constante, se dibuja un triángulo de base *h* y altura *h/2*.

Una versión del algoritmo en *pseudocódigo*:

```
Sierpinsky (x, y, h):
   if (h > MIN) {
      Sierpinsky (x, y, h/2)
      Sierpinsky (x+h/4, y+h/4, h/2)
      Sierpinsky (x+h/2, y, h/2)
   } else dibujaTriangulo (x, y, h)
```		

----

### 1.5.3. Sierpinski en Racket

```scheme
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

```scheme
(sierpinski 40)
```

```scheme
(clear)
(move -350)
(sierpinski 700)
```

----

### 1.5.4. Recursión mutua

- En la recursión mutua definimos una función en base a una segunda, que a su vez se define en base a la primera. 
- También debe haber un caso base que termine la recursión
- Por ejemplo:
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

----

### 1.5.5. Otra figura recursiva: curvas de Hilbert

- La curva de Hilbert es una curva fractal que tiene la propiedad de rellenar completamente el espacio

- Su dibujo tiene una formulación recursiva:

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/hilbert.png" width="600px"/>

La curva H3 se puede construir a partir de la curva H2. El algoritmo recursivo se formula dibujando la curva i-ésima a partir de la curva i-1.

----

### Algoritmo recursivo

- El algoritmo para dibujar una curva de Hilbert de orden 3 a la *izquierda* de la tortuga sería el siguiente (seguir los pasos con el dibujo):

	1. Gira la tortuga 90
	2. Dibuja una curva de orden 2 a la derecha
	3. Avanza w dibujando
	4. Gira -90
	5. Dibuja una curva de orden 2 a la izquierda
	6. Avanza w dibujando
	7. Dibuja una curva de orden 2 a la izquierda
	8. Gira -90
	9. Avanza w dibujando
	10. Dibuja una curva de orden 2 a la derecha
	11. Gira 90

- El algoritmo para dibujar a la izquierda es simétrico.

----

### Algoritmo en Scheme

```scheme
#lang r6rs
(import (rnrs)
      (graphics turtles))

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
```


Podemos probarlo con distintos parámetros de grado de curva y longitud de trazo.

Curva de Hilbert de nivel 3 con trazo de longitud 20:

```scheme
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

----

### 2. Listas estructuradas

- Recordemos las listas: una pareja que enlaza en su parte derecha el resto de la lista y que termina con una parte derecha en la que hay una lista vacía. 

- Funciones:

    - `(car lista)` para obtener el primer elemento de una lista
    - `(cdr lista)` para obtener el resto de la lista
    - `(cons dato lista)` para construir una nueva lista con el dato como primer elemento

- En este apartado vamos a estudiar cómo trabajar con *listas que contienen otras listas*. 

----

#### 2.1. Definición y ejemplos

- Llamaremos **lista estructurada** a una lista que contiene otras sublistas. Lo contrario de lista estructurada es una **lista plana**, una lista formada por elementos que no son listas. Llamaremos **hojas** a los elementos de una lista que no son sublistas.

- A las listas estructuradas cuyas hojas son símbolos se les denomina en el contexto de la programación funcional _expresiones-S_ ([S-expression](http://en.wikipedia.org/wiki/S-expression)).

- Por ejemplo, la lista estructurada:

    ```scheme
    {a b {c d e} {f {g h}}}
    ```

    es una lista estructurada con 4 elementos:

    - El elemento `'a`, una hoja
    - El elemento `'b`, otra hoja
    - La lista plana `{c d e}`
    - La lista estructurada `{f {g h}}`

- Una lista formada por parejas la consideraremos una lista plana, ya que no contiene ninguna sublista. Por ejemplo, la lista

    ```scheme
    {{a . 3} {b . 5} {c . 12}}
    ```

    es una lista plana de tres elementos (hojas) que son parejas.

----

### Función `(hoja? dato)`

- Un dato es una hoja si no es una lista:

    ```scheme
    (define (hoja? dato)
        (not (list? dato)))
    ```

- Por ejemplo, supongamos la siguiente lista:

    ```scheme
    {{1 2} 3 4 {5 6}}
    ```

    Es una lista de 4 elementos, siendo el primero y el último otras sublistas y el segundo y el tercero hojas. Podemos comprobar si son o no hojas sus elementos:

    ```scheme
    (define lista '((1 2) 3 4 (5 6)))
    (hoja? (car lista)) ; ⇒ #f
    (hoja? (cadr lista)) ; ⇒ #t
    (hoja? (caddr lista)) ; ⇒ #t
    (hoja? (cadddr lista)) ; ⇒ #f
    ```

- La lista vacía no es una hoja

    ```scheme
    (hoja? '()) ; ⇒ #f
    ```

----

### Función `(plana? lista)`

- Una definición recursiva de lista plana:

>Una lista es plana si y solo si el primer elemento es una hoja y el resto es plana.

- Y el caso base:

>Una lista vacía es plana.

- Usando esta definición recursiva, podemos implementar en Scheme la función `(plana? lista)` que comprueba si una lista es plana:

```scheme
(define (plana? lista)
   (or (null? lista)
       (and (hoja? (car lista))
            (plana? (cdr lista)))))
```

- Ejemplos:

```scheme
(plana? '(a b c d e f)) ; ⇒ #t
(plana? (list (cons 'a 1) "Hola" #f)) ; ⇒ #t
(plana? '(a (b c) d)) ; ⇒ #f
(plana? '(a () b)) ; ⇒ #f
```

----

### Función `(estructurada? lista)`

- Una lista es estructurada cuando alguno de sus elementos es otra lista:

```
(define (estructurada? lista)
   (if (null? lista)
      #f
      (or (list? (car lista))
          (estructurada? (cdr lista)))))
```

- Ejemplos:

```scheme
(estructurada? '(1 2 3 4)) ; ⇒ #f
(estructurada? (list (cons 'a 1) (cons 'b 2) (cons 'c 3))) ; ⇒ #f
(estructurada? '(a () b)) ; ⇒ #t
(estructurada? '(a (b c) d)) ; ⇒ #t
```

Realmente bastaría con haber hecho una de las dos definiciones y escribir la otra como la negación de la primera:

```scheme
(define (estructurada? lista)
   (not (plana? lista)))
```

----

### 2.1.2. Ejemplos de listas estructuradas

- Las listas estructuradas son muy útiles para representar información jerárquica en donde queremos representar elementos que contienen otros elementos.

- Por ejemplo, las expresiones de Scheme son listas estructuradas:

    ```scheme
    (= 4 (+ 2 2))
    (if (= x y) (* x y) (+ (/ x y) 45))
    (define (factorial x) (if (= x 0) 1 (* x (factorial (- x 1)))))
    ```

- El análisis sintáctico de una oración puede generar una lista estructurada de símbolos, en donde se agrupan los distintos elementos de la oración:

    ```scheme
    {{Juan} {compró} {la entrada {de los Miserables}} {el viernes por la tarde}}
    ```

----

### 2.1.3. *Pseudo árboles* con niveles

- Las listas estructuradas definen una estructura de niveles, donde la lista inicial representa el primer nivel, y cada sublista representa un nivel inferior. Los datos de las listas representan las hojas.

- Por ejemplo, la representación en forma de niveles de la lista `{{a b c} d e}` es la siguiente:

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/expresion-e-1.png" width="350px"/>

Las hojas `d` y `e` están en el nivel 1 y en las posiciones 2 y 3 de la lista y las hojas `a`, `b` y `c` en el nivel 2 y en la posición 1 de la lista.

> UNA LISTA ESTRUCTURADA NO ES UN ÁRBOL  
> Una lista estructurada no es un árbol propiamente dicho, porque todos los datos están en las hojas.

- Otro ejemplo. ¿Cuál sería la representación en niveles de la siguiente lista estructurada?: 
    ```scheme
	{let {{x 12}
	      {y 5}}
	   {+ x y}}}
    ```

<p style="margin-bottom:3cm;"/>

----

### 2.2. Funciones recursivas sobre listas estructuradas

- `(num-hojas lista)`: número de hojas de una lista estructurada
- `(altura lista)`: nivel mayor de una lista estructurada

----

### 2.2.1. Número de hojas

```scheme
(num-hojas '((1 2) (3 4 (5) 6) (7))) ⇒ 7
```

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/num-hojas-estructurada.png" width=400px"/>

- Podemos definir la función obteniendo el primer elemento y el resto de la lista, y contando recursivamente el número de hojas del primer elemento y del resto. 
- Al ser una lista estructurada, **el primer elemento puede ser a su vez otra lista, por lo que llamamos a la recursión para contar sus hojas**.
- La definición de este caso general usando _pseudocódigo_ es:

> El número de hojas de una lista estructurada es la suma del número de hojas de su primer elemento (que puede ser otra lista) y del número de hojas del resto.

- En Scheme:

```scheme
(define (num-hojas lista)
   (cond
      ((null? lista) 0)
      ((hoja? lista) 1)
      (else (+ (num-hojas (car lista))
               (num-hojas (cdr lista))))))
```

- Hay que hacer notar que el parámetro `lista` puede ser tanto una lista como un dato atómico. Estamos aprovechándonos de la característica de Scheme de ser débilmente tipeado para hacer un código bastante conciso.

----

### Versión con funciones de orden superior

- Podemos usar también las funciones de orden superior `map` y `fold-right` para obtener una versión más concisa. 
- Una lista estructurada tiene como elementos hojas o listas. 
- Podemos entonces mapear una expresión lambda con _la propia función que estamos definiendo_ sobre sus elementos, poniendo como caso especial el hecho de que la lista sea una hoja. 
- El resultado será una lista de números (el número de hojas de cada componente), que podemos sumar haciendo un `fold-right` con la función `+`:

```scheme
(define (num-hojas-fos lista)
    (if (hoja? lista) 
        1
        (fold-right + 0 (map num-hojas-fos lista))))
```

- Una versión alternativa de la función anterior es la siguiente (también correcta):

```scheme
(define (num-hojas-fos lista)
    (fold-right + 0 (map (lambda (sublista)
                           (if (hoja? sublista)
                               1
                               (num-hojas-fos sublista))) lista)))
```

----

### 2.2.2. Altura de una lista estructurada

- La *altura* de una lista estructurada viene dada por su número de niveles
- Una lista plana tiene una altura de 1, la lista `'((1 2 3) 4 5)` tiene una altura de 2.

```scheme
(altura '(1 (2 3) 4)) ⇒ 2
(altura '(1 (2 (3)) 3)) ⇒ 3
```

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/altura-estructurada.png" width="300px"/>

> Para calcular la altura de una lista estructurada tenemos que obtener (de forma recursiva) la altura de su primer elemento, y la altura del resto de la lista, sumarle 1 a la altura del primer elemento y devolver el máximo de los dos números.

En Scheme:

```scheme
(define (altura lista)
   (cond 
      ((null? lista) 0)
      ((hoja? lista) 0)
      (else (max (+ 1 (altura (car lista)))
                 (altura (cdr lista))))))
```

----

### Versión con funciones de orden superior

Y la segunda versión, usando las funciones de orden superior `map` para obtener la altura de las sublistas y `fold-right` para quedarse con el máximo.

```scheme
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

---

### 2.2.3. Otras funciones recursivas

Vamos a diseñar otras funciones recursivas que trabajan con la estructura jerárquica de las listas estructuradas.

* `(pertenece-lista? dato lista)`: busca una hoja en una lista estructurada
* `(nivel-lista dato lista)`: devuelve el nivel en el que se encuentra un dato en una lista
* `(cuadrado-lista lista)`: eleva todas las hojas al cuadrado (suponemos que la lista estructurada contiene números)
* `(map-lista f lista)`: similar a map, aplica una función a todas las hojas de la lista estructurada y devuelve el resultado (otra lista estructurada)

---

### `(pertenece-lista? dato lista)`

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

---

### `(nivel-lista dato lista)`

- Veamos la función `(nivel-lista dato lista)` que recorre la lista buscando el dato y devuelve el nivel en que se encuentra. Si el dato no se encuentra en la lista, se devolverá 0. Si el dato se encuentra en más de un lugar de la lista se devolverá el nivel del primero que se encuentre.

Ejemplos:

```scheme
(nivel-hoja 'b '(a b (c))) ; ⇒ 1
(nivel-hoja 'b '(a (b) c)) ; ⇒ 2
(nivel-hoja 'b '(a c d ((b))) ; ⇒ 4
(nivel-hoja 'b '(a c d ((e)))) ; ⇒ 0
```

- Vamos a implementar la función con una recursión por la cola en la que pasamos como parámetro el nivel en el que se encuentra la recursión. 

```scheme
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

----

### `(cuadrado-lista lista)`

- Devuelve una lista estructurada con la misma estructura y sus números elevados al cuadrado. 

```scheme
(define (cuadrado-lista lista)
  (cond ((null? lista) '())
        ((hoja? lista) (* lista lista))
        (else (cons (cuadrado-lista (car lista))
                    (cuadrado-lista (cdr lista))))))
```

- Por ejemplo:

```scheme
(cuadrado-lista '(2 3 (4 (5)))) ⇒ (4 9 (16 (25))
```

----

### `(map-lista f lista)`

- Devuelve una lista estructurada igual que la original con el resultado de aplicar a cada uno de sus hojas la función f 
 
```scheme
(define (map-lista f lista)
  (cond ((null? lista) '())
        ((hoja? lista) (f lista))
        (else (cons (map-lista f (car lista))
                    (map-lista f (cdr lista))))))
```
	
- Por ejemplo:

```scheme
(map-lista (lambda (x) (* x x)) '(2 3 (4 (5)))) ⇒ (4 9 (16 (25))
```

----

