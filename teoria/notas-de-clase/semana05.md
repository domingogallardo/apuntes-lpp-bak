
----

### Tema 2: Programación funcional (3/3) bis

Notas de clase Semana 5   
[Enlace a los apuntes](http://domingogallardo.github.io/lpp/teoria/Tema02-ProgramacionFuncional.html#6)

----


### Contenidos

- 1 El paradigma de Programación Funcional
- 2 Scheme como lenguaje de programación funcional
- 3 Tipos de datos compuestos en Scheme
- 4 Listas en Scheme
- 5 Funciones como tipos de datos de primera clase
- 6 **Ambito de variables, clausuras y `let`**
    - 6.1 **Ámbitos de variables**
    - 6.2 **Forma especial `let`**
    - 6.3 **Clausuras**

----

### 6.1. Ámbitos de variables

- El concepto del **ámbito** de vida de las variables es un concepto fundamental en los lenguajes de programación. 
- En inglés se utiliza el término *[scope](https://en.wikipedia.org/wiki/Scope_(computer_science))*. 

> Cuando se define una variable, asociándole un valor, esta asociación tiene una extensión determinada, ya sea en términos de tiempo de compilación (**ámbito léxico**) como en términos de tiempo de ejecución (**ámbito dinámico**). El ámbito de una variable determina cuándo podemos referirnos a ella para recuperar el valor asociado.

>Al conjunto de variables disponibles en una parte del programa o en una parte de su ejecución se denomina **contexto** o **entorno** (*context* o *environment*).

----

### Variables de ámbito global

- Una variable definida en el programa con la instrucción `define` tiene un ámbito global. 

```
(define a "hola")
(define b (string-append "adios" a))
(define cuadrado (lambda (x) (* x x)))
```

- Todas las variables definidas fuera de funciones forman parte del **entorno global** del programa.


----

### Variables de ámbito local

- Se crea un **entorno local** cuando se invoca una función.
- Consideramos los parámetros de la invocación como variables de ámbito local que toman el valor de la invocación.
- Podemos usar en un entorno local una variable con el mismo nombre que en el entorno global. Cuando se ejecute el código de la función se evaluará la variable de ámbito local.

Ejemplo:

```scheme
(define x 5)
(define (suma-3 x)
   (+ x 3))

(suma-3 12)
⇒ ?
```

<p style="margin-bottom:2cm;"/>


- Una vez realizada la invocación, desparece el entorno local y las variables locales definidas en él, recuperándose el contexto global. 

```scheme
(define x 5)
(define (suma-3 x)
   (+ x 3))

(+ (suma-3 12) x)
⇒ ?
```

<p style="margin-bottom:2cm;"/>

----

### Uso de variables de ámbito global en un entorno local

- En el entorno local también se pueden utilizar variables definidas en el entorno global. 

```
(define y 12)
(define (suma-3-bis x)
   (+ x y 3))

(suma-3-bis 5) 
⇒ ?
```

<p style="margin-bottom:2cm;"/>

----

### 6.2 Forma especial `let`

- Permite crear un entorno local en el que se da valor a variables y se evalúa una expresión.

Sintaxis:

```scheme
(let ((<var1> <exp-1>)
        ...
      (<varn> <exp-n>))
    <cuerpo>)
```

Las variables `var1`, … `varn` toman los valores devueltos por las expresiones `exp1`, … `expn` y el cuerpo se evalúa con esos valores.


```scheme
(define x 10)
(define y 20)
(let ((x 1)
      (y 2)
      (z 3))
    (+ x y z))
⇒ 
```

<p style="margin-bottom:2cm;"/>

----

### El ámbito de las variables definidas en el `let` es local

- Las variables definidas en el `let` sólo tienen valores en el entorno creado por la forma especial.

```scheme
(define x 10)
(define y 20)
(let ((x 1)
      (y 2)
      (z 3))
    (+ x y z))
⇒ ?
```

<p style="margin-bottom:2cm;"/>


- Cuando ha terminado la evaluación del `let` el ámbito local desaparece y quedan los valores definidos en el ámbito global. Por ejemplo, si preguntamos por los valores de las siguientes variables obtendríamos los siguientes resultados:

```scheme
x ⇒ 5
z ⇒ error, no definida
```

----

##### `Let` permite usar variables definidas en un ámbito en el que se ejecuta el `let`

- Ejemplo:

```scheme
(define z 8)
(let ((x 1)
      (y 2))
   (+ x y z))
⇒ ?
```

<p style="margin-bottom:2cm;"/>

----

##### Variables en las definiciones del `let`

- Las expresiones que dan valor a las variables del `let` se evalúan todas en el entorno en el que se ejecuta el `let`, antes de crear las variables locales.

```scheme
(define x 1)
(let ((w (+ x 3))
      (z (+ w 2))) ;; Error: w no está definida
    (+ w z))
```

----

##### Semántica del `let`

Para evaluar una expresión `let` debemos seguir las siguientes reglas:

1. Evaluar todas las expresiones de la derecha de las variables y guardar sus valores en variables auxiliares locales.
2. Definir un ámbito local en el que se ligan las variables del let con los valores de las variables auxiliares.
3. Evaluar el cuerpo del let en el ámbito local

----

##### Let se define utilizando lambda

La semántica anterior queda clara cuando comprobamos que let se puede definir en función de lambda. En general, la expresión:

```scheme
(let ((<var1> <exp1>) ... (<varn> <expn>)) <cuerpo>)
```

se puede implementar con la siguiente llamada a lambda:

```scheme
((lambda (<var1> ... <varn>) <cuerpo>) <exp1> ... <expn>)
```

Por ejemplo:

```
(let ((x (+ 2 3))
      (y (+ x 3)))
    (+ x y))
```

Equivale a:

```scheme
((lambda (x y) (+ x y)) (+ 2 3) (+ x 3))
```

----

### Let dentro de funciones

- Podemos usar `let` en el cuerpo de funciones para crear nuevas variables locales, además de los parámetros de la función:

```scheme
(define (suma-cuadrados x y)
    (let ((cuadrado-x (cuadrado x))
          (cuadrado-y (cuadrado y)))
        (+ cuadrado-x cuadrado-y)))
(suma-cuadrados 4 10)
⇒ 116
```

- El uso de `let` permite aumentar la legibilidad de los programas, dando nombre a expresiones:

```scheme
(define (distancia x1 y1 x2 y2)
   (let ((distancia-x (- x2 x1))
         (distancia-y (- y2 y1)))
      (sqrt (+ (cuadrado distancia-x)
               (cuadrado distancia-y)))))
```

Otro ejemplo:

```scheme
(define (intersecta-intervalo a1 a2 b1 b2)
    (let ((dentro-b1 (and (>= b1 a1)
                          (<= b1 a2)))
          (dentro-b2 (and (>= b2 a1)
                          (<= b2 a2))))
        (or dentro-b1 dentro-b2)))
```

----

### 6.3. Clausuras

- Una clausura es una función devuelta por otra función. 
- La clausura **captura las variables locales** de la función principal y puede usarlas en su propio código cuando este se invoque posteriormente.

- Ejemplo:

```
(define (make-sumador k)
    (lambda (x) (+ x k)))

(define f (make-sumador 10))
(define g (make-sumador 5)
(f 2) ⇒ ?
(g 2) ⇒ ?
```

<p style="margin-bottom:2cm;"/>

- En la invocación anterior `(f 2)`, cuando se ejecuta la expresión `(+ x k)` las variables tienen los siguientes valores:

```
x: 2 (variable local de la clausura)
k: 10 (valor capturado del entorno local en el que se creó la clausura)
```

----

### Las variables locales creadas en un `let` también se capturan en las clausuras

Veamos el siguiente ejemplo, en el que creamos una función en un entorno local creado por un `let`:

```scheme
(define x 10)
(define y 12)
(define (prueba x)
    (let ((y 3))
        (lambda (z) 
            (+ x y z))))
(define h (prueba 5))
(h 2)
⇒ ?
```

<p style="margin-bottom:2cm;"/>

- Con la invocación `(h 2)` se invoca a la clausura, lo que crea un entorno local en el que se encuentran las siguientes variables:

    ```
    z: 2 (parámetro de la clausura)
    x: 5 (variable local de la función prueba capturada en el momento de creación de la clausura))
    y: 3 (variable local del let capturada en el momento de creación de la clausura)
    ```

- En este contexto se ejecuta la expresión `(+ x y z)`, que devuelve el valor 10.

<p style="margin-bottom:2cm;"/>

----

**-- HASTA AQUÍ EL CONTENIDO DEL PRIMER PARCIAL --**

----

### Tema 3: Procedimientos y estructuras recursivas

----

### Contenidos

- **1 Procedimientos recursivos**
    - **1.1 Pensando recursivamente**
	- **1.2 El coste de la recursión**
	- **1.3 Soluciones al coste de la recursión: procesos iterativos**
	- **1.4 Soluciones al coste de la recursión: memoization**
    - 1.5 Recursión y gráficos de tortuga
- 2 Listas estructuradas
- 3 Árboles

----

### 1. Procedimientos recursivos

- La formulación recursiva pura de una función es natural y muy fácil de entender (una vez aprendida)
- A veces la función resultante no es eficiente
- Veremos dos posibles soluciones: procesos iterativos y *memoization*
- La semana que viene veremos algoritmos recursivos para dibujar curvas fractales

----

### 1.1. Pensando recursivamente

- Hemos diseñado muchas funciones recursivas sobre listas.
- Ya hemos usado estos consejos:
    - Para diseñar procedimientos recursivos no vale intentarlo resolver por prueba y error. Hay que diseñar la solución recursiva desde el principio. 
    - Debemos fijarnos en *lo que devuelve la función*. Supondremos que la llamada recursiva funciona correctamente y devuelve el resultado correcto. Y después debemos transformar este resultado correcto en el resultado de la solución completa.
    - Debemos **confiar en que la llamada recursiva va a hacer su trabajo y devolver el resultado correcto**, sin preocuparte de cómo lo va a hacer. 

----

### Otro ejemplo: `(palindroma? lista)`

- ¿cómo definimos una lista palíndroma de forma recursiva?. Por ejemplo, las siguientes listas son palíndromas:

```
{1 2 3 3 2 1}
{1 2 1}
{1}
'()
```

- Comenzamos con una definición **no recursiva**:

> Una lista es palíndroma cuando es igual a su inversa. 

- Esta definición no es recursiva porque no llamamos a la recursión con un caso más sencillo.

- La definición **recursiva** del caso general es la siguiente:

> Una lista es palíndroma cuando su primer elemento es igual que el último y la lista resultante de quitar el primer y el último elemento también es palíndroma

En Scheme:

```scheme
(define (palindromo? lista)
    (or (null? lista)
        (null? (cdr lista))
        (and (equal? (car lista) (ultimo lista))
             (palindromo? (quitar-primero-ultimo lista)))))
```

La función auxiliar `quitar-primero-ultimo` la podemos definir así:

```scheme
(define (quitar-ultimo lista)
    (if (null? (cdr lista))
        '()
        (cons (car lista)
              (quitar-ultimo (cdr lista)))))

(define (quitar-primero-ultimo lista)
    (cdr (quitar-ultimo lista)))
```

----

### 1.2. El coste de la recursión

- Vamos a analizar el coste de la recursión de una forma empírica, sin realizar una análisis matemático riguroso.
- Veremos que el coste se dispara cuando tenemos dos llamadas recursivas.
- Veremos formas de resolver este coste.

----

### 1.2.1. La pila de la recursión

- Para analizar una función vamos por primera vez a *entrar en la recursión* y hacer una traza de las llamadas recursivas del siguiente:

```scheme
(define (mi-length items)
    (if (null? items)
        0
        (+ 1 (mi-length (cdr items)))))
```

- ¿Cómo podemos representar las llamadas y el resultado de `(mi-length '(a b c d))`

<p style="margin-bottom:2cm;"/>

```
(mi-length '(a b c d))
(+ 1 (mi-length '(b c d)))
(+ 1 (+ 1 (mi-length '(c d))))
(+ 1 (+ 1 (+ 1 (mi-length '(d)))))
(+ 1 (+ 1 (+ 1 (+ 1 (mi-length '())))))
(+ 1 (+ 1 (+ 1 (+ 1 0))))
(+ 1 (+ 1 (+ 1 1)))
(+ 1 (+ 1 2))
(+ 1 3)
4
```

- Cada llamada a la recursión **deja una función en espera de ser evaluada cuando la recursión devuelva un valor** (en el caso anterior el +). Esta función, junto con sus argumentos, se almacenan en la **pila de la recursión**.
- Cuando la recursión devuelve un valor, los valores se recuperan de la pila, se realiza la llamada y se devuelve el valor a la anterior llamada en espera. 
- Si la recursión está mal hecha y nunca termina se genera un *stack overflow* porque la memoria que se almacena en la pila sobrepasa la memoria reservada para el intérprete DrRacket.

----

#### Coste espacial de la recursión

- El coste espacial de un programa es una función que relaciona la memoria consumida por una llamada para resolver un problema con alguna variable que determina el tamaño del problema a resolver.

- En el caso de la función `mi-length` el tamaño del problema viene dado por la longitud de la lista. El coste espacial de `mi-lenght` es *O(n)*, siendo *n* la longitud de la lista.

----

##### 1.2.3. El coste depende del número de llamadas a la recursión

- Veamos con un ejemplo que el coste de las llamadas recursivas puede dispararse. Supongamos la famosa [secuencia de Fibonacci]: 0,1,1,2,3,5,8,13,...

[secuencia de Fibonacci]:http://en.wikipedia.org/wiki/Fibonacci_number

- Formulación matemática de la secuencia de Fibonacci:

```
>Fibonacci(n) = Fibonacci(n-1) + Fibonacci(n-2)  
>Fibonacci(0) = 0  
>Fibonacci(1) = 1
```

- Formulación recursiva en Scheme:

```
(define (fib n)
    (cond ((= n 0) 0)
        ((= n 1) 1)
        (else (+ (fib (- n 1))
                 (fib (- n 2))))))
```

- Evaluación de una llamada a Fibonacci:

![Llamada recursiva a Fibonacci][fibonacci]

[fibonacci]:./imagenes/tema04-procedimientos_estructuras_recursivas/fibonacci.png

- Cada llamada a la recursión produce otras dos llamadas, por lo que el número de llamadas finales es 2^n siendo n el número que se pasa a la función.

- El coste espacial y temporal es exponencial, O(2^n). 

- ¿Qué pasa si intentamos evaluar `(fibonaci 35)`?

----

#### <a name="1-3"></a> 1.3. Soluciones al coste de la recursión: procesos iterativos

- Diferenciamos entre procedimientos y procesos: un procedimiento es un algoritmo y un proceso es la ejecución de ese algoritmo.
- Es posible definir procedimientos recursivos que generen procesos iterativos en los que no se dejen llamadas recursivas en espera ni se incremente la pila de la recursión. 
- **Solución**: Construimos la recursión de forma que en cada llamada se haga un cálculo parcial y en el caso base se pueda devolver directamente el resultado obtenido.

- Este estilo de recursión se denomina *recursión por la cola* ([tail recursion](http://en.wikipedia.org/wiki/Tail_call), en inglés).

----

##### 1.3.1. Factorial iterativo

- Definimos la función `(fact-iter-aux product n)` que es la que define el proceso iterativo
- Tiene un parámetro adicional (`product`) que es el parámetro en el que se irán guardando los cálculos intermedios
- Al final de la recursión el factorial debe estar calculado en `product` y se devuelve

```
(define (factorial-iter n)
    (fact-iter-aux n n))

(define (fact-iter-aux product n)
    (if (= n 1)
        product
        (fact-iter-aux (* product (- n 1))  (- n 1))))
```

Secuencia de llamadas:

```
(factorial-iter 4)
(factorial-iter-aux 4 4)
(factorial-iter-aux 12 3)
(factorial-iter-aux 24 2)
(factorial-iter-aux 24 1)
24
```

----

### Versión iterativa de mi-length

¿Cómo sería la versión iterativa de mi-length?

<p style="margin-bottom:2cm;"/>

Solución:

```scheme
(define (mi-length-iter lista)
    (mi-length-iter-aux lista 0))

(define (mi-length-iter-aux lista result)
    (if (null? lista)
        result
        (mi-length-iter-aux (cdr lista) (+ result 1))))
```

----

### Procesos iterativos

- La recursión resultante es menos elegante
- Se necesita una parámetro adicional en el que se van acumulando los resultados parciales
- La última llamada a la recursión devuelve el valor acumulado
- El proceso resultante de la recursión es iterativo en el sentido de que no deja llamadas en espera ni incurre en coste espacial

---

### Fibonacci iterativo

- Cualquier programa recursivo se puede transformar en otro que genera un proceso iterativo.
- En general, las versiones iterativas son menos intuitivas y más difíciles de entender y depurar.
- Ejemplo: Fibonacci iterativo

```scheme
(define (fib-iter n)
    (fib-iter-aux 1 0 n))

(define (fib-iter-aux a b count)
    (if (= count 0)
        b
        (fib-iter-aux (+ a b) a (- count 1))))
```

----

### Triángulo de Pascal

```
1
1   1
1   2   1
1   3   3   1
1   4   6   4   1
1   5  10   10  5   1
1   6  15  20   15  6   1
1   7  21  35   35  21  7   1
             ...
```

-- Formulación matemática:

```
> Pascal (0,0) = Pascal (1,0) = Pascal (1,1) = 1  
> Pascal (fila, columna) = Pascal (fila-1,columna-1) + Pascal (fila-1, columna)
```

La versión recursiva pura:

```scheme
(define (pascal row col)
    (cond ((= col 0) 1)
          ((= col row) 1)
          (else (+ (pascal (- row 1) (- col 1))
                   (pascal (- row 1) col) ))))
(pascal 4 2)
⇒ ?
(pascal 8 4)
⇒ ?
(pascal 27 13)
⇒ ?
```

----

### Triángulo de Pascal versión iterativa

- Utilizamos enfoque iterativo: cada fila se genera a partir de la anterior y 

```scheme
(define (pascal-iter fila col)
    (list-ref (pascal-iter-aux '(1 1) fila) col))

(define (pascal-iter-aux fila n)
    (if (= n (length fila))
        fila
        (pascal-iter-aux (pascal-sig-fila fila) n)))

(define (pascal-sig-fila fila)
    (append '(1)
	        (pascal-sig-fila-central fila)
            '(1)))

(define (pascal-sig-fila-central fila)
    (if (= 1 (length fila))
        '()
        (append (list (+ (car fila) (car (cdr fila))))
	                  (pascal-sig-fila-central (cdr fila)))))
```

----

#### 1.4. Soluciones al coste de la recursión: memoization

- Una alternativa que mantiene la elegancia de los procesos recursivos y la eficiencia de los iterativos es la  [memoization](http://en.wikipedia.org/wiki/Memoization). Si miramos la traza de `(fibonacci 4)` podemos ver que el coste está producido por la repetición de llamadas; por ejemplo `(fibonacci 3)` se evalúa 2 veces.

- En programación funcional la llamada a `(fibonacci 3)` siempre va a devolver el mismo valor.

- Podemos guardar el valor devuelto por la primera llamada en alguna estructura (una lista de asociación, por ejemplo) y no volver a realizar la llamada a la recursión las siguientes veces.

----

### 1.4.1. Fibonacci con memoization

- Usamos los métodos procedurales `put` y `get` que implementan un diccionario *clave-valor* (para probarlos hay que importar la librería de Scheme que permite mutar las parejas):

```scheme
(import (rnrs)
        (rnrs mutable-pairs))

(define lista (list '*table*))

(define (get key lista)
    (let ((record (assq key (cdr lista))))
        (if (not record)
            '()
            (cdr record))))

(define (put key value lista)
    (let ((record (assq key (cdr lista))))
        (if (not record)
	        (set-cdr! lista
	            (cons (cons key value)
                     (cdr lista)))
	        (set-cdr! record value)))
	'ok)
```

- La función `(put key value lista)` asocia un valor a una clave y la guarda en la lista (con mutación).

- La función `(get key lista)` devuelve el valor de la lista asociado a una clave.

Ejemplos:

```
(define mi-lista (list '*table*))
(put 1 10 mi-lista)
(get 1 mi-lista) ; ⇒ 10
(get 2 mi-lista) ; ⇒ '()
```

- La función `fib-memo` realiza el cálculo de la serie de Fibonacci utilizando el proceso recursivo visto anteriormente y la técnica de memoización, en la que se consulta el valor de Fibonacci de la lista antes de realizar la llamada recursiva:

```scheme
(define (fib-memo n lista)
    (cond ((= n 0) 0)
          ((= n 1) 1)
          ((not (null? (get n lista)))
           (get n lista))
          (else (let ((result 
                       (+ (fib-memo (- n 1) lista)
                          (fib-memo (- n 2) lista))))
                  (begin
                    (put n result lista)
                    result)))))
```

- Podemos comprobar la diferencia de tiempos de ejecución entre esta versión y la anterior. El coste de la función *memoizada* es O(n). Frente al coste O(2^n) de la versión inicial que la hacía imposible de utilizar.

```scheme
(define lista (list '*table*))
(fib-memo 200 lista)
⇒ 280571172992510140037611932413038677189525
```

