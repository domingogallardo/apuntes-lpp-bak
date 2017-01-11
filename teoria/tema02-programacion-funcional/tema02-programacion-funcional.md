
## Tema 2: Programación funcional 

### Contenidos

- [1. El paradigma de Programación Funcional](#1)
    - [1.1 Historia y características del paradigma funcional](#1-1)
	- [1.2. Programación declarativa](#1-2)
	- [1.3. Modelo de computación de sustitución](#1-3)
- [2. Scheme como lenguaje de programación funcional](#2)
	- [2.1. Funciones y formas especiales](#2-1)
	- [2.2. Formas especiales en Scheme: `define`, `if`, `cond`](#2-2)
	- [2.3. Forma especial `quote` y símbolos](#2-3)
	- [2.4. Listas](#2-4)
	- [2.5. Recursión](#2-5)
- [3. Tipos de datos compuestos en Scheme](#3)
	- [3.1. El tipo de dato pareja](#3-1)
	- [3.2. Las parejas son objetos de primera clase](#3-2)
	- [3.3.  Diagramas *caja-y-puntero*](#3-3)
- [4. Listas en Scheme](#4)
    - [4.1. Implementación de listas en Scheme](#4-1)
    - [4.2. Listas con elementos compuestos](#4-2)
    - [4.3. Funciones recursivas sobre listas](#4-3)
    - [4.4. Funciones con número variable de argumentos](#4-4)
- [5. Funciones como tipos de datos de primera clase](#5)
    - [5.1. Un primer ejemplo](#5-1)
    - [5.2. Forma especial `lambda`](#5-2)
    - [5.3. Funciones como argumentos de otras funciones](#5-3)
    - [5.4. Funciones en estructuras de datos](#5-4)
    - [5.5. Generalización](#5-5)
    - [5.6. Funciones de orden superior](#5-6)
- [6. Ámbito de variables, `let` y clausuras](#6)
    - [6.1. Ámbitos de variables](#6-1)
    - [6.2 Forma especial `let`](#6-2)
    - [6.3. Clausuras](#6-3)

### Bibliografía - SICP

En este tema explicamos conceptos de los siguientes capítulos del
libro *Structure and Intepretation of Computer Programs*:

- [1.1.1 - Expressions](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html#%_sec_1.1.1)
- [1.1.2 - Naming and environment](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html#%_sec_1.1.2)
- [1.1.3 - Evaluating combinations](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html#%_sec_1.1.3)
- [1.1.4 - Compound procedures](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html#%_sec_1.1.4)
- [1.1.5 - The Substitution Model for Procedure Application](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html#%_sec_1.1.5)
- [1.1.6 - Conditional Expressions and Predicates](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-10.html#%_sec_1.1.6)
- [1.3 - Formulating Abstractions with Higher-Order Procedures](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-12.html#%_sec_1.3)
- [2.2 - Hierarchical Data and the Closure Property](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-15.html#%_sec_2.2) (Introducción de la sección)
- [2.2.1 - Representing Sequences](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-15.html#%_sec_2.2.1)
- [2.3.1 - Quotation](https://mitpress.mit.edu/sicp/full-text/book/book-Z-H-16.html#%_sec_2.3.1)

### <a name="1"></a> 1. El paradigma de Programación Funcional

#### <a name="1-1"></a> 1.1. Historia y características del paradigma funcional

En una definición muy breve y concisa la programación funcional define
un programa como una función matemática que convierte unas entradas en
unas salidas, sin ningún estado interno y ningún efecto lateral.

La no existencia de estado interno (variables las que se guardan y se
modifican valores) y la ausencia de efectos laterales es una
característica también de la **programación declarativa**. En este
sentido, la programación funcional es un tipo concreto de programación
declarativa.

Otras características adicionales del paradigma funcional son las
siguientes:

* Uso profuso de la recursión en la definición de las funciones
* Funciones como tipos de datos primitivos
* Listas como estructuras de datos fundamentales

##### 1.1.1 Orígenes históricos

En los años 30, junto con la máquina de Turing, se propusieron
distintos modelos computacionales que formalizaban el concepto de
*algoritmo*. Uno de estos modelos fue el denominado
[*Cálculo lambda*](https://en.wikipedia.org/wiki/Lambda_calculus)
propuesto por Alonzo Church en los años 30 y basado en la evaluación
de expresiones matemáticas. En este formalismo se expresan los
algoritmos mediante funciones matemáticas en las que puede ser usada
la recursión. Una función matemática recibe parámetros de entrada y
devuelve un valor. La evaluación de la función se realiza evaluando
sus expresiones matemáticas mediante la sustitución de los parámetros
formales por los valores reales que se utilizan en la invocación.

Turing demostró que este modelo matemático era equivalente al su
máquina, demostrando que es posible construir una maquina equivalente
a cualquier función lambda existente, una máquina que devuelva
exactamente los mismos resultados que la función lambda. El hecho de
que ambos modelos computacionales sean equivalentes significa que
cualquier cualquier algoritmo que podamos definir con una máquina de
Turing, puede ser definido también con una función del cálculo
lambda. Y viceversa: cualquier función definida con el cálculo lambda
puede ser implementada por alguna máquina de Turing.

El cálculo lambda es un formalismo matemático, basado en operaciones
abstractas. Dos décadas después, cuando los primeros computadores
electrónicos estaban empezando a utilizarse en grandes empresas y en
universidades, este formalismo dio origen a algo mucho más tangible y
práctico: un lenguaje de alto nivel, mucho más expresivo que el
ensamblador, con el que expresar operaciones y funciones **evaluadas
en el computador**.

##### 1.1.2 Historia y características del Lisp

* [Lisp](http://en.wikipedia.org/wiki/Lisp_(programming_language)) es
  el primer lenguaje de programación de alto nivel basado en el
  paradigma funcional.
* Creado en 1958 por John McCarthy.
* Lisp es un lenguaje revolucionario e introduce nuevos conceptos de
  programación no existentes en la época en la que nace: funciones
  como objetos primitivos, funciones de orden superior, polimorfismo,
  listas, recursión, símbolos, homogeneidad de datos y programas,
  bucle REPL (*Read-Eval-Print Loop*)
* La herencia del Lisp llega a lenguajes derivados de él (Scheme,
  Golden Common Lisp) y a nuevos lenguajes de paradigmas no
  estrictamente funcionales, como C#, Python, Ruby, Objective-C o
  Scala.

Lisp fue el primer lenguaje de programación interpretado, con muchas
características dinámicas que se ejecutan en tiempo de ejecución
(*run-time*). Entre estas características podemos destacar la gestión
de la memoria (creación y destrucción automática de memoria reservada
para datos), la detección de excepciones y errores en tiempo de
ejecución o la creación en tiempo de ejecución de funciones anónimas
(expresiones *lambda*). Todas estas características se ejecutan
mediante un *sistema de tiempo de ejecución* (*rutime system*)
presente en el tiempo de ejecución de los programas. A partir del Lisp
muchos otros lenguajes han usado estas características de
interpretación o de sistemas de tiempo de ejecución. Por ejemplo,
lenguajes como BASIC, Python, Ruby o JavaScript son lenguajes
interpretados. Y lenguajes como Java o C# tienen una avanzada
plataforma de tiempo de ejecución con soporte para la gestión de la
memoria dinámica (*recolección de basura*,
[*garbage collection*](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)))
o la
[compilación *just in time*](https://en.wikipedia.org/wiki/Just-in-time_compilation).

Lisp es mucho más que un lenguaje funcional. Lisp se diseñó con el
objetivo de ser un lenguaje de alto nivel capaz de resolver problemas
prácticos de Inteligencia Artificial, no con la idea de ser un
lenguaje formal basado un único modelo de computación. Por ello en
Lisp (y en Scheme) existen primitivas que se salen del paradigma
funcional puro y permiten programar de formar imperativa (no
declarativa), usando mutación de estado y pasos de ejecución. Veremos
estas primitivas más adelante, en el tema en el que estudiemos el
paradigma de programación imperativa. Estudiaremos allí qué efecto
tienen estas primitivas en los programas y funciones que
escribamos. Durante la primera parte de la asignatura no utilizaremos
las instrucciones imperativas de Scheme, sino que escribiremos siempre
código funcional.

Con el paso de los años y el avance en los diseños de compiladores e
intérpretes ha sido posible diseñar lenguajes de programación que
siguen más estrictamente las características declarativas del
paradigma funcional y que también son útiles y prácticos para
desarrollar programas en el mundo real, como Haskell o Clojure.

##### 1.1.3. Aplicaciones prácticas de la programación funcional

En los años 60 la programación funcional (Lisp) fue dominante en
departamentos de investigación en Inteligencia Artificial (MIT por
ejemplo). En los años 70, 80 y 90 se fue relegando cada vez más a los
nichos académicos y de investigación; en la empresa se impusieron los
lenguajes imperativos y orientados a objetos.

En la primera década del 2000 han aparecido lenguajes
*multi-paradigma* (muchos de ellos interpretados) como Ruby, Python,
Groovy, Objective-C, Lua, Scala o Swift que incluyen el paradigma
funcional. También han aparecido y se han hecho populares lenguajes
exclusivamente funcionales como Haskell, Clojure o Erlang.

El auge reciente de estos lenguajes y del paradigma funcional se debe
a varios factores, entre ellos que es un paradigma que facilita:

- la programación de sistemas concurrentes, con múltiples hilos de
  ejecución o con múltiples computadores ejecutando procesos
  conectados concurrentes.
- la definición y composición de múltiples operaciones sobre *streams*
  de forma muy concisa y compacta, aplicable a la programación de
  sistemas distribuidos en Internet.
- la programación interactiva y evolutiva.

###### Programación de sistemas concurrentes

Veremos más adelante que una de las características principales de la
programación funcional es que no se usa la *mutación* (no se modifican
los valores asignados a variables ni parámetros). Esta propiedad lo
hace un paradigma excelente para implementar programas concurrentes,
en los que existen múltiples hilos de ejecución. La programación de
sistemas concurrentes es muy complicada con el paradigma imperativo
tradicional, en el que la modificación del estado de una variable
compartida por más de un hilo puede provocar *condiciones de carrera*
y errores difícilmente localizables y reproducibles.

Como dice [Bartosz Milewski](https://twitter.com/BartoszMilewski),
investigador y teórico de ciencia de computación, en su
[respuesta en Quora](https://www.quora.com/Why-do-software-engineers-like-functional-programming/answer/Bartosz-Milewski)
a la pregunta *¿por qué a los ingenieros de software les gusta la
programación funcional?*:

> Porque es la única forma práctica de escribir programas
> concurrentes. Intentar escribir programas concurrentes en lenguajes
> imperativos, no sólo es difícil, sino que lleva a *bugs* que son muy
> difíciles de descubrir, reproducir y arreglar. En los lenguajes
> imperativos y, en particular, en los lenguajes orientados a objetos
> se ocultan las mutaciones y se comparten datos sin darse cuenta, por
> lo que son extremadamente propensos a los errores de concurrencia
> producidos por las condiciones de carrera.

###### Definición y composición de operaciones sobre streams

El paradigma funcional ha originado un estilo de programación sobre
*streams* de datos, en el que se concatenan operaciones como `filter`
o `map` para definir de forma sencilla procesos y transformaciones
asíncronas aplicables a los elementos del *stream*. Este estilo de
programación ha hecho posible nuevas ideas de programación, como la
programación *reactiva*, basada en eventos, o los *futuros* o
*promesas* muy utilizados en lenguajes muy populares como JavaScript
para realizar peticiones asíncronas a servicios web.

Por ejemplo, en el artículo
[Exploring the virtues of microservices with Play and Akka](http://zeroturnaround.com/rebellabs/exploring-the-virtues-of-microservices-with-play-and-akka/)
se explica con detalle las ventajas del uso de lenguajes y primitivas
para trabajar con sistemas asíncronos basados en eventos en servicios
como Tumblr o Netflix.

Otro ejemplo es el
[uso de Scala en Tumblr](http://highscalability.com/blog/2012/2/13/tumblr-architecture-15-billion-page-views-a-month-and-harder.html)
con el que se consigue crear código que no tiene estado compartido y
que es fácilmente paralelizable entre los más de 800 servidores
necesarios para atender picos de más de 40.000 peticiones por segundo:

> "Scala promueve que no haya estado compartido. El estado mutable se
> evita usando sentencias en Scala. No se usan máquinas de estado de
> larga duración. El estado se saca de la base de datos, se usa, y se
> escribe de nuevo en la base de datos. La ventaja principal es que
> los desarrolladores no tienen que preocuparse sobre hilos o
> bloqueos.

###### Programación evolutiva

En la metodología de programación denominada *programación evolutiva*
o *iterativa* los programas complejos se construyen a base de ir
definiendo y probando elementos computacionales cada vez más
complicados. Los lenguajes de programación funcional encajan
perfectamente en esta forma de construir programas.

Como Abelson y Sussman comentan en el SICP:

> En general, los objetos computacionales pueden tener estructuras muy
> complejas, y sería extremadamente inconveniente tener que recordar y
> repetir sus detalles cada vez que queremos usarlas. En lugar de
> ello, se construyen programas complejos componiendo, paso a paso,
> objetos computacionales de creciente complejidad.

> El intérprete hace esta construcción paso-a-paso de los programas
> particularmente conveniente porque las asociaciones nombre-objeto se
> pueden crear de forma incremental en interacciones sucesivas. Esta
> característica favorece el desarrollo y prueba incremental de
> programas, y es en gran medida responsable del hecho de que un
> programa Lisp consiste normalmente de un gran número de
> procedimientos relativamente simples.

No hay que confundir una metodología de programación con un paradigma
de programación. Una metodología de programación proporciona
sugerencias sobre cómo debemos diseñar, desarrollar y mantener una
aplicación que va a ser usada por usuarios finales.

#### <a name="1-2"></a> 1.2. Programación declarativa

Hablamos de *programación declarativa* para referirnos a lenguajes de
programación (o sentencias de código) en los que se *declaran* los
valores, objetivos o características finales de los elementos del
programa, pero no se especifican detalles de implementación, ni de
control de flujo. Estos elementos se resuelven por la componente de
run-time del lenguaje. Por ejemplo, un conjunto de reglas de Prolog
son sentencias declarativas. O una definición de una interfaz en Java.

Una característica fundamental del código declarativo es que no
utiliza pasos de ejecución, ni asignación destructiva (en la que se
modifica o muta el valor de la variable). Define un conjunto de reglas
y definiciones *de estilo matemático*. En la programación funcional se
cumplen estas características, porque se definen funciones que
devuelven un valor a partir de unos parámetros de entrada, sin
modificar ningún estado del programa ni utilizar pasos de ejecución
definidos como tales.

El siguiente ejemplo es una **declaración** en Scheme de una función
que toma como entrada un número y devuelve su cuadrado:

```scheme
(define (cuadrado x)
   (* x x))
```

La programación declarativa no es exclusiva de los lenguajes
funcionales. Existen muchos lenguajes no funcionales con
características declarativas. Por ejemplo Prolog, en el que un
programa se define como un conjunto de reglas lógicas y su ejecución
realiza una deducción lógica matemática que devuelve un resultado. En
dicha ejecución no son relevantes los pasos internos que realiza el
sistema sino las relaciones lógicas entre los datos y los resultados
finales.


##### 1.2.1. Programación imperativa

Repasemos un par de características propias de la programación
imperativa:

- Pasos de ejecución con asignación
- Estado local mutable en las funciones

###### Asignación y pasos de ejecución

El estilo de programación imperativa se basa en pasos de ejecución que
modifican el estado de variables:

```java
int x = 10;
int x = x + 1;
```

La expresión `x = x + 1` es una expresión de
[asignación](https://en.wikipedia.org/w/index.php?title=Assignment_(computer_science)&redirect=no)
que modifica el valore anterior de una variable por un nuevo valor. El
*estado* de las variables (su valor) cambia con la ejecución de los
pasos del programa.

En programación imperativa una variable guarda una referencia a una
posición de memoria (o estructura de datos) que puede ser modificada
posteriormente mediante una nueva asignación o una modificación
(mutación) de sus atributos.

Por ejemplo, en C podemos asignar y modificar un carácter de una
cadena

```c
char *miNombre = "Alejandro Perez"
miNombre[3] = 'a'
```

O en Java podemos modificar los atributos de un objeto:

```java
Point2D p1 = new Point2D(3.0, 2.0); // la coord x de p1 es 3.0
p1.setCoordX(10.0);
p1.getCoordX(); // la coord x de p1 es 10.0
```

Si el objeto está asignado a más de una variable tendremos un **efecto
lateral**
(*[side effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science))*
en el que el dato guardado en una variable cambia después de una
sentencia en la que no se ha usado esa variable:

```java
Point2D p1 = new Point2D(3.0, 2.0); // la coord x de p1 es 3.0
Point2D p2 = p1;
p1.setCoordX(10.0);
p2.getCoordX(); // la coord x de p2 es 10.0, sin que ninguna sentencia haya modificado p2
```

En programación funcional, por contra, las definiciones son
inmutables, una vez asignado un valor a un identificador no se puede
modificar éste. En programación funcional se entienden las variables
como variables matemáticas, no como referencias a una posiciones de
memoria que puede ser modificada. Los valores son inmutables y no
existen efectos laterales.

Por ejemplo, la forma especial `define` en Scheme crea un nuevo
identificador y le da el valor definido de forma permanente. Si
escribimos el siguiente código en un programa en Scheme R6RS:

```scheme
#lang r6rs
(import (rnrs base))

(define a 12)
(define a 200)
```

tendremos el siguiente error:

```
module: duplicate definition for identifier in: a
```

*Nota*: en el intérprete REPL del DrRacket sí que podemos definir más
 de una vez la misma función o identificador. Se ha diseñado así para
 facilitar el uso del intérprete para la prueba de expresiones en
 Scheme.


En los lenguajes de programación es habitual mezclar sentencias
imperativas y sentencias declarativas. Por ejemplo, en el siguiente
código Java las líneas 1 y 3 las podríamos considerar declarativas y
las 2 y 4 imperativas:

```
1. int x = 1;
2. x = x+1;
3. int y = x+1;
4. y = x;
```

###### Estado local

Otra característica de la programación imperativa es lo que se
denomina **estado local mutable** en funciones, procedimientos o
métodos. Se trata la posibilidad de que una invocación a un método o
una función modifique un cierto estado y de forma que la siguiente
invocación devuelva un valor distinto. Es una característica básica de
la programación a objetos, donde los objetos guardan valores que se
modifican con la invocaciones a sus métodos.

Por ejemplo, en Java, podemos definir un contador que incrementa su
valor. Cada llamada al método `valor()` devolverá un valor distinto:

```java
public class Contador {
   	int c;
    
    public Contador(int valorInicial) {
        c = valorInicial;
    }
    
    public int valor() {
        c++;
        return c;
    }
}

Contador cont = new Contador(10);
cont.valor(); // 11
cont.valor(); // 12
cont.valor(); // 13
```

También se pueden definir funciones con estado local mutable en C:

```c
int function contador () {
    static int c = 0;
	
	c++;
	return c;
}
	
contador() ;; 1
contador() ;; 2
contador() ;; 3
```	

Por el contrario, los lenguajes funcionales puros tienen la propiedad
de *transparencia referencial*: si se sustituye una expresión por su
valor el resultado final no debe cambiar. Como consecuencia, en
programación funcional, una función siempre devuelve el mismo valor
cuando se le llama con los mismos parámetros. Las funciones no
modifican ningún estado, no acceden a ninguna variable ni objeto
global y modifican su valor.

###### Resumen

Un resumen de las características fundamentales de la programación
declarativa frente a la programación imperativa. En los siguientes
apartados explicaremos más estas características.

**Características de la programación declarativa**

* Variable = nombre dado a un valor (declaración)
* No existe asignación ni cambio de estado
* No existe mutación, se cumple la *transferencia referencial*: dentro
  de un mismo ámbito todas las ocurrencias de una variable y las
  llamadas a funciones devuelven el mismo valor

**Características de la programación imperativa**

* Variable = nombre de una zona de memoria
* Asignación
* Referencias
* Pasos de ejecución

#### <a name="1-3"></a> 1.3. Modelo de computación de sustitución

Un modelo computacional es un formalismo (conjunto de reglas) que
definen el funcionamiento de un programa. En el caso de los lenguajes
funcionales basados en la evaluación de expresiones, el modelo
computacional define cuál va a ser el resultado de evaluar una
determinada expresión.

El **modelo de sustitución** es un modelo muy sencillo que permite
definir la semántica de la evaluación de expresiones en lenguajes
funcionales como Scheme. Se basa en una versión simplificada de la
regla de reducción del cálculo lambda.

Es un modelo basado en la reescritura de unos términos por
otros. Aunque se trata de un modelo abstracto, es posible escribir un
programa que simule el comportamiento del modelo, con el que sea
posible implementar un intérprete de estas expresiones.

Supongamos un conjunto de definiciones en Scheme:

```scheme
(define (doble x) 
    (+ x x))
(define (cuadrado y) 
    (* y y))
(define (f z) 
    (+ (cuadrado (doble z)) 1))
```

Supongamos que, una vez realizadas esas definiciones, se evalúa la
siguiente expresión:

```
(f (+ 2 1))
```

¿Cuál será su resultado? Si lo hacemos de forma intuitiva podemos
pensar que `37`. Si lo comprobamos en el intérprete de Scheme veremos
que devuelve 37. ¿Hemos seguido algunas reglas específicas? ¿Qué
reglas son las que sigue el intérprete? ¿Podríamos implementar
nosotros un intérprete similar? Sí, usando las reglas del modelo de
sustitución.

El modelo de sustitución define cuatro reglas sencillas para evaluar
una expresión. Llamemos a la expresión *e*:

1. Si *e* es un valor primitivo (por ejemplo, un número), devolver ese
   mismo valor.
2. Si *e* es un identificador, devolver su valor asociado con un
   `define` (se lanzará un error si no existe ese valor).
3. Si *e* es una expresión del tipo *(f arg1 ... argn)*, donde *f* es
   el nombre de una función primitiva (`+`, `-`, ...), evaluar uno a
   uno los argumentos *arg1 ... *argn* y llamar a la función con el
   resultado.
4. Si *e* es una expresión del tipo *(f arg1 ... argn)*, donde *f* es
   el nombre de una función definida con un `define`, sustituir *f*
   por su cuerpo, reemplazando cada parámetro formal del procedimiento
   por el correspondiente argumento de la llamada. Tenemos en este
   punto dos versiones del modelo (ambas son equivalentes): podemos
   evaluar el argumento y después hacer la sustitución o podemos hacer
   la sustitución con la expresión sin evaluar. En el primer caso
   estaremos siguiendo un orden normal y en el segundo un orden
   aplicativo. En cualquier caso, evaluar la expresión resultante.

Scheme utiliza el orden aplicativo.

##### 1.3.1 Orden normal vs. orden aplicativo

En el orden aplicativo se realizan las evaluaciones de *dentro a
fuera* de los paréntesis. Cuando se llega a una expresión primitiva se
evalúa. En el orden normal se realizan todas las sustituciones hasta
que se tiene una larga expresión formada por expresiones primitivas;
se evalúa entonces.

Veamos un ejemplo de cada caso.

Supongamos las siguientes definiciones de funciones:

```scheme
(define (doble x) 
    (+ x x))
(define (cuadrado y) 
    (* y y))
(define (f z) 
    (+ (cuadrado (doble z)) 1))
```

¿Cuál sería el resultado de evaluar `(f (+ 2 1))` con orden
aplicativo?. Vamos a verlo paso a paso, poniendo entre paréntesis la
regla de las anteriores que se aplica en cada caso:

```
(f (+ 2 1)) ->                ; evaluamos (+ 2 1) por ser + una función primitiva (Regla 3)
(f 3) ->                      ; sustituimos (f 3) por su definición (Regla 4)
(+ (cuadrado (doble 3)) 1) -> ; sustituimos (doble 3) (Regla 4)
(+ (cuadrado (+ 3 3)) 1) ->   ; evaluamos (+ 3 3) (Regla 3)
(+ (cuadrado 6) 1) ->         ; sustituimos (cuadrado 6) (Regla 4)
(+ (* 6 6) 1) ->              ; evaluamos (* 6 6) (Regla 3)
(+ 36 1) ->                   ; evaluamos (+ 36 1) (Regla 3)
37
```

¿Y en orden normal?

```
(f (+ 2 1)) ->                      ; sustituimos (f (+ 2 1)) 
                                    ; por su definición, con z = (+ 2 1) (Regla 4)
(+ (cuadrado (doble (+ 2 1))) 1) -> ; sustituimos (doble (+ 2 1)) (Regla 4)
(+ (cuadrado (+ (+ 2 1) 
              (+ 2 1))) 1) ->       ; sustituimos (cuadrado ...) (Regla 4)
(+ (* (+ (+ 2 1) 
         (+ 2 1))
      (+ (+ 2 1) 
         (+ 2 1))) 1) ->            ; evaluamos (+ 2 1) (Regla 3)
(+ (* (+ 3 3)
      (+ 3 3)) 1) ->                ; evaluamos (+ 3 3) (Regla 3)
(+ (* 6 6) 1) ->                    ; evaluamos (* 6 6) (Regla 3)
(+ 36 1) ->                         ; evaluamos (+ 36 1) (Regla 3)
37
```

En programación funcional el resultado de evaluar una expresión es el
mismo independientemente del tipo de orden. Pero si estamos fuera del
paradigma funcional y las funciones tienen estado y cambian de valor
entre distintas invocaciones sí que importan si escogemos un orden.

Por ejemplo, supongamos una función `(random x)` que devuelve un
entero aleatorio entre 0 y *x*. Esta función no cumpliría el paradigma
funcional, porque devuelve un valor distinto con el mismo parámetro de
entrada.

Evaluamos las siguientes expresiones con orden aplicativo y normal,
para comprobar que el resultado es distinto

```scheme
(define (zero x) (- x x))
(zero (random 10))
```

### <a name="2"></a> 2. Scheme como lenguaje de programación funcional

Vamos ver ejemplos concretos de características de un lenguaje de
programación funcional estudiando las características funcionales de
Scheme.

En concreto, veremos:

- Definición de funciones y otras primitivas de programación funcional
  en Scheme
- Símbolos y primitiva `quote`
- Definición de funciones recursivas en Scheme

#### <a name=2-1></a>2.1 Funciones y formas especiales

En el seminario de Scheme hemos visto un conjunto de primitivas que
podemos utilizar en Scheme.

Podemos clasificar las primitivas en **funciones** y **formas
especiales**. Las funciones se evalúan usando el modelo de sustitución
aplicativo ya visto:

- Primero se evalúan los argumentos y después se sustituye la llamada
  a la función por su cuerpo y se vuelve a evaluar la expresión
  resultante.
- Las expresiones siempre se evalúan desde los paréntesis interiores a
  los exteriores.

Las *formas especiales* son expresiones primitivas de Scheme que
tienen una forma de evaluarse propia, distinta de las funciones.

#### <a name="2-2"></a>2.2. Formas especiales en Scheme: define, if, cond

Veamos la forma de evaluar las distintas formas especiales en
Scheme. En estas formas especiales no se aplica el modelo de
sustitución, al no ser invocaciones de funciones, sino que cada una se
evalúa de una forma diferente.

##### 2.2.1 Forma especial `define`

**Sintaxis**

```scheme
(define <identificador> <expresión>)
```

**Evaluación**

1. Evaluar *<expresión>*
2. Asociar el valor resultante con el *<identificador>*

**Ejemplo**

```scheme
(define base 10)
(define altura 12)
(define area (/ (* base altura) 2))
```

###### Forma especial `define` para definir funciones

**Sintaxis**

```
(define (<nombre-funcion> <argumentos>)
	<cuerpo>)
```

**Evaluación**

La semana que viene veremos con más detalle la semántica, y
explicaremos la forma especial `lambda` que es la que realmente crea
la función. Hoy nos quedamos en la siguiente descripción de alto nivel
de la semántica:

1. Crear la función con el *cuerpo*
2. Dar a la función el nombre *nombre-función*

**Ejemplo**

```scheme
(define (factorial x)
    (if (= x 0)
        1
		(* x (factorial (- x 1)))))
```

##### 2.2.2. Forma especial `if`

**Sintaxis**

```scheme
(if <condición> <expresión-true> <expresión-false>)
```

**Evaluación**

1. Evaluar *<condición>*
2. Si el resultado es `#t` evaluar la *<expresión-true>*, en otro
   caso, evaluar la *<expresión-false>*

**Ejemplo**

```scheme
(if (> 10 5) (substring "Hola qué tal" (+ 1 1) 4) (/ 12 0))
```

##### 2.2.3. Forma especial `cond`

**Sintaxis**

```scheme
(cond 
	(<exp-cond-1> <exp-consec-1>)
	(<exp-cond-2> <exp-consec-2>)
	...
	(else <exp-consec-else>))
```

**Evaluación**

1. Se evalúan de forma ordenada todas las expresiones hasta que una de
   ellas devuelva `#t`
2. Si alguna expresión devuelve `#t`, se devuelve el valor del
   consecuente de esa expresión
3. Si ninguna expresión es cierta, se devuelve el valor resultante de
   evaluar el consecuente del `else`


**Ejemplo**

```scheme
(cond
   ((> 3 4) '3-es-mayor-que-4)
   ((< 2 1) '2-es-menor-que-1)
   ((= 3 1) '3-es-igual-que-1)
   ((> 3 5) '3-es-mayor-que-2)
   (else 'ninguna-condicion-es-cierta))
```

#### <a name="2-3"></a>2.3. Forma especial `quote` y símbolos

**Sintaxis**

```scheme
(quote <identificador>)
(quote <expresion>)
```

**Evaluación**

- Se devuelve el identificador o la expresión **sin evaluar**. La
  expresión puede ser cualquier expresión correcta de Scheme, datos
  atómicos, parejas o listas. Se abrevia en con el carácter `'`

**Ejemplo**

```scheme
(quote x)
'hola
'(+ 1 2 3 4)
(quote (1 2 3 4))
'(* (+ 1 (+ 2 3)) 5)
```

A diferencia de los lenguajes imperativos, Scheme trata a los
*identificadores* (nombres que se les da a las variables) como datos
del lenguaje de tipo **symbol**. En el paradigma funcional a los
identificadores se les denomina *símbolos*.

Los símbolos son distintos de las cadenas. Una cadena es un tipo de
dato compuesto, mientras que los símbolos se almacenan con un valor
único denominado *valor hash*.

Ejemplos de funciones Scheme con símbolos:

```scheme
(define x 12)
(symbol? 'x) ; ⇒ #t
(symbol? x) ; ⇒ #f ¿Por qué?
(symbol? 'hola-que<>)
(symbol⇒string 'hola-que<>)
'mañana
'lápiz ; aunque sea posible, no vamos a usar acentos en los símbolos
; pero sí en los comentarios
(symbol? "hola") ; #f
(symbol?  #f) ; #f
(symbol? (car '(hola cómo estás))) ; #t
(equal? 'hola 'hola)
(equal? 'hola "hola")
```

Un símbolo es un identificador que puede asociarse o ligarse (*bind*)
a un valor (cualquier dato *de primera clase*).

Cuando escribimos un símbolo en el prompt de Scheme el intérprete lo
evalúa y devuelve su valor:

```scheme
(define pi 3.14159)
pi
⇒3.14159
```

Los nombres de las funciones (`equal?, `sin, `+, ...) son también
símbolos (los de las macros no) y Scheme también los evalúa (en un par
de semanas hablaremos de las funciones como objetos primitivos en
Scheme):

```scheme
sin
⇒ #<procedure:sin>
+
⇒ #<procedure:+>
(define (cuadrado x) (* x x))
⇒ #<procedure:cuadrado>
```

##### 2.2.4. Símbolos como tipos primitivos

Los símbolos son tipos primitivos del lenguaje: pueden pasarse como
parámetros o ligarse a variables.

```scheme
(define x 'hola)
x
⇒ hola
```

#### <a name="2-4"></a>2.4. Listas

Otra de las características fundamentales del paradigma funcional es
la utilización de listas. Repasamos las características y funciones
más importantes de Scheme para trabajar con listas.

* Creación de listas: función `list` y forma especial `quote`

	```scheme
	(define a 1)
	(define b 2)
	(define c 3)
	(list a b c)) ⇒ {1 2 3}
	'(a b c) ⇒ {a b c}
	```

* Lista vacía: `()`

* Lista con otras listas como elemento:

        (define lista1 '(1 (2 3) (4 5 (6))))
 
     La `lista1` tiene 3 elementos:
     
     * 1
     * La lista `{2 3}`
     * La lista `{4 5 {6}}`

* Selección de elementos de una lista:
    * Primer elemento: función `car`
    * Resto de elementos: función `cdr`

	```scheme
	(define lista2 '((1 2) 3 4))
	(car lista2) ;⇒ {1 2}
	(cdr lista2) ;⇒ {3 4}
	```

* Creación de nuevas listas:

	* Función `cons` para crear una lista nueva resultado de añadir un
      elemento al comienzo de la lista. Esta función es la forma
      habitual de construir nuevas listas a partir de una lista ya
      existente y un nuevo elemento.
   
	   ```scheme
	   (cons 1 '(1 2 3 4)) ;⇒ {1 1 2 3 4}
	   (cons 'hola '(como estás)) ;⇒ {hola como estás}
	   (cons '(1 2) '(1 2 3 4)) ; ⇒ {{1 2} 1 2 3 4}
	   ```

	* Función `append` para crear una lista nueva resultado de
      concatenar dos o más listas
   
	   ```scheme
	   (append list2 list2 list3)
	   ```

* Diferencia entre creación de listas con la función `list` y con la
  forma especial `quote`:

    * La evaluación de la función `list` funciona como cualquier
      función, primero se evalúan los argumentos y después se invoca a
      la función con los argumentos evaluados. Por ejemplo, en la
      siguiente invocación se obtiene una lista con cuatro elementos
      resultantes de las invocaciones de las funciones dentro del
      paréntesis:

    ```scheme
    (list 1 (/ 2 3) (+ 2 3) (cons 3 4)) ; ⇒ {1 2/3 5 {3 . 4}}
    ```

    * Sin embargo, usamos `quote` obtenemos una lista con sublistas
      con símbolos en sus primeras posiciones:

    ```scheme
    '(1 (/ 2 3) (+ 2 3) (cons 3 4)) ; ⇒ {1 {/ 2 3} {+ 2 3} {cons 3 4}}
    ```

#### <a name="2-5"></a> 2.5. Recursión

Otra característica fundamental de la programación funcional es la no
existencia de bucles. Un bucle implica la utilización de pasos de
ejecución en el programa y esto es característico de la programación
imperativa. Las iteraciones se realizan con recursión.

Para entender correctamente la recursión hay que mirarla *de forma
declarativa*, como una definición matemática, entendiendo lo que hace
la llamada recursiva y *confiando en que devuelve lo que tiene que
devolver*. No es conveniente *entrar en la recursión* e intentar
comprobar su funcionamiento haciendo una traza de las sucesivas
llamadas, sino suponer que la llamada recursiva se ejecuta, devuelve
el valor que debería (*confiamos en la recursión*) y calcular el valor
devuelto como el resultado de la operación una vez devuelta la llamada
recursiva.

##### 2.5.2. Función `(suma-hasta x)`

Vamos a empezar con un ejemplo sencillo, la función `(suma-hasta x)`
que devuelve la suma de los números hasta `x`.

Por ejemplo, `(suma-hasta 5)` devolverá `0+1+2+3+4+5 = 15`. 

Vamos a empezar por diseñar el caso general de la recursión. ¿Cómo
podemos hacer este cálculo **de forma recursiva**? ¿Podemos expresar
`(suma-hasta 5)` como **lo que devuelve una llamada recursiva a un
problema más pequeño** y algo más? Fijaros que sí:

Para calcular la suma hasta 5, llamamos a la recursión para que
calcule la suma hasta 4 y al resultado que devuelve la llamada le sumo
el propio número 5. Lo podemos expresar con el siguiente dibujo:

<img src="imagenes/suma-hasta.png" width="600px"/>

Generalizamos este ejemplo y lo expresamos en Scheme de la siguiente
forma:

```
(suma-hasta x) => (+ (suma-hasta (- x 1)) x)
```

Nos falta el caso base de la recursión. Debemos preguntarnos **¿cuál
es el caso más sencillo del problema, que podemos calcular sin hacer
ninguna llamada recursiva?**. En este caso podría ser el caso en el
que `x` es 0, en el que devolveríamos 0.

Podemos ya escribirlo todo en Scheme:

```scheme
(define (suma-hasta x)
   (if (= 0 x)
      0
      (+ (suma-hasta (- x 1)) x)))
```

##### 2.5.4. Función `(suma-lista lista-nums)`

Veamos ahora un ejemplo con listas. Supongamos que queremos definir
una función `suma-lista` que reciba como parámetro una lista de
números y devuelva la suma de todos ellos.

Siempre tenemos que empezar escribiendo un ejemplo de la función:

```
(suma-lista '(12 3 5 1 8)) = 29
```

Para diseñar una implementación recursiva de la función tenemos que
pensar en cómo descomponer el ejemplo en una llamada recursiva a un
problema más pequeño y en cómo tratar el valor devuelto por la
recursión para obtener el valor esperado.

Por ejemplo, en este caso podemos pensar que para sumar la lista de
números `(12 3 5 1 8)` podemos obtener un problema más sencillo (una
lista más pequeña) haciendo el `cdr` de la lista de números y llamando
a la recursión con el resultado. La llamada recursiva devolverá la
suma de esos números (confiamos en la recursión) y a ese valor basta
con sumarle el primer número de la lista. Lo podemos representar en el
siguiente dibujo:

<img src="imagenes/suma-lista.png" width="600px"/>

Podemos generalizar este ejemplo y expresarlo en Scheme de la siguiente forma:

```
(suma-lista lista) => (+ (car lista) (suma-lista (cdr lista)))
```

Falta el caso base, que es el caso más sencillo en que podemos
devolver un valor sin llamar a la recursión. En este caso, podría ser
cuando le pesamos a la función una lista sin elementos, en donde hay
que devolver 0.

Con todo junto, quedaría la recursión como sigue

```
(define (suma-lista lista)
   (if (null? lista)
       0
	   (+ (car lista) (suma-lista (cdr lista)))))
```
   
##### 2.5.5. Función recursiva `veces`

Como último ejemplo vamos a definir la función `(veces lista id)` que
cuenta el número de veces que aparece un identificador en una lista.

¿Cómo planteamos el caso general? Llamaremos a la recursión con el
resto de la lista. Esta llamada nos devolverá el número de veces que
aparece el identificador en este resto de la lista. Y después sumamos
al valor devuelto 1 si el primer elemento de la lista coincide con el
identificador.

En Scheme:

```
(veces lista identificador) => (if (equal? (car lista) identificador)
                                   (+ 1 (veces (cdr lista) identificador))
                                   (veces (cdr lista) identificador))
```

Como caso base, si la lista es vacía devolvemos 0.

La versión completa:

```
(define (veces lista id)
  (cond
    ((null? lista) 0)
    ((equal? (car lista) id) (+ 1 (veces (cdr lista) id)))
    (else (veces (cdr lista) id))))

(veces '(a b a a b b) 'a) 
⇒ 3 
```


### <a name="3"></a> 3. Tipos de datos compuestos en Scheme

#### <a name="3-1"></a> 3.1. El tipo de dato pareja

##### 3.1.1. Función de construcción de parejas `cons`

Ya hemos visto en el seminario de Scheme que el tipo de dato compuesto
más simple es la pareja: una entidad formada por dos elementos. Se
utiliza la función `cons` para construirla:

```scheme
(cons 1 2) ; ⇒ {1 . 2}
(define c (cons 1 2))
```

Dibujamos la pareja anterior y la variable `c` que la referencia de la
siguiente forma:

<img src="imagenes/pareja.png" width="200px"/>

*Tipo compuesto pareja*

La instrucción `cons` construye un dato compuesto a partir de otros
dos datos (que llamaremos izquierdo y derecho). La expresión `{1 . 2}`
es la forma que el intérprete tiene de imprimir las parejas.

##### 3.1.2. Funciones de acceso `car` y `cdr`

Una vez construida una pareja, podemos obtener el elemento
correspondiente a su parte izquierda con la función `car` y su parte
derecha con la función `cdr`:

```scheme
(define c (cons 1 2))
(car c) ; ⇒ 1
(cdr c) ; ⇒ 2
```

###### Definición declarativa

Las funciones `cons`, `car` y `cdr` quedan perfectamente definidas con
las siguientes ecuaciones algebraicas:

```scheme
(car (cons x y)) = x
(cdr (cons x y)) = y
```

###### ¿De dónde vienen los nombres `car` y `cdr`?

Inicialmente los nombres eran CAR y CDR (en mayúsculas). La historia
se remonta al año 1959, en los orígenes del Lisp y tiene que ver con
el nombre que se les daba a ciertos registros de la memoria del IBM
709.

Podemos leer la explicación completa en
[The origin of CAR and CDR in LISP](http://www.iwriteiam.nl/HaCAR_CDR.html).

##### 3.1.3. Función pair?

La función `pair?` nos dice si un objeto es atómico o es una pareja:

```scheme
(pair? 3) ; ⇒ #f
(pair? (cons 3 4)) ; ⇒ #t
```

##### 3.1.4. Las parejas pueden contener cualquier tipo de dato

Ya hemos comprobado que Scheme es un lenguaje *débilmente
tipeado*. Las funciones pueden devolver y recibir distintos tipos de
datos.

Por ejemplo, podríamos definir la siguiente función `suma` que sume
tanto números como cadenas:

```scheme
(define (suma x y)
  (cond 
    ((and (number? x) (number? y)) (+ x y))
    ((and (string? x) (string? y)) (string-append x y))
    (else 'error)))
```

En la función anterior los parámetros `x` e `y` pueden ser números o
cadenas (o incluso de cualquier otro tipo). Y el valor devuelto por la
función será un número, una cadena o el símbolo `'error`.

Sucede lo mismo con el contenido de las parejas. Es posible guardar en
las parejas cualquier tipo de dato y combinar distintos tipos. Por
ejemplo:

```scheme
(define c (cons 'hola #f))
(car c) ; ⇒ 'hola
(cdr c) ; ⇒ #f
```

##### 3.1.5. Las parejas son objetos inmutables

Recordemos que en los paradigmas de programación declarativa y
funcional no existe el *estado mutable*. Una vez declarado un valor,
no se puede modificar. Esto debe suceder también con las parejas: una
vez creada una pareja no se puede modificar su contenido.

En Lisp y Scheme estándar (R6RS) las parejas sí que pueden ser
mutadas. Pero durante toda esta primera parte de la asignatura no lo
contemplaremos, para no salirnos del paradigma funcional.

En Swift y otros lenguajes de programación es posible definir
**estructuras de datos inmutables** que no pueden ser modificadas una
vez creadas. Lo veremos también más adelante.

#### <a name="3-2"></a> 3.2. Las parejas son objetos de primera clase

En un lenguaje de programación un elemento es de primera clase cuando puede:

* Asignarse a variables
* Pasarse como argumento
* Devolverse por una función
* Guardarse en una estructura de datos mayor

Las parejas son objetos de primera clase.

Una pareja puede asignarse a una variable:

```scheme
(define p1 (cons 1 2))
(define p2 (cons #f "hola"))
```

Una pareja puede pasarse como argumento y devolverse en una función:

```scheme
(define (suma-parejas p1 p2)
    (cons (+ (car p1) (car p2))
          (+ (cdr p1) (cdr p2))))

(suma-parejas '(1 . 5) '(4 . 12))
⇒ {5 . 17}
```

Y, por último, las parejas *pueden formar parte de otras parejas*. Es
lo que se denomina la propiedad de clausura de la función `cons`:

> El resultado de un `cons` puede usarse como parámetro de nuevas llamadas a `cons`.

Ejemplo:

```scheme
(define p1 (cons 1 2))
(define p2 (cons 3 4))
(define p (cons p1 p2))
```

Expresión equivalente:

```scheme
(define p (cons (cons 1 2)
                (cons 3 4)))
```
 
 Podríamos representar esta estructura así:

<img src="imagenes/pareja-pareja.png" width="300px"/>

*Propiedad de clausura: las parejas pueden contener parejas*

Pero se haría muy complicado representar muchos niveles de
anidamiento. Por eso utilizamos la siguiente representación:

<img src="imagenes/pareja-pareja2.png" width="250px"/>

Llamamos a estos diagramas *diagramas caja-y-puntero*
(*box-and-pointer* en inglés).

#### <a name="3-3"></a> 3.3. Diagramas *caja-y-puntero*

Al escribir expresiones complicadas con `cons` anidados es conveniente
para mejorar su legibilidad utilizar el siguiente formato:

```scheme
(define p (cons (cons 1
                      (cons 3 4))
                2))
```

Para entender la construcción de estas estructuras es importante
recordar que las expresiones se evalúan *de dentro a afuera*.

¿Qué figura representaría la estructura anterior?

Solución:

<img src="imagenes/pareja-pareja3.png" width="200px"/>

Es importante tener en cuenta que cada caja del diagrama representa
una pareja creada en la memoria del intérprete con la instrucción
`cons` y que el resultado de evaluar una variable en la que se ha
guardado una pareja devuelve la pareja recién creada. Por ejemplo, si
el intérprete evalúa `p` después de haber hecho la sentencia anterior
devuelve la pareja contenida en `p`, no se crea una pareja nueva.

Por ejemplo, si después de haber evaluado la sentencia anterior
evaluamos la siguiente:

```scheme
(define p2 (cons 5 (cons p 6)))
```

El diagrama caja y puntero resultante sería el siguiente:

<img src="imagenes/box-and-pointer2.png" width="250px"/>

Vemos que en la pareja que se crea con `(cons p 6)` se guarda en la
parte izquierda **la misma pareja que hay en `p`**. Lo representamos
con una flecha que apunta a la misma pareja que `p`.

**IMPORTANTE**: El funcionamiento de la evaluación de las parejas es
  similar al de los objetos en lenguajes orientados a objetos como
  Java. Cuando se evalúa una variable que contiene una pareja se
  devuelve la propia pareja, no una copia. En programación funcional,
  como el contenido de las parejas es inmutable, no hay problemas de
  *efectos laterales* por el hecho de que una pareja esté
  compartida. Veremos que cuando introduzcamos la mutación en Scheme
  aparecerán estos efectos laterales.

Es conveniente que pruebes a crear distintas estructuras de parejas
con parejas y a dibujar su diagrama caja y puntero. Y también a
recuperar un determinado dato (pareja o dato atómico) una vez creada
la estructura.

La siguiente función `print-pareja` puede ser útil a la hora de
mostrar por pantalla los elementos de una pareja

```scheme
(define (print-pareja pareja)
    (if (pair? pareja)
        (begin 
            (display "{")
            (print-dato (car pareja))
            (display " . ")
            (print-dato (cdr pareja))
            (display "}"))))

(define (print-dato dato)
    (if (pair? dato)
        (print-pareja dato)
        (display dato)))
```

**!Cuidado¡**: la función anterior contiene sentencias como `begin` o
   llamadas a `display` dentro del código de la función, que son
   propias de la programación imperativa. **No hacerlo en programación
   funcional**.

##### 3.3.1. Funciones c????r

Al trabajar con estructuras de parejas anidades es muy habitual
realizar llamadas del tipo:

```scheme
(cdr (cdr (car p)))
⇒ 4
```

Es equivalente a la función `cadar` de Scheme:

```scheme
(cddar p)
⇒ 4
```

El nombre de la función se obtiene concatenando a la letra "c", las
letras "a" o "d" según hagamos un car o un cdr y terminando con la
letra "r".

Hay definidas 2^4 funciones de este tipo: `caaaar`, `caaadr`, …,
`cddddr`.

### <a name="4"></a> 4. Listas en Scheme 

#### <a name="4-1"></a> 4.1. Implementación de listas en Scheme

Recordemos que Scheme permite manejar listas como un tipo de datos
básico. Hemos visto funciones para crear, añadir y recorrer listas.

Como repaso, podemos ver las siguientes expresiones. Fijaros que las
funciones `car`, `cdr` y `cons` son exactamente las mismas funciones
que las vistas anteriormente.

¿Por qué? ¿Qué relación hay entre las parejas y las listas?

Hagamos algunas pruebas, probando si los resultados son listas o
parejas usando las funciones `list?` y `pair?`.

Por ejemplo, una pareja formada por dos números es una pareja, pero no
es una lista:

```scheme
(define p1 (cons 1 2))
(pair? p1) ; ⇒ #t
(list? p1) ; ⇒ #f
```

Y una lista vacía es una lista, pero no es una pareja:

```scheme
(list? '()) ; ⇒ #t
(pair? '()) ; ⇒ #f
```

¿Una lista es una pareja? Pues sí:

```scheme
(define lista '(1 2 3))
(list? lista) ; ⇒ #t
(pair? lista) ; ⇒ #t
```

Por último, una pareja con una lista vacía como segundo elemento es
una pareja y una lista:

```scheme
(define p1 (cons 1 '()))
(pair? p1) ; ⇒ #t
(list? p1) ; ⇒ #t
```

Con estos ejemplos ya tenemos pistas para deducir la relación entre
listas y parejas en Scheme (y Lisp). Vamos a explicarlo.

##### 4.1.1. Definición de listas con parejas

Una lista es (definición recursiva):

* Una pareja que contiene en su parte izquierda el primer elemento de
  la lista y en su parte derecha el resto de la lista
* Un símbolo especial `'()` que denota la lista vacía

Por ejemplo, una lista muy sencilla con un solo elemento, `{1}`, se
define con la siguiente pareja:

```scheme
(cons 1 '())
```
	
La pareja cumple las condiciones anteriores: 

* La parte izquierda de la pareja es el primer elemento de la lista
  (el número 1)
* La parte derecha es el resto de la lista (la lista vacía)

<img src="imagenes/pareja-lista.png" width="150px"/>

*La lista {1}*

El objeto es al mismo tiempo una pareja y una lista. La función
`list?` permite comprobar si un objeto es una lista:

```scheme
(define l (cons 1 '()))
(pair? l)
(list? l)
```

Por ejemplo, la lista '(1 2 3 4) se construye con la siguiente
secuencia de parejas:

```scheme
(cons 1
      (cons 2
            (cons 3
                  (cons 4 
                        '()))))
```

La primera pareja cumple las condiciones de ser una lista:

* Su primer elemento es el 1
* Su parte derecha es la lista '(2 3 4)

<img src="imagenes/lista.png" width="400px"/>

*Parejas formando una lista*

Al comprobar la implementación de las listas en Scheme, entendemos por
qué las funciones `car` y `cdr` nos devuelven el primer elemento y el
resto de la lista.

##### 4.1.2. Lista vacía

La lista vacía es una lista:

```scheme
(list? '())
⇒ #t
```

Y no es un símbolo ni una pareja:

```scheme
(symbol? '())
⇒ #f
(pair? '())
⇒ #f
```

Para saber si un objeto es la lista vacía, podemos utilizar la función
`null?`:

```scheme
(null? '())
⇒ #t
```	

#### <a name="4-2"></a> 4.2. Listas con elementos compuestos

Las listas pueden contener cualquier tipo de elementos, incluyendo
otras parejas.

La siguiente estructura se denomina *lista de asociación*. Son listas
cuyos elementos son parejas (*clave*, *valor*):

```scheme
(list (cons 'a 1)
      (cons 'b 2)
      (cons 'c 3))
⇒ {{a . 1} {b . 2} {c . 2}}
```


¿Cuál sería el diagrama *box and pointer* de la estructura anterior?

<img src="imagenes/lista-parejas.png" width="400px"/>

La expresión equivalente utilizando conses es:

```scheme
(cons (cons 'a 1)
      (cons (cons 'b 2)
            (cons (cons 'c 3)
                  '())))
```

##### 4.2.1. Listas de listas

Si la pareja que guardamos como elemento de la lista es la cabeza de
otra lista tenemos una lista que contiene a otra lista:

```scheme
(define lista (list 1 (list 1 2 3) 3))
```

La lista anterior también se puede definir con quote:

```scheme
(define lista '(1 (1 2 3) 3))
```

El diagrama *box and pointer* de la lista es:

<img src="imagenes/lista-lista.png" width="500px"/>

*Lista que contiene otra lista como segundo elemento*

##### 4.2.2. Distintos niveles de abstracción

Es muy importante utilizar el nivel de abstracción correcto a la hora
de trabajar con listas que contienen otros elementos compuestos, como
otras parejas u otras listas.

Sólo hace falta *bajar* al nivel de caja y puntero cuando estemos
definiendo funciones de bajo nivel que tratan la estructura de datos
para obtener elementos concretos de las parejas.

Si, por el contrario, estamos recorriendo la lista principal y
queremos tratar sus elementos, debemos *verla* como una lista normal y
recorrerla con las funciones `car` para obtener su primer elemento y
`cdr` para obtener el resto. O `list-ref` para obtener un elemento
determinado (que puede ser atómico o compuesto).

Por ejemplo, la lista anterior `{1 {1 2 3} 3}` es una lista de 3
elementos. Si queremos obtener su segundo elemento (la lista `{1 2
3}`) bastaría con:

```scheme (define lista '(1 (1 2 3) 3)) (car (cdr lista)) (list-ref
lista 1) ```

#### <a name="4-3"></a> 4.3. Funciones recursivas y listas

##### 4.3.1. Implementación recursiva de funciones sobre listas

Vamos a ver cómo se implementan de forma recursiva alguna de funciones
que trabajan con listas, incluyendo algunas de las funciones de
Scheme. Para no solapar con las definiciones de Scheme pondremos el
prefijo `mi-` en todas ellas:

- `mi-list-ref`: implementación de la función `list-ref`
- `mi-append`: implementación de la funión `append` 


###### Función `mi-list-ref`

La función `(mi-list-ref n lista)` devuelve el elemento `n` de una
lista (empezando a contar por 0):

```scheme
(define lista '(a b c d e f g))
(mi-list-ref lista 2)
⇒ c
```

Veamos con el ejemplo anterior cómo hacer la formulación recursiva.

Hemos visto que, en general, cuando queremos resolver un problema de
forma recursiva tenemos que hacer una llamada recursiva a un problema
más sencillo, **confiar en que la llamada nos devuelva el resultado
correcto** y usar ese resultado para resolver el problema original.

En este caso nuestro problema es obtener el número que está en la
posición 2 de la lista `{a b c d e f g}`. Suponemos que la función que
nos devuelve una posición de la lista ya la tenemos implementada y que
la llamada recursiva nos va a devolver el resultado correcto. ¿Cómo
podemos simplificar el problema original? Veamos la solución para este
caso concreto:

> Para devolver el elemento 2 (empezando a contar por 0) de la lista
> `{a b c d e f g}` podemos hacer el `cdr` de la lista (obtendríamos
> `{b c d e f g}`) y devolver su elemento 1. Sería el valor `c`.

Generalizamos el ejemplo anterior, para cualquier `n` y cualquier lista:

> Para devolver el elemento que está en la posición `n` de una lista,
> hago el cdr de la lista y devuelvo su elemento n-1.

Y, por último, formulamos el caso base de la recursión, el problema
más sencillo que se puede resolver directamente, sin hacer una llamada
recursiva:

> Para devolver el elemento que está en la posición 0 de una lista,
> devuelvo el `car` de la lista

La implementación de todo esto en Scheme sería la siguiente (incluimos
un segundo caso base en el que se intenta obtener la posición de una
lista vacía; se llegaría a este caso si la posición que pedimos es
mayor que el número de elementos de la lista):

```scheme
(define (mi-list-ref lista n)
(cond
    ((null? lista)  (error "indice demasiado largo"))
    ((= n 0) (car lista))
    (else (mi-list-ref (cdr lista) (- n 1)))))
```

###### Función `list-tail`

La función `(mi-list-tail lista n)` devuelve la lista resultante de
quitar n elementos de la lista original:

```scheme
(mi-list-tail '(1 2 3 4 5 6 7) 2)
⇒ {3 4 5 6 7}
```

Piensa en cómo se implementaría de forma recursiva. Esta vez vamos a
mostrar directamente la implementación, sin dar explicaciones de cómo
se ha llegado a ella:

```scheme
(define (mi-list-tail lista n)
    (cond
        ((null? lista)  (error "indice demasiado largo"))
        ((= n 0) lista)
        (else (mi-list-tail (cdr lista) (- n 1)))))
```

###### Función `mi-append` 

Veamos ahora cómo podríamos implementar de forma recursiva la función
`append` que une dos listas. La llamaremos `(mi-append lista1
lista2)`.

Por ejemplo:

```
(mi-append '(a b c) '(d e f)) ;⇒ {a b c d e f}
```

Para resolver el problema de forma recursiva, haremos el `cdr` de la
primera lista, llamaremos a la recursión para que una el resultado con
la segunda lista`:

```
(mi-append (cdr '(a b c)) '(d e f)) ;⇒
(mi-append '(b c) '(d e f) ;⇒ {b c d e f}
```

Y añadiremos el primer elemento a la lista resultante usando un `cons`:

```
(cons (car '(a b c)) (mi-append (cdr '(a b c)) '(d e f))) ;⇒
(cons 'a '(b c d e f)) ;⇒ {a b c d e f}
```

En general:

```
(mi-append lista1 lista2) ⇒ (cons (car lista1) (mi-append (cdr lista1) lista2))
```

El caso base, el caso en el que la función puede devolver un valor
directamente sin llamar a la recursión, es aquel en el que `lista1` es
`null?`. En ese caso devolvemos `lista2`:

```
(mi-append '() '(a b c)) ;⇒ '{a b c}
```

La formulación recursiva completa queda como sigue:

```scheme
(define (mi-append l1 l2)
    (if (null? l1)
        l2
        (cons (car l1)
              (mi-append (cdr l1) l2))))
```

###### Función `mi-reverse`

Veamos cómo implementar de forma recursiva la función `mi-reverse` que
invierte una lista

```scheme
(mi-reverse '(1 2 3 4 5 6))
⇒ {6 5 4 3 2 1}
```

La idea es sencilla: llamamos a la recursión para hacer la inversa del
`cdr` de la lista y añadimos el primer elemento a la lista resultante
que devuelve ya invertida la llamada recursiva.

Podemos definir una función auxiliar `(añade-al-final dato lista)` que
añade un dato al final de una lista usando `append`:

Veamos directamente su implementación, usando `mi-append` para añadir
un elemento al final de la lista:

```scheme
(define (añade-al-final dato lista)
    (append lista (list dato)))
```

La función `mi-reverse` quedaría entonces como sigue:

```scheme
(define (mi-reverse lista)
    (if (null? lista) '()
    (añade-al-final (car lista) (mi-reverse (cdr lista)))))
```

##### 4.3.2. Ejemplos de funciones recursivas que usan listas

###### Función `cuadrados-hasta`

La función `(cuadrados-hasta x)` devuelve una lista con los cuadrados
de los números hasta x:

> Para construir una lista de los cuadrados hasta x, añado el cuadrado
> de x a la lista de los cuadrados hasta x-1

El caso base de la recursión es el caso en el que x es 1, entonces
devolvemos una lista formada por el 1.

En Scheme:

```scheme
(define (cuadrados-hasta x)
   (if (= x 1)
      '(1)
      (cons (cuadrado x)
            (cuadrados-hasta (- x 1)))))
```

Ejemplo:

```scheme
(cuadrados-hasta 10)
⇒ {100 81 64 49 36 25 16 9 4 1}
```

###### Función `filtra-pares`

Es muy habitual recorrer una lista y comprobar condiciones de sus
elementos, construyendo una lista con los que cumplan una determinada
condición.

Por ejemplo, la siguiente función `filtra-pares` construye una lista
con los números pares de la lista que le pasamos como parámetro:

```scheme
(define (filtra-pares lista)
   (cond
      ((null? lista) '())
	  ((even? (car lista)) (cons (car lista)
	                             (filtra-pares (cdr lista))))
      (else (filtra-pares (cdr lista)))))
```

Ejemplo:

```scheme
(filtra-pares '(1 2 3 4 5 6))
⇒ {2 4 6}
```

###### Función `primo?`

El uso de listas es uno de los elementos fundamentales de la
programación funcional.

Como ejemplo, vamos a ver cómo trabajar con listas para construir una
función que calcula si un número es primo. La forma de hacerlo será
calcular la lista de divisores del número y comprobar si su longitud
es dos. En ese caso será primo.

Por ejemplo:

```
(divisores 8) ;⇒ {1 2 4 8} longitud = 4, no primo
(divisores 9) ;⇒ {1 3 9} longitud = 3, no primo
(divisores 11) ;⇒ {1 11} longitud = 2, primo
```

Podemos definir entonces la función `(primo? x)` de la siguiente forma:

```scheme
(define (primo? x)
   (=  2 
      (length (divisores x))))
```

¿Cómo implementamos la función `(divisores x)` que nos devuelve la
lista de los divisores de un número `x`. Vamos a construirla de la
siguiente forma:

1. Creamos una lista de todos los números del 1 a x
2. Filtramos la lista para dejar los divisores de x

La función `(lista-hasta x)` devuelve una lista de números 1..x:

```scheme
(define (lista-hasta x)
   (if (= x 0)
      '()
      (cons x (lista-hasta (- x 1)))))
```

Ejemplos:

```
(lista-hasta 2) ;⇒ {1 2}
(lista-hasta 10) ;⇒ {1 2 3 4 5 6 7 8 9 10}
```

Definimos la función `(divisor? x y)` que nos diga si x es divisor de
y:

```scheme
(define (divisor? x y)
      (= 0 (mod y x)))
```

Ejemplos:

```
(divisor 2 10) ;⇒ #t
(divisor 3 10) ;⇒ #f
```

Una vez que hemos definido La función `divisor?` podemos utilizarla
para definir la función recursiva `(filtra-divisores lista x)` que
devuelve una lista con los números de `lista` que son divisores de
`x`:

```scheme
(define (filtra-divisores lista x)
   (cond
      ((null? lista) '())
      ((divisor? (car lista) x) (cons (car lista)
                                      (filtra-divisores (cdr lista) x)))
      (else (filtra-divisores (cdr lista) x))))
```

Ya podemos implementar la función que devuelve los divisores de un
número `x` generando los números hasta `x` y filtrando los divisores
de ese número. Por ejemplo, para calcular los divisores de 10:

```
(filtra-divisores {1 2 3 4 5 6 7 8 9 10} 10) ;⇒ {1 2 5 10}
```

Se puede implementar de una forma muy sencilla:

```scheme
(define (divisores x)
   (filtra-divisores (lista-hasta x) x))
```

Y una vez definida esta función, ya puede funcionar correctamente la
función `primo?`.


#### <a name="4-4"></a> 4.4. Funciones con número variable de argumentos

Hemos visto algunas funciones primitivas de Scheme, como `+` o `max`
que admiten un número variable de argumentos. ¿Podemos hacerlo también
en funciones definidas por nosotros?

La respuesta es sí, utilizando lo que se denomina notación
*dotted-tail* (punto-cola) para definir los parámetros de la
función. En esta notación se coloca un punto antes del último
parámetro. Los parámetros antes del punto (si existen) tendrán como
valores los argumentos usados en la llamada y el resto de argumentos
se pasarán en forma de lista en el último parámetro.

Por ejemplo, si tenemos la definición

```scheme
(define (funcion-dos-o-mas-args x y . lista-args) 
    <cuerpo>)
```

podemos llamar a la función anterior con dos o más argumentos:

```scheme
(funcion-dos-o-mas-args 1 2 3 4 5 6)
```
	
En la llamada, los parámetros `x` e `y` tomarán los valores 1 y 2. El
parámetro `lista-args` tomará como valor una lista con los argumentos
restantes `(3 4 5 6)`.

También es posible permitir que todos los argumentos sean opcionales
no poniendo ningún argumento antes del punto::

```scheme
(define (funcion-cualquier-numero-args . lista-args) 
    <cuerpo>)
```
	
Si hacemos la llamada

```scheme
(funcion-cualquier-numero-args 1 2 3 4 5 6)
```
	
el parámetro `lista-args` tomará como valor la lista `(1 2 3 4 5 6)`.

Veamos un sencillo ejemplo.

Podemos implementar una función `mi-suma` que tome al menos dos
argumentos y después un número variable de argumentos y devuelva la
suma de todos ellos. Es muy sencillo: recogemos todos los argumentos
en la lista de argumentos variables y llamamos a la función
`suma-lista` que suma una lista de números:

```scheme
(define (mi-suma x y . lista-nums)
    (if (null? lista-nums)
        (+ x y)
        (+ x (+ y (suma-lista lista-nums)))))
```

### <a name="5"></a>5. Funciones como tipos de datos de primera clase

Hemos visto que la característica fundamental de la programación
fundamental es la definición de funciones. Hemos visto también que no
producen efectos laterales y no tienen estado. Una función toma unos
datos como entrada y produce un resultado como salida.

Para simbolizar el hecho de que las funciones toman parámetros de
entrada y devuelven una única salida, vamos a representar las
funciones como un símbolo especial, una pequeña casa invertida con
unas flechas en la parte superior que representan las entradas y una
única flecha que representa la salida. Por ejemplo, podemos
representar de la siguiente forma la función que eleva al cuadrado un
número:

<img src="imagenes/funcion-cuadrado.png" width="80px"/>

También podemos representar la función que suma dos parejas:

<img src="imagenes/esquema-suma-parejas.png" width="200px"/>

Una de las características fundamentales de la programación funcional
es considerar a las funciones como *objetos de primera
clase*. Recordemos que un tipo de primera clase es aquel que:

1. Puede ser asignado a una variable
2. Puede ser pasado como argumento a una función
3. Puede ser devuelto como resultado de una invocación a una función
4. Puede ser parte de un tipo mayor

Vamos a ver que las funciones son ejemplos de todos los casos
anteriores: vamos a poder crear funciones sin nombre y asignarlas a
variables, pasarlas como parámetro de otras funciones y guardarlas en
tipos de datos compuestos como listas. En este primer apartado veremos
los puntos 1, 2 y 4. Veremos las funciones devueltas por funciones en
el siguiente apartado, cuando hablemos de *clausuras*.

La posibilidad de usar funciones como objetos de primera clase es una
característica fundamental de los lenguajes funcionales. Es una
característica de muchos lenguajes multi-paradigma con características
funcionales como
[JavaScript](http://helephant.com/2008/08/19/functions-are-first-class-objects-in-javascript/),
[Python](https://thenewcircle.com/static/bookshelf/python_fundamentals_tutorial/functional_programming.html),
[Swift](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html)
o incluso en la última versión de Java,
[Java 8](http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html),
(donde se denominan *expresiones lambda*).

#### <a name="5-1"></a>5.1. Un primer ejemplo

Vamos a empezar con un ejemplo introductorio: la función `(map func
lista)` de Scheme que toma como parámetro **una función** y una
lista. La función de vuelve la lista resultante de *mapear* (aplicar)
la función `func` a todos y cada unos de los elementos de la lista
original.

Veamos cómo funciona. La función que pasamos como parámetro debe tener
un único argumento. Por ejemplo la función `cuadrado`:

```scheme
(define (cuadrado x)
    (* x x))
```

Probamos la función `map` pasando la función anterior como parámetro:

```scheme
(map cuadrado '(1 2 3 4 5)) ⇒ (1 4 9 16 25)
```

Vemos que la función `map` aplica la función `cuadrado` a todos los
elementos de la lista y devuelve la lista resultante. Podemos
comprobar que se pasa como parámetro *una función*, o sea, código, que
es ejecutado dentro de la función map, procesando cada elemento de la
lista y añadiendo el resultado a la lista resultante.

Podemos pasar cualquier función a `map`, siempre que sea una función
de un parámetro que se pueda aplicar a los elementos de la lista:

```scheme
(define (doble x)
   (* x 2))

(define (suma-1 x)
   (+ x 1))
```

Ejemplo de invocación a `map` con las funciones anteriores:

```scheme
(map doble '(1 2 3 4 5)) ⇒ (2 4 6 8 10)
(map suma-1 '(1 2 3 4 5)) ⇒ (2 3 4 5 6)
```

La posibilidad de pasar funciones como argumentos proporciona una gran
potencia y flexibilidad en el lenguaje. En este ejemplo podemos
cambiar el comportamiento de `map` dependiendo de la función que
pasamos como parámetro. En un caso eleva al cuadrado los números, en
otro los multiplica por dos y en el último les suma 1.

Vemos que para pasar por parámetro la función basta con poner su
nombre. Para pasar la función que eleva al cuadrado un número ponemos
como parámetro el nombre de la función: `cuadrado`. Es posible incluso
pasar una función a la que no le hemos dado nombre, usando lo que se
denomina una *expresión lambda*.

Por ejemplo, la siguiente expresión lambda construye una función que
suma 3 a un número:

```
(lambda (x) (+ x 3))
```

Si escribimos la expresión lambda como parámetro de `map` tendremos
una función que suma 3 a todos los elementos de una lista:

```
(map (lambda (x) (+ x 3)) '(1 2 3 4 5)) ⇒ (4 5 6 7 8)
```

#### <a name="5-2"></a>5.2. Forma especial `lambda`

Veamos más despacio cómo funciona la forma especial `lambda`. 

De la misma forma que podemos usar cadenas o enteros sin darles un
nombre, en Scheme es posible usar una función (código) sin darle un
nombre mediante la forma especial `lambda`.

##### Sintaxis de la forma especial `lambda`

La sintaxis de la forma especial `lambda` es:

```
(lambda (<arg1> ... <argn>) 
    <cuerpo>)
```

El cuerpo del lambda define un *bloque de código* y sus argumentos son
los parámetros necesarios para ejecutar ese bloque de código. Llamamos
a la función resultante una *función anónima*.

Otros ejemplos:

Una función anónima que suma dos parejas:

```
(lambda (p1 p2)
    (cons (+ (car p1) (car p2))
          (+ (cdr p1) (cdr p2))))
```

Una función anónima que devuelve el mayor de dos números:

```
(lambda (a b)
    (if (> a b)
        a
        b))
```

##### Semántica de la forma especial `lambda`

La invocación a la forma especial `lambda` construye una función
anónima en tiempo de ejecución.

Por ejemplo, si ejecutamos una expresión lambda en el intérprete
veremos que devuelve un procedimiento:

```scheme
(lambda (x) (* x x))
⇒ #<procedure>
```

El procedimiento construido es un bloque de código que devuelve el
cuadrado de un número.


¿Qué podemos hacer con este procedimiento? 

Podemos asignarlo a un identificador. Por ejemplo, en la siguiente
expresión, primero se evalúa la *expresión lambda* y el procedimiento
resultante se asocia al identificador `cuadrado`.

```
> (define cuadrado (lambda (x) (* x x)))
```

Si escribimos el identificador `cuadrado` Scheme devuelve el
procedimiento asociado a esta variable:

```
> cuadrado
#<procedure:cuadrado>
```

Ahora podemos usar la función cuadrado como si la hubiéramos creado de
la forma habitual:

```
(cuadrado 3) ⇒ 9
```

También podemos invocar a una función anónima sin darle un nombre:

```
((lambda (x) (* x x)) 3) ⇒ 9
```

La llamada a `lambda` crea un procedimiento y el paréntesis a su
izquierda lo invoca con el parámetro 3:

```
((lambda (x) (* x x)) 3) => (#<procedure> 3) ⇒ 9
```

Y también podemos pasar el procedimiento como parámetro de otra
función, como en el siguiente ejemplo. El procedimiento se asigna al
parámetro `f` y se invoca en el cuerpo. En el ejemplo la función
`aplica-dos-veces` recibe un procedimiento que se aplica al parámetro
`x` y se vuelve aplicar al número resultante.

```
(define (aplica-dos-veces f x)
   (f (f x)))
```

Lo podemos invocar pasándole cualquier función de un argumento y
cualquier dato:

```
(aplica-dos-veces (lambda (x)
                     (+ x 5)) 10) ⇒ 20
(aplica-dos-veces (lambda (s)
                     (string-append s "-jeje")) "Hola") ⇒ "Hola-jeje-jeje"
```

Es importante remarcar que con `lambda` estamos creando una función en
*tiempo de ejecución*. Es código que creamos para su posterior
invocación.

No hay que dejar que la sintaxis o la palabra `lambda` confunda. Lo
único que estamos haciendo es crear *un bloque de código* y definir
sus argumentos. Quizás ayuda ver cómo define la función anónima que
devuelve el cuadrado de un número en distintos lenguajes.

**Java 8**

```
Integer x -> {x*x}
```

**Scala**

```
(x:Int) => {x*x}
```

**Objective C**

```
^int (int x)
{
   x*x
};
```

**Swift**

```
{ (x: Int) -> Int in return x*x }
```

##### Identificadores y funciones

Tras conocer `lambda` ya podemos explicarnos por qué cuando escribimos
en el intérprete de Scheme el nombre de una función, se evalúa a un
*procedure*:

```
> +
⇒ <procedure:+>
```

El identificador se evalúa y devuelve el *objeto función* al que está
ligado. En Scheme los nombres de las funciones son realmente símbolos
a los que están ligados *objetos de tipo función*.

Podemos asignar funciones ya existentes a nuevos identificadores
usando `define`, como en el ejemplo siguiente:

```
> +
⇒ <procedure:+>
> (define suma +)
> (suma 1 2 3 4)
⇒ 10
```

Es muy importante darse cuenta que la expresión `(define suma +)` se
evalúa de forma idéntica a `(define y x)`. Primero se evalúa el
identificador `+`, que devuelve el *objeto función* suma, que se
asigna a la variable `suma`. El resultado final es que tanto `+` como
`suma` tienen como valor el mismo procedimiento:

<img src="imagenes/suma.png" width="100px"/>


La forma especial `define` para definir una función no es más que
*azucar sintáctico*.

```
(define (<nombre> <args>)
    <cuerpo>)
```

siempre se convierte internamente en:

```
(define <nombre> 
    (lambda (<args>)
        <cuerpo>))
```

Por ejemplo

```
(define (cuadrado x)
    (* x x))
```

es equivalente a:

```
(define cuadrado 
    (lambda (x) (* x x)))
```

##### Predicado `procedure?`

Podemos comprobar si algo es una función utilizando el predicado de
Scheme `procedure?`.

Por ejemplo:

```
(procedure? (lambda (x) (* x x)))
⇒ #t
(define suma +)
(procedure? suma)
⇒ #t
(procedure? '+)
⇒ #f
```

Hemos visto que las funciones pueden asignarse a variables. También
cumplen las otras condiciones necesarias para ser consideradas objetos
de primera clase.

#### <a name="5-3"></a> 5.3. Funciones argumentos de otras funciones 

Hemos visto ya un ejemplo de cómo pasar una función como parámetro de
otra. Veamos algún otro.

Por ejemplo, podemos definir la función `aplica` que recibe una
función en el parámetro `func` y dos valores en los parámetros `x` e
`y` y devuelve el resultado de invocar a la función que pasamos como
parámetro con `x` e `y`. La función que se pase como parámetro debe
tener dos argumentos

Para realizar la invocación a la función que se pasa como parámetro
basta con usar `func` como su nombre. La función se ha ligado al
nombre `func` en el momento de la invocación a `aplica`, de la misma
forma que los argumentos se ligan a los parámetros `x` e `y`:

```
(define (aplica f x y)
   (f x y))
```

Algunos ejemplos de invocación, usando funciones primitivas, funciones
definidas y expresiones lambda:

```
(aplica + 2 3) ; ⇒ 5
(aplica * 4 5) ; ⇒ 10
(aplica string-append "hola" "adios") ; ⇒ "holaadios"

(define (string-append-con-guion s1 s2)
    (string-append s1 "-" s2))

(aplica string-append-con-guion "hola" "adios") ; ⇒ "hola-adios"

(aplica (lambda (x y) (sqrt (+ (* x x) (* y y)))) 3 4) ⇒ 5
```

Otro ejemplo, la función `aplica-2` que toma dos funciones `f` y `g` y
un argumento `x` y devuelve el resultado de aplicar `f` a lo que
devuelve la invocación de `g` con `x`:

```
(define (aplica-2 f g x)
   (f (g x)))

(define (suma-5 x)
   (+ x 5))
(define (doble x)
   (+ x x))
(aplica-2 suma-5 doble 3)
⇒ 11
```

#### <a name="5-4"></a> 5.4. Funciones en estructuras de datos

La última característica de los tipos de primera clase es que pueden
formar parte de tipos de datos compuestos, como listas.

Para construir una lista de funciones debemos llamar a `list` con las
funciones:

```
(define lista (list cuadrado suma-1 doble))
lista
⇒ {#<procedure:cuadrado>  #<procedure:suma-1>  #<procedure:doble>}
```

También podemos evaluar una expresión lambda y añadir el procedimiento
resultante. Por ejemplo, para añadir otra función a la lista anterior
podemos llamar a `cons`:

```
(define lista2 (cons (lambda (x) (+ x 5)) lista))
lista2
⇒ {#<procedure> #<procedure:cuadrado> #<procedure:suma-1> #<procedure:doble>}
```

Una vez creada una lista con funciones, ¿cómo podemos invocar a alguna
de ellas?. Debemos tratarlas de la misma forma que tratamos cualquier
otro dato guardado en la lista, las recuperamos con las funciones
`car` o `list-ref` y las invocamos. Por ejemplo, para invocar a la
primera función de `lista2`:

```
((car lista2) 10) ; ⇒ 15
```

##### Funciones que trabajan con listas de funciones

Veamos un ejemplo de una función `(aplica-funcs lista-funcs x)` que
recibe una lista de funciones en el parámetro `lista-funcs` y las
aplica todas al número que pasamos en el parámetro `x`.

Por ejemplo, si construimos una lista con las funciones `cuadrado`,
`cubo` y `suma-1`:

```
(define lista (list cuadrado cubo suma-1))
```

la llamada a `(aplica-funcs lista 5)` debería devolver el resultado de
aplicar primero `suma-1` a 5, después `cubo` al resultado y después
`cuadrado`:

```
(cuadrado (cubo (suma-1 5))
⇒ 46656
```

Para implementar `aplica-funcs` tenemos que usar una recursión. Si
vemos el ejemplo, podemos comprobar que es sencillo definir el caso
general:

```
(aplica-funcs (cuadrado cubo suma-1) 5) => (cuadrado (aplica-funcs (cubo suma-1) 5))
=> (cuadrado 216) => 46656
```

El caso general de la recursión de la función `aplica-funcs` se define
entonces como:

```
(aplica-funcs lista-funcs x) => ((car lista-funcs) (aplica-funcs (cdr lista-funcs)))
```

El caso base sería en el que la lista de funciones tiene sólo una
función:

```
(if (null? (cdr lista-funcs)) ; la lista de funciones solo tiene una función
    ((car lista-funcs) x) ; invocamos a la función con el parámetro x
    ...
```

La implementación completa es:

```
(define (aplica-funcs lista-funcs x)
    (if (null? (cdr lista-funcs))
        ((car lista-funcs) x)
        ((car lista-funcs)
            (aplica-funcs (cdr lista-funcs) x))))
```

Un ejemplo de uso:

```
(define lista-funcs (list (lambda (x) (* x x))
                          (lambda (x) (* x x x))
                          (lambda (x) (+ x 1))))
(aplica-funcs lista-funcs 5)
⇒ 46656
```

#### <a name="5-5"></a> 5.5 Generalización

La posibilidad de pasar funciones como parámetros de otras es una
poderosa herramienta de abstracción. Por ejemplo, supongamos que
queremos calcular el sumatorio de `a` hasta `b`:

```
(define (sum-x a b)
    (if (> a b)
        0
        (+ a (sum-x (+ a 1) b))))

(sum-x 1 10)
⇒ 55
```

Supongamos ahora que queremos calcular el sumatorio de `a` hasta `b`
sumando los números al cuadrado:

```
(define (sum-cuadrado-x a b)
    (if (> a b)
        0
        (+ (* a a) (sum-cuadrado-x (+ a 1) b))))

(sum-cuadrado-x 1 10)
⇒ 385
```

Y el sumatorio de `a` hasta `b` sumando los cubos:

```
(define (sum-cubo-x a b)
    (if (> a b)
        0
        (+ (* a a a) (sum-cubo-x (+ a 1) b))))

(sum-cubo-x 1 10)
⇒ 3025
```

Vemos que el código de las tres funciones anteriores es muy similar,
cada función la podemos obtener haciendo un *copy-paste* de otra
previa. Lo único que cambia es la función a aplicar a cada número de
la serie.

Siempre que hagamos *copy-paste* al programar tenemos que empezar a
sospechar que no estamos generalizando suficientemente el código. Un
*copy-paste* arrastra también *bugs* y obliga a realizar múltiples
modificaciones del código cuando en el futuro tengamos que cambiar
cosas.

La posibilidad de pasar una función como parámetro es una herramienta
poderosa a la hora de generalizar código. En este caso, lo único que
cambia en las tres funciones anteriores es la función a aplicar a los
números de la serie. Podemos tomar esa función como un parámetro
adicional y definir una función genérica `sum-f-x` que generaliza las
tres funciones anteriores. Tendríamos el sumatorio desde `a` hasta `b`
de `f(x)`:

```
(define (sum-f-x f a b)
    (if (> a b)
        0
        (+ (f a) (sum-f-x f (+ a 1) b))))
```

Las funciones anteriores son casos particulares de esta función que
las generaliza. Por ejemplo, para calcular el sumatorio desde 1 hasta
10 de `x` al cubo:

```
(define (cubo x)
    (* x x x))

(sum-f-x cubo 1 10)
⇒ 3025
```

#### <a name="5-6"></a> 5.6. Funciones de orden superior

Llamamos funciones de orden superior (*higher order functions* en
inglés) a las funciones que toman otras como parámetro o devuelven
otra función. Permiten generalizar soluciones con un alto grado de
abstracción.

Los lenguajes de programación funcional como Scheme, Scala o Java 8
tienen ya predefinidas algunas funciones de orden superior que
permiten tratar listas o *streams* de una forma muy concisa y
compacta.

Para trabajar con las funciones de orden superior definidas en Scheme
R6RS (`map`, `filter`, `fold-right`) hay que importar todas las
bibliotecas con la instrucción `(import (rnrs))`:

```scheme
#lang r6rs
(import (rnrs))
```

Vamos a comenzar viendo la función `map` de Scheme, como ejemplo
típico de función de orden superior. Después veremos las funciones
`filter` y `fold-right` y las definiremos también nosotros para
comprobar su implementación recursiva. Y terminaremos viendo cómo la
utilización de funciones de orden superior es una excelente
herramienta de la programación funcional que permite hacer código muy
conciso y expresivo.

La combinación de funciones de nivel superior con listas es una de las
características más potentes de la programación funcional.

##### 5.6.1. Función `map`

Veamos de nuevo la función `map`. Recordemos que `map` aplica una
función a cada uno de los elementos de la lista que pasamos como
parámetro:

```scheme
(map cuadrado '(1 2 3 4 5))
⇒ {1 4 9 25}
```

Otro ejemplo, en el que obtenemos una lista de números resultantes de
sumar cada pareja de números de una lista:

```scheme
(define (suma-pareja pareja)
    (+ (car pareja) (cdr pareja)))

(map suma-pareja (list (cons 2 4) (cons 3 6) (cons 5 3)))
⇒ {6 9 8}
```

También podríamos hacerlo con una expresión lambda:

```
(map (lambda (pareja)
         (+ (car pareja) (cdr pareja))) (list (cons 2 4) (cons 3 6) (cons 5 3)))
⇒ {6 9 8}
```

> CONSEJO DE USO  
> La función `map` recibe una lista de *n* elementos y devuelve otra
> de *n* elementos transformados.

###### Implementación de `map`

¿Cómo se podría implementar `map` de forma recursiva? Llamamos a la
función `mi-map`. La implementación es la siguiente:

```
(define (mi-map f lista)
    (if (null? lista)
        '()
        (cons (f (car lista))
              (mi-map f (cdr lista)))))
```

##### 5.6.2. Función `filter`

Veamos otra función de orden superior que trabaja sobre listas.

La función `(filter predicado lista)` toma como parámetro un predicado
y una lista y devuelve como resultado los elementos de la lista que
cumplen el predicado.

Un ejemplo de uso:

```
(filter even? '(1 2 3 4 5 6 7 8))
⇒ {2 4 6 8}
```

Otro ejemplo: supongamos que queremos filtrar una lista de parejas de
números, devolviendo aquellas que parejas que cumplen que su parte
izquierda es mayor o igual que la derecha. Lo podríamos hacer con la
siguiente expresión:

```
(filter (lambda (pareja)
            (>= (car pareja) (cdr pareja))) (list (cons 10 4) (cons 2 4) (cons 8 8) (cons 10 20)))
⇒ {{10 . 4} {8 . 8}}
```

> CONSEJO DE USO  
> La función `filter` recibe una lista de *n* elementos y devuelve
> otra de con *n* o menos elementos originales filtrados por una
> condición.


###### Implementación de `filter`

Podemos implementar la función `filter` de forma recursiva:

```
(define (mi-filter pred lista)
  (cond
    ((null? lista) '())
    ((pred (car lista)) (cons (car lista)
                              (mi-filter pred (cdr lista))))
    (else (mi-filter pred (cdr lista)))))
```

##### 5.6.3. Función `fold-right`

Por último vamos a ver la función `(fold-right func base lista)` que
permite recorrer una lista aplicando una función binaria de forma
acumulativa a sus elementos. El nombre `fold` significa
*plegado*. Utilizaremos la función cuando necesitemos obtener un dato
a partir de los elementos de una lista.

La explicación de su funcionamiento es la siguiente:

> El resultado de hacer `(fold-rigt f base lista)` de una función `f`
> **de dos argumentos**, un dato base y una lista, es el resultado de
> aplicar de derecha a izquierda la función `f` en cascada a cada dato
> de la lista y al resultado de la invocación `f` anterior. La primera
> invocación a `f` se hace con el último dato de la lista y el caso
> base.

Por ejemplo, supongamos la función de dos argumentos que suma dos valores. 

```scheme
(define (suma dato resultado)
    (+ dato resultado))
```

Llamamos a los parámetros `dato` y `resultado` para remarcar que el
primer parámetro se va a coger de la lista y el segundo del resultado
calculado. La llamada a `fold-right` funcionaría de la siguiente
forma:

```
(fold-right suma 0 '(1 2 3))
⇒ (suma 1 (suma 2 (suma 3 0)))
⇒ (suma 1 (suma 2 3))
⇒ (suma
⇒ 6
```

Vemos que se llama en cascada de derecha a izquierda a la función
`suma` comenzando con el último número de la lista (el `3`) y el caso
base que se pasa en la invocación a `fold-right` (el `0`).

Podemos comprobar la potencia de la función `fold-right` con los
siguientes ejemplos

```
(fold-right string-append "" '("hola" "que" "tal"))
⇒ "holaquetal"
(fold-right (lambda (x y) (* x y)) 1 '(1 2 3 4 5 6 7 8))
⇒ 40320
(fold-right cons '() '(1 2 3 4))
⇒ {1 2 3 4}
```

Veamos un último ejemplo. Supongamos que queremos definir una función
que sume todos los números de una lista de parejas:

```
(suma-parejas (list (cons 3 6) (cons 2 9) (cons -1 8) (cons 9 3))) ; ⇒ 39
```

Podemos hacerlo usando `fold-right` con una función de plegado que
realice la suma de los números de la pareja de la lista y del
resultado ya sumado.

La función de plegado sería:

```
(define (suma-pareja-numero pareja resultado)
    (+ (car pareja) (cdr pareja) resultado))
```

Y podríamos definir `suma-parejas` como:

```
(define (suma-parejas lista)
    (fold-right suma-pareja-numero 0 lista))
```

También lo podríamos hacer todo con una expresión lambda:

```
(define (suma-parejas lista)
    (fold-right (lambda (pareja resultado)
                   (+ (car pareja) (cdr pareja) resultado)) 0 lista))
```

> CONSEJO DE USO  
> La función `fold-right` recibe una lista de datos y devuelve un único resultado

###### Implementación de `fold-right`

Podríamos implementar de forma recursiva la función `fold-right`:

```
(define (mi-fold-right func base lista)
  (if (null? lista)
      base
      (func (car lista) (mi-fold-right func base (cdr lista)))))
```

##### 5.6.4. Uso de funciones de orden superior

El uso de funciones de orden superior y las expresiones lambda
proporciona muchísima expresividad en un lenguaje de programación. Es
posible escribir código muy conciso, que hace cosas complicadas en
pocas líneas.

No lo hemos hecho hasta ahora, pero es posible utilizar en el cuerpo
de las expresiones lambda los parámetros de la función principal en la
que se usa esta expresión. Veamos un ejemplo.

Supongamos que queremos definir una función `(suma-n lista n)` que
devuelve la lista resultante el resultado de sumar un número `n` a
todos los elementos de una lista.

Podemos hacerlo de forma recursiva:

```
(define (suma-n lista n)
    (if (null? lista)
        '()
        (cons (+ (car lista) n)
              (suma-n (cdr lista)))))
```

Funciona de la siguiente manera:

```
(suma-n '(1 2 3 4) 10)
⇒ (11 12 13 14)
```

Pero podemos implementar la función de otra forma, utilizando la
función de orden superior `map` y una expresión lambda que sume el
número `n` a los elementos de la lista:

```
(define (suma-n lista n)
    (map (lambda (x) (+ x n)) lista))
```

Vemos que utilizamos el parámetro `n` en el cuerpo de la expresión
lambda. De esta forma la función que se aplica a los elementos de la
lista es una función que suma este número a cada elemento. La variable
`x` en el parámetro de la expresión lambda es la que va tomando el
valor de los elementos de la lista.

```
(suma-n '(1 2 3 4) 10) => (map (lambda (x) (+ x 10)) (11 12 13 14)) =>  (11 12 13 14)
```

Veamos otro ejemplo. Supongamos que queremos definir la función
`(contienen-letra caracter lista-pal)` que devuelve las palabras de
una lista que contienen un determinado carácter.

Por ejemplo:

```scheme
(contienen-letra #\a '("En" "un" "lugar" "de" "la" "Mancha")) ⇒ ("lugar" "la" "Mancha")
```

Podemos implementar `contienen-letra` usando la función de orden
superior `filter`, escribiendo en la expresión lambda un predicado que
compruebe si la palabra contiene el carácter:

```scheme
(define (contienen-letra caracter lista-pal)
   (filter (lambda (pal)
              (letra-en-str? caracter pal)) lista-pal))
```

La función `(letra-en-str? caracter pal)` es un predicado auxiliar que
necesitamos, que comprueba si una cadena contiene un carácter.

Por ejemplo:

```
(letra-en-str? #\a "Hola") ⇒ #t
(letra-en-str? #\a "Pepe") ⇒ #f
```

Sólo nos falta implementar esta función `letra-en-str`. Lo podemos
implementar obteniendo una lista de caracteres a partir de la cadena y
volviendo a usar la función `filter`:

```
(define (letra-en-pal? caracter palabra)
    (not (null? (filter (lambda (c)
                           (equal? c caracter)) (string->list palabra))))
```


### <a name="6"></a>6. Ámbitos de variables, let y closures

Ahora que hemos introducido la forma especial `lambda` y la idea de
que una función es un objeto de primera clase, podemos revisar el
concepto de ámbito de variables en Scheme e introducir un importante
concepto: clausura
(*[closure](https://en.wikipedia.org/wiki/Closure_(computer_programming))*
en inglés).

#### <a name="6-1"></a> 6.1. Ámbitos de variables

El concepto del **ámbito** de vida de las variables es un concepto
fundamental en los lenguajes de programación. En inglés se utiliza el
término
*[scope](https://en.wikipedia.org/wiki/Scope_(computer_science))*.

Cuando se define una variable, asociándole un valor, esta asociación
tiene una extensión determinada, ya sea en términos de tiempo de
compilación (**ámbito léxico**) como en términos de tiempo de
ejecución (**ámbito dinámico**). El ámbito de una variable determina
cuándo podemos referirnos a ella para recuperar el valor asociado.

Al conjunto de variables disponibles en una parte del programa o en
una parte de su ejecución se denomina **contexto** o **entorno**
(*context* o *environment*).

##### Variables de ámbito global

Una variable definida en el programa con la instrucción `define` tiene
un ámbito global.

```
(define a "hola")
(define b (string-append "adios" a))
(define cuadrado (lambda (x) (* x x)))
```

Todas las variables definidas fuera de funciones forman parte del
**entorno global** del programa.

##### Variables de ámbito local

Como en la mayoría de lenguajes de programación, en Scheme se crea un
**entorno local** cada vez que se invoca a una función. En este
entorno local los argumentos de la función toman los valores de los
parámetros usados en la llamada a la función. Consideramos, por tanto,
estos parámetros como variables de ámbito local de la función.

Podemos usar en un entorno local una variable con el mismo nombre que
en el entorno global. Cuando se ejecute el código de la función se
evaluará la variable de ámbito local.

Por ejemplo, supongamos las expresiones:

```scheme
(define x 5)
(define (suma-3 x)
   (+ x 3))

(suma-3 12)
⇒ 15
```

Cuando se ejecuta la expresión `(+ x 3)` en la invocación a `(suma-3
12)` el valor de `x` es 12, no es 5, devolviéndose 15.

Una vez realizada la invocación, desparece el entorno local y las
variables locales definidas en él, recuperándose el contexto
global. Por ejemplo, en la siguiente expresión, una vez realizada la
invocación a `(suma-3 12)` se devuelve el número 15 y se evalúa en el
entorno global la expresión `(+ 15 x)`. En este contexto la variable
`x` vale 5 por lo que la expresión devuelve 20.

```scheme
(define x 5)
(define (suma-3 x)
   (+ x 3))

(+ (suma-3 12) x)
⇒ 20
```

En el entorno local también se pueden utilizar variables definidas en
el entorno global. Por ejemplo:

```
(define y 12)
(define (suma-3-bis x)
   (+ x y 3))

(suma-3-bis 5) 
⇒ 20
```

La expresión `(+ x 3 y)` se evalúa en el entorno local en el que `x`
vale 5. Al no estar definida la variable `y` en este entorno local, se
usa su definición de ámbito global.

#### <a name="6-2"></a> 6.2 Forma especial let

##### Forma especial let

En Scheme se define la forma especial let que permite crear un entorno
local en el que se da valor a variables y se evalúa una expresión.

Sintaxis:

```scheme
(let ((<var1> <exp-1>)
        ...
      (<varn> <exp-n>))
    <cuerpo>)
```

Las variables `var1`, … `varn` toman los valores devueltos por las
expresiones `exp1`, … `expn` y el cuerpo se evalúa con esos valores.

Por ejemplo:

```scheme
(define x 10)
(define y 20)
(let ((x 1)
      (y 2)
      (z 3))
    (+ x y z))
⇒ 6
```

##### El ámbito de las variables definidas en el let es local

Las variables definidas en el `let` sólo tienen valores en el entorno
creado por la forma especial.

```scheme
(define x 10)
(define y 20)
(let ((x 1)
      (y 2)
      (z 3))
    (+ x y z))
⇒ 6
```

Cuando ha terminado la evaluación del `let` el ámbito local desaparece
y quedan los valores definidos en el ámbito global.

```scheme
x ⇒ 5
y ⇒ error, no definida
```

##### `Let` permite usar variables definidas en un ámbito en el que se ejecuta el `let`

Al igual que en la invocación a funciones, desde el ámbito definido
por el `let` se puede usar las variables del entorno en el que se está
ejecutando el `let`. Por ejemplo, en el siguiente código se usa la
variable z definida en el ámbito global.

```scheme
(define z 8)
(let ((x 1)
      (y 2))
   (+ x y z))
⇒ 11
```

##### Variables en las definiciones del `let`

Las expresiones que dan valor a las variables del `let` se evalúan
todas en el entorno en el que se ejecuta el `let`, antes de crear las
variables locales.  No se realiza una asignación secuencial:

```scheme
(define x 1)
(let ((w (+ x 3))
      (z (+ w 2))) ;; Error: w no está definida
    (+ w z))
```

##### Semántica del `let`

Para evaluar una expresión `let` debemos seguir las siguientes reglas:

1. Evaluar todas las expresiones de la derecha de las variables y
   guardar sus valores en variables auxiliares locales.
2. Definir un ámbito local en el que se ligan las variables del let
   con los valores de las variables auxiliares.
3. Evaluar el cuerpo del let en el ámbito local

##### Let se define utilizando lambda

La semántica anterior queda clara cuando comprobamos que let se puede
definir en función de lambda. En general, la expresión:

```scheme
(let ((<var1> <exp1>) ... (<varn> <expn>)) <cuerpo>)
```

se puede implementar con la siguiente llamada a lambda:

```scheme
((lambda (<var1> ... <varn>) <cuerpo>) <exp1> ... <expn>)
```

Para ejecutar un `let` con un `lambda` se debe crear un procedimiento
en tiempo de ejecución con tantas variables como las variables del
`let`, con el cuerpo del `let`, y se debe invocar a dicho
procedimiento con las expresiones de las variables del `let`. Esas
expresiones se evalúan antes de invocar al procedimiento y la
invocación se realiza con los resultados. La invocación crea un
entorno local con los parámetros del procedimiento (las variables del
`let`) asociados a los valores, y en este ámbito local se ejecuta el
cuerpo del procedimiento (el cuerpo del `let`).

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


##### Let dentro de funciones

Podemos usar `let` en el cuerpo de funciones para crear nuevas
variables locales, además de los parámetros de la función

```scheme
(define (suma-cuadrados x y)
    (let ((cuadrado-x (cuadrado x))
          (cuadrado-y (cuadrado y)))
        (+ cuadrado-x cuadrado-y)))
(suma-cuadrados 4 10)
⇒ 116
```

Cuando se invoca `(suma-cuadrados 4 10)` se crea un entorno local en
el que las variables `x` e `y` toman el valor `4` y `10` y en el que
se ejecuta la forma especial `let`. Esta forma especial crea a su vez
un entorno local en el que se definen las variables `cuadrado-x` y
`cuadrado-y` que toman los valores devueltos por las expresiones
`(cuadrado x)` y `(cuadrado y)`: `16` y `100`. En este entorno local
se evalúa la expresión `(+ cuadrado-x cuadrado-y)`.

El uso de `let` permite aumentar la legibilidad de los programas,
dando nombre a expresiones:

Por ejemplo:

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


#### <a name="6-3"></a> 6.3. Clausuras

Vamos a terminar explicando el concepto de *clausura*. Hemos visto que
las funciones son objetos de primera clase de Scheme y que es posible
crear funciones en tiempo de ejecución con la forma especial `lambda`.

Una clausura es una función devuelta por otra función. La clausura
**captura las variables locales** de la función principal y puede
usarlas en su propio código cuando este se invoque posteriormente.

Veamos un ejemplo. Supongamos que definimos la siguiente función
`(make-sumador k)` que devuelve otra función.

```
(define (make-sumador k)
    (lambda (x) (+ x k)))

(define f (make-sumador 10))
(f 2)
⇒ 12
```

En la función `(make-sumador k)` se llama a la forma especial lambda
para crear un procedimiento. El procedimiento devuelto es lo que se
denomina **clausura**. En este caso la clausura captura la variable
local `k` (el parámetro de `make-sumador`) y usa su valor cuando
posteriormente se invoca. Cuando se invoca a `(f 2)` se ejecuta la
clausura y se crea un nuevo entorno local en el que `x` (el parámetro
de la clausura) vale `2` y en el que se usa la variable `k` capturada.

En la invocación anterior `(f 2)`, cuando se ejecuta la expresión `(+
x k)` las variables tienen los siguientes valores:

```
x: 2 (variable local de la clausura)
k: 10 (valor capturado del entorno local en el que se creó la clausura
```

##### Las variables locales creadas en un `let` también se capturan en las clausuras

Veamos el siguiente ejemplo, en el que creamos una función en un
entorno local creado por un `let`:

```scheme
(define x 10)
(define y 12)
(define (prueba x)
    (let ((y 3))
        (lambda (z) 
            (+ x y z))))
(define h (prueba 5))
(h 2)
⇒ 8
```

Sucede lo siguiente:

1. Se invoca la expresión `(prueba 5)`. Esto crea un entorno local en
   el que se le da a la variable `x` (el parámetro de `prueba`) el
   valor `5`. En este contexto se ejecuta el `let`, que crea otro
   entorno local en el que `y` vale 3. En el contexto del `let` se
   crea una clausura con la invocación de la expresión lambda `(lambda
   (z) (+ x y z))`.
2. La clausura captura las variables locales `x` e `y` con sus valores
   5 y 3 y la función `prueba` la devuelve como resultado de la
   invocación.
3. La clausura se guarda en la variable `h`.
4. Con la invocación `(h 2)` se invoca a la clausura, lo que crea un
   entorno local en el que se encuentran las siguientes variables:

    ```
    z: 2 (parámetro de la clausura)
    x: 5 (variable local de la función prueba capturada en el momento de creación de la clausura))
    y: 3 (variable local del let capturada en el momento de creación de la clausura)
    ```

5. En este contexto se ejecuta la expresión `(+ x y z)`, que devuelve 10.

----

Lenguajes y Paradigmas de Programación, curso 2015-16  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Domingo Gallardo, Cristina Pomares
