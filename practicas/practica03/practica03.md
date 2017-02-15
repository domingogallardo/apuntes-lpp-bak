# Práctica 3: Parejas, datos compuestos y listas

## Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero
`practica03.rkt` con una cabecera inicial con tu nombre y apellidos, y
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

Define la función `(calcular-lista lista)` que recibe una lista que
contiene símbolos (operadores +,-,*,/) y parejas de números
(operandos), representando operaciones matemáticas. Se considera que
la lista siempre está bien formada, con sus elementos en este orden:
`(símbolo pareja símbolo pareja...)`. Tiene que devolver una lista con
los resultados de las operaciones.

Ejemplos:

```scheme
(calcular-lista (list '+ (cons 2 3) '* (cons 4 5))) ; ⇒ {5 20}
(calcular-lista (list '/ (cons -6 2) '- (cons 4.5 0.5))) ; ⇒ {-3 4.0}
```

### Ejercicio 2

a) Define la función `(bounding-box triangulo)` que reciba un
triangulo (una lista formada por 3 parejas que representan las
coordenadas cartesianas de los tres puntos de un triángulo. La función
devuelve el rectángulo que engloba al triángulo, representando el
rectángulo por sus puntos inferior izquierdo y superior derecho.

La estructura a devolver será un pareja formada a su vez por dos
parejas que representan los dos puntos del rectángulo.

Ejemplos:

```scheme
(bounding-box (list (cons -10 -10) (cons 0 -20) (cons 8 6)))
; ⇒ {{-10. -20} . {8 . 6}}
; Representación en DrRacket: {{-10 . -20} 8 . 6}
(bounding-box (list (cons 0.5 0.25) (cons 0.25 -10) (cons -5.5 10)))
; ⇒ {{-5.5 . -10.0} . {0.5 . 10.0}} 
; Representación en DrRacket: {{-5.5 . -10.0} 0.5 . 10.0}
```

b) Define la función `(lista-bounding-box lista-triangulos)` que recibe
una lista de triángulos y devuelve una lista con el bounding box de
cada uno.

Ejemplo:

```scheme
(lista-bounding-box (list (list (cons 10 0) (cons 1 20) (cons 30 10))
                          (list (cons -10.5 0) (cons 1 0) (cons -10 10))
                          (list (cons 10 -10) (cons -10 10) (cons 0	0))))
; ⇒  {{{1 . 0} . {30 . 20}} 
;     {{-10.5 . 0} . {1.0 . 10}}
;     {{-10 . -10} . {10 . 10}}}
; Representación en DrRacket:
; {{{1 . 0} 30 . 20} {{-10.5 . 0} 1.0 . 10} {{-10 . -10} 10 . 10}}
```

### Ejercicio 3

Implementa la función recursiva `(cadena-mayor lista)` que recibe un
lista de cadenas y devuelve una pareja con la cadena de mayor longitud
y dicha longitud.  En el caso de que haya más de una cadena con la
máxima longitud, se devolverá la última de ellas que aparezca en la
lista.

**Pista**: puedes utilizar la función `string-length`

```scheme
(cadena-mayor '("vamos" "a" "obtener" "la" "cadena" "mayor")) ; ⇒  {"obtener" . 7}
(cadena-mayor '("prueba" "con" "maximo" "igual")) ; ⇒ {"maximo" . 6} 
(cadena-mayor '("hola")) ; ⇒ {"hola" . 4} 
```

### Ejercicio 4

a) Implementa la función `(intercambia-elem pareja)` que recibe una
pareja y devuelve otra pareja con los elementos izquierdo y derecho
intercambiados.

Ejemplo:
```
(intercambia-elem (cons 10 5))  ; ⇒ {5 . 10}
```

b) Implementa las funciones `(suma-izq n pareja)` y `(suma-der n
pareja)` definidas de la siguiente forma:

- `(suma-izq n pareja)` : recibe un número y una pareja de números, y
  devuelve una nueva pareja en la que se ha sumado el número n a la
  parte izquierda de pareja.
- `(suma-der n pareja)` : recibe un número y una pareja de números, y
  devuelve una nueva pareja en la que se ha sumado el número n a la
  parte derecha de pareja.

Ejemplos:
```
(suma-izq 5 (cons 10 20))  ; ⇒ {15 . 20}
(suma-der 6 (cons 10 20))  ; ⇒ {10 . 26}
```

c) Implementa la función recursiva `(suma-impares-pares lista-num)`
que devuelva una pareja cuya parte izquierda sea la suma de todos los
números impares de la lista y la parte derecha la suma de todos sus
números pares. En el caso en que ningún número par o impar deberá
devolver 0. Debes utilizar las funciones auxiliares definidas en el
apartado anterior. También puedes utilizar las funciones predefinidas
`even?` y `odd?`.

Ejemplos:

```scheme
(suma-impares-pares '(3 2 1 4 8 7 6 5)) ; ⇒ {16 . 20}
(suma-impares-pares '(3 1 5))           ; ⇒ {9 . 0}
```

### Ejercicio 5

Implementa la función recursiva `(resultados-equipo lista-resultados)`
que recibe un lista de parejas, en donde cada pareja representa el
resultado de un partido de un equipo cuando juega como local (los
goles marcados por nuestro equipo y los marcados por el equipo
contrario), y se debe devolver una lista con el número de partidos
ganados, empatados y perdidos de nuestro equipo.

```scheme
(resultados-equipo '((1 . 2) (4 . 4) (3 . 5) (6 . 2) (9 . 9)))  ; ⇒ {1 2 2}
(resultados-equipo '((2 . 2))) ; ⇒ {0 1 0}
(resultados-equipo '((3 . 2) (4 . 3) (3 . 5)))  ; ⇒ {2 0 1}
```

### Ejercicio 6

Queremos guardar la siguiente información de una persona utilizando
**dos parejas**:

- nombre (string)
- edad (entero)
- sexo (símbolos 'hombre o 'mujer)

a) Escribe la función `(crea-persona nombre edad sexo)` que devuelva
una persona (estructura formada por dos parejas) con esa información.

Escribe las funciones `(nombre-persona persona)`, `(edad-persona
persona)` y `(sexo-persona persona)` que devuelvan la información
correspondiente.

Ejemplo:

```scheme
(define p1 (crea-persona "Paula" 40 'mujer))
(nombre-persona p1) ; ⇒ "Paula"
(edad-persona p1) ; ⇒ 40
(sexo-persona p1) ; ⇒ 'mujer
```

b) Escribe las funciones `(edad-total lista-personas)` y `(edad-media
lista-personas)` que recibe una lista de personas y devuelve la edad
total y la edad media. La primera función debe ser recursiva y la
segunda puede llamar a la primera.

Ejemplo:

```scheme
(define personas (list (crea-persona "Juan" 23 'hombre)
                       (crea-persona "Ana" 18 'mujer)
                       (crea-persona "Pablo" 30 'hombre)
                       (crea-persona "Paula" 20 'mujer)
                       (crea-persona "Antonio" 25 'hombre)))
(edad-total personas) ; ⇒ 116
(edad-media persoans) ; ⇒ 116/5
```

c) Escribe las funciones recursivas `(filtra-sexo sexo personas)` y
`(mayores-edad edad personas)` que devuelvan las personas de un sexo
determinado o mayores de una edad determinada.

Escribe por último la función `(mayores-edad-sexo edad sexo personas)`
que llame a las dos funciones anteriores para obtener aquellas
personas de un sexo determinado y que sean mayores de una edad
determinada.

Ejemplo:

```scheme
(filtra-sexo 'mujer personas) ; ⇒ Lista con las personas "Ana" y "Paula"
(mayores-edad 20 personas) ; ⇒ Lista con las personas "Juan", "Pablo" y "Antonio"
(mayores-edad-sexo 25 'hombre personas) ; ⇒ Lista con la personas "Pablo"
```

----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  
