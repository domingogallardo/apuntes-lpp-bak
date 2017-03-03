## Práctica 6: Listas estructuradas

### Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica06.rkt` con una cabecera inicial con tu nombre y apellidos, y
las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Un conjunto de **pruebas** que comprueben su funcionamiento
  utilizando la librería `schemeunit`.

## Ejercicios

### Ejercicio 1

a) Define la función `(cuenta-pred-lista pred lista)` que recibe una
lista estructurada y un predicado y cuenta todos los elementos que
cumplan el predicado. 

Ejemplos:

```scheme
(cuenta-pred-lista even? '(((1 2) 3 (4) 5) 6)) ; ⇒ 3
(cuenta-pred-lista pair? '(((1 . 2) 3 (4 . 3) 5) 6)) ; ⇒ 2
(cuenta-pred-lista string? '(("hola" 3 ((a "que") hola) "hora" 6 (("es"))) "time")) ; ⇒ 5
```

b) Define la función del apartado anterior utilizando funciones de
orden superior.

### Ejercicio 2

a) Implementa la función `(cumplen-predicado pred lista)` que devuelva
una lista con todos los elementos de lista estructurada que cumplen un
predicado.

Ejemplo:

```scheme
(cumplen-predicado even? '(1 (2 (3 (4))) (5 6))) ; ⇒ {2 4 6}
(cumplen-predicado pair? '(((1 . 2) 3 (4 . 3) 5) 6)) ; ⇒ {{1 . 2} {4 . 3}
```

b) Implementa la función del apartado anterior utilizando funciones de
orden superior.


### Ejercicio 3

Utilizando funciones de orden superior, implementa la función
`(sustituye-elem lista elem-old elem-new)` que recibe como argumentos
una lista estructurada y dos elementos, y devuelve otra lista con la
misma estructura, pero en la que se ha sustituido las ocurrencias de
`elem-old` por `elem-new`.

```scheme
(sustituye-elem  '(a b (c d (e d)) c (d (d) g)) 'd 'X)  ; ⇒  {a b {c X {e X}} c {X {X} g}}
(sustituye-elem  '(a b (c d (e d))) 'f 'X)  ; ⇒  {a b {c d {e d}}}
```

### Ejercicio 4

a) Implementa la función recursiva `(cuenta-hojas-debajo-nivel lista
n)` que recibe una lista estructurada y un número que indica un nivel,
y devuelve el número de hojas que hay por debajo de ese nivel.

```scheme
(cuenta-hojas-debajo-nivel '(10 2 (4 6 (9 3) (8 7) 12) 1) 2) ; ⇒ 4
(cuenta-hojas-debajo-nivel '(10 2 (4 6 (9 3) (8 7) 12) 1) 3) ; ⇒ 0
```

b) Implementa la función del apartado anterior utilizando funciones de
orden superior.


### Ejercicio 5

Define la función `(nivel-elemento lista)` que reciba una lista
estructurada y devuelva una pareja `(elem . nivel)`, donde la parte
izquierda es el elemento que se encuentra a mayor nivel y la parte
derecha el nivel en el que se encuentra. Puedes definir alguna
función auxiliar si lo necesitas.

```scheme
(nivel-elemento '(2 (3))) ;⇒ (3 . 2)
(nivel-elemento '((2) (3 (4)((((((5))) 6)) 7)) 8)) ; ⇒ (5 . 8)
```

----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  
