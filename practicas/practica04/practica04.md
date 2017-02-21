# Práctica 4: Funciones como datos de primera clase

## Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica04.rkt` con una cabecera inicial con tu nombre y apellidos, y
las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Un conjunto de **pruebas** que comprueben su funcionamiento
  utilizando la librería `schemeunit`.
  
**Nota:** No es necesario incluir demostraciones de las funciones con
`display`, pero sí un par de pruebas por cada función con `schemeunit`
para comprobar su correcto funcionamiento.

## Ejercicios

### Ejercicio 1

a) Implementa la función `(aplica-funciones lista-parejas)` de forma
recursiva. La función recibe una lista de parejas cuya parte izquierda
es una función y la parte derecha un argumento para dicha función, y
devuelve la lista con los resultados de aplicar cada función a su
argumento.

Ejemplo:

```scheme
(aplica-funciones (list (cons even? 6) (cons null? '(4)) (cons list 8) (cons car '(1 2 3))  
; ⇒ {#t #f {8} 1}
```

b) Implementa la función anterior usando una función de orden
superior. Defínela con el nombre `aplica-funciones-FOS`

c) Implementa la función `(suma-impares-pares lista-num)` de la
práctica 3 utilizando la función de orden superior `fold-right`.

### Ejercicio 2

Implementa, usando funciones de orden superior, las funciones
siguientes funciones de la práctica 3:

- `(edad-total lista-personas)`
- `(filtra-sexo sexo personas)`
- `(mayores-edad edad personas)`
- `(mayores-edad-sexo edad sexo personas)`

En el caso de la última función, no debes llamar a las funciones
anteriores, sino usar una llamada a una única función de orden
superior en su cuerpo.


### Ejercicio 3

a) Utilizando una función de orden superior, implementa la función
`(resultados‐quiniela lista‐parejas)` que devuelve una lista de 1, X,
2 a partir de una lista de parejas que representa resultados de
partidos de fútbol. El resultado es 1 cuando el número izquierdo de la
pareja es mayor que el derecho, un 2 cuando es al revés, y una X
cuando los dos números son iguales.

**Pista**: Puedes definir una función `(resultado-partido partido)`
  que reciba una pareja y devuelva 1, X o 2.

```scheme
(resultados-quiniela '((1 . 2) (4 . 4) (3 . 5) (6 . 2) (9 . 9)))  ; ⇒ {2 X 2 1 X}
(resultados-quiniela '((2 . 2))) ; ⇒ {X}
(resultados-quiniela '((3 . 2) (4 . 3) (3 . 5)))  ; ⇒ {1 1 2}
```

b) Utilizando una composición de funciones de orden superior,
implementa la función `(cuenta-resultados resultado lista-resultados)`
donde `resultado` puede ser un uno, una equis o un dos y cuenta el
número de ese resultado en la lista de partidos.

Ejemplo:

```scheme
(cuenta-resultados 'X '((1 . 2) (4 . 4) (3 . 5) (6 . 2) (9 . 9))) ; ⇒ 2
```

c) Usando la función anterior escribe la función
`(cuenta-resultados-quiniela lista-resultados)` que recibe un lista de
parejas que representan partidos de fútbol, y debe devolver una lista
con el número de unos, número de X y número doses que hay en la
quiniela. No hace falta que sea recursiva ni implementarla con
funciones de orden superior.


```scheme
(cuenta-resultados-quiniela '((1 . 2) (4 . 4) (3 . 5) (6 . 2) (9 . 9)))  ; ⇒ {1 2 2}
(cuenta-resultados-quiniela '((2 . 2))) ; ⇒ {0 1 0}
(cuenta-resultados-quiniela '((3 . 2) (4 . 3) (3 . 5)))  ; ⇒ {2 0 1}
```

### Ejercicio 4

Utilizando una composición de las funciones de orden superior
`fold-right` y `map`, implementa la función `(cadena-mayor lista)` que
recibe un lista de cadenas y devuelve una pareja con la cadena de
mayor longitud y dicha longitud.  En el caso de que haya más de una
cadena con la máxima longitud, se devolverá la última de ellas que
aparezca en la lista.

```scheme
(cadena-mayor '("vamos" "a" "obtener" "la" "cadena" "mayor")) ; ⇒  {"obtener" . 7}  
(cadena-mayor '("prueba" "con" "maximo" "igual")) ; ⇒ {"maximo" . 6} 
(cadena-mayor '("hola")) ; ⇒ {"hola" 4} 
``` 

### Ejercicio 5

a) Define las funciones `(min-lista lista)` y `(max-lista lista)`,
de forma que devuelvan el mínimo o el máximo de una lista respectivamente,
utilizando la función de orden superior `fold-right`.

**Pista**: Puedes usar como dato base de la función `fold-right`
el primer dato de la lista.

```scheme
(min-lista '(20 10 23 101 90 19 45)) ; ⇒ 10
(max-lista '(20 10 23 101 90 19 45)) ; ⇒ 101
```

b) Utilizando funciones de orden superior, define las siguientes funciones:

- `(max-car-pareja lista-parejas)`: devuelve el max de todos los car de las parejas de la lista
- `(min-car-pareja lista-parejas)`: devuelve el min de todos los car de las parejas de la lista
- `(max-cdr-pareja lista-parejas)`: devuelve el max de todos los cdr de las parejas de la lista
- `(min-cdr-pareja lista-parejas)`: devuelve el min de todos los cdr de las parejas de la lista

Ejemplo:

```scehem
(max-car-pareja (list (cons 21 30)
                      (cons 34 65)
                      (cons 99 12)
                      (cons 45 86))) ; ⇒  99

(min-cdr-pareja (list (cons 21 30)
                      (cons 34 65)
                      (cons 99 12)
                      (cons 45 86))) ; ⇒ 12
```


c) Define la función `(min-max-pareja lista pred parte-pareja)` que
generaliza las funciones anteriores, usando la función de orden
superior `fold-right`. El argumento `pred` puede ser la función `>` o
`<` y el argumento `parte-pareja` la función `car` o `cdr`.

Ejemplos:

```scheme
(min-max-pareja (list (cons 20 30) (cons 21 34) (cons 18 45)) > car) ; ⇒ 21
(min-max-pareja (list (cons 20 30) (cons 21 34) (cons 18 45)) > cdr) ; ⇒ 45)
```

d) Define por último la función `(bounding-box-puntos lista-parejas)`
que reciba una lista de puntos2D (parejas) y devuelva el bounding-box
que engloba a todos los puntos.  El bounding-box a devolver será en
forma de pareja, representado por los puntos inferior izquierdo y
superior derecho del rectángulo. Debes usar la función definida
en el apartado anterior.

Ejemplo:

```scheme
(bounding-box-puntos (list (cons 2  2) 
                           (cons 30  20)
                           (cons 10 0)
                           (cons 1 10)
                           (cons 15 12)
                           (cons 35 10))) ; ⇒ {{1 . 0} {35 . 20}}
```

----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  
