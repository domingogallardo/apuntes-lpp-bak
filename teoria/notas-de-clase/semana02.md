
----

### Tema 1: Historia y conceptos de los lenguajes de programación

1. Historia de los lenguajes de programación
2. **Elementos de los lenguajes de programación**
3. **Abstracción**
4. **Paradigmas de programación**
5. **Compiladores e intérpretes**

----

### Definición de la Encyclopedia of Computer Science

A programming language is a set of characters, rules for combining them, and rules specifying their effects when executed by a computer, which have the following four characteristics:

1.	It requires no knowledge of machine code on the part of the user
2.	It has machine independence
3.	Is translated into machine language
4.	Employs a notation that is closer to that of the specific problem being solved than is machine code

----

###  Características de un LP

1. Define un proceso que se ejecuta en un computador
2. Es de alto nivel, cercano a los problemas que se quieren resolver (abstracción)
3. Permite construir nuevas abstracciones que se adapten al dominio que se programa

----

### Elementos de un LP

Para Abelson y Sussman, todos los lenguajes de progamación permiten combinar ideas simples en ideas más complejas mediante los siguientes tres mecanismos

* **Expresiones primitivas** que representan las entidades más simples del lenguaje
* **Mecanismos de combinación** con los que se construyen elementos compuestos a partir de elementos más simples
* **Mecanismos de abstracción** con los que dar nombre a los elementos compuestos y manipularlos como unidades

----

### Sintaxis y semántica

- *Sintaxis*: un conjunto de reglas que definen qué expresiones de texto son correctas. Por ejemplo, en C todas las sentencias deben terminar en ';'
- Los lenguajes de programación se ejecutan en un computador y tienen una determinada semántica que define cuál será el resultado de la ejecución de un programa.

----

### Los lenguajes son para las personas

- Los lenguajes de programación deben ser precisos, deben poder traducirse sin ambigüedad en lenguaje máquina para que sean ejecutados por computadores. Pero deben ser utilizados (leídos, comentados, probados, etc.) por personas.

- La programación es una actividad colaborativa y debe basarse en la comunicación.

----

### 3. Abstracción

- Una misión fundamental de los lenguajes de programación es proporcionar herramientas que sirvan para construir estas abstracciones. Cuando definimos una abstracción le damos un **nombre** a una entidad del lenguaje (una variable, una función, una clase, etc.).
- Escoger un buen nombre para los elementos que vamos construyendo en nuestro programas es fundamental para conseguir un código legible y reutilizable.

----

### Modelar como una actividad fundamental

* Para escribir un programa que preste unos servicios es fundamental modelar el dominio sobre el que va a trabajar
* Es necesario definir distintas abstracciones (tanto APIs, como datos) que nos permitan tratar sus elementos y comunicarnos correctamente con los usuarios que van a utilizar el programa.
* Las abstracciones que vamos construyendo van apoyándose unas en otras y permiten hacer compresible y comunicable un problema complejo
* Ejemplo: el modelado del funcionamiento de una biblioteca contiene abstracciones como "libros", "préstamo", "reserva", o "libros disponibles" que representan conceptos del dominio que deben ser implementados en nuestra solución

<img src="../tema01-historia-lenguajes-programacion/imagenes/casosDeUsoRegistrado.png" width="500px"/>

----

### Abstracciones computacionales

Existen abstracciones propias de la informática (*computer science*), que se utilizan en múltiples dominios. Por ejemplo, abstracciones de datos como:

* Listas
* Árboles
* Grafos
* Tablas hash

También existen abstracciones que nos permiten tratar con dispositivos y ordenadores externos:

* Fichero
* Raster gráfico
* Protocolo TCP/IP

----

### Construcción de abstracciones

- Uno de los trabajos principales de un informático es la construcción de abstracciones que permitan ahorrar tiempo y esfuerzo a la hora de tratar con la complejidad del mundo real.
- Cita de Joel Spolsky en su blog [Joel on Software](http://www.joelonsoftware.com/articles/LeakyAbstractions.html)

> TCP is what computer scientists like to call an abstraction: a simplification of something much more complicated that is going on under the covers. As it turns out, a lot of computer programming consists of building abstractions. What is a string library? It's a way to pretend that computers can manipulate strings just as easily as they can manipulate numbers. What is a file system? It's a way to pretend that a hard drive isn't really a bunch of spinning magnetic platters that can store bits at certain locations, but rather a hierarchical system of folders-within-folders containing individual files that in turn consist of one or more strings of bytes.

----

### Distintos aspectos de los lenguajes de programación

La programación es una disciplina compleja, que tiene que tener en cuenta múltiples aspectos de los lenguajes de programación y las API:

1. Programas como procesos *runtime* que se ejecutan en un computador. Tenemos que entender qué pasa cuando se crea un objeto, cuánto tiempo permanece en memoria, cuál es el ámbito de una variable, etc.

2. Programas como declaraciones estáticas. Hay que considerar un programa desde el punto de vista de una declaración de nuevos tipos, nuevos métodos, tipos genéricos, herencia entre clases, etc.

3. Programas como comunicación y actividad social. Tenemos que tener en cuenta que un programa va a ser usado por otras personas, leído, extendido, mantenido, modificado. Los programas siempre se van a modificar.

----

### 4. Paradigmas de programación

- Un paradigma define un conjunto de características, patrones y estilos de programación basados en alguna idea fundamental. Por ejemplo el paradigma funcional se basa en la idea que una computación se puede especificar como un conjunto de funciones que transforman valores de entrada en valores de salida.
- Es conveniente ver un paradigma como un **estilo de programación** que puede usarse en distintos lenguajes de programación y expresarse con distintas sintaxis. Por ejemplo, se puede escribir código que use programación lógica en Prolog (sería lo más natural), pero también en Java, usando algún API específica.
- Existen lenguajes de programación creados para reforzar el uso de un cierto paradigma. Por ejemplo Lisp, Scheme o Haskell usan el paradigma funcional o Prolog el paradigma lógico.
- Normalmente todos los lenguajes tienen características de más de un paradigma. Por motivos prácticos los lenguajes más populares no se limitan de forma estricta o pura a un único paradigma de programación.
- Existen lenguajes que refuerzan y promueven la expresión de código en más de un paradigma de programación. Y lo hacen no por necesidad o accidente, sino con el intento explícito de fusionar más de un paradigma en una forma única de programar. Estos lenguajes se denominan lenguajes *multi-paradigma*: Scala o Swift.

----

### Paradigmas más importantes

* Paradigma funcional
* Paradigma lógico
* Paradigma imperativo o procedural
* Paradigma orientado a objetos

-----

### Paradigma funcional

* La computación se realiza mediante la evaluación de expresiones
* Definición de funciones
* Funciones como datos primitivos
* Valores sin efectos laterales, no existen referencias a celdas de memoria en las que se guarda un estado modificable
* Programación declarativa (en la programación funcional *pura*)

Lenguajes: Lisp, Scheme, Haskell, Scala, Clojure.

Ejemplo de código (Lisp):

	(define (factorial x)
	   (if (= x 0)
	      1
	      (* x (factorial (- x 1)))))

	(factorial 8)
	40320
	(factorial 30)
	265252859812191058636308480000000

----

#### Paradigma lógico

* Definición de reglas
* Unificación como elemento de computación
* Programación declarativa

Lenguajes: Prolog, Mercury, Oz.

Ejemplo de código (Prolog):

	padrede('juan', 'maria'). % juan es padre de maria
	padrede('pablo', 'juan'). % pablo es padre de juan
	padrede('pablo', 'marcela').
	padrede('carlos', 'debora').

	hijode(A,B) :- padrede(B,A).
	abuelode(A,B) :-  padrede(A,C), padrede(C,B).
	hermanode(A,B) :- padrede(C,A) , padrede(C,B), A \== B.        

	familiarde(A,B) :- padrede(A,B).
	familiarde(A,B) :- hijode(A,B).
	familiarde(A,B) :- hermanode(A,B).

	?- hermanode('juan', 'marcela').
	yes
	?- hermanode('carlos', 'juan').
	no
	?- abuelode('pablo', 'maria').
	yes
	?- abuelode('maria', 'pablo').
	no

----

### Paradigma imperativo

* Definición de procedimientos
* Definición de tipos de datos
* Chequeo de tipos en tiempo de compilación
* Cambio de estado de variables
* Pasos de ejecución de un proceso

Ejemplo (Pascal):

	type
	   tDimension = 1..100;
	   eMatriz(f,c: tDimension) = array [1..f,1..c] of real;

	   tRango = record
	      f,c: tDimension value 1;
	   end;

	   tpMatriz = ^eMatriz;

	procedure EscribirMatriz(var m: tpMatriz);
	var filas,col : integer;
	begin
	   for filas := 1 to m^.f do begin
	      for col := 1 to m^.c do
	         write(m^[filas,col]:7:2);
	      writeln(resultado);
	      writeln(resultado)
	     end;    
	end;

----

### Paradigma orientado a objetos

* Definición de clases y herencia
* Objetos como abstracción de datos y procedimientos
* Polimorfismo y chequeo de tipos en tiempo de ejecución

Ejemplo (Java):

	public class Bicicleta {
	    public int marcha;
	    public int velocidad;

	    public Bicicleta(int velocidadInicial, int marchaInicial) {
	        marcha = marchaInicial;
	        velocidad = velocidadInicial;
	    }

	    public void setMarcha(int nuevoValor) {
	        marcha = nuevoValor;
	    }

	    public void frenar(int decremento) {
	        velocidad -= decremento;
	    }

	    public void acelerar(int incremento) {
	        velocidad += incremento;
	    }
	}

	public class MountainBike extends Bicicleta {
	    public int alturaSillin;

	    public MountainBike(int alturaInicial,
	                        int velocidadInicial,
	                        int marchaInicial) {
	        super(velocidadInicial, marchaInicial);
	        alturaSillin = alturaInicial;
	    }   

	    public void setAltura(int nuevoValor) {
	        alturaSillin = nuevoValor;
	    }   
	}

	public class Excursion {
        public static void main(String[] args) {
           MountainBike miBicicleta = new MoutainBike(10,10,3);
           miBicicleta.acelerar(10);
           miBicicleta.setMarcha(4);
           miBicicleta.frenar(10);
        }
	}

----

### 5. Compiladores e intérpretes

- En el nivel de abstracción más bajo está el ensamblador
- Dependiendo del tipo de lenguaje de programación en el que esté escrito este programa, el código máquina que se estará ejecutando será:

* el resultado de la compilación del programa original (en el caso de un lenguaje compilado)
* el código de un programa (intérprete) que realiza la interpretación del programa original (en el caso de un lenguaje interpretado)

----

### Compilación

La siguiente figura (tomada, como las demás de este apartado del *Programming Language Pragmatics*) muestra el proceso  de generación y ejecución de un programa compilado.

<img src="../tema01-historia-lenguajes-programacion/imagenes/compilacion.png" width="500px"/>

- El proceso de compilación de un programa consiste en la traducción del código fuente original al código máquina específico 
- El código máquina resultante sólo corre en el procesador para el que se ha generado. 

- Ejemplos: C, C++, Swift
- Diferentes momentos en la vida de un programa: tiempo de compilación y tiempo de ejecución
- Mayor eficiencia

----

### Interpretación

<img src="../tema01-historia-lenguajes-programacion/imagenes/interpretacion.png" style=" width:500px"/>

*Interpretación*

- Ejemplos: BASIC, Lisp, Scheme, Python, Ruby
- No hay diferencia entre el tiempo de compilación y el tiempo de ejecución
- Mayor flexibilidad: el código se puede construir y ejecutar "on the fly" (funciones lambda o clousures)

- Los lenguajes interpretados suelen proporcionar un *shell* o intérprete. Se trata de un entorno interactivo en el que podemos definir y evaluar expresiones. - Este entorno se denomina en los círculos de programación funcional un *REPL* (*Read*, *Eval*, *Print*, *Loop*) y ya se utilizó en los primeros años de implementación del Lisp. 
- El uso del *REPL* promueve una programación interactiva en la que continuamente evaluamos y comprobamos el código que desarrollamos.

----

### Enfoques mixtos

- Existen también enfoques mixtos, como el usado por el lenguaje de programación Java, en el que se realizan ambos procesos.
- En una primera fase el compilador de Java (`javac`) realiza una traducción del código fuente original a un _código intermedio_ binario independiente del procesado, denominado _bytecode_. Este código binario es multiplataforma.
- El código intermedio es interpretado después por un el intérprete (`java`) que ya sí que es dependiente de la plataforma. En la figura el intérprete se denomina _Virtual machine_ (no confundir con el concepto de _máquina virtual_ que permite emular un sistema operativo, por ejemplo VirtualBox).

<img src="../tema01-historia-lenguajes-programacion/imagenes/maquina-virtual.png" width="500px"/>

- Ejemplos: Java, Scala

----

## Tema 2: Programación funcional

----

### Contenidos

- 1 El paradigma de Programación Funcional
    - 1.1 Historia y características del paradigma funcional
	- 1.2. Programación declarativa
	- 1.3. Modelo de computación de sustitución
- 2 Scheme como lenguaje de programación funcional
	- 2.1. Funciones y formas especiales
	- 2.2. Formas especiales en Scheme: `define`, `if`, `cond`
	- 2.3. Forma especial `quote` y símbolos
	- 2.4. Listas
	- 2.5. Recursión
- 3- Tipos de datos compuestos en Scheme
- 4- Listas en Scheme
- 5- Funciones como tipos de datos de primera clase
- 6- Ambito de variables, clausuras y `let`

----

### 1.1. Historia y características del paradigma funcional

- La programación funcional define un programa como una función matemática que convierte unas entradas en unas salidas, sin ningún estado interno y ningún efecto lateral.

- Otras características adicionales del paradigma funcional son las siguientes:
    - Uso profuso de la recursión en la definición de las funciones
    - Funciones como tipos de datos primitivos
    - Listas como estructuras de datos fundamentales

----

### Orígenes históricos

- Cálculo lambda de Alonzo Church
- Turing demostró que el cálculo lambda es equivalente a una MT
- El cálculo lambda es un formalismo matemático. Dos décadas después dio origen a algo mucho más tangible y práctico: el Lisp, un lenguaje de alto nivel con el que expresar operaciones y funciones **evaluadas en el computador**.

----

### Historia del Lisp

* [Lisp](http://en.wikipedia.org/wiki/Lisp_(programming_language)) es el primer lenguaje de programación de alto nivel basado en el paradigma funcional.
* Creado en 1958 por John McCarthy.
* Lisp es un lenguaje revolucionario e introduce nuevos conceptos de programación no existentes en la época en la que nace: funciones como objetos primitivos, funciones de orden superior, polimorfismo, listas, recursión, símbolos, homogeneidad de datos y programas, bucle REPL (*Read-Eval-Print Loop*)
* La herencia del Lisp llega a lenguajes derivados de él (Scheme, Golden Common Lisp) y a nuevos lenguajes de paradigmas no estrictamente funcionales, como C#, Python, Ruby, Objective-C o Scala.

----

### Características del Lisp

- Primer lenguaje de programación interpretado, con muchas características dinámicas que se ejecutan en tiempo de ejecución (*run-time*)
    - Gestión de la memoria
    - Detección de excepciones y errores en tiempo de ejecución 
    - Creación en tiempo de ejecución de funciones anónimas (expresiones *lambda*)
- *Sistema de tiempo de ejecución* (*rutime system*) presente en el tiempo de ejecución de los programas
- Otros lenguajes han usado estas características
    - BASIC, Python, Ruby o JavaScript son lenguajes interpretados
    - Lenguajes como Java o C# tienen una avanzada plataforma de tiempo de ejecución con soporte para la gestión de la memoria dinámica (*recolección de basura*, [*garbage collection*](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))) o la [compilación *just in time*](https://en.wikipedia.org/wiki/Just-in-time_compilation).

----

### Programación funcional pura

- Lisp es mucho más que un lenguaje funcional de programación funcional pura.
- Ha sido posible diseñar lenguajes de programación que siguen más estrictamente las características declarativas del paradigma funcional y que también son útiles y prácticos para desarrollar programas en el mundo real, como Haskell o Clojure.

----

### Aplicaciones prácticas de la programación funcional

- En los años 60 la programación funcional (Lisp) fue dominante en departamentos de investigación en Inteligencia Artificial (MIT por ejemplo).
- En los años 70-90 se fue relegando cada vez más.
- En la primera década del 2000 han aparecido lenguajes *multi-paradigma* (muchos de ellos interpretados) como Ruby, Python, Groovy, Objective-C, Lua, Scala o Swift que incluyen el paradigma funcional. También han aparecido y se han hecho populares lenguajes exclusivamente funcionales como Haskell, Clojure o Erlang.
- El auge reciente de estos lenguajes y del paradigma funcional se debe a que facilita:
    - La programación de sistemas concurrentes, con múltiples hilos de ejecución o con múltiples computadores ejecutando procesos conectados concurrentes.
    - La definición y composición de múltiples operaciones sobre *streams* de forma muy concisa y compacta, aplicable a la programación de sistemas distribuidos en Internet.
    - La programación interactiva y evolutiva.

---

### Programación de sistemas concurrentes

- La programación funcional no permite la mutación.
- Paradigma excelente para implementar programas concurrentes
- Como dice [Bartosz Milewski](https://twitter.com/BartoszMilewski), investigador y teórico de ciencia de computación, en su [respuesta en Quora](https://www.quora.com/Why-do-software-engineers-like-functional-programming/answer/Bartosz-Milewski) a la pregunta *¿por qué a los ingenieros de software les gusta la programación funcional?*:

> Porque es la única forma práctica de escribir programas concurrentes. Intentar escribir programas concurrentes en lenguajes imperativos, no sólo es difícil, sino que lleva a *bugs* que son muy difíciles de descubrir, reproducir y arreglar. En los lenguajes imperativos y, en particular, en los lenguajes orientados a objetos se ocultan las mutaciones y se comparten datos sin darse cuenta, por lo que son extremadamente propensos a los errores de concurrencia producidos por las condiciones de carrera.

----

### 1.2. Programación declarativa

----

### Definición

-  La *programación declarativa* es el estilo de programación de lenguajes de programación (o sentencias de código) en los que se *declaran* los valores, objetivos o características finales de los elementos del programa, pero no se especifican detalles de implementación, ni de control de flujo. 
- Estos elementos se resuelven por la componente de run-time del lenguaje.
- Por ejemplo, un conjunto de reglas de Prolog son sentencias declarativas.

----

### No mutación ni pasos de ejecución

- Una característica fundamental del código declarativo es que no utiliza pasos de ejecución.
- Tampoco ni asignación destructiva (en la que se modifica o muta el valor de la variable). 
- Conjunto de reglas y definiciones *de estilo matemático*. 
- En programación funcional se definen funciones que devuelven un valor a partir de unos parámetros de entrada, sin modificar ningún estado del programa ni utilizar pasos de ejecución definidos como tales.

```scheme
(define (cuadrado x)
   (* x x))
```

----

### Programación imperativa

- Pasos de ejecución con asignación
- Estado local mutable en las funciones


----

### Asignación y pasos de ejecución

- Java:

```java
int x = 10;
int x = x + 1;
```

- La expresión `x = x + 1` es una expresión de [asignación](https://en.wikipedia.org/w/index.php?title=Assignment_(computer_science)&redirect=no) 
- Existe una **mutación** del valor de `x`

- En C podemos asignar y modificar un carácter de una cadena
zz
```c
char *miNombre = "Alejandro Perez"
miNombre[3] = 'a'
```

- En POO (Java):

```java
Point2D p1 = new Point2D(3.0, 2.0); // la coord x de p1 es 3.0
p1.setCoordX(10.0);
p1.getCoordX(); // la coord x de p1 es 10.0
```

- La mutación produce efectos laterales:

```java
Point2D p1 = new Point2D(3.0, 2.0); // la coord x de p1 es 3.0
Point2D p2 = p1;
p1.setCoordX(10.0);
p2.getCoordX(); // la coord x de p2 es 10.0, sin que ninguna sentencia haya modificado p2
```

----

### Inmutabilidad en programación funcional

- En programación funcional las definiciones son inmutables
- Ejemplo, la forma especial `define` en Scheme crea un nuevo identificador y le da el valor definido de forma permanente. Si escribimos el siguiente código en un programa en Scheme R6RS:

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

- *Nota*: en el intérprete REPL del DrRacket sí que podemos definir más de una vez la misma función o identificador. Se ha diseñado así para facilitar el uso del intérprete para la prueba de expresiones en Scheme.


- En los lenguajes de programación es habitual mezclar sentencias imperativas y sentencias declarativas:

```
1. int x = 1;
2. x = x+1;
3. int y = x+1;
4. y = x;
```

----

### Estado local

- Otra característica de la programación imperativa es lo que se denomina **estado local mutable** 
- Por ejemplo, en Java, podemos definir un contador que incrementa su valor. Cada llamada al método `valor()` devolverá un valor distinto:

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

- También se pueden definir funciones con estado local mutable en C:

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

- Por el contrario, los lenguajes funcionales puros tienen la propiedad de *transparencia referencial*: si se sustituye una expresión por su valor el resultado final no debe cambiar.
- En programación funcional una función siempre devuelve el mismo valor cuando se le llama con los mismos parámetros

----

### Resumen

**Características de la programación declarativa**

* Variable = nombre dado a un valor (declaración)
* No existe asignación ni cambio de estado
* No existe mutación, se cumple la *transferencia referencial*: dentro de un mismo ámbito todas las ocurrencias de una variable y las llamadas a funciones devuelven el mismo valor

**Características de la programación imperativa**

* Variable = nombre de una zona de memoria
* Asignación
* Referencias
* Pasos de ejecución

----

### Modelo de computación de sustitución

----

### Ejemplo


```scheme
(define (doble x) 
    (+ x x))
(define (cuadrado y) 
    (* y y))
(define (f z) 
    (+ (cuadrado (doble z)) 1))
```

Supongamos que, una vez realizadas esas definiciones, se evalúa la siguiente expresión:

```
(f (+ 2 1))
```

- ¿Cuál será su resultado? Si lo hacemos de forma intuitiva podemos pensar que `37`. 
- Si lo comprobamos en el intérprete de Scheme veremos que devuelve 37. 
- ¿Qué reglas son las que sigue el intérprete? ¿Podríamos implementar nosotros un intérprete similar? Sí, usando las reglas del modelo de sustitución.

----

### 1.3. Modelo de sustitución

El modelo de sustitución define cuatro reglas sencillas para evaluar una expresión. Llamemos a la expresión *e*:

1. Si *e* es un valor primitivo (por ejemplo, un número), devolver ese mismo valor.
2. Si *e* es un identificador, devolver su valor asociado con un `define` (se lanzará un error si no existe ese valor).
3. Si *e* es una expresión del tipo *(f arg1 ... argn)*, donde *f* es el nombre de una función primitiva (`+`, `-`, ...), evaluar uno a uno los argumentos *arg1 ... *argn* y llamar a la función con el resultado.
4. Si *e* es una expresión del tipo *(f arg1 ... argn)*, donde *f* es el nombre de una función definida con un `define`, sustituir *f* por su cuerpo, reemplazando cada parámetro formal del procedimiento por el correspondiente argumento de la llamada. Tenemos en este punto dos versiones del modelo (ambas son equivalentes): podemos evaluar el argumento y después hacer la sustitución o podemos hacer la sustitución con la expresión sin evaluar. En el primer caso estaremos siguiendo un orden normal y en el segundo un orden aplicativo. En cualquier caso, evaluar la expresión resultante.

Scheme utiliza el orden aplicativo.

----

### Orden normal vs. orden aplicativo

- En el orden aplicativo se realizan las evaluaciones de *dentro a fuera* de los paréntesis. 
- En el orden normal se realizan todas las sustituciones hasta que se tiene una larga expresión formada por expresiones primitivas; se evalúa entonces.

- ¿Cuál sería el resultado de evaluar `(f (+ 2 1))` con orden aplicativo?:

- ¿Y en orden normal?

----

#### Orden aplicativo y normal son equivalentes

- En programación funcional el resultado de evaluar una expresión es el mismo independientemente del tipo de orden. 
- Si estamos fuera del paradigma funcional y las funciones tienen estado y cambian de valor entre distintas invocaciones sí que importan si escogemos un orden.

- Por ejemplo, supongamos una función `(random x)` que devuelve un entero aleatorio entre 0 y *x*. Esta función no cumpliría el paradigma funcional, porque devuelve un valor distinto con el mismo parámetro de entrada.

Evaluamos las siguientes expresiones con orden aplicativo y normal, para comprobar que el resultado es distinto

```scheme
(define (zero x) (- x x))
(zero (random 10))
```

----

### 2. Scheme como lenguaje de programación funcional

----

Vamos ver ejemplos concretos de características de un lenguaje de programación funcional estudiando las características funcionales de Scheme.

En concreto, veremos:

- Definición de funciones y otras primitivas de programación funcional en Scheme
- Símbolos y primitiva `quote`
- Definición de funciones recursivas en Scheme

----

### 2.1. Funciones y formas especiales

- Podemos clasificar las primitivas en **funciones** y **formas especiales**. Las funciones se evalúan usando el modelo de sustitución aplicativo ya visto:
    - Primero se evalúan los argumentos y después se sustituye la llamada a la función por su cuerpo y se vuelve a evaluar la expresión resultante.
    - Las expresiones siempre se evalúan desde los paréntesis interiores a los exteriores.

- Las *formas especiales* son expresiones primitivas de Scheme que tienen una forma de evaluarse propia, distinta de las funciones.

----

### Forma especial `define`

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

----

### Forma especial `define` para definir funciones

**Sintaxis**

```
(define (<nombre-funcion> <argumentos>)
	<cuerpo>)
```

**Evaluación**

La semana que viene veremos con más detalle la semántica, y explicaremos la forma especial `lambda` que es la que realmente crea la función. Hoy nos quedamos en la siguiente descripción de alto nivel de la semántica:

1. Crear la función con el *cuerpo*
2. Dar a la función el nombre *nombre-función*

**Ejemplo**

```scheme
(define (factorial x)
    (if (= x 0)
        1
		(* x (factorial (- x 1)))))
```

----

### Forma especial `if`

**Sintaxis**

```scheme
(if <condición> <expresión-true> <expresión-false>)
```

**Evaluación**

1. Evaluar *<condición>*
2. Si el resultado es `#t` evaluar la *<expresión-true>*, en otro caso, evaluar la *<expresión-false>*

**Ejemplo**

```scheme
(if (> 10 5) (substring "Hola qué tal" (+ 1 1) 4) (/ 12 0))
```

----

### Forma especial `cond`

**Sintaxis**

```scheme
(cond 
	(<exp-cond-1> <exp-consec-1>)
	(<exp-cond-2> <exp-consec-2>)
	...
	(else <exp-consec-else>))
```

**Evaluación**

1. Se evalúan de forma ordenada todas las expresiones hasta que una de ellas devuelva `#t`
2. Si alguna expresión devuelve `#t`, se devuelve el valor del consecuente de esa expresión
3. Si ninguna expresión es cierta, se devuelve el valor resultante de evaluar el consecuente del `else`


**Ejemplo**

```scheme
(cond
   ((> 3 4) '3-es-mayor-que-4)
   ((< 2 1) '2-es-menor-que-1)
   ((= 3 1) '3-es-igual-que-1)
   ((> 3 5) '3-es-mayor-que-2)
   (else 'ninguna-condicion-es-cierta))
```

----

### 2.3. Forma especial `quote`

**Sintaxis**

```scheme
(quote <identificador>)
(quote <expresion>)
```

**Evaluación**

- Se devuelve el identificador o la expresión **sin evaluar**. La expresión puede ser cualquier expresión correcta de Scheme, datos atómicos, parejas o listas. Se abrevia en con el carácter `'`

**Ejemplo**

```scheme
(quote x)
'hola
'+
'2.3
'2/3
'(2 . 3)
'(+ 1 2 3 4)
(quote (1 2 3 4))
'(* (+ 1 (+ 2 3)) 5)
```

----

### Identificadores (símbolos)

- A diferencia de los lenguajes imperativos, Scheme trata a los *identificadores* (nombres que se les da a las variables) como datos del lenguaje de tipo **symbol**. En el paradigma funcional a los identificadores se les denomina *símbolos*.
- Los símbolos son distintos de las cadenas. Una cadena es un tipo de dato compuesto, mientras que los símbolos se almacenan con un valor único denominado *valor hash*.

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

----

### Evaluación de símbolos

- Un símbolo es un identificador que puede asociarse o ligarse (*bind*) a un valor (cualquier dato *de primera clase*).
- Cuando escribimos un símbolo en el prompt de Scheme el intérprete lo evalúa y devuelve su valor:

```scheme
(define pi 3.14159)
pi
⇒3.14159
```

- Los nombres de las funciones (`equal?, `sin, `+, ...) son también símbolos (los de las macros no) y Scheme también los evalúa (en un par de semanas hablaremos de las funciones como objetos primitivos en Scheme):

```scheme
sin
⇒ #<procedure:sin>
+
⇒ #<procedure:+>
(define (cuadrado x) (* x x))
⇒ #<procedure:cuadrado>
```


- Los símbolos son tipos primitivos del lenguaje: pueden pasarse como parámetros o ligarse a variables.

```scheme
(define x 'hola)
x
⇒ hola
```

### 2.4. Listas

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

	* Función `cons` para crear una lista nueva resultado de añadir un elemento al comienzo de la lista. Esta función es la forma habitual de construir nuevas listas a partir de una lista ya existente y un nuevo elemento.
   
	   ```scheme
	   (cons 1 '(1 2 3 4)) ;⇒ {1 1 2 3 4}
	   (cons 'hola '(como estás)) ;⇒ {hola como estás}
	   (cons '(1 2) '(1 2 3 4)) ; ⇒ {{1 2} 1 2 3 4}
	   ```

	* Función `append` para crear una lista nueva resultado de concatenar dos o más listas
   
	   ```scheme
	   (append list2 list2 list3)
	   ```

* Diferencia entre creación de listas con la función `list` y con la forma especial `quote`:

    * La evaluación de la función `list` funciona como cualquier función, primero se evalúan los argumentos y después se invoca a la función con los argumentos evaluados. Por ejemplo, en la siguiente invocación se obtiene una lista con cuatro elementos resultantes de las invocaciones de las funciones dentro del paréntesis:

    ```scheme
    (list 1 (/ 2 3) (+ 2 3) (cons 3 4)) ; ⇒ {1 2/3 5 {3 . 4}}
    ```

    * Sin embargo, usamos `quote` obtenemos una lista con sublistas con símbolos en sus primeras posiciones:

    ```scheme
    '(1 (/ 2 3) (+ 2 3) (cons 3 4)) ; ⇒ {1 {/ 2 3} {+ 2 3} {cons 3 4}}
    ```

----

### 2.5. Recursión

- Otra característica fundamental de la programación funcional es la no existencia de bucles. Un bucle implica la utilización de pasos de ejecución en el programa y esto es característico de la programación imperativa.
- Las iteraciones se realizan con recursión.
- **Importante**: Para diseñar y entender bien la recursión:
    - Hay que mirar la definición de la función recursiva *de forma declarativa*, como una definición matemática, entendiendo lo que hace la llamada recursiva y *confiando en que devuelve lo que tiene que devolver*. 
   - No es conveniente *entrar en la recursión* e intentar comprobar su funcionamiento haciendo una traza de las sucesivas llamadas, sino suponer que la llamada recursiva se ejecuta, devuelve el valor que debería (*confiamos en la recursión*) y calcular el valor devuelto como el resultado de la operación una vez devuelta la llamada recursiva.

----

###  Función `suma-hasta`

Función `(suma-hasta x)` que suma los números 0+1+2+...+x.

Por ejemplo, la suma hasta 8 es:

`0+1+2+3+4+5+6+7+8` 

Podemos expresar esta expresión de forma recursiva como `(suma-hasta 7) + 8`

Definición matemática, generalizando la expresión anterior:

> Para sumar hasta *x* debemos sumar a *x* el resultado de sumar hasta *x-1*.

Y escribiéndolo en Scheme:

```scheme
(define (suma-hasta x)
   (if (= 0 x)
      0
      (+ x (suma-hasta (- x 1)))))
```

----

### Función `longitud`

Podemos calcular la longitud de una lista con la siguiente expresión recursiva:

> La longitud de una lista *l* es 1 + la longitud del resto de *l* 

En Scheme, la función `(longitud lista)` que calcula la longitud de una lista:

```scheme
(define (longitud lista)
   (if (null? lista)
      0
      (+ 1 (longitud (cdr lista)))))
```

----

### Función `suma-lista`

La suma de todos los números de una lista de enteros se puede calcular con la siguiente expresión recursiva:

> La suma de los números de una lista es el primer elemento + la suma de los elementos del resto.

En Scheme:

```
(define (suma-lista lista)
   (if (null? lista)
       0
	   (+ (car lista) (suma-lista (cdr lista)))))
```

----

###  Función recursiva `veces`

Vamos a definir la función `(veces lista id)` que cuenta el número de veces que aparece un identificador en una lista. La definimos recursivamente de la siguiente forma:

> Para calcular el número de veces que aparece un identificador en una lista, cuento el número de veces que aparece en el resto de la lista y le sumo 1 si el primer elemento de la lista coincide con el identificador.

En Scheme:

```scheme
(define (veces lista id)
  (cond
    ((null? lista) 0)
    ((equal? (car lista) id) (+ 1 (veces (cdr lista) id)))
    (else (veces (cdr lista) id))))

(veces '(a b a a b b) 'a) 
⇒ 3 
```

----
