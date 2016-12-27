## Práctica 5: Funciones como datos de primera clase y procedimientos recursivos

### Entrega de la práctica

Para entregar la práctica debes subir a Moodle el fichero `practica05.rkt` con una cabecera inicial con tu nombre y apellidos, y las soluciones de cada ejercicio separadas por comentarios. Cada solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Una visualización por pantalla de uno de los ejemplos incluidos en el enunciado que **demuestre qué hace la función**, usando la función de `display`.
- Un conjunto de **pruebas** que comprueben su funcionamiento utilizando la librería `schemeunit`. Estas pruebas deben incluir los ejemplos proporcionados en los ejercicios y un mínimo de **2 casos de prueba sustancialmente distintos** a estos ejemplos.

### Ejercicios Tema 2

#### Ejercicio 1

a) Define la función `(prueba x)` de forma que las siguientes expresiones devuelvan el valor indicado:

```scheme
(define f (prueba 10))
(f 3) => 13
```

```scheme
(define g (prueba 5))
(g 3) => 8
```

b) Escribe en cada apartado una expresión de Scheme correcta en la que se invoque a la función definida y que devuelva 12 (en ambos casos). Debe ser una única expresión, sin usar `define`.

b.1)

```
(define g1 (lambda (x)
              (+ x 8)))
```

b.2)

```scheme
(define (g2 x)
 (lambda (y)
  (* x y)))
```

#### Ejercicio 2

Indica qué valor devuelve la siguiente invocación a `(f "hola")`, qué expresión se evalúa en la invocación y qué variables se utilizan para calcular ese resultado, su valor y si son globales, locales o capturadas. Intenta resolver el problema sin ejecutarlo en el intérprete de Scheme. Usa sólo Scheme para comprobar si lo has hecho correctamente.

```scheme
(define x 10)
(define y 3)
(define (prueba x)
   (let ((y (+ x 4))
         (z (+ y 3)))
      (lambda (str)
         (+ (string-length str) x y z))))
(define f (prueba 2))
(f "hola") ; ⇒ ?
```

#### Ejercicio 3

a) Implementa la función recursiva `(construye-conversores lista)` que recibe una lista de parejas con un número que representa el tipo de cambio con respecto al Euro y un símbolo que identifica el tipo de moneda, y devuelve una lista de procedimientos que implementan conversores-moneda (funciones que reciben un número (cantidad en euros) y devuelven una pareja con la cantidad correspondiente según el tipo de cambio de `moneda` y la descripción de la moneda).

Ejemplo:
```scheme
(define lista-conversores (construye-conversores (list (cons 1.11072 'Dólar-estadounidese) (cons 0.77444 'Libra-esterlina) (cons 125.774 'Yen-japonés))))

(display lista-conversores)  ; ⇒ (#<procedure> #<procedure> #<procedure>)
((cadr lista-conversores) 10)  ;⇒ {7.7444 . Libra-esterlina)
((list-ref lista-conversores 2) 10)  ; ⇒ {1257.74 . Yen-japonés}
```

**Nota:** Para realizar las pruebas, utiliza la siguiente función que permite comparar dos parejas, cuya parte izquierda representa una cantidad y su parte derecha el tipo de moneda. Para ello, además copia en esta práctica la función `iguales-reales?` definida en la práctica 1.
```
(define (iguales-monedas? x y)
  (and (iguales-reales? (car x) (car y))
       (equal? (cdr x) (cdr y))))
```

b) Implementa la función `(construye-conversores-FOS lista)` equivalente a la función anterior, pero usando funciones de orden superior.

### Ejercicios Tema 3

#### Ejercicio 4

a) Implementa la función `(suma-iter lista)` que sume los números de una lista mediante recursión por la cola (_tail recursion_).

Ejemplo:

```Scheme
(suma-iter '(1 2 3 4 5)) ; ⇒ 15
```

b) Implementa utilizando recursión por la cola la función `(cuadrado-lista-iter lista)` que toma como argumento una lista de números y devuelve una lista con sus cuadrados.

Ejemplo:

`(cuadrado-lista-iter '(2 3 4 5))  ; ⇒ (4 9 16 25)`


#### Ejercicio 5

Implementa utilizando recursión por la cola la función `(asteriscos-iter n)` ​que devuelva una lista con n listas, cada una de las cuales debe tener n asteriscos. Puedes usar funciones auxiliares, pero todas las funciones que uses deben ser implementadas mediante recursión por la cola.

Ejemplo:
```
(asteriscos-iter 5) ; ⇒ {{*} {* *} {* * *} {* * * *} {* * * * *}}
```

----

Lenguajes y Paradigmas de Programación, curso 2015-16  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  
