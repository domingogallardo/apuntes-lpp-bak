
----

### Tema 2: Programación funcional (2/3)

Notas de clase Semana 3   
[Enlace a los apuntes](http://domingogallardo.github.io/lpp/teoria/Tema02-ProgramacionFuncional.html#3)

----

### Contenidos

- El paradigma de Programación Funcional
- Scheme como lenguaje de programación funcional
- **3. Tipos de datos compuestos en Scheme**
	- **3.1. El tipo de dato pareja**
	- **3.2. Las parejas son objetos de primera clase**
	- **3.3.  Diagramas *caja-y-puntero***
- **4. Listas en Scheme**
    - **Implementación de listas en Scheme**
    - **Listas con elementos compuestos**
    - **Funciones recursivas sobre listas**
    - **Funciones con número variable de argumentos**
- Funciones como tipos de datos de primera clase
- Ambito de variables, clausuras y `let`


----

### 3. Tipos de datos compuestos en Scheme

<!--<p style="margin-bottom: 2cm;"/>-->
---

### El tipo de dato pareja

- Ya lo hemos visto en el seminario
- `cons` para construir parejas:

```scheme
(cons 1 2) ; ⇒ {1 . 2}
(define c (cons 1 2))
```

----

### Funciones de acceso `car` y `cdr`

- `car` y `cdr` devuelven la parte izquierda y derecha:

```scheme
(define c (cons 1 2))
(car c) ; ⇒ 1
(cdr c) ; ⇒ 2
```

----

### Definición declarativa


```scheme
(car (cons x y)) = x
(cdr (cons x y)) = y
```

----

### Función pair?


```scheme
(pair? 3) ; ⇒ #f
(pair? (cons 3 4)) ; ⇒ #t
```

----

### Ejemplo de función que recibe distintos tipos de datos

- Scheme es débilmente tipeado
- Podemos pasar cualquier tipo de dato en los parámetros de las
  funciones, por ejemplo a la siguiente función `suma`

```scheme
(define (suma x y)
  (cond 
    ((and (number? x) (number? y)) (+ x y))
    ((and (string? x) (string? y)) (string-append x y))
    (else 'error)))
```

Lo probamos ...


<p style="margin-bottom:3cm;"/>

----

### Las parejas pueden contener cualquier tipo de dato

```scheme
(define c (cons 'hola #f))
(car c) ; ⇒ 'hola
(cdr c) ; ⇒ #f
```

----

### Las parejas son objetos inmutables

- En programación funcional una vez creada una parea no está permitido
  modificar (mutar) su contenido.
- En Scheme hay funciones para mutar parejas, pero no las veremos
  hasta ver el paradigma de programación imperativa

----

### Las parejas son objetos de primera clase

En un lenguaje de programación un elemento es de primera clase cuando
puede:

* Asignarse a variables
* Pasarse como argumento
* Devolverse por una función
* Guardarse en una estructura de datos mayor

Las parejas son objetos de primera clase.

----

### Asignación a una variable (1)

Una pareja puede asignarse a una variable:

```scheme
(define p1 (cons 1 2))
(define p2 (cons #f "hola"))
```

----

### Paso como argumento y devolverse como resultado de una función (2 y 3)

```scheme
(define (suma-parejas p1 p2)
    (cons (+ (car p1) (car p2))
          (+ (cdr p1) (cdr p2))))
```

Lo probamos ...

<p style="margin-bottom:2cm"/>

----

### Formar parte de otras parejas (4)

> El resultado de un `cons` puede usarse como parámetro de nuevas
> llamadas a `cons`.


```scheme
(define p1 (cons 1 2))
(define p2 (cons 3 4))
(define p (cons p1 p2))
```


```scheme
(define p (cons (cons 1 2)
                (cons 3 4)))
```

<img src="../tema02-programacion-funcional/imagenes/pareja-pareja.png" width="300px"/>

Diagramas *caja-y-puntero* (*box-and-pointer* en inglés):

<img src="../tema02-programacion-funcional/imagenes/pareja-pareja2.png" width="250px"/>

----

### Ejemplos de diagramas *caja-y-puntero*

- Es conveniente indentar correctamente los `cons`:

```scheme
(define p (cons (cons 1
                      (cons 3 4))
                2))
```

- Es importante recordar que las expresiones se evalúan *de dentro a
  afuera*.
- ¿Qué estructura se construye con la sentencia anterior? Dibuja el
  diagrama *box-and-pointer*.

<p style="margin-bottom:3cm;"/>

- ¿Cuál sería el diagrama resultante de la siguiente expresión?


```scheme
(define p2 (cons 5 (cons p 6)))
```

<p style="margin-bottom:4cm;"/>

- ¿Cómo sería la expresión formada por `car` y `cdr`s que devolviera 3
  a partir de la variable `p2`?

<p style="margin-bottom:4cm;"/>

----

### Funciones c????r

- Al trabajar con estructuras de parejas anidades es muy habitual
  realizar llamadas del tipo:

```scheme
(cdr (cdr (car p)))
```

- Es equivalente a la función `cadar` de Scheme:

```scheme
(cddar p)
```

- El nombre de la función se obtiene concatenando a la letra "c", las
  letras "a" o "d" según hagamos un car o un cdr y terminando con la
  letra "r".

- Hay definidas 2^4 funciones de este tipo: `caaaar`, `caaadr`, …,
  `cddddr`.

----

### 4. Listas en Scheme 

----

### Relación entre listas y parejas en Lisp y Scheme

- Hemos comprobado que las listas y las parejas tienen las mismas
  funciones: `car`, `cdr` y `cons``. ¿Por qué? ¿Qué relación hay entre
  las parejas y las listas?
- Hagamos algunas pruebas.

¿Una pareja es una lista?
Lo probamos ...

```scheme
(define p1 (cons 1 2))
(pair? p1) 
(list? p1) 
```

<p style="margin-bottom:2cm;"/>

¿Una lista vacía es una lista? ¿Es una pareja?
Lo probamos ...

```scheme
(list? '())
(pair? '())
```

<p style="margin-bottom:2cm;"/>

¿Una lista es una pareja?
Lo probamos ...

```scheme
(define lista '(1 2 3))
(list? lista)
(pair? lista)
```

<p style="margin-bottom:2cm;"/>

¿Una pareja con una lista vacía como parte izquierda es una lista? 
Lo probamos ...

```scheme
(define p1 (cons 1 '()))
(pair? p1)
(list? p1)
```
<p style="margin-bottom:2cm;"/>

Con estos ejemplos ya tenemos pistas para deducir la relación entre
listas y parejas en Scheme (y Lisp). Vamos a explicarlo.

----

### Definición de listas con parejas

Una lista es (definición recursiva):

* Una **pareja** que contiene:
    * *En su parte izquierda*: el primer elemento de la lista
    * *En su parte derecha*: el resto de la lista (volvemos a aplicar
      la definición)
* Un **símbolo especial** `'()` que denota la lista vacía.

----

### Ejemplo más sencillo: `{1}`

```scheme
(cons 1 '())
```
	
La pareja cumple las condiciones anteriores: 

* La parte izquierda de la pareja es el primer elemento de la lista
  (el número 1)
* La parte derecha es el resto de la lista (la lista vacía)

<img src="../tema02-programacion-funcional/imagenes/pareja-lista.png" width="150px"/>

- Es al mismo tiempo una pareja y una lista:

```scheme
(define l (cons 1 '()))
(pair? l)
(list? l)
```

---- 

### Otro ejemplo: `{1 2 3 4}`

```scheme
(cons 1
      (cons 2
            (cons 3
                  (cons 4 
                        '()))))
```

- La primera pareja cumple las condiciones de ser una lista:

* Su primer elemento es el 1
* Su parte derecha es la lista '(2 3 4)

<img src="../tema02-programacion-funcional/imagenes/lista.png" width="400px"/>

----

### Lista vacía

La lista vacía es una lista:

```scheme
(list? '())
```

No es un símbolo ni una pareja:

```scheme
(symbol? '())
(pair? '())
```

Función `null?`:

```scheme
(null? '())
```	

----

### Listas con elementos compuestos

- *Lista de asociación*, listas cuyos elementos son parejas (*clave*,
  *valor*):

```scheme
(list (cons 'a 1)
      (cons 'b 2)
      (cons 'c 3))
```

¿Cuál sería el diagrama *box and pointer* de la estructura anterior?

<p style="margin-bottom:4cm;"/>

- Expresión equivalente utilizando *conses* es:

```scheme
(cons (cons 'a 1)
      (cons (cons 'b 2)
            (cons (cons 'c 3)
                  '())))
```

----

### Listas de listas

```scheme
(define lista (list 1 (list 1 2 3) 3))
```

Definición con `quote`:

```scheme
(define lista '(1 (1 2 3) 3))
```

¿Cuál sería el diagrama *box and pointer* de la estructura anterior?

<p style="margin-bottom:4cm;"/>

----

### Distintos niveles de abstracción

- Una vez que conocemos la implementación de listas con parejas, no va
  a a ser necesario casi nunca *bajar* a este nivel de implementación
- Podemos volver a la *abstracción* inicial en la que las funciones
  `car` y `cdr` trabajan sobre listas:
    - `(car lista)`: devuelve el primer elemento de la lista
    - `(cdr lista)`: devuelve el resto de la lista
    - `(list-ref lista n)`: devuelve la posición `n` de la lista
    - `(cons dato lista)`: devuelve una nueva lista con `dato` en su primera posición y `lista` como su resto

<p style="margin-bottom:4cm;"/>

----

### Funciones recursivas y listas

Vamos a ver cómo se implementan de forma recursiva:

- Funciones recursivas que trabajan **sobre listas** (para no solapar
  con las definiciones de Scheme pondremos el prefijo `mi-` en todas
  ellas):
    - Función `mi-list-ref`
    - Función `mi-list-tail`
    - Función `mi-append`
    - Función `mi-reverse`
- Funciones recursivas que trabajan **con listas**:
    - Función `cuadrados-hasta`
    - Función `filtra-pares`
    - Función `primo?`


----

### Recordatorio: Diseño de funciones recursivas

¿Cómo diseñamos una *definición recursiva* de una función? 

- Es recomendable comenzar por el **caso general**.
- Debemos buscar una forma de resolver el problema principal haciendo
  una llamada a la recursión con una versión más pequeña del problema,
  **confiar en que la recursión funciona correctamente** devolviendo
  lo que tiene que devolver y obtener con este valor devuelto la
  solución al problema principal. Es recomendable probar con un
  ejemplo concreto.
- Una vez formulado el caso general, buscamos el **caso base de la
  recursión**: el caso más sencillo posible en el que no es necesario
  hacer una llamada recursiva para devolver la solución.

---

### Función `mi-list-ref`

- La función `(mi-list-ref n lista)` devuelve el elemento `n` de una
  lista (empezando a contar por 0). Un ejemplo concreto:

    ```
    (mi-list-ref '(a b c d e f g) 4) ⇒ c
    ```

- ¿Podemos formular `(mi-list-ref '(a b c d e f g) 4)` de forma
  recursiva?:

> Para devolver el elemento 4 de lista `{a b c d e f g}`, podemos
> quitar el primer elemento de la lista (obtendremos `{b c d e f g}`)
> y devolver su elemento 3.

- En general, para cualquier `n` y cualquier lista:

> Para devolver el elemento `n` de una lista, hacemos el cdr de la
> lista y devolvemos su elemento n-1.

- Por último, formulamos el caso base de la recursión, el problema más
  sencillo que se puede resolver directamente, sin hacer una llamada
  recursiva:

> Para devolver el elemento que está en la posición 0 de una lista,
> devuelvo el `car` de la lista

- Implementación en Scheme (incluimos un segundo caso base en el que
  se intenta obtener la posición de una lista vacía; se llegaría a
  este caso si la posición que pedimos es mayor que el número de
  elementos de la lista):

    ```scheme
    (define (mi-list-ref lista n)
        (cond
            ((null? lista)  (error "indice demasiado largo"))
            ((= n 0) (car lista))
            (else (mi-list-ref (cdr lista) (- n 1)))))
    ```

----

### Función `mi-list-tail`

- La función `(mi-list-tail lista n)` devuelve la lista resultante de
  quitar n elementos de la lista original:

    ```scheme
    (mi-list-tail '(1 2 3 4 5 6 7) 2) ⇒ {3 4 5 6 7}
    ```

- ¿Cómo se implementaría de forma recursiva?

<p style="margin-bottom:2cm;"/>

- Solución en Scheme

    ```scheme
    (define (mi-list-tail lista n)
        (cond
            ((null? lista)  (error "indice demasiado largo"))
            ((= n 0) lista)
            (else (mi-list-tail (cdr lista) (- n 1)))))
    ```

----

### Función `mi-append` 

- Por ejemplo

    ```
    (mi-append '(a b c) '(d e f)) ;⇒ {a b c d e f}
    ```

- Formulación recursiva del ejemplo:

    ```
    (mi-append '(a b c) '(d e f)) = (cons 'a (mi-append '(b c) '(d e f))) = 
    (cons 'a {b c d e f}) = {a b c d e f}
    ```

- En general:

    ```
    (mi-append lista1 lista2) = (cons (car lista1) (mi-append (cdr lista1) lista2))
    ```

- El caso base es  aquel en el que `lista1` es `null?`. En ese caso devolvemos `lista2`:

    ```
    (mi-append '() '(a b c)) ;⇒ '{a b c}
    ```

- La formulación recursiva completa queda como sigue:

    ```scheme
    (define (mi-append l1 l2)
        (if (null? l1)
            l2
            (cons (car l1)
                  (mi-append (cdr l1) l2))))
    ```

----

### Función `mi-reverse`

- Ejemplo

    ```scheme
    (mi-reverse '(1 2 3 4 5 6)) ; ⇒ {6 5 4 3 2 1}
    ```

- La idea es sencilla: llamamos a la recursión para hacer la inversa
  del `cdr` de la lista y añadimos el primer elemento a la lista
  resultante.

- Podemos definir una función auxiliar `(añade-al-final dato lista)`
  que añade un dato al final de una lista usando `append`:

    ```scheme
    (define (añade-al-final dato lista)
        (append lista (list dato)))
    ```

- La función `mi-reverse` quedaría entonces como sigue:

    ```scheme
    (define (mi-reverse lista)
        (if (null? lista) '()
            (añade-al-final (car lista) (mi-reverse (cdr lista)))))
    ```

----

### Funciones recursivas que usan listas

----

### Función `cuadrados-hasta`

- La función `(cuadrados-hasta x)` devuelve una lista con los
  cuadrados de los números hasta x:

    ```
    (cuadrados-hasta 5) ; ⇒ {25 16 9 4 1}
    ```

¿Cómo formulamos el ejemplo de forma recursiva?

<p style="margin-bottom: 3cm;"/>

```
(cuadrados-hasta 5) = (cons (cuadrado 5) (cuadrados-hasta 4) =
(cons 25 {16 9 4 1}) = {25 16 9 4 1}
```

- En general:

> Para construir una lista de los cuadrados hasta x, añado el cuadrado
> de x a la lista de los cuadrados hasta x-1

- El caso base de la recursión es el caso en el que x es 1, entonces
  devolvemos `{1}

- Ya podemos realizar la definición Scheme:

    ```scheme
    (define (cuadrados-hasta x)
        (if (= x 1)
            '(1)
            (cons (cuadrado x)
                  (cuadrados-hasta (- x 1)))))
    ```

----

### Función `filtra-pares`

- Es muy habitual recorrer una lista y comprobar condiciones de sus
  elementos, construyendo una lista con los que cumplan una
  determinada condición.

- La función `filtra-pares` construye una lista con los números pares
  de la lista que le pasamos como parámetro:

    ```scheme
    (filtra-pares '(1 2 3 4 5 6)) ;⇒ {2 4 6}
    ```

¿Cómo la definimos de forma recursiva?

<p style="margin-bottom:3cm;"/>


- Solución en Scheme

    ```scheme
    (define (filtra-pares lista)
        (cond
            ((null? lista) '())
	        ((even? (car lista))
                 (cons (car lista) (filtra-pares (cdr lista))))
            (else (filtra-pares (cdr lista)))))
    ```

----

### Ejemplo final: Función `primo?`

- El uso de listas es uno de los elementos fundamentales de la
  programación funcional.

- Veamos un algoritmo sencillo que permite calcular si un número es
  primo, usando alguna de las funciones anteriores sobre
  listas. Calcularemos la lista de divisores del número y
  comprobaremos si su longitud es dos:

    ```
    (divisores 8) ;⇒ {1 2 4 8} longitud = 4, no primo
    (divisores 9) ;⇒ {1 3 9} longitud = 3, no primo
    (divisores 11) ;⇒ {1 11} longitud = 2, primo
    ```

- En Scheme:

    ```scheme
    (define (primo? x)
        (=  2 
        (length (divisores x))))
    ```

----

### Función recursiva `(lista-hasta x)`


- La función `(lista-hasta x)` devuelve una lista de números 1..x:

    ```
    (lista-hasta 2) ;⇒ {1 2}
    (lista-hasta 10) ;⇒ {1 2 3 4 5 6 7 8 9 10}
    ```

- Implementación recursiva:


    ```scheme
    (define (lista-hasta x)
        (if (= x 0)
            '()
            (cons x (lista-hasta (- x 1)))))
    ```

----

### Función `(divisor? x y)`

```scheme
(define (divisor? x y)
      (= 0 (mod y x)))
```

----

### Función recursiva `(filtra-divisores lista x)`

- Función que filtra aquellos números `lista` que son divisores del
  número `x`

```scheme
(define (filtra-divisores lista x)
   (cond
      ((null? lista) '())
      ((divisor? (car lista) x) (cons (car lista)
                                      (filtra-divisores (cdr lista) x)))
      (else (filtra-divisores (cdr lista) x))))
```

----

### Función `(divisores x)` 


- Se puede implementar de una forma muy sencilla:

    ```scheme
    (define (divisores x)
        (filtra-divisores (lista-hasta x) x))
    ```

- Por ejemplo, para calcular los divisores de 10:

    ```
    (filtra-divisores {1 2 3 4 5 6 7 8 9 10} 10) ;⇒ {1 2 5 10}
    ```

- Una vez definida esta función, ya puede funcionar correctamente la
  función `primo?`

----
