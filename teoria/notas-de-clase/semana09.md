----

### Tema 4: Programación imperativa

Notas de clase Semana 9   

### Contenidos

- 1 Historia y características de la programación imperativa
    - 1.1. Historia de la programación imperativa
	- 1.2. Características principales de la programación imperativa
- 2. Programación imperativa en Scheme
    - 2.1. Pasos de ejecución
	- 2.2. Mutación con formas especiales *set!*
	- 2.3. Igualdad de referencia y de valor
- 3 Estructuras de datos mutables
    - 3.1. Mutación de elementos
	- **3.2. Funciones mutadoras: `append!`**
    - **3.3. Lista ordenada mutable**
	- **3.4. Diccionario mutable**
	- **3.5. Ejemplo de mutación con listas de asociación**
- **4 Clausuras con mutación = estado local mutable**
    - **4.1. Estado local mutable**
	- **4.2. Paso de mensajes**

----

### 3.2. Funciones mutadoras: `append!`

- Normalmente las funciones mutadoras no devuelven una estructura, sino que modifican la que se pasa como parámetro
- Por convenio, indicaremos que una función es mutadora terminando su nombre con un signo de admiración


Como ejemplo inicial, la siguiente función es la versión mutadora de `append`. 
La llamamos `append!`:

<img src="imagenes/tema05-programacion_imperativa/append.png" width="400px">


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

Algunas puntualizaciones:

- Al igual que `set!`, `set-car!` o `set-cdr!`, la función `append!` no devuelve ningún valor, sino que modifica directamente la lista que se pasa como primer parámetro
- Al modificarse la lista, todas las referencias que apuntan a ellas quedan también modificadas
- La función daría un error en el caso en que la llamáramos con una lista vacía como primer argumento


----

### 3.3. Lista ordenada mutable

Vamos a presentar un tipo de dato mutable completo, una lista ordenada en la que insertaremos elementos de forma ordenada.

La barrera de abstracción del tipo de dato `olist` es la siguiente:

- `(make-olist)`: devuelve una lista ordenada vacía
- `(primero-olist olist)`: devuelve el primer elemento de una lista ordenada
- `(resto-olist olist)`: devuelve el resto de la lista ordenada
- `(vacia-olist? olist)`: devuelve `#t` o `#f` dependiendo de si la lista tiene o no elementos
- `(borra-primero-olist! olist)`: elimina (con mutación) el primer elemento de la lista
- `(inserta-olist! olist n)`: inserta (con mutación) de forma ordenada un número en la lista

Implementaremos el tipo de datos con una lista normal con una pareja adicional en cabeza. Esta pareja adicional funcionará de **cabecera de la lista** y será la referencia inmutable a la que apuntará cualquier variable que apunte a la lista. De esta forma podremos insertar elementos en primera posición de la lista. En la cabecera usaremos por convenio el símbolo `'*olist*'`.

```scheme
(define lista '(*olist* 10 20 30))
```
<img src="imagenes/tema05-programacion_imperativa/olist.png" width="600px">

**Selectores**

La implementación de los selectores es la siguiente:


```scheme
(define (primero-olist olist)
    (cadr olist))

(define (resto-olist olist)
    (cdr olist))

(define (vacia-olist? olist)
    (null? (cdr olist)))
```

- La función `first-olist` devuelve el primer elemento de la lista ordenada. Devuelve el `cadr` para saltar la cabecera.
- La función `empty-olist?` devuelve `#t` si la lista ordenada tiene algún elemento. Para ello comprueba si hay alguna pareja tras la cabecera.
- La función `rest-olist` devuelve el resto de la lista ordenada. Devuelve la referencia a la segunda pareja de la lista original. Esta pareja hará de cabecera de la lista devuelta, conteniendo en su parte izquierda el primer elemento de la lista original (no importa, porque puede haber cualquier valor como cabecera).

Por ejemplo, puedes comprobar en el siguiente código el funcionamiento de estos selectores:

```scheme
(define lista '(*olist* 10 20))
(primero-olist lista) ; ⇒ 10
(primero-olist (resto-olist lista)) ; ⇒ 20
(vacia-olist? (resto-olist (resto-olist lista))) ; ⇒ #t
```

**Constructor**

El constructor `(make-olist)` devuelve una lista sólo con la cabecera.

```scheme
(define (make-olist)
    (list '*olist*))
```

**Mutadores**

Las funciones mutadoras modifican la estructura de datos.

Comenzamos con la función `(borra-primero-olist! olist)` que elimina con mutación el primer elemento de la lista ordenada:

<img src="imagenes/tema05-programacion_imperativa/olist2.png" width="600px">

```scheme
(define (borra-primero-olist! olist)
    (set-cdr! olist (cddr olist))
    olist)
```

Definimos la función mutadora `inserta-olist!` que modifica la lista ordenada añadiendo una nueva pareja, insertándola en la posición correcta modificando las referencias.

La función `(add-item! item ref)` es la función clave que crea una nueva pareja con el `item` y la añade en el `cdr` de la pareja a la que apunta `ref`.

```scheme
(define (add-item! ref item)
    (set-cdr! ref (cons item (cdr ref))))

(define (inserta-olist! olist n)
    (cond 
        ((null? (cdr olist)) (add-item! olist n))
        ((< n (cadr olist)) (add-item! olist n))
        ((= n (cadr olist)) #f)   ; el valor devuelto no importa
        (else (inserta-olist! (cdr olist) n))))
```

<img src="imagenes/tema05-programacion_imperativa/olist3.png" width="600px">

Ejemplo de uso:

```scheme
(define c (make-olist))
(inserta-olist! c 5)
(inserta-olist! c 8)
```

----

### 3.4. Diccionario mutable

- Veamos ahora un ejemplo más: un _diccionario_ mutable definido mediante una lista de asociación formada por parejas de clave y valor

```scheme
(define l-assoc (list (cons 'a 1) (cons 'b 2) (cons 'c 3)))
```
	
- La función de Scheme `assq` recorre la lista de asociación y devuelve la tupla que contiene el dato que se pasa como parámetro como clave

```scheme
(assq 'a l-assoc) ; ⇒ (a.1)
(assq 'b l-assoc) ; ⇒ (b.2)
(assq 'c l-assoc) ; ⇒ (c.3)
(assq 'd l-assoc) ; ⇒  #f
(cdr (assq 'c l-assoc)) ; ⇒ 3
```

- La función `assq` busca en la lista de asociación usando la igualdad de referencia `eq?` 

```scheme
(define p (cons 1 2))
(define l-assoc (list (cons 'a 1) (cons p 2) (cons 'c 3)))
(assq (cons 1 2) l-assoc) ; ⇒ #f
(assq p l-assoc) ; ⇒ {{1 . 2} . 2}
```

La barrera de abstracción del mapa es la siguiente:

- `(make-dic)`: construye un diccionario vacío
- `(put-dic! dic clave valor)`: inserta en el diccionario un nuevo valor asociado a una clave
- `(get-dic dic clave)`: devuelve el valor asociado a una clave en un diccionario

```scheme
(define (make-dic)
  (list '*dic*))

(define (get-dic dic clave)
  (let ((pareja (assq clave (cdr dic))))
    (if (not pareja)
        #f
        (cdr pareja))))

(define (put-dic! dic clave valor)
  (let ((pareja (assq clave (cdr dic))))
    (if (not pareja)
        (set-cdr! dic
                  (cons (cons clave valor)
                        (cdr dic)))
        (set-cdr! pareja valor)))
  'ok)
```

Ejemplos de uso:
  
```scheme
(define dic (make-dic))
(put-dic! dic 'a 10) ; ⇒ ok
(get-dic dic 'a) ; ⇒ 10
(put-dic! dic 'b '(a b c)) ; ⇒ ok
(get-dic dic 'b) ; ⇒ {a b c}
(put-dic! dic 'a 'ardilla) ; ⇒ ok
(get-dic dic 'a) ; ⇒ ardilla
```

----

### 3.5. Ejemplo de mutación con listas de asociación

Una vez introducidos distintas estructuras de datos mutables, incluyendo listas de asociación, vamos a terminar estos ejemplos con un ejemplo práctico en el que intervienen las listas y las listas de asociación. Se trata de escribir un procedimiento `regular->assoc!` que transforme una lista regular en una lista de asociación sin crear nuevas parejas. La lista regular `(k1 v1 k2 v2 k3 v3 ...)` deberá convertirse en la lista de asociación `((k1 . v1) (k2 . v2) (k3 . v3) ...)`. 

Ejemplo:

```scheme
(define my-list (list 'a 1 'b 2 'c 3))
my-list ; ⇒ (a 1 b 2 c 3)
(regular->assoc! my-list)
my-list ; ⇒ ((a . 1) (b . 2) (c . 3))
```

Una posible solución a este problema sería la siguiente (no es la única solución):

Cuando trabajamos con este tipo de problemas, es muy útil ayudarse con los diagramas caja y puntero. Vamos a crear los diagramas caja y puntero para el ejemplo anterior:

Antes de llamar a regular->assoc!:

![](./imagenes/tema05-programacion_imperativa/mutable1.png)

Después de llamar a regular->assoc!:

![](./imagenes/tema05-programacion_imperativa/mutable2.png)

Por un lado, no podemos crear nuevas parejas, por lo que cada pareja en el primer diagrama corresponde a una pareja particular en el segundo diagrama. Por otro lado, no podemos modificar la primera pareja de la lista (porque perderíamos la ligadura de la variable `my-list`), por lo que es primera pareja tiene que permanecer en el primer lugar. Por otra parte, `a` y `1` van a formar parte del mismo par en la lista de asociación, por lo que vamos a considerar el siguiente cambio:

Antes de llamar a `regular->assoc!`:

![](./imagenes/tema05-programacion_imperativa/mutable3.png)

Después de llamar a `regular->assoc!`:

![](./imagenes/tema05-programacion_imperativa/mutable4.png)

Vamos a nombrar el par mostrado en rojo como `p`:

![](./imagenes/tema05-programacion_imperativa/mutable5.png)

```scheme
(define (manejar-dos-parejas! p)
	(let ((key (car p)))                   ;; 1
		(set-car! p (cdr p))               ;; 2
		(set-cdr! p (cdr (car p)))         ;; 3
		(set-cdr! (car p) (car (car p)))   ;; 4
		(set-car! (car p) key)))           ;; 5
```

Se han numerado las líneas para una mejor explicación. En la línea 1, `(let ((key (car p)))`, utilizamos un `let` para guardar el valor actual del `(car p)`, ese valor será la clave de la primera pareja de la lista de asociación; necesitamos almacenarlo para no perderlo.

En la línea 2, `(set-car! p (cdr p))`, cambiamos el `car` de `p` para que apunte a la siguiente pareja (la azul):

![](./imagenes/tema05-programacion_imperativa/mutable6.png)

En la línea 3, `(set-cdr! p (cdr (car p)))`, cambiamos el `cdr` de `p` para que apunte al `cdr` de la pareja en azul:

![](./imagenes/tema05-programacion_imperativa/mutable7.png)

En la línea 4, `(set-cdr! (car p) (car (car p)))`, cambiamos el `cdr` de la pareja en azul al `car` de la misma pareja. Hacemos ésto porque en una lista de asociación, los valores se guardan en los  `cdrs` y las claves en los `cars`:

![](./imagenes/tema05-programacion_imperativa/mutable8.png)

Por último, en la línea 5 `(set-car! (car p) key)))`, completamos el problema poniendo la clave que habíamos guardado, en el `car` de la pareja azul:

![](./imagenes/tema05-programacion_imperativa/mutable9.png)

Reordenamos el diagrama para verlo más claro:

![](./imagenes/tema05-programacion_imperativa/mutable10.png)

Hemos definido un procedimiento que maneja un subproblema (dos parejas) del problema. Ahora sólo nos queda definir la función que maneja toda la lista:

```scheme
(define (regular->assoc l)
  (if (null? l) 
     'ok
     (begin (manejar-dos-parejas! l)
            (regular->assoc! (cdr l)))))
```

----

### 4. Clausuras con mutación = estado local mutable

----

### 4.1. Estado local mutable

Si combinamos una clausura con la posibilidad de mutar las variables capturadas por la clausura obtenemos un estado local mutable asociado a funciones creadas en tiempo de ejecución. 

Veamos, por ejemplo, la siguiente función `(make-contador i)`:

```scheme
(define (make-contador i)
  (let ((x i))
    (lambda ()
      (set! x (+ x 1))
      x)))
```

La función `make-contador` define una clausura que captura la variable local `x` inicializada a `i`. En cada invocación a la clausura se ejecuta una sentencia `set` que modifica el valor de la variable capturada. Y después se devuelve el nuevo valor de la variable. Estamos implementando un contador asociado a la clausura.


```scheme
(define f (make-contador 10))
(f) ⇒ 11
(f) ⇒ 12
```

El estado (el valor del contador) se mantiene y se modifica entre distintas invocaciones a `f`. A diferencia del paradigma funcional podemos comprobar que distintas invocaciones a la misma función devuelven valores distintos (dependiente del estado de la clausura). 

Podemos crear distintas clausuras, cada una con su variable capturada, en distintas invocaciones a `make-contador`:

```scheme
(define h (make-contador 10))
(define g (make-contador 100))
(h) ⇒ 11
(h) ⇒ 12
(g) ⇒ 101
(g) ⇒ 102
```

Una forma alternativa de crear la clausura, sin usar la forma especial `lambda` es definiéndola con un `define` en el cuerpo de `make-contador`. Creamos también la variable local con un `define` en lugar de con un `let` para remarcar que estamos usando un paradigma imperativo. Después de los dos `define` la última sentencia devuelve la función `incrementa` (la clausura).

```scheme
(define (make-contador i)
  (define x i)
  (define (incrementa)
    (set! x (+ x 1))
    x)
  incrementa)
```

----

### 4.2. Paso de mensajes

En el ejemplo anterior creamos una clausura que siempre incrementa el valor del contador. ¿Cómo podríamos crear una clausura que permitiera hacer distintas cosas con el valor capturado? 

Una forma de hacerlo es definir en la clausura un parámetro adicional (un símbolo que llamamos _mensaje_) y devolver distintas clausuras en función del valor de ese parámetro adicional:

```scheme
(define (make-contador i)
  (define x i)
  (define (get)
    x)
  (define (inc-1)
    (set! x (+ x 1))
    x)
  (define (inc y)
    (set! x (+ x y))
    x)

  (define (dispatcher mensaje)
    (cond
      ((equal? mensaje 'get) get)
      ((equal? mensaje 'inc-1) inc-1)
      ((equal? mensaje 'inc) inc)
      (else "Error: mensaje desconocido")))
  dispatcher)
```

La función `make-contador` devuelve la clausura `dispatcher` que se encarga de procesar el mensaje y devolver la clausura asociada a ese mensaje. Cuando invoquemos al _dispatcher_ con un símbolo, devuelve otro procedimiento que hay que volver a invocar con los parámetros adecuados (sin parámetros en los dos primeros casos y con el valor a incrementar en el último caso):

- Si recibe el símbolo `'get` devuelve la clausura que devuelve el valor del contador
- Si recibe el símbolo `'inc-1` devuelve la clausura que incrementa el valor del contador en 1
- Si recibe el símbolo `'inc` devuelve la clausura que incrementa el valor del contador una cantidad determinada

Por ejemplo:

```scheme
(define c (make-contador 100))
((c 'get)) ⇒ 100
((c 'inc-1)) ⇒ 101
((c 'inc) 10) ⇒ 111
```

