## Práctica 1: Introducción a Scheme

**Importante**: Antes de empezar esta práctica debes haber terminado
  *todos* los ejercicios del seminario de Scheme.

### Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica01.rkt` con una cabecera inicial con tu nombre y apellidos, y
las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- la **definición de las funciones** que resuelven el ejercicio
- una visualización por pantalla de uno de los ejemplos incluidos en
  el enunciado que **demuestre qué hace la función**, usando la
  función de `display`
- y un conjunto de **pruebas** que comprueben su funcionamiento,
  utilizando el API SchemeUnit.

Por ejemplo, supongamos que el primer ejercicio de la práctica 1 sea
implementar la función `suma-cuadrados` que vimos en la sesión de
introducción a Scheme y se proponen en el enunciado los siguientes
ejemplos:

```scheme
(suma-cuadrados 10 10) ⇒ 200
(suma-cuadrados -2 9) ⇒  85
```

La solución se podría entregar de la siguiente forma:

**`practica01.rkt`**:

```scheme
;; José Fernandez Muñoz

#lang r6rs
(import (rnrs base)
        (rnrs io simple)
        (schemeunit))


;;
;; Ejercicio 1: suma-cuadrados
;;

(define (suma-cuadrados x y)
    (+ (* x x) (* y y)))

;; Demostración
(display "\n\nEjercicio 1\n")
(display "La suma de los cuadrados de 10 y 10 es: ")
(display (suma-cuadrados 10 10))

;; Pruebas
(check-equal?  (suma-cuadrados 10 10)  200)
(check-equal?  (suma-cuadrados -2 9)  85)
(check-equal?  (suma-cuadrados 0.5 9)  81.25)

;;
;; Ejercicio 2:
;;

...

```

En los casos de prueba se pueden incluir los ejemplos del enunciado
del ejercicio y alguno más que compruebe que la implementación
funciona correctamente.

### Ejercicios

#### Ejercicio 1

a) Implementa la función `(binario-a-decimal b3 b2 b1 b0)` que reciba
4 bits que representan un número en binario y devuelva el número
decimal equivalente.

```scheme
(binario-a-decimal 1 1 1 1) ⇒ 15
(binario-a-decimal 0 1 1 0) ⇒ 6
(binario-a-decimal 0 0 1 0) ⇒ 2
```

**Nota**: recuerda que para realizar esta conversión, se utiliza la siguiente fórmula:

```
n = b3 * 2ˆ3 + b2 * 2ˆ2 + b1 * 2ˆ1 + b0 * 2ˆ0
```

**Pista**: puedes utilizar la función `expt`


b) Implementa la función `(binario-a-hexadecimal b3 b2 b1 b0)` que
reciba 4 bits de un número representado en binario y devuelva su
representación equivalente en hexadecimal.

```scheme
(binario-a-hexadecimal 1 1 1 1) ⇒ F
(binario-a-hexadecimal 0 1 1 0) ⇒ 6
(binario-a-hexadecimal 1 0 1 0) ⇒ A
```

**Nota**: para realizar esta conversión, como paso intermedio puedes pasar
primero el número binario a su representación decimal (utilizando la
función definida en el apartado anterior) y después a su
correspondiente hexadecimal. Recuerda que la represetación hexadecimal
de los números decimales del 0 al 9 no cambia, y que el número decimal
10 se representa con el carácter A, el 11 con el B, y asi
sucesivamente hasta el 15 que es el F en hexadecimal.

**Pista**: puedes utilizar las funciones `integer->char` y `char->integer`

#### Ejercicio 2

Supongamos que estamos implementando un **juego de guerra de barcos** en
el que los barcos están situados en coordenadas del plano definidas
por la posición _x_ y la posición _y_, ambos números reales (metros).

Cada barco puede lanzar un torpedo a otro barco con una velocidad
determinada (_v_, en km/h). El torpedo tiene combustible, y seguirá
moviéndose hasta que se termine. La distancia a la que eso sucede la
denominamos el **alcance** del torpedo (ver la siguiente figura):

<img src="imagenes/barcos.png" width="400px"/>

Cuanto más alta es la velocidad del torpedo, antes termina su
combustible, con una relación cuadrática. En concreto, la velocidad
(en km/h) define el tiempo de terminación del combustible (_t_, en
segundos) con la siguiente expresión:

```
t= 50000 / v^2
```

Recuerda que conociendo la velocidad de un móvil y el tiempo que está
moviéndose podemos calcular el espacio recorrido con la siguiente
expresión:

```
e = v * t
```

Esto es, multiplicando la velocidad (en m/segundo) por el tiempo
(segundos) resulta el espacio en metros recurrido.

Dadas todas estas condiciones, debes programar en Scheme la función 

```scheme
(dentro-alcance? x1 y1 x2 y2 v)
```

que tome como parámetros las coordenadas `x1`, `y1` del barco 1 que lanza
el torpedo, `x2`, `y2` del barco 2 al que se le lanza y `v` la velocidad
del torpedo. 

La función debe comprobar si el barco 2 está dentro del alcance del
torpedo, tal y como lo hemos definido previamente y devolver el 
booleano correspondiente.

Debes modularizar la implementación, creando las funciones auxiliares
que necesites para que el código sea legible y auto-documentado (los
nombres de las funciones y los parámetros deben ser lo más
descriptivos posibles).

Ejemplos:

```
(dentro-alcance? 0 0 500 500 30) ⇒ #f
(dentro-alcance? 100 200 500 500 20) ⇒ #t
```

#### Ejercicio 3

Implementa la función `(tirada-ganadora t1 t2)` que reciba 2 parejas
como argumento, donde cada pareja representa una tirada con 2 dados
(contiene dos números). La función debe determinar qué tirada es la
ganadora, teniendo en cuenta que será aquella cuya suma de sus 2 dados
esté más próxima al número 7. La función devolverá 1 si `t1` es la
ganadora, 2 si `t2` es la ganadora o bien 0 si hay un empate. Este
último caso se producirá cuando la diferencia con 7 de ambas tiradas
es la misma.

```scheme
(tirada-ganadora (cons 1 3) (cons 1 6)) ⇒ 2
(tirada-ganadora (cons 1 5) (cons 2 2)) ⇒ 1
(tirada-ganadora (cons 6 2) (cons 3 3)) ⇒ 0
```

#### Ejercicio 4

Define la función `tipo-triangulo` que recibe como parámetro las
coordenadas en el plano de los vértices de un triángulo representados
con parejas. La función devuelve un string con el tipo de triángulo
correspondiente: equilátero, isósceles o escaleno.


Ejemplos:

```scheme
(tipo-triangulo (cons 1 1) (cons  1 6) (cons 6 1)) ⇒ "isosceles"
(tipo-triangulo (cons -2 3) (cons  2 6) (cons 5 3)) ⇒ "escaleno"
(tipo-triangulo (cons -4 3) (cons  0 0) (cons -4.5891 -1.9641)) ⇒ "equilatero"
```

### Ejercicio 5


Define la función `calculadora` que recibe una lista como
parámetro. La lista contiene tres elementos: el primero es un símbolo
(`+`, `-`, `*` o `/`) que indica un operador y los dos siguientes
elementos corresponden a los operandos. La función realiza la
operación y devuelve el resultado.


Ejemplos:

```scheme
(calculadora '(+ 2 3)) ⇒ 5
(calculadora '(* 3 4)) ⇒ 12
(calculadora '(- 7 4)) ⇒ 3
(calculadora '(/ 6 3)) ⇒ 2
```

----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares
