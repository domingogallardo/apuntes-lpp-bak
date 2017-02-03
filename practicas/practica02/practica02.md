# Práctica 2: Programación funcional en Scheme

## Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica02.rkt` con una cabecera inicial con tu nombre y apellidos, y
las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Una visualización por pantalla de uno de los ejemplos incluidos en
  el enunciado que **demuestre qué hace la función**, usando la
  función de `display`.
- Un conjunto de **pruebas** que comprueben su funcionamiento
  utilizando la librería `schemeunit`. 

## Ejercicios


### Ejercicio 1

Define la función `(cuenta-simbolos lista)` que reciba una lista de
elementos y devuelva el número de símbolos que contiene.

```scheme
(cuenta-simbolos '(2 40 #f)) ; ⇒ 0
(cuenta-simbolos '(2 4 #f a)) ; ⇒ 1
(cuenta-simbolos '(2 a "hola" #f hola)) ; ⇒ 2
```

### Ejercicio 2

Implementa la función `(minimo lista)` que reciba una lista numérica
como argumento y devuelva el menor número de la lista. Suponemos
listas de 1 o más elementos. No puedes utilizar la función `min` de
Scheme, aunque puedes definirte y utilizar una función auxiliar
`menor`.

**Pista**: Podemos expresar el caso general de la recursión de la siguiente forma:

> El mínimo de los elementos de una lista es el menor entre
> el primer elemento de la lista y el mínimo del resto de la lista.


Ejemplos:

```scheme
(minimo '(9 8 6 4 3)) ; ⇒ 3
(minimo '(9 8 3 6 4)) ; ⇒ 3
```

#### Ejercicio 3

Implementa la función `(binario-a-decimal lista-bits)` que reciba una lista de bits que representan
un número en binario (el primer elemento será el bit más significativo) y devuelva el número decimal
equivalente. 

**Pista**: puedes utilizar la función length.

```scheme
(binario-a-decimal '(1 1 1 1)) ; ⇒ 15
(binario-a-decimal '(1 1 0)) ; ⇒ 6
(binario-a-decimal '(1 0)) ; ⇒ 2
```

#### Ejercicio 4

Implementa el predicado `(repetidos? lista)` que recibe una lista y
devuelve `#t` si algún elemento está repetido en la lista, y `#f` en
caso contrario.

**Pista**: puedes definir y utilizar la función auxiliar
(member? elem lista), que comprueba si un elemento está en la lista

```scheme
(repetidos? '(1 2 3 5 4 5 6)) ; ⇒ #t
(repetidos? '(adios hola que tal)) ; ⇒ #f
(repetidos? '(#t #f #t #t #t)) ; ⇒ #t
```

#### Ejercicio 5

Implementa el predicado `(adyacentes-iguales? lista)` que recibe una lista de elementos
y devuelve #t si tiene al menos dos elementos adyacentes iguales o #f en caso contrario. 

```scheme
(adyacentes-iguales? '(12 30 5  5 2)) ; ⇒ #t
(adyacentes-iguales? '("esto" "es" "una" "lista" "de" "strings")) ; ⇒ #f
(adyacentes-iguales? (list (cons 1 2) (cons 1 2) (cons 3 4))) ; ⇒ #t
(adyacentes-iguales? '(12 30 #t #\a 5 #f #f)) ; ⇒ #t
```

----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares
