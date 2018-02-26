# Práctica 4: Funciones como datos de primera clase y funciones de orden superior

## Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica04.rkt` con una cabecera inicial con tu nombre y apellidos, y
las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Un conjunto de **pruebas** que comprueben su funcionamiento
  utilizando la librería `schemeunit`.
  

## Ejercicios

### Ejercicio 1 ###

Implementa, utilizando funciones de orden superior, las funciones
`longitud-impar`, `suma-longitudes` y `mayusculas` que reciben una
lista de símbolos y devuelven:

- `(longitud-impar lista-simbolos)`: una lista con aquellos símbolos
  que tienen longitud impar.
- `(suma-longitudes lista-simbolos)`: la suma de las longitudes
  de todos los símbolos de la lista.
- `(mayusculas lista-simbolos)`: una lista con los símbolos escritos
  en mayúscula.
  
**Pista**: Puedes usar las funciones `symbol->string`,
`string->symbol` y `string-upcase`.


Ejemplos:

```scheme
(longitud-impar '(me gusta LPP porque aprendo nuevos paradigmas de programación)) 
; ⇒ {gusta LPP aprendo}
(suma-longitudes '(me gusta LPP porque aprendo nuevos paradigmas de programación))
; ⇒ 53
(mayusculas '(me gusta LPP porque aprendo nuevos paradigmas de programación))
; ⇒ {ME GUSTA LPP PORQUE APRENDO NUEVOS PARADIGMAS DE PROGRAMACIÓN}
```


### Ejercicio 2 ###

Implementa utilizando funciones de orden las funciones
`(resultados‐quiniela lista‐parejas)`, `(cuenta-resultados resultado
lista-resultados)` y `(cuenta-resultados-lista lista-resultados)` de
la práctica 3.



### Ejercicio 3 ###

Sin utilizar el intérprete DrRacket, rellena los siguientes huecos
para obtener el resultado esperado. Después usa el intérprete para
comprobar si has acertado.

a)

```scheme 
(______ list 0 '(1 2 3))
; ⇒ {1 {2 {3 0}}}
```


b)

```scheme
(______ list "hola" '("como" "estas" "adios"))
; ⇒ {{{"hola" "como"} "estas"} "adios"}
```

Los siguientes apartados se realizan a partir de la siguiente lista:

```scheme
(define lista '((2 . 7) (3 . 5) (10 . 4) (5 . 5)))
```


c) Queremos obtener una lista donde cada número es la suma de las parejas que son pares

```scheme
(filter ________
        (________ (lambda (x) (+ (car x)
                                 (cdr x)))
               lista))
; ⇒ {8 14 10}
```


d) Queremos obtener una lista de parejas invertidas donde la "nueva" parte izquierda es mayor que la derecha.

```scheme
(filter ___________
        (map ____________ lista))
; ⇒ {{7 . 2} {5 . 3}}
```

e) Queremos obtener una lista cuyos elementos son las partes izquierda de aquellas parejas cuya suma sea par.

```
(fold-right __________ '()
        (_________ (lambda (x) (even? (+ (car x) (cdr x)))) lista))
; ⇒ {3 10 5}
```



### Ejercicio 4 ###

a) Implementa las funciones constructoras `(make-multiplicador k)` y
`(make-exponenciador k)` similares al ejemplo visto en teoría
`(make-sumador k)`.

La función `make-multiplicador` construye una función multiplicadora
por `k`. Y la función `make-exponenciador` construye una función de un
argumento `x` que eleva `k` a `x`.

**Pista**: Una diferencia importante entre ambas funciones es que
`(make-exponenciador k)` debe fijar a `k` el primer parámetro de la
función `expt`, mientras que `(make-sumador k)` y `(make-multiplicador
k)` fijan el segundo parámetro.

Ejemplo:

```scheme
(define f1 (make-multiplicador 10))
(f1 3) ; ⇒ 30
(define f2 (make-multiplicador 5))
(f2 3) ; ⇒ 15
(define f3 (make-exponenciador 2))
(f3 3) ; ⇒ 8
(define f4 (make-exponenciador 5))
(f4 3) ; ⇒ 125
```


b) Generaliza las funciones anteriores definiendo la función
`(fija-arg f i k)` que recibe una función de dos argumentos `f`, un
valor `i` que indica si queremos fijar el primer parámetro (cuando
i = 1) o el segundo parámetro (i = 2) y el valor `k` al que fijamos el
parámetro correspondiente.

Ejemplo:

```scheme
(define f1 (fija-arg + 2 10))
(f1 8) ; ⇒ 18
(define f2 (fija-arg expt 1 2))
(f2 5) ; ⇒ 32
(define f3 (fija-arg string-append 2 "****"))
(f3 "Hola") ; ⇒ "Hola****"
(define f4 (fija-arg string-append 1 "****"))
(f4 "Hola") ; ⇒ "****Hola"
```

### Ejercicio 5 ###


a) Implementa usando funciones de orden superior la función `(suma-n-izq n
lista-parejas)` que recibe una lista de parejas y devuelve otra lista
a la que hemos sumado `n` a todas las partes izquierdas.

Ejemplo

```scheme
(suma-n-izq 10 '((1 . 3) (0 . 9) (5 . 8) (4 . 1)))
; ⇒ {{11 . 3} {10 . 9} {15 . 8} {14 . 1}}
```


b) Implementa usando funciones de orden superior la función `(aplica-2 func
lista-parejas)` que recibe una función de dos argumentos y una lista
de parejas y devuelve una lista con el resultado de aplicar esa
función a los elementos izquierdo y derecho de cada pareja.

Ejemplo:

```scheme
(aplica-2 + '((2 . 3) (1 . -1) (5 . 4)))
; ⇒ {5 0 9}
(aplica-2 (lambda (x y)
             (if (even? x)
                 y
                 (* y -1))) '((2 . 3) (1 . 3) (5 . 4) (8 . 10)))
; ⇒ {3 -3 -4 10}
```


### Ejercicio 6 ###

a) Utilizando una composición de las funciones de orden superior
`fold-right` y `map`, implementa la función `(cadena-mayor lista)` que
recibe un lista de cadenas y devuelve una pareja con la cadena de
mayor longitud y dicha longitud.  En el caso de que haya más de una
cadena con la máxima longitud, se devolverá la última de ellas que
aparezca en la lista.

```scheme
(cadena-mayor '("vamos" "a" "obtener" "la" "cadena" "mayor")) ; ⇒  {"obtener" . 7}  
(cadena-mayor '("prueba" "con" "maximo" "igual")) ; ⇒ {"maximo" . 6} 
(cadena-mayor '("hola")) ; ⇒ {"hola" . 4} 
``` 

b) La función `map` de Scheme también puede mapear una función de dos
argumentos sobre dos listas. Por ejemplo:

```scheme
(map (lambda (x y)
         (+ x y)) '(1 2 3 4) '(4 4 4 4))
; ⇒ {5 6 7 8}
```

Implementa la función `(filtra-simbolos lista-simbolos lista-num)` de
de la práctica 3, usando una composición de funciones en las que se
use `map` como en el ejemplo anterior.


----

Lenguajes y Paradigmas de Programación, curso 2017-18  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Domingo Gallardo, Cristina Pomares, Antonio Botía, Francisco Martínez
