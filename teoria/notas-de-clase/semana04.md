
----

### Tema 2: Programación funcional (3/3)

Notas de clase Semana 4   
[Enlace a los apuntes](http://domingogallardo.github.io/lpp/teoria/Tema02-ProgramacionFuncional.html#5)

----

### Contenidos

- 1 El paradigma de Programación Funcional
- 2 Scheme como lenguaje de programación funcional
- 3 Tipos de datos compuestos en Scheme
- 4 Listas en Scheme
    - Implementación de listas en Scheme
    - Listas con elementos compuestos
    - Funciones recursivas sobre listas
    - **Funciones con número variable de argumentos**
- 5 **Funciones como tipos de datos de primera clase**
    - 5.1 **Un primer ejemplo**
    - 5.2 **Forma especial `lambda`**
    - 5.3 **Funciones como argumentos de otras funciones**
    - 5.4 **Funciones en estructuras de datos**
    - 5.5 **Funciones de orden superior**
- 6 Ambito de variables, clausuras y `let`

----

### Funciones con número variable de argumentos

----

- Definición de número variable de argumentos con la notación de punto:

    ```scheme
    (define (funcion-dos-o-mas-args x y . lista-args) 
        <cuerpo>)
    ```

- Podemos llamar a la función anterior con dos o más argumentos:

    ```scheme
    (funcion-dos-o-mas-args 1 2 3 4 5 6)
    ```
	
- En la llamada, los parámetros `x` e `y` tomarán los valores 1 y 2. 
- El parámetro `lista-args` tomará como valor una lista con los argumentos restantes `(3 4 5 6)`.

<p style="margin-bottom:2cm;"/>

- También es posible permitir que todos los argumentos sean opcionales no poniendo ningún argumento antes del punto::

    ```scheme
    (define (funcion-cualquier-numero-args . lista-args) 
        <cuerpo>)
    ```
	
- Ejemplo:

    ```scheme
    (define (mi-suma x y . lista-nums)
        (if (null? lista-nums)
            (+ x y)
            (+ x (+ y (suma-lista lista-nums)))))
    ```

<p style="margin-bottom:2cm;"/>

----

### 5. Funciones como tipos de datos de primera clase

----

### Una función es un bloque de código con varias entradas y una salida

Función que eleva al cuadrado un número:

<img src="./imagenes/tema03-programacion_funcional/funcion-cuadrado.png" style="width: 80px;"/>

Función que suma dos parejas:

<img src="./imagenes/tema02-programacion_funcional/esquema-suma-parejas.png" style="width: 200px;"/>


----

### Las funciones son objetos de primera clase de un lenguaje

Recordemos que un tipo de primera clase es aquel que:

1. Puede ser asignado a una variable
2. Puede ser pasado como argumento a una función
3. Puede ser devuelto como resultado de una invocación a una función
4. Puede ser parte de un tipo mayor

Con funciones:

1. Una función se puede asignar a una variable
2. Una función se puede pasar como parámetro de otras funciones 
3. Una función se puede devolver como resultado de una invocación a otra función
4. Una función se puede guardar en tipos de datos compuestos como listas

Es una característica de muchos lenguajes multi-paradigma con características funcionales como [JavaScript](http://helephant.com/2008/08/19/functions-are-first-class-objects-in-javascript/), [Python](https://thenewcircle.com/static/bookshelf/python_fundamentals_tutorial/functional_programming.html), [Swift](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html) o incluso en la última versión de Java, [Java 8](http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html), (donde se denominan *expresiones lambda*).

----

### <a name="5-1"></a>5.1. Un primer ejemplo: la función `map`

- Función `(map func lista)` de Scheme 
- Toma como parámetro **una función** y una lista
- Devuelve la lista resultante de  *mapear* (aplicar) la función `func` a todos y cada unos de los elementos de la lista original

Ejemplo:


```scheme
(define (cuadrado x)
    (* x x))

(map cuadrado '(1 2 3 4 5)) 
⇒ (1 4 9 16 25)
```

- Podemos pasar cualquier función a `map`, siempre que sea una función de un parámetro que se pueda aplicar a los elementos de la lista. ¿Qué devolverían las siguientes expresiones?


```scheme
(define (doble x)
   (* x 2))

(define (suma-1 x)
   (+ x 1))

(map doble '(1 2 3 4 5))
(map suma-1 '(1 2 3 4 5))
```

<p style="margin-bottom:3cm"/>

----

### ¿Podemos "pasar código" a `map` sin tener que definir una función?

- Sí, usando una **expresión lambda**:

```
(map (lambda (x) (+ x 3)) '(1 2 3 4 5)) ⇒ (4 5 6 7 8)
```

----

### 5.2. Forma especial `lambda`

----

### Sintaxis de la forma especial `lambda`

La sintaxis de la forma especial `lambda` es:

```
(lambda (<arg1> ... <argn>) 
    <cuerpo>)
```

El cuerpo del lambda define un *bloque de código* y sus argumentos son los parámetros necesarios para ejecutar ese bloque de código. Llamamos a la función resultante una *función anónima*.

Una función anónima que suma dos parejas:

```
(lambda (p1 p2)
    (cons (+ (car p1) (car p2))
          (+ (cdr p1) (cdr p2))))
```

Una función anónima que devuelve el mayor de dos números:

```
(lambda (a b)
    (if (> a b)
        a
        b))
```

----

### Semántica de la forma especial `lambda`

- La invocación a la forma especial `lambda` construye una función anónima en tiempo de ejecución.

```scheme
(lambda (x) (* x x))
⇒ #<procedure>
```

- El procedimiento construido es un bloque de código que devuelve el cuadrado de un número. 

- ¿Qué podemos hacer con este procedimiento? 

----

### Podemos asignar el procedimiento a un identificador (símbolo)

```
> (define cuadrado (lambda (x) (* x x)))
```

- Scheme devuelve el procedimiento al evaluar el identificador `cuadrado`:

```
> cuadrado
#<procedure:cuadrado>
```

Podemos usar la función cuadrado como si la hubiéramos creado de la forma habitual:

```
(cuadrado 3) ⇒ 9
```

----

### Podemos invocar a la función anónima directamente


```
((lambda (x) (* x x)) 3) ⇒ 9
```

La llamada a `lambda` crea un procedimiento y el paréntesis a su izquierda lo invoca con el parámetro 3:

```
((lambda (x) (* x x)) 3) => (#<procedure> 3) ⇒ 9
```

----

### Podemos pasar el procedimiento como parámetro de otra función

```
(define (aplica-dos-veces func x)
   (func (func x)))
```

- Lo podemos invocar pasándole cualquier función de un argumento y cualquier dato:

```
(aplica-dos-veces (lambda (x)
                     (+ x 5)) 10) ⇒ 20
(aplica-dos-veces (lambda (s)
                     (string-append s "-jeje")) "Hola") ⇒ "Hola-jeje-jeje"
```

- Es importante remarcar que con `lambda` estamos creando una función en *tiempo de ejecución*. 

----

### Expresiones lambda en distintos lenguajes de programación


**Java 8**

```
Integer x -> {x*x}
```

**Scala**

```
(x:Int) => {x*x}
```

**Objective C**

```
^int (int x)
{
   x*x
};
```

**Swift**

```
{ (x: Int) -> Int in return x*x }
```

----

### Identificadores y funciones

- En Scheme una función está ligada al símbolo que define su nombre:

```
> +
⇒ <procedure:+>
```

- Podemos asignar funciones ya existentes a nuevos identificadores usando `define`, como en el ejemplo siguiente:

```
> +
⇒ <procedure:+>
> (define suma +)
> (suma 1 2 3 4)
⇒ 10
```

<img src="./imagenes/tema03-programacion_funcional/suma.png" style="width:100px;"/>


----

### La forma especial `define` para definir una función no es más que *azucar sintáctico*

La forma especial `define`:

```
(define (<nombre> <args>)
    <cuerpo>)
```

siempre se convierte internamente en:

```
(define <nombre> 
    (lambda (<args>)
        <cuerpo>))
```

Por ejemplo

```
(define (cuadrado x)
    (* x x))
```

es equivalente a:

```
(define cuadrado 
    (lambda (x) (* x x)))
```

----

### Predicado `procedure?`

- Podemos comprobar si algo es una función utilizando el predicado de Scheme `procedure?`.

Por ejemplo:

```
(procedure? (lambda (x) (* x x)))
⇒ #t
(define suma +)
(procedure? suma)
⇒ #t
(procedure? '+)
⇒ #f
```

----

### 5.3. Funciones argumentos de otras funciones 

Veamos más ejemplos de funciones que reciben otras funciones.

----

### Función `aplica`

- Función `(aplica func x y)` que recibe una función de dos argumentos (`func`) y dos argumentos `x` e `y`. Queremos que en el cuerpo de `aplica` se invoque la función `func` con los argumentos `x` e `y`

- Para realizar la invocación a la función que se pasa como parámetro basta con usar `func` como su nombre. La función se ha ligado al nombre `func` en el momento de la invocación a `aplica`, de la misma forma que los argumentos se ligan a los parámetros `x` e `y`:

```
(define (aplica func x y)
   (func x y))
```

¿Ejemplos de invocación?

<p style="margin-bottom:3cm;"/>

```
(aplica + 2 3) ; ⇒ 5
(aplica * 4 5) ; ⇒ 10
(aplica string-append "hola" "adios") ; ⇒ "holaadios"

(define (string-append-con-guion s1 s2)
    (string-append s1 "-" s2))

(aplica string-append-con-guion "hola" "adios") ; ⇒ "hola-adios"

(aplica (lambda (x y) (sqrt (+ (* x x) (* y y)))) 3 4) ⇒ 5
```

----

### Función `aplica-2` 

- Otro ejemplo, la función `aplica-2` que toma dos funciones `f` y `g` y un argumento `x` y devuelve el resultado de aplicar `f` a lo que devuelve la invocación de `g` con `x`:

```
(define (aplica-2 f g x)
   (f (g x)))
```

¿Ejemplos de invocación?

<p style="margin-bottom:3cm;"/>

```
(define (suma-5 x)
   (+ x 5))
(define (doble x)
   (+ x x))
(aplica-2 suma-5 doble 3)
⇒ 11
```

----

### 5.4. Funciones en estructuras de datos

----

### Construcción de listas de funciones

Para construir una lista de funciones debemos llamar a `list` con las funciones como parámetros

```
(define lista (list cuadrado suma-1 doble))
lista
⇒ {#<procedure:cuadrado>  #<procedure:suma-1>  #<procedure:doble>}
```

También podemos evaluar una expresión lambda y añadir el procedimiento resultante:

```
(define lista2 (cons (lambda (x) (+ x 5)) lista))
lista2
⇒ {#<procedure> #<procedure:cuadrado> #<procedure:suma-1> #<procedure:doble>}
```

----

### Invocación a funciones de una lista

- Una vez creada una lista con funciones, ¿cómo podemos invocar a alguna de ellas?. 
- Debemos tratarlas de la misma forma que tratamos cualquier otro dato guardado en la lista, las recuperamos con las funciones `car` o `list-ref` y las invocamos. 

- Por ejemplo, para invocar a la primera función de `lista2`:

```
((car lista2) 10) ; ⇒ 15
```

----

### Ejemplo de función que trabaja con listas de funciones: `aplica-funcs`

- Veamos un ejemplo de una función `(aplica-funcs lista-funcs x)` que recibe una lista de funciones en el parámetro `lista-funcs` y las aplica todas al número que pasamos en el parámetro `x`.

- Por ejemplo, si construimos una lista con las funciones `cuadrado`, `cubo` y `suma-1`:

```
(define lista (list cuadrado cubo suma-1))
```

la llamada a `(aplica-funcs lista 5)` devolverá :

```
(cuadrado (cubo (suma-1 5))
⇒ 46656
```

- ¿Cómo implementamos `aplica-funcs` de forma recursiva?

<p style="margin-bottom:3cm;"/>


Ejemplo de recursión:

```
(aplica-funcs (cuadrado cubo suma-1) 5) => (cuadrado (aplica-funcs (cubo suma-1) 5))
=> (cuadrado 216) => 46656
```

Caso general de la recursión:

```
(aplica-funcs lista-funcs x) => ((car lista-funcs) (aplica-funcs (cdr lista-funcs)))
```

Caso base:

```
(if (null? (cdr lista-funcs)) ; la lista de funciones solo tiene una función
    ((car lista-funcs) x) ; invocamos a la función con el parámetro x
    ...
```

Implementación completa:

```
(define (aplica-funcs lista-funcs x)
    (if (null? (cdr lista-funcs))
        ((car lista-funcs) x)
        ((car lista-funcs)
            (aplica-funcs (cdr lista-funcs) x))))
```

Un ejemplo de uso:

```
(define lista-funcs (list (lambda (x) (* x x))
                          (lambda (x) (* x x x))
                          (lambda (x) (+ x 1))))
(aplica-funcs lista-funcs 5)
⇒ 46656
```

----

### 5.5. Funciones de orden superior

> Las funciones de orden superior (*higher order functions* en inglés) son funciones que reciben como parámetro otras funciones.

----

### Ejemplo de uso de parámetros de tipo función para generalizar

- La posibilidad de pasar funciones como parámetros de otras es una poderosa herramienta de abstracción. Veamos un ejemplo.

- Supongamos que queremos calcular el sumatorio de `a` hasta `b`. ¿Cómo lo haríamos de forma recursiva?

<p style="margin-bottom:3cm;"/>

```
(define (sum-x a b)
    (if (> a b)
        0
        (+ a (sum-x (+ a 1) b))))

(sum-x 1 10)
⇒ 55
```

- Supongamos ahora que queremos calcular el sumatorio de `a` hasta `b` **sumando los números al cuadrado**. ¿Cómo lo haríamos?


<p style="margin-bottom:3cm;"/>

```
(define (sum-cuadrado-x a b)
    (if (> a b)
        0
        (+ (* a a) (sum-cuadrado-x (+ a 1) b))))

(sum-cuadrado-x 1 10)
⇒ 385
```

- ¿Y el sumatorio de `a` hasta `b` **sumando los cubos**?

<p style="margin-bottom:3cm;"/>

```
(define (sum-cubo-x a b)
    (if (> a b)
        0
        (+ (* a a a) (sum-cubo-x (+ a 1) b))))

(sum-cubo-x 1 10)
⇒ 3025
```

- El código de las tres funciones anteriores es muy similar. 
- Podemos generalizarlo definiendo una función que haga la recursión y que reciba como parámetro otra función que se aplica a los números.
- Podemos definir una función genérica `sum-f-x` que generaliza las tres funciones anteriores: el sumatorio desde `a` hasta `b` de `f(x)`:

```
(define (sum-f-x f a b)
    (if (> a b)
        0
        (+ (f a) (sum-f-x f (+ a 1) b))))
```

Las funciones anteriores son casos particulares de esta función que las generaliza. Por ejemplo, para calcular el sumatorio desde 1 hasta 10 de `x` al cubo:

```
(define (cubo x)
    (* x x x))

(sum-f-x cubo 1 10)
⇒ 3025
```

----

### ¿Qué es una función de orden superior?

- Llamamos funciones de orden superior (*higher order functions* en inglés) a las funciones que toman otras como parámetro o devuelven otra función. Permiten generalizar soluciones con un alto grado de abstracción.

- Los lenguajes de programación funcional como Scheme, Scala o Java 8 tienen ya predefinidas algunas funciones de orden superior que permiten tratar listas o *streams* de una forma muy concisa y compacta.

Veremos:

- La función `map` de Scheme, como ejemplo típico de función de orden superior
- Después definiremos nosotros las funciones `filter` y `fold`. 
- Terminaremos viendo cómo la utilización de funciones de orden superior es una excelente herramienta de la programación funcional que permite hacer código muy conciso y expresivo.

> La combinación de funciones de nivel superior con listas es una de las características más potentes de la programación funcional.


----

### Función `map`

- Recordatorio:

```
(map cuadrado '(1 2 3 4 5))
⇒ (1 4 9 25)
```

- ¿Cómo se podría implementar `map` de forma recursiva?

<p style="margin-bottom:3cm;"/>

----

### Implementación de `map`

Llamamos a la función `mi-map`. La implementación es la siguiente:

```
(define (mi-map f lista)
    (if (null? lista)
        '()
        (cons (f (car lista))
              (mi-map f (cdr lista)))))
```

----

### Función `filter`

- La función `(filter predicado lista)` toma como parámetro un predicado y una lista y devuelve como resultado los elementos de la lista que cumplen el predicado.
- En Scheme R6RS existe esa función si importamos todas las bibliotecas:

    ```scheme
    #lang r6rs
    (import (rnrs))
    ```

- Ejemplo de uso:

    ```
    (filter even? '(1 2 3 4 5 6 7 8))
    ⇒ (2 4 6 8)
    ```

- ¿Cómo se podría implementar `mi-filter` de forma recursiva?

<p style="margin-bottom:3cm;"/>

```
(define (mi-filter pred lista)
  (cond
    ((null? lista) '())
    ((pred (car lista)) (cons (car lista)
                              (mi-filter pred (cdr lista))))
    (else (mi-filter pred (cdr lista)))))
```

----

### Función `fold`

- La función `(fold func base lista)` permite recorrer una lista aplicando una función binaria de forma acumulativa a sus elementos.
- El nombre `fold` significa *plegado*.
- La función equivalente en Scheme R6RS es `fold-right`.

Su definición recursiva es:

> Para hacer el *fold* de una función `f` de dos argumentos y de una lista, debemos llamar a `f` con el primer elemento de la lista y el resultado de hacer el *fold* del resto de la lista. En el caso en que la lista no tenga elementos, se devolverá el caso base.

Veamos cómo funcionan las llamadas recursivas:

```
(fold + 0 '(1 2 3))
⇒ (+ 1 (fold + 0 '(2 3)))
⇒ (+ 1 (+ 2 (fold + 0 '(3))))
⇒ (+ 1 (+ 2 (+ 3 (fold + 0 '()))))
⇒ (+ 1 (+ 2 (+ 3 0)))
⇒ 6
```

La implementación recursiva en Scheme es la siguiente:

```
(define (fold func base lista)
  (if (null? lista)
      base
      (func (car lista) (fold func base (cdr lista)))))
```

Ejemplos de uso:

```
(fold string-append "" '("hola" "que" "tal"))
⇒ "holaquetal"
(fold (lambda (x y) (* x y)) 1 '(1 2 3 4 5 6 7 8))
⇒ 40320
(fold cons '() '(1 2 3 4))
⇒ (1 2 3 4)
```

----

### Uso de funciones de orden superior y expresiones lambda

----

### Función `(suma-n lista n)`

- Es posible utilizar en el cuerpo de las expresiones lambda algún parámetro de la función principal.

- Supongamos que queremos definir una función `(suma-n lista n)` que devuelve la lista resultante el resultado de sumar un número `n` a todos los elementos de una lista.

- Podemos hacerlo de forma recursiva:

```
(define (suma-n lista n)
    (if (null? lista)
        '()
        (cons (+ (car lista) n)
              (suma-n (cdr lista)))))
```

Funciona de la siguiente manera:

```
(suma-n '(1 2 3 4) 10)
⇒ (11 12 13 14)
```

¿Podemos implementar `suma-n` usando una llamada a `map`?

<p style="margin-bottom:3cm;"/>

```
(define (suma-n lista n)
    (map (lambda (x) (+ x n)) lista))
```

¿Cómo funciona?:

```
(suma-n '(1 2 3 4) 10) => (map (lambda (x) (+ x 10)) (11 12 13 14)) =>  (11 12 13 14)
```

----

#### Función `(contienen-letra? caracter lista-pal)`

- Último ejemplo, queremos definir la función `(contienen-letra caracter lista-pal)` que devuelve las palabras de una lista que contienen un determinado carácter, usando la función `filter` y alguna función auxiliar

Por ejemplo:

```scheme
(contienen-letra #\a '("En" "un" "lugar" "de" "la" "Mancha")) ⇒ ("lugar" "la" "Mancha")
```
- Necesitamos el predicado auxiliar `(letra-en-str? caracter pal)` que comprueba si una cadena contiene un carácter. Por ejemplo:

```
(letra-en-str? #\a "Hola") ⇒ #t
(letra-en-str? #\a "Pepe") ⇒ #f
```

- ¿Lo podemos implementar obteniendo una lista de caracteres a partir de la cadena y usando la función `filter`?

<p style="margin-bottom:3cm;"/>

```
(define (letra-en-pal? caracter palabra)
    (not (null? (filter (lambda (c)
                           (equal? c caracter)) (string->list palabra))))
```

- Ahora ya podemos implementar `contienen-letra` usando otra vez la función de orden superior `filter` y la función anterior en la expresión lambda que hace el filtrado:

```scheme
(define (contienen-letra caracter lista-pal)
   (filter (lambda (pal)
              (letra-en-str? caracter pal)) lista-pal))
```



