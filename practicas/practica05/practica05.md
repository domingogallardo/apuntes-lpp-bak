## Práctica 5: Procedimientos recursivos

### Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica05.rkt` con una cabecera inicial con tu nombre y apellidos, y
las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Un conjunto de **pruebas** que comprueben su funcionamiento
  utilizando la librería `schemeunit`.

## Ejercicios

### Ejercicio 1

Define utilizando recursión por la cola (_tail recursion_) la función
`(min-lista lista)` de la práctica anterior que recibe una lista
numérica y devuelve el mínimo de sus elementos.

Ejemplo:

```scheme
(min-lista '(2 5 9 12 5 0 4)) ; ⇒ 0
```

### Ejercicio 2

Define utilizando recursión por la cola (_tail recursion_) la función
`(aplica-funciones lista-parejas)` de la práctica anterior, que recibe
una lista de parejas `{{función . argumento} ...}` y devuelve la lista
con los resultados de aplicar cada función al argumento situado en la
parte derecha de la pareja.

Ejemplo:

```scheme
(aplica-funciones (list (cons list 2) (cons even? 5) (cons not #f))) 
; ⇒ {{2} #f #t}
```


### Ejercicio 3

Implementa la función `(composicion-conmutativa lista-parejas x)`
mediante recursión por la cola (_tail recursion_), que recibe una
lista de parejas formadas por 2 funciones y debe devolver una lista
cuyos elementos serán #t o #f dependiendo de si la composición del par
de funciones correspondiente es o no conmutativa. La primera función
de la composición se aplicará con el argumento x recibido.

Es decir, se trata de comprobar para todo par de funciones `{f . g}`
contenidas en una pareja, si se cumple `f(g(x)) = g(f(x))`.
                        
Ejemplos:

```scheme
(composicion-conmutativa? 
    (list (cons (lambda(x) (+ x 2)) (lambda(x) (- x 5)))) 10) ; ⇒ {#t}
(composicion-conmutativa? 
    (list (cons (lambda(x) (+ x 2)) (lambda(x) (- x 5)))
          (cons (lambda(x) (* x 2)) (lambda(x) (/ x 5)))) 10) ; ⇒ {#t #t}
(composicion-conmutativa? 
    (list (cons (lambda(x) (+ x 2)) (lambda(x) (- x 5)))
          (cons (lambda(x) (* x 2)) (lambda(x) (+ x 5)))) 10) ; ⇒ {#t #f}
(composicion-conmutativa? 
    (list (cons (lambda(x) (/ x 2)) (lambda(x) (- x 5)))) 10) ; ⇒ {#f}
```

### Ejercicio 4

Define la función `(pascal-memo fila col)` que devuelve la posición
`(fila, col)` del Triángulo de Pascal visto en teoría, utilizando la
técnica _memoization_. 

Debes modificar la función `get` que hay en teoría cambiándola por la
siguiente para que funcione correctamente cuando la clave es una pareja:

```scheme
(define (busca-pareja pareja-key lista)
  (cond ((null? lista) #f)
        ((equal? (caar lista) pareja-key) (car lista))
        (else (busca-pareja pareja-key (cdr lista)))))
        
(define (get pareja-key lista)
  (define record (busca-pareja pareja-key (cdr lista)))
  (if (not record)
      '()
      (cdr record)))
```


### Ejercicio 5

a) Usando gráficos de tortura, implementa la función
`(piramide-hexagonal lado decremento)` que dibuje hexágonos
concéntricos.

Por ejemplo, la llamada a `(piramide-hexagonal 150 10)` debe dibujar
la siguiente figura:

<img src="imagenes/hexagono.png" width="300px"/>

b) Define la función `(alfombra-sierpinski tam)` que construya la
Alfombra de Sierpinski (una variante del Triágulo de Sierpinski que
hemos visto en teoría) de lado `tam` píxeles utilizando gráficos de
tortuga. 

Por ejemplo, la llamada a `(alfombra-sierpinski 500)` debe dibujar la
siguiente figura:

<img src="imagenes/alfombra-sierpinski.png" width="400px"/>

----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  
