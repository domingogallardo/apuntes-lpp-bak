## Práctica 9: Estructuras de datos mutables, clausuras, ámbito de variables y estado local

### Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica09.rkt` con una cabecera inicial con tu nombre y apellidos, y
las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Un conjunto de **pruebas** que comprueben su funcionamiento
  utilizando la librería `schemeunit`.

## Ejercicios

### Ejercicio 1

¿Qué devolvería o imprimiría Scheme en las siguientes expresiones?
Hazlo sin consultar el intérprete.

a) Rellena los huecos:

```scheme
(define x 1)
(define (foo)
   (+ x 1))
(define (bar)
   (define x 2)
   (foo))
(bar) ; ⇒ ________
```

b) Rellena los huecos:

```scheme
(define y 1)
(define (foo)
   (define y 2)
   (lambda (x)
      (+ x y)))
(define (bar f)
    (define y 3)
    (lambda (x)
        (f x)))
(define g (bar (foo)))
(g 10) ; ⇒ __________
```

c) Rellena los huecos:

```scheme
(define (foo z)
   (define y 5)
   (set! z (+ z 1))
   (if (> z 0)
      (let ((y (+ z 10))
            (z 20))
         (lambda (x)
            (+ x y z)))
      (lambda (x)
         (+ x  y z))))

(define f (foo -10))
(define g (foo 10))
(f 1) ; ⇒ ________
(g 1) ; ⇒ ________
```

d) Indica qué aparecería por pantalla:

```scheme
(define x 0)
(define (foo x)
   (define y 0)
   (define z (/ x y))
   (lambda (x)
      (/ z x)))
(display "A")
(define g (foo x))
(display "B")
(display (g 0))
```

e) Rellena los huecos:

```scheme
(define (foo)
  (define x 100)
  (lambda (y)
    (+ x y)))

(define (bar)
  (define x 1)
  (lambda (y)
    (set! x (+ x y))
    x))

(define (aplica f g)
  (define x 10)
  (g x) 
  (f (g x)))

(define f1 (foo)) 
(define f2 (bar))
(aplica f1 f2) ; ⇒ _________
```


### Ejercicio 2

Define la función `(make-flip)` que construya una función que cada vez
que se ejecute devuelva 1, 0, 1 ...

Ejemplo:

```scheme
(define flip (make-flip))
(flip) ; ⇒ 1
(flip) ; ⇒ 0
(flip) ; ⇒ 1
```

### Ejercicio 3

a) Define el tipo de dato mutable `Cola` que implementa una cola FIFO
en la que se añaden y se sacan elementos con mutación. La barrera de
abstracción se define por las siguientes funciones:

- `(make-cola)`: construye una cola vacía y la devuelve.
- `(encolar! dato cola)`: añade un dato al final de la cola.
- `(desencolar! cola)`: devuelve el primer dato de la cola y lo
  elimina de la cola. No devuelve nada si la cola está vacía.
- `(vacia-cola? cola)`: #t si la cola está vacía

Ejemplo:

```scheme
(define cola (make-cola))
(encolar! 1 cola)
(encolar! 2 cola)
(encolar! 'a cola)
(encolar! 'b cola)
(desencolar! cola)  ; ⇒ 1
(vacia-cola? cola)  ; ⇒ #f
(desencolar! cola)  ; ⇒ 2
(desencolar! cola)  ; ⇒ 'a
(desencolar! cola)  ; ⇒ 'b
(vacia-cola? cola)  ; ⇒ #t
```

b) Implementa la función `(memorizador func)` que recibe una función
de dos argumentos y devuelve una clausura que se comporta como la
función original, pero que guarda los resultados que se van generando
en una cola local (usa la implementación de la cola del apartado
anterior). Cuando recibe en uno de sus argumentos el símbolo `'res`,
devuelve el primer resultado encolado y lo desencola.

Ejemplo:

```scheme
(define f (memorizador +))
(define g (memorizador string-append))
(f 5 7) ; ⇒ 12
(f 3 2) ; ⇒ 5
(f 1 6) ; ⇒ 7
(g "hola" "que tal") ; ⇒ "holaque tal"
(g "lo siguiente es " "Swift") ; ⇒ "lo siguiente es Swift"
(f 'res 'res) ; ⇒ 12
(g 'res 'res) ; ⇒ "holaque tal"
(f 'res 'res) ; ⇒ 5
(g 'res 'res) ; ⇒ "lo siguiente es Swift"
(f 8 9) ; ⇒ 17
(f 'res 'res) ; ⇒ 7
(f 'res 'res) ; ⇒ 17
```

### Ejercicio 4

Define la función `(mi-cons x y)` que almacene dos valores `x` e
`y`. Devuelve una función _dispatch_ que espera un mensaje: `'car`,
`'cdr`, `set-car!` o `'set-cdr!`. Utilizando estado local construye
`mi-cons` para implementar con el _dispatch_ una pareja y sus
funciones asociadas. 

No puedes usar la función `cons`.

Ejemplo:

```scheme
(define a (mi-cons 1 2))
((a 'car)) ; ⇒ 1
((a 'cdr)) ; ⇒ 2
((a 'set-car!) 40)
((a 'set-cdr!) 60)
((a 'car)) ; ⇒ 40
((a 'cdr)) ; ⇒ 60
```

### Ejercicio 5

Define la función `cuenta-bancaria` que simule una cuenta en un banco
con estado local.  Al crear la cuenta bancaria se inicializa con un
saldo inicial. Debe proporcionar acceso a ingresos, reintegros,
consulta de saldo e historial de operaciones realizadas.

```scheme
(define c (cuenta-bancaria 100))
((c 'reintegro) 50)
((c 'ingreso) 250)
((c 'reintegro) 25)
((c 'saldo)) ; ⇒ 275
((c 'historial)) ; ⇒ {*hist* {reintegro . 25} {ingreso . 250} {reintegro . 50}}
```


----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  
