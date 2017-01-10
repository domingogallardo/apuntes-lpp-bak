
----

### Tema 3: Procedimientos y estructuras recursivas

Notas de clase Semana 8   
[Enlace a los apuntes](http://domingogallardo.github.io/lpp/teoria/Tema03-ProcedimientosEstructurasRecursivas.html#3)

----

### Contenidos

- 1 Procedimientos recursivos
    - 1.1 Pensando recursivamente
	- 1.2 El coste de la recursión
	- 1.3 Soluciones al coste de la recursión: procesos iterativos
	- 1.4. Soluciones al coste de la recursión: memoization
    - 1.5 Recursión y gráficos de tortuga
- 2 Listas estructuradas
    - 2.1. Definición y ejemplos
    - 2.2. Funciones recursivas sobre listas estructuradas
- **3 Árboles**
    - **3.1. Definición de árboles en Scheme**
    - **3.2. Funciones recursivas sobre árboles**

----

### 3.1. Definición de árbol

- Un **árbol** es una estructura de datos definida por un valor raíz, que es el padre de toda la estructura, del que salen otros subárboles hijos ([Wikipedia](https://en.wikipedia.org/wiki/Tree_(data_structure))).
- Un **árbol** se puede definir recursivamente de la siguiente forma:
    - Una colección de un **dato** (el valor de la raíz del árbol) y una **lista de hijos** que también son árboles.
    - Una **hoja** será un árbol sin hijos (un dato con una lista de hijos vacía).

Un ejemplo de árbol:

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/arbol-sencillo.png" width="250px"/>

- ¿Cuál es el dato de su raíz? 
- ¿Cuáles son sus hijos? 
- ¿Qué datos tienen sus hijos? 
- ¿Cuáles son los árboles hoja?

<p style="margin-bottom:4cm;"/>

El árbol anterior tiene como dato de la raíz es el símbolo `+` y tiene 3 árboles hijos:

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/arboles-hijos.png" width="300px"/>

- El primer hijo es un árbol hoja, con valor 5 y sin hijos
- El segundo hijo es un árbol con valor `*` y dos hijos hoja, el 2 y el 3
- El tercer hijo es otro árbol hoja, con valor 10

---

### Representación de árboles con listas

- En Scheme tenemos como estructura de datos principal la lista. 
- ¿Cómo construimos un árbol usando listas? Tenemos que guardar en la lista: el dato de la raíz y los hijos.

<p style="margin-bottom:3cm;"/>

- La forma de hacerlo será usar **una lista de _n+1_ elementos** para representar un árbol con n hijos:
    - el primer elemento la lista será el dato de la raíz
    - el resto serán los árboles hijos

> Árbol: '(dato hijo-1 hijo-2 ... hijo-n)

- Los nodos hoja serán por tanto listas de un elemento, el propio dato (no tiene más elementos porque no tiene hijos)

> Nodo hoja: '(dato)

¿Cómo representaríamos de esta forma el árbol anterior?

<p style="margin-bottom:4cm;"/>

```scheme
{+ {5} {* {2} {3}} {10}}
```

Los elementos de esta lista son:

- El primer elemento es el símbolo `+`, el dato valor de la raíz del árbol
- El segundo elemento es la lista `{5}`, que representa el árbol hoja formado por un 5
- El tercer elemento es la lista `{* {2} {3}}`, que representa el árbol con un dato `*` y dos hijos
- El cuarto elemento es la lista `{10}`, que representa el árbol hoja formado por un 10

Podríamos definir el árbol con la siguiente sentencia:

```scheme
(define arbol1 '(+ (5) (* (2) (3)) (10)))
```
<p style="margin-bottom:3cm;"/>

Otro ejemplo más. ¿Cómo se implementa en Scheme el árbol de la siguiente figura?

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/binario-2.png" width="300px"/>

<p style="margin-bottom:4cm;"/>

Se haría con la lista de la siguiente sentencia:

```scheme
(define arbol2 '(40 (18 (3) (23 (29))) (52 (47))))
```

----

### Barrera de abstracción

- Vamos a definir funciones básicas para trabajar con árboles
    - Funciones para obtener el dato y los hijos
    - Función para construir un árbol nuevo 
- Estas funciones proporcionarán lo que se denomina **barrera de abstracción** del tipo datos *árbol*.
- Una **barrera de abstracción** es un conjunto de funciones que permiten trabajar con un tipo de datos escondiendo su implementación. 
- Por convenio, en todas las funciones ponemos el mismo sufijo, el nombre del tipo de dato, en este caso **tree** (vamos a hacer una mezcla un poco extraña, escribiendo el nombre del tipo de dato en inglés, y el nombre de las funciones en castellano).
- Definimos dos conjuntos de funciones: **constructores** para construir un nuevo árbol y **selectores** para obtener los elementos del árbol. Vamos a empezar por los selectores.

----

### Selectores

Funciones que obtienen los elementos de un árbol:

```scheme
(define (dato-tree arbol) 
    (car arbol))

(define (hijos-tree arbol) 
    (cdr arbol))

(define (hoja-tree? arbol) 
   (null? (hijos-tree arbol)))
```

Es importante tener claro los tipos devueltos por las dos primeras funciones:

- `(dato-tree arbol)`: devuelve el dato de la raíz del árbol.
- `(hijos-tree arbol)`: devuelve una lista de árboles hijos. En algunas ocasiones llamaremos *bosque* a una lista de árboles.

---

### Ejemplo de selectores

Por ejemplo, si usamos el árbol `arbol1` 

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/arbol-sencillo.png" width="200px"/>

```scheme
(define arbol1 '(+ (5) (* (2) (3)) (10)))
```

las funciones anteriores devuelven los siguientes valores:

```scheme
(dato-tree arbol1) ; ⇒ +
(hijos-tree arbol1) ; ⇒ {{5} {* {2} {3}} {10}}
(hoja-tree? (car (hijos-tree arbol1))) ; ⇒ #t
```

---

### Tipos devueltos por las funciones
				   
Es muy importante considerar en cada caso con qué tipo de dato estamos trabajando y usar la barrera de abstracción adecuada en cada caso:

- La función `hijos-tree` siempre devuelve una lista de árboles, que podemos recorrer usando `car` y `cdr`.
- El `car` de una lista de árboles (devuelta por `hijos-tree`) siempre es un árbol y debemos de usar las funciones de su barrera de abstracción: `dato-tree` e `hijos-tree`.
- La función `dato-tree` devuelve un dato de árbol, del tipo que guardemos en el árbol.

> USO CORRECTO DE LAS BARRERAS DE ABSTRACCIÓN  
> Usaremos `car` y `cdr` cuando trabajemos con listas y `dato-tree` y `hijos-tree` cuando lo hagamos con árboles

Ejemplo: ¿cómo obtendríamos el número 2 del `arbol1`?

<p style="margin-bottom:4cm;"/>

```scheme
(dato-tree (car (hijos-tree (cadr (hijos-tree arbol1))))) ; ⇒ 2
```

----

### Constructores

Funciones que permiten construir un nuevo árbol:

```scheme
(define (make-tree dato lista-arboles)  
   (cons dato lista-arboles))

(define (make-hoja-tree dato) 
    (make-tree dato '()))
```

- La función `(make-tree dato lista-arboles)` recibe un dato y una lista de árboles y devuelve un árbol formado por el dato en su raíz y la lista de hijos.
- La función `(make-hoja-tree dato)` recibe un dato y devuelve un árbol hoja (un árbol sin hijos).

----

### Ejemplo de uso de los constructores

El árbol `arbol1` 

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/arbol-sencillo.png" width="200px"/>

se puede construir con las siguientes llamadas a los constructores:

```scheme
(make-tree '+ (list (make-hoja-tree 5)
                             (make-tree '* 
                                        (list (make-hoja-tree 2) 
                                              (make-hoja-tree 3)))
                             (make-hoja-tree 10)))
```

----

### Diferencia entre árboles y listas estructuradas

- Es importante diferenciar la barrera de abstracción de los árboles de la de las listas estructuradas.
- Aunque un árbol se implementa en Scheme con una lista estructurada, a la hora de trabajar con árboles usaremos su barrera de abstracción propia, para resaltar trabajar con el nivel de abstracción correcto y esconder la implementación.
- El siguiente esquema resumen las características de la barrera de abstracción de listas y árboles:

<img src="../tema03-procedimientos-estructuras-recursivas/imagenes/barrera-abstraccion.png" width="550px">

----

### 3.2. Funciones recursivas sobre árboles

Vamos a diseñar las siguientes funciones recursivas:

* `(suma-datos-tree tree)`: devuelve la suma de todos los nodos
* `(to-list-tree tree)`: devuelve una lista con los datos del árbol
* `(cuadrado-tree tree)`: eleva al cuadrado todos los datos de un árbol manteniendo la estructura del árbol original
* `(map-tree f tree)`: devuelve un árbol con la estructura del árbol original aplicando la función f a subdatos.
* `(altura-tree tree)`: devuelve la altura de un árbol

Todas comparten un patrón similar de recursión mutua.

----

### `(suma-datos-tree tree)`

- Vamos a implementar una función recursiva que sume todos los datos de un árbol.
- Un árbol siempre va a tener un dato y una lista de hijos (que puede ser vacía) que obtenemos con las funciones `dato-tree` e `hijos-tree`. 

Ejemplo:

```scheme
(define arbol2 '(40 (18 (3) (23 (29))) (52 (47))))
(suma-datos-tree arbol2) ; ⇒ 212
```

- Podemos plantear entonces el problema de sumar los datos de un árbol como:

> Para sumar los datos de un árbol sumamos el dato de su raíz a la suma  que devuelva la llamada a una función auxiliar que sume los datos de su lista de hijos (un bosque):

```scheme
(define (suma-datos-tree tree)
    (+ (dato-tree tree)
       (suma-datos-bosque (hijos-tree tree))))
```

- Esta función suma los datos de **UN** árbol. 
- La podemos utilizar para construir la función auxiliar que suma una lista de árboles:

> Para sumar los datos de una lista de árboles sumamos los datos del primero (con la función anterior) y añado lo que me devuelve la llamada recursiva que suma el resto de árboles:

```scheme
(define (suma-datos-bosque bosque)
   (if (null? bosque)
       0
       (+ (suma-datos-tree (car bosque)) (suma-datos-bosque (cdr bosque)))))
```

- Tenemos una recursión mutua: para sumar los datos de una lista de árboles llamamos a la suma de un árbol individual que a su vez llama a la suma de sus hijos, etc. La recursión termina cuando calculamos la suma de un árbol hoja. Entonces se pasa a `suma-datos-bosque` una lista vacía y ésta devolverá 0.



----

### Versión alternativa con funciones de orden superior

- Al igual que hacíamos con las listas estructuradas, es posible conseguir una versión más concisa y elegante utilizando funciones de orden superior:

```scheme
(define (suma-datos-tree-fos tree)
    (+ (dato-tree tree)
       (fold-right + 0 (map suma-datos-tree-fos (hijos-tree tree)))))
```	

- La función `map` aplica la propia función que estamos definiendo a cada uno de los árboles de `(hijos-tree tree)`, devolviendo una lista de números. Esta lista de número la sumamos haciendo un `fold-right + 0`. Una traza de su funcionamiento sería la siguiente:

```scheme
(suma-datos-tree-fos '(1 (2 (3) (4)) (5) (6 (7)))) ⇒
(+ 1 (fold-right + 0 (map suma-datos-tree-fos '((2 (3) (4)) 
                                                (5)
                                                (6 (7)))))) ⇒
(+ 1 (fold-right + '(9 5 13))) ⇒
(+ 1 27) ⇒
28
```

----

### `(to-list-tree tree)`

- Queremos diseñar una función `(to-list-tree tree)` que devuelva una lista con los datos del árbol en un recorrido **preorden**.
- ¿Podríamos hacerlo usando el patrón anterior?

Ejemplo:

```scheme
(to-list-tree '(* (+ (5) (* (2) (3)) (10)) (- (12)))) 
; ⇒ (* + 5 * 2 3 10 - 12)
```

<p style="margin-bottom:4cm;"/>

```scheme
(define (to-list-tree tree)
   (cons (dato-tree tree)
         (to-list-bosque (hijos-tree tree))))

(define (to-list-bosque bosque)
   (if (null? bosque)
       '()
       (append (to-list-tree (car bosque))
               (to-list-bosque (cdr bosque)))))
```


----

### Versión con `map`

Una definición alternativa usando funciones de orden superior:

```scheme
(define (to-list-tree-fos tree)
    (cons (dato-tree tree)
          (fold-right append '() (map to-list-tree-fos (hijos-tree tree)))))
```

----

### `(cuadrado-tree tree)`

- La función `(cuadrado-tree tree)` que toma un árbol de números y devuelve un árbol con la misma estructura y sus datos elevados al cuadrado.
- También usamos el patrón de recursión mutua, pero ahora para **construir** un árbol nuevo:

Ejemplo:

```scheme
(cuadrado-tree '(2 (3 (4) (5)) (6))) 
; ⇒ (4 (9 (16) (25)) (36))
```


```scheme
; Construye un árbol nuevo elevado al cuadrado el que se pasa como parámetro
(define (cuadrado-tree tree) 
   (make-tree (cuadrado (dato-tree tree))
              (cuadrado-bosque (hijos-tree tree))))  

; Devuelve una lista de árboles elevados al cuadrado
(define (cuadrado-bosque bosque) 
   (if (null? bosque)
       '()
       (cons (cuadrado-tree (car bosque))
             (cuadrado-bosque (cdr bosque)))))
```

---

### Versión con `map`:

```scheme
(define (cuadrado-tree tree)
   (make-tree (cuadrado (dato-tree tree))
   	          (map cuadrado-tree (hijos-tree tree))))
```

----

### `(map-tree f tree)`

La función `map-tree` es una función de orden superior que generaliza la función anterior. Definimos un parámetro adicional en el que se pasa la función a aplicar a los elementos del árbol.

Ejemplos:

```scheme
(map-tree cuadrado '(2 (3 (4) (5)) (6)))
; ⇒ (4 (9 (16) (25)) (36))
(map-tree (lambda (x) (+ x 1)) '(2 (3 (4) (5)) (6)))
; ⇒ (3 (4 (5) (6)) (7))
```

```scheme
(define (map-tree f tree)
   (make-tree (f (dato-tree tree))
              (map-bosque f (hijos-tree tree))))  

(define (map-bosque f bosque)
   (if (null? bosque)
       '()
       (cons (map-tree f (car bosque))
             (map-bosque f (cdr bosque)))))
```

----

### Usando  `map`

```scheme
(define (map-tree f tree)
  (make-tree (f (dato-tree tree))
             (map (lambda (x)
                    (map-tree f x)) (hijos-tree tree))))
```

----

### `(altura-tree tree)`

Vamos por último a definir una función que devuelve la altura de un árbol (el nivel del nodo de mayor nivel). Un nodo hoja tiene de altura 0.

Ejemplos:

```scheme
(altura-tree '(2)) ;  ⇒ 0
(altura-tree '(4 (9 (16) (25)) (36))) ; ⇒ 2
```

```scheme
(define (altura-tree tree)
   (if (hoja-tree? tree)
       0
       (+ 1 (max-altura-bosque (hijos-tree tree)))))  

(define (max-altura-bosque bosque)
    (if (null? bosque)
        0
        (max (altura-tree (car bosque))
             (max-altura-bosque (cdr bosque)))))
```

----

### Con funciones de orden superior

La función `max-altura-bosque` puede implementarse de una forma más concisa todavía usando las funciones con funciones de orden superior:


```scheme
(define (max-altura-bosque-fos bosque)
   (fold-right max 0 (map altura-tree bosque)))
```
	
La función `map` mapea la función `altura-tree` a todos los elementos del *bosque* (lista de árboles) devolviendo una lista de números, de la que obtenemos el máximo plegando la lista con la función `max`.

----

### Tema 4: Programación imperativa

----

### Contenidos

- **1 Historia y características de la programación imperativa**
    - **1.1. Historia de la programación imperativa**
	- **1.2. Características principales de la programación imperativa**
- **2 Programación imperativa en Scheme**
    - **2.1. Pasos de ejecución**
	- **2.2. Mutación con formas especiales `set!`**
	- **2.3. Igualdad de referencia y de valor**
- **3 Estructuras de datos mutables**
    - **3.1. Mutación de elementos**
	- **3.2. Funciones mutadoras: `append!`**
    - 3.3. Lista ordenada mutable
	- 3.4. Tabla hash mutable]
	- 3.5. Ejemplo de mutación con listas de asociación
- 4 Clausuras con mutación = estado local mutable
   
----

### 1. Historia y características de la programación imperativa

----

### Orígenes de la programación imperativa

- La programación imperativa es la forma natural de programar un computador, es el estilo de programación que se utiliza en el ensamblador, el estilo más cercano a la arquitectura del computador
- Características de la arquitectura [arquitectura clásica de Von Newmann](http://en.wikipedia.org/wiki/Von_Neumann_architecture):
	- memoria donde se almacenan los datos (referenciables por su dirección de memoria) y el programa
	- unidad de control que ejecuta las instrucciones del programa (contador del programa)
	- Los primeros lenguajes de programación (como el Fortran) son abstracciones del ensamblador y de esta arquitectura. Lenguajes más modernos como el BASIC o el C han continuado con esta idea.

----

### Programación procedural

- Uso de procedimientos y subrutinas
- Los cambios de estado se localizan en estos procedimientos
- Los procedimientos especifican parámetros y valores devueltos (un primer paso hacia la abstracción y los modelos funcionales y declarativos)
- Primer lenguaje con estas ideas: ALGOL

----

### Programación estructurada

- Artículo a finales de los 60 de Edsger W. Dijkstra: [GOTO statement considered harmful](http://www.cs.utexas.edu/users/EWD/ewd02xx/EWD215.PDF) en el que se arremete contra la sentencia GOTO de muchos lenguajes de programación de la época
- La programación estructurada mantiene la programación imperativa, pero haciendo énfasis en la necesidad de que los programas sean correctos (debe ser posible de comprobar formalmente los programas), modulares y mantenibles.
- Lenguajes: Pascal, ALGOL 68, Ada

----

### Programación Orientada a Objetos

- La POO también utiliza la programación imperativa, aunque extiende los conceptos de modularidad, mantenibilidad y estado local
- Se populariza a finales de los 70 y principios de los 80

----

### Características principales de la programación imperativa

- Idea principal de la programación imperativa: la computación se realiza cambiando el estado del programa por medio de sentencias que definen pasos de ejecución del computador
- Estado del programa modificable
- Sentencias de control que definen pasos de ejecución

Vamos a ver unos ejemplos de estas características usando el lenguaje de programación Java.

----

### Modificación de datos

- Uno de los elementos de la arquitectura de Von Newmann es la existencia de celdas de memoria referenciables y modificables

```java
int x = 0;
x = x + 1;
```

- Otro ejemplo típico de este concepto en los lenguajes de programación es el array: una estructura de datos que se almacena directamente en memoria y que puede ser accedido y modificado.
- En Java los arrays son tipeados, mutables y de tamaño fijo

```java
String[] unoDosTres = {"uno","dos","tres"};
unoDosTres[0] = unoDosTres[2];
```
	
- Un ejemplo de un método que recibe un parámetro de tipo array en Java. El parámetro se pasa por referencia:

```java
public void llenaCadenas(String[] cadenas, String cadena) {
    for (int i = 0; i < cadenas.length; i++) {
        cadenas[i] = cadena;
    }
}
```

### Almacenamiento y referencia de datos en variables

- Todos los lenguajes de programación definen variables que contienen datos
- Las variables pueden mantener valores (tipos de valor o value types) o referencias (tipos de referencia o reference types)
- En C, C++ o Java, los datos primitivos como int o char son de tipo valor y los objetos y datos compuestos son de tipo referencia
- La asignación de un valor a una variable tiene implicaciones distintas si el tipo es de valor (se copia el valor) o de referencia (se copia la referencia)

Copia de valor (datos primitivos en Java):

```java
int x = 10;
int y = x;
x = 20;
System.out.println(y); // Sigue siendo 10
```

Copia de referencia (objetos en Java):

```java
// import java.awt.geom.Point2D;
Point2D p1 = new Point2D.Double(2.0, 3.0);
Point2D p2 = p1;
p1.setLocation(12.0, 13.0);
System.out.println("p2.x = " + p2.getX()); // 12.0
System.out.println("p2.y = " + p2.getY()); // 13.0
```

- El uso de las referencias para los objetos de clases y para los tipos compuestos está generalizado en la mayoría de lenguajes de programación
- Tiene efectos laterales pero permite obtener estructuras de datos eficientes
- Veremos que en Swift las clases tienen una semántica de referencia y las estructuras de valor.

----

### Igualdad de valor y de referencia

- Todos los lenguajes de programación imperativos que permite la distinción entre valores y referencias implementan dos tipos de igualdad entre variables
- Igualdad de valor (el contenido de los datos de las variables es el mismo)
- Igualdad de referencia (las variables tienen la misma referencia)
- Igualdad de referencia => Igualdad de valor (pero al revés no)

- En Java la igualdad de referencia se define con `==` y la de valor con el método `equals`:

```java
Point2D p1 = new Point2D.Double(2.0, 3.0);
Point2D p2 = p1;
Point2D p3 = new Point2D.Double(2.0, 3.0);
System.out.println(p1==p2);           // true
System.out.println(p1==p3);           // false
System.out.println(p1.equals(p3));    // true
```

----

### Sentencias de control

- También tiene su origen en la arquitectura de Von Newmann
- Sentencia que modifica el contador de programa y determina cuál será la siguiente instrucción a ejecutar
- Tipos de sentencias de control en programación estructurada:
	- Las sentencias de secuencia definen instrucciones que son ejecutados una detrás de otra de 
	forma síncrona. Una instrucción no comienza hasta que la anterior ha terminado.
	- Las sentencias de selección definen una o más condiciones que determinan las instrucciones 
	que se deberán ejecutar.
	- Las sentencias de iteración definen instrucciones que se ejecutan de forma repetitiva hasta 
	que se cumple una determinada condición.

**Bucles y variables**

Ejemplo imperativo que imprime en Java una tabla con los productos de los números del 1 al 9:

```java
for (int i = 1; i <= 9 ; i++) {
    System.out.println("Tabla del " + i);
    System.out.println("-----------");
    for (int j = 1; j <= 9; j++) {
        System.out.println(i + " * " + j + " = " + i * j);
    }
    System.out.println();
}
```	

----

### 2. Programación imperativa en Scheme

- Al igual que LISP, Scheme tiene características imperativas
- Vamos a ver algunas de ellas
	- Pasos de ejecución
	- Asignación con la forma especial `set!`
	- Datos mutables con las formas especiales `set-car!` y `set-cdr!`
- Una nota importante: todos los ejemplos que hay a continuación necesitan importar la librería `mutable-pairs`:

```scheme
#lang r6rs
(import (rnrs)
      (rnrs mutable-pairs))
```

----

### Pasos de ejecución

- Es posible definir pasos de ejecución con la forma especial `begin`
- Todas las sentencias de la forma especial se ejecutan de forma secuencial, una tras otra
- Tanto en la forma especial `let` como en `lambda` y en la definición de funciones es posible definir cuerpos de función con múltiples sentencias que se ejecutan también de forma secuencial

Ejemplo `begin`:

```scheme
(begin
   	(display "Escribe un número: ")
	(define x (read))
    (display "Escribe otro: ")
	(define y (read))
	(define maximo (max x y))
    (display (string-append "El máximo de "
                            (number->string x)
                            " y "
                            (number->string y)
                            " es "
                            (number->string maximo))))
```


Ejemplo de pasos de ejecución en la definición de una función:

```scheme
(define (display-tres-valores a b c)
   (display a)
   (newline)
   (display b)
   (newline)
   (display c)
   (newline))
```

----

### Forma especial `set!`

- La forma especial `set!` permite asignar un nuevo valor a una variable
- La variable debe haber sido previamente creada con `define`
- La forma especial no devuelve ningún valor, modifica el valor de la variable usada

Sintaxis:

```text
(set! <variable> <nuevo-valor>)
```

Por ejemplo, la típica asignación de los lenguajes imperativos se puede realizar de esta forma en Scheme:

```scheme
(define a 10)
(set! a (+ a 1))
a ; ⇒ 11
```

Ejemplo (usando `let`):

```scheme
(define a '(1 2 3 4))
(define b '(hola adios))
(let ((aux a))
    (set! a b)
    (set! b aux)))
```

----

### Datos mutables

- En Scheme se definen las formas especiales `set-car!` y `set-cdr!` que permite modificar (mutar) la parte izquierda o derecha de una pareja una vez creada
- Al igual que `set!`, no devuelven ningún valor

Sintaxis:

```text
(set-car! <pareja> <nuevo-valor>)
(set-cdr! <pareja> <nuevo-valor>)
```

Ejemplo

```scheme
(define p (cons 1 2))
(set-car! p 10)
(set-cdr! p 20)
p ; ⇒ {10 . 20}
```

----

### Efectos laterales

- La introducción de la asignación y los datos mutables hace posible que Scheme se comporte como un lenguaje imperativo en el que más de una variable apunta a un mismo valor y se producen efectos laterales
- Un efecto lateral se produce cuando el valor de una variable cambia debido a una sentencia en la que no aparece la variable
- Podemos comprobar ahora que las parejas son datos que se copian por referencia

Ejemplo:

```scheme
(define p1 (cons 1 2))
(define p2 p1)
p1 ; ⇒ (1 . 2)
p2 ; ⇒ (1 . 2)
(set-car! p1 20)
p1 ; ⇒ (20 . 2)
p2 ; ⇒ (20 . 2)
```

----

### Igualdad de referencia y de valor

- La utilización de referencias, la mutación y los efectos laterales hace también necesario definir dos tipos de igualdades: igualdad de referencia e igualdad de valor.
- Igualdad de referencia: dos variables son iguales cuando apuntan al mismo valor
- Igualdad de valor: dos variables son iguales cuando contienen el mismo valor
- En Scheme la función `eq?` comprueba la igualdad de referencia y `equal?` la igualdad de valor
- Igualdad de referencia implica igualdad de valor, pero no al revés

Ejemplo:

```scheme
(define p1 (cons 10 20))
(define p2 p1)
(define p3 (cons 10 20))
(equal? p3 p1)  ; ⇒ #t
(eq? p3 p1) ; ⇒ #f
(eq? p2 p1) ; ⇒ #t
(set! p3 p1) ;; La asignación copia referencias
(eq? p3 p1) ; ⇒ #t
```

----

### 3. Estructuras de datos mutables

- La utilización de las formas especiales `set-car!` y `set-cdr!` permite un estilo nuevo de manejo de las estructuras de datos ya vistas (listas o árboles)
- Es posible implementar funciones más eficientes que actualizan la estructura modificando directamente las referencias de unas celdas a otras
- Las operaciones no construyen estructuras nuevas, sino que modifican la ya existente

----

### Mutación de elementos

- Un ejemplo sencillo en el que vamos a mutar un elemento de una estructura de datos formada por parejas. 
- Supongamos la siguiente estructura:

```scheme
(define datos (cons 2
                    (cons (cons 5
                                (cons 8 9))
                          (cons 3 4))))
```

¿Cuál sería el diagrama _box-and-pointer_? Fíjate que hay elementos de las parejas que son datos atómicos y otros que son referencias a otras parejas.

<p style="margin-bottom:4cm;"/>

<img src="../tema04-programacion-imperativa/imagenes/datos.png" width="250px">

- Vamos ahora a *mutar* la estructura utilizando las sentencias `set-car!` y `set-cdr!`. 
- Para mutar una pareja debemos obtener una referencia a la pareja y modificar su parte derecha o su parte izquierda con las sentencias anteriores.

Por ejemplo, ¿cómo cambiaríamos el `8` por un `18`? Deberíamos obtener la pareja `(8 . 9)` que está al final de la estructura y modificar su parte izquierda:

<p style="margin-bottom:3cm;"/>

```scheme
(set-car! (cdr (car (cdr datos))) 18)
```

- La expresión `(cdr (car (cdr datos)))` devuelve la pareja que queremos modificar y la sentencia `set-car!` modifica su parte izquierda.

--- 

### Mutación de referencias a otras parejas

- En el ejemplo anterior hemos mutado un dato por otro. 
- También podemos mutar las referencias a las parejas. Por ejemplo, la parte derecha de la pareja que contiene el 5 para que apunte a la pareja `(3 . 4)`:

<img src="../tema04-programacion-imperativa/imagenes/datos-mutados.png" width="250px">

¿Cómo lo haríamos? 

<p style="margin-bottom:3cm;"/>


Habría que obtener la pareja que contiene el 5 con la expresión `(car (cdr datos))` y mutar su parte derecha (`set-cdr!`) con la referencia a la pareja `(3 . 4)` que se obtiene con la expresión `(cdr (cdr datos))`:

```scheme
(set-cdr! (car (cdr datos)) (cdr (cdr datos)))
```

----

### Funciones mutadoras con estructuras de datos: `(append! lista1 lista2)`

- Las sentencias mutadoras permiten mayor eficiencia en la modificación de estructuras de datos
- Primer ejemplo: concatenación de listas
- Definimos una función `(append! lista1 lista2)` que concatena dos listas usando la mutación: se modifica la parte derecha de la última pareja de la primera lista sustituyendo su lista vacía por una referencia a la primera pareja de la segunda lista.

<img src="../tema04-programacion-imperativa/imagenes/append.png" width="400px">

```scheme
(define (append! l1 l2)
    (if (null? (cdr l1))
        (set-cdr! l1 l2)
        (append! (cdr l1) l2)))
```

Ejemplo:
		      
```scheme
(define a '(1 2 3 4))
(define b '(5 6 7))
(append! a b)
a ;-> (1 2 3 4 5 6 7)
```

- Al igual que `set!`, `set-car!` o `set-cdr!`, la función `append!` no devuelve ningún valor, sino que modifica directamente la lista que se pasa como primer parámetro
- Por convenio, indicaremos que una función es mutadora terminando su nombre con un signo de admiración
- Al modificarse la lista, todas las referencias que apuntan a ellas quedan también modificadas
- La función daría un error en el caso en que la llamáramos con una lista vacía como primer argumento
