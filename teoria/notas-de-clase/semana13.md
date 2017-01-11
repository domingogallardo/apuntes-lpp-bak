
## Tema 5: Programación Funcional con Swift

Notas de clase de la semana 13  

### Contenidos

- 1 Introducción
- 2 Funciones
- 3 Tipos función
- 4 Recursión
- 5 Tipos
- 6 Opcionales
- **7 Inmutabilidad**
- **8 Clausuras**
- **9 Funciones de orden superior**
- **10 Genéricos**

----

### 7. Inmutabilidad

Una de las características funcionales importantes de Swift es el
énfasis en la inmutabilidad para reforzar la seguridad del
lenguaje. Veamos algunas características relacionadas con esto.

#### Palabra clave let

La palabra clave `let` permite definir constantes. El valor asignado
puede no conocerse en tiempo de compilación:

```swift
let maximoNumeroDeIntentosDeLogin = 10
let respuesta: String = respuestaUsuario.respuesta()
```

#### Semántica de copia en estructuras

Una forma de evitar los efectos laterales es definir una semántica de
copia en la asignación. En Swift la semántica de una asignación
depende del tipo de objeto. Las estructuras (_structs_) tienen
semántica de copia y las clases tienen una semántica de referencia.

El la
[biblioteca estándar de Swift](https://developer.apple.com/library/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html#//apple_ref/doc/uid/TP40014608)
la mayor parte de los tipos definidos son estructuras. Los tipos
básicos de Swift como `Int`, `Double`, `Bool`, `String`, etc. son
todos ellos estructuras y, por tanto, tienen semántica de copia.

```swift
var str1 = "Hola"
var str2 = str1
str1.appendContentsOf("Adios")
print(str1) // Imprime "HolaAdios"
print(str2) // Imprime "Hola"
```

Los arrays también son estructuras y, por tanto, también tienen
semántica de copia:

```swift
var array1 = [1, 2, 3, 4]
var array2 = array1
array1[0] = 10
print(array1) // [10, 2, 3, 4]
print(array2) // [1, 2, 3, 4]
```

A pesar de tener una semántica de copia, la asignación de un array de
una variable a otra no realiza una copia de todo el array. El
compilador de Swift optimiza estas sentencias y sólo realiza la copia
en el momento en que hay una modificación de una de las variables que
comparten el array.

#### Estructuras mutables y `let`

Si definimos un valor de una estructura con un `let` ese valor será
inmutable y no podrá modificarse, a pesar de que el `Struct` tenga
métodos que mutan sus valores.

Por ejemplo, hemos visto que el método `appendContentsOf(_:)` de un
`String` modifica la propia cadena. Si definimos una cadena con `let`
no podremos modificarla:

```swift
var cadenaMutable = "Hola"
let cadenaInmutable = "Adios"
cadenaMutable.appendContentsOf(cadenaInmutable) // cadenaMutable es "HolaAdios"
// cadenaInmutable.appendContentsOf("Adios")
// La sentencia anterior genera un error: 
// "cannot use mutating member on immutable value: 'cadenaInmutable' is a 'let' constant"
```

#### Tipos valor y tipos referencia

Un _tipo valor_ es un tipo que tiene semántica de copia en las
asignaciones y cuando se pasan como parámetro en llamadas a funciones.

Los tipos valor son muy útiles porque evitan los efectos laterales en
los programas y simplifican el comportamiento del compilador en la
gestión de memoria. Al no existir referencias, se simplifica
enormemente la gestión de memoria de estas estructuras. No es
necesario llevar la cuenta de qué referencias apuntan a un determinado
valor, sino que se puede liberar la memoria en cuanto se elimina el
ámbito actual.

Frente a un tipo valor, un tipo de referencia es aquel en los que los
valores se asignan a variables con una semántica de referencia. Cuando
se realizan varias asignaciones de una misma instancia a distintas
variables todas ellas guardan una referencia a la misma instancia. Si
la instancia se modifica, todas las variables reflejarán el nuevo
valor. En Swift todas las instancias de clases tienen esta semántica.

En Swift las estructuras son tipos valor y las clases tipos de
referencia. Comentaremos más diferencias en el tema de programación
orientada a objetos.

### 8. Expresiones de clausuras

Ya hemos visto previamente que en Swift las funciones son objetos de
primera clase del lenguaje y que es posible definir funciones y
pasarlas como parámetro de otras funciones.

Swift permite definir expresiones compactas con las que construir
estas funciones que se pasan como parámetro de otras funciones. Se
denominan _expresiones de clausuras_ (_closure expressions_). Estas
expresiones proporcionan optimizaciones de sintaxis para escribir
clausuras de forma concisa y clara. Vamos a ver las distintas
optimizaciones utilizando un único ejemplo del método `sort(_:)`.

#### El método Sort

La biblioteca estándar de Swift proporciona un método llamado
`sort(_:)`, que ordena un array de un tipo conocido, basado en la
salida de la clausura que se le proporciona. Es una de las distintas
funciones de orden superior que se definen en las colecciones (más
adelante veremos otras). Una llamada al método devuelve un nuevo array
con la ordenación indicada. El array original no se modifica.

En el ejemplo vamos a ordenar un array de cadenas en orden alfabético
inverso. Este es el array inicial a ser ordenado:

```swift
let nombres = ["Cristina", "Alex", "Eva", "Boyan", "Daniel"]
```

El método `sort(_:)` acepta una clausura que toma dos argumentos del
mismo tipo que los del array y devuelve un valor `Bool` indicando si
el primer valor debería aparecer antes o después del segundo valor una
vez que los valores están ordenados. La clausura de ordenación
devuelve `true` si el primer valor debería aparecer antes del segundo
valor y `false` en otro caso.

En este ejemplo ordenamos un array de valores `String`, por lo que la
clausura de ordenación tiene que ser una función del tipo `(String,
String) -> Bool`.

Una forma de proporcionar la clausura de ordenación es escribir una
función normal del tipo correcto y pasarla como un argumento al método
`sort(_:)`:


```swift
func haciaAtras(s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var alreves = nombres.sort(haciaAtras)
// alreves es igual a ["Eva", "Daniel", "Cristina", "Boyan", "Alex"]
```

Si la primera cadena (`s1`) es mayor que la segunda cadena (`s2`), la
función `haciaAtras(_:_:)` devolverá `true`, indicando que `s1`
debería aparecer antes que `s2` en el array ordenado. La ordenación
mayor o menor se refiere a la ordenación alfabética, al estar tratando
con caracteres.

La versión anterior esta es una forma bastante complicada de escribir
lo que básicamente es una función de una única expresión (`a > b`). En
este ejemplo, sería preferible escribir la clausura de ordenación
_inline_, utilizando la sintaxis de expresiones de clausuras.

#### Sintaxis de las expresiones de clausura

La sintaxis de las expresiones de clausura tiene la siguiente forma
general:

```text
{ ( <parametros>) -> <tipo devuelto> in
   <sentencias>
}
```

Si aplicamos esta sintaxis al ejemplo anterior:

```swift
alreves = nombres.sort({ (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

Hay que hacer notar que la declaración de los parámetros y el tipo
devuelto por esta clausura _inline_ es idéntica a la declaración de la
función `haciaAtras(_:_:)`. En ambos casos, se escribe como `(s1:
String, s2: String) -> Bool`. Sin embargo, en la expresión de clausura
los parámetros y el tipo devuelto se escribe dentro de las llaves, no
fuera.

El comienzo del cuerpo de la clausura se introduce por la palabra
clave `in`. Esta palabra clave indica que la definición de los
parámetros y del tipo devuelto por la clausura ha terminado, y que el
cuerpo de la clausura va a comenzar.

Como el cuerpo de la clausura es corto, podemos incluso escribirlo en
una única línea:

```swift
alreves = nombres.sort( { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

#### Inferencia del tipo por el contexto

Como la clausura de ordenación se pasa como argumento de un método,
Swift puede inferir los tipos de sus parámetros y el tipo del valor
que devuelve. El método `sort(_:)` se llama sobre un array de cadenas,
por lo que su argumento debe ser una función del tipo `(String,
String) -> Bool`. Esto significa que los tipos `(String, String)` y
`Bool` no necesitan escribirse como parte de la definición de la
expresión de la clausura. Debido a que todos los tipos pueden ser
inferidos, la flecha del tipo devuelto y los paréntesis alrededor de
los nombres de los parámetros también pueden omitirse:

```swift
alreves = nombres.sort( { s1, s2 in return s1 > s2 } )
```

#### Devoluciones implícitas en clausuras con una única expresión

En clausuras con una única expresión podemos omitir también la palabra
clave `return`:

```swift
alreves = nombres.sort( { s1, s2 in s1 > s2 } )
```

#### Abreviaturas en los nombres de los argumentos

Swift proporciona automáticamente abreviaturas para los nombres de
argumentos de las clausuras _inline_ que pueden usarse para referirse
a los valores de los argumentos de la clausura usando los nombres
`$0`, `$1`, `$2`, etc.

Si se usa estos argumentos abreviados, se puede omitir la definición
de la lista de los argumentos:

```swift
alreves = nombres.sort( { $0 > $1 } )
```

#### Funciones operadoras

Incluso hay una forma aun más corta de escribir la expresión de
clausura anterior.  Swift define una implementación específica de
cadenas del operador mayor-que (`>`) como una función que tiene dos
parámetros de tipo `String` y devuelve un `Bool`. Esto es exactamente
lo que necesita el método `sort(_:)`. Podemos, por tanto, pasar
simplemente este operador mayor-que, y Swift inferirá que queremos
usar el específico de cadenas:

```swift
alreves = nombres.sort(>)
```

#### Clausuras al final

Si necesitamos pasar una expresión de clausura a una función como el
argumento final de la clausura y la expresión es larga, puede ser útil
escribirla en su lugar como una clausura al final (_trailing
closure_). Una clausura al final es una expresión de clausura que se
escribe fuera de (y después de) los paréntesis de la función a la que
se le pasa como parámetro:

```swift
alreves = nombres.sort() { $0 > $1 }
```

Si se proporciona una expresión de clausura como único argumento de
una función o método y se pasa como una clausura al final, no es
necesario escribir los paréntesis tras el nombre de la función:

```swift
alreves = nombres.sort { $0 > $1 }
```

Las clausuras al final son útiles sobre todo cuando la clausura es
suficientemente larga que no es posible escribirla _inline_ en una
única línea. Como ejemplo, el tipo `Array` de Swift tiene el método
`map(_:)` que toma una expresión de clausura como único argumento (en
el siguiente apartado hablaremos de esta y otras funciones de orden
superior). La clausura se llama una vez para cada elemento del array y
devuelve un valor transformado (posiblemente de otro tipo) para ese
elemento.

Después de aplicar la clausura proporcionada a cada elemento del
array, el método `map(_:)` devuelve una array nuevo que contiene todos
los nuevos valores transformados, en el mismo orden que sus valores
correspondientes en el array original.

Por ejemplo, podemos usar el método `map(_:)` con una clausura al
final para convertir un array de valores `Int` en un array de valores
`String`. El array `[16, 58, 510]` se usa para crear el array
["UnoSeis", "CincoOcho", "CincoUnoCero"]:

```swift
let digitos = [
    0: "Cero", 1: "Uno", 2: "Dos",   3: "Tres", 4: "Cuatro",
    5: "Cinco", 6: "Seis", 7: "Siete", 8: "Ocho", 9: "Nueve"
]
let numeros = [16, 58, 510]
```

El código de arriba crea un diccionario que relaciona los dígitos
enteros con sus nombres en castellano y un array de enteros que se
convertirán en cadenas.

Ahora podemos usar el array de nombres para crear un array de valores
`String`, pasando una expresión de clausura al método `map(_:)` del
array como una clausura al final:

```swift
let cadenas = numeros.map {
    (numero) -> String in
    var numero = numero
    var salida = ""
    while numero > 0 {
        salida = digitos[numero % 10]! + salida
        numero /= 10
    }
    return salida
}
// las cadenas se infieren de tipo [String]
// su valor es ["UnoSeis", "CincoOcho", "CincoUnoCero"]
```

El método `map(_:)` llama a la expresión de clausura una vez por cada
elemento del array. La variable `numero` se incializa con el valor
`numero` del parámetro de la clausura para poder modificarlo dentro
del cuerpo de la clausura (los parámetros de las funciones y las
clausuras son siempre constanes). El bucle `while` usa el diccionario
de dígitos para construir la cadena correspondiente al valor `Int`.


#### Valores capturados

Una clausura puede capturar constantes y variables del contexto en el
que se define. La clausura puede referirse y modificar esos valores
dentro de su cuerpo, incluso si ya no existe el ámbito (_scope_)
original en el que se definieron estas constantes y variables.

En Swift, la forma más sencilla de una clausura que captura valores es
una función anidada (_nested function_) escrita en el cuerpo de otra
función. Una función anidada puede capturar cualquiera de los
argumentos de su función exterior y también puede capturar cualquier
constante y variable definida dentro de la función exterior.

Veamos un ejemplo similar al que vimos en Scheme. La función
`construyeIncrementador` contiene una función anidada llamada
`incrementador`. Esta función captura dos valores de su contexto:
`totalAcumulado` y `cantidad`. Después de capturar estos valores,
`incrementador` es devuelto por `construyeIncrementador` como una
clausura que incrementa `totalAcumulado` en `cantidad` cada vez que se
llama.

```swift
func construyeIncrementador(incremento cantidad: Int) -> () -> Int {
    var totalAcumulado = 0
    func incrementador() -> Int {
        totalAcumulado += cantidad
        return totalAcumulado
    }
    return incrementador
}
```

El tipo devuelto de `construyeIncrementador` es `() -> Int`. Esto
significa que devuelve una función que no tiene parámetros y que
devuelve un `Int` cada vez que es llamada.

La función `construyeIncrementador(incremento:)` tiene un único
parámetro `Int` con nombre externo `incremento` y nombre local
`cantidad`. El argumento pasado a este parámetro especifica cuánto
será incrementado `totalAcumulado` cada vez que se llama a la función
`incrementador` devuelta. La función `construyeIncrementador` define
una función anidada llamada `incrementador`, que realiza el incremento
real. Esta función simplemente añade `cantidad` a `totalAcumulado`, y
devuelve el resultado.

Si la consideramos aislada, la función anidada `incrementador()`
podría parecer extraña:

```swift
func incrementador() -> Int {
    totalAcumulado += cantidad
    return totalAcumulado
}
```

La función no tiene ningún parámetro, y sin embargo se refiere a
`totalAcumulado` y a `cantidad` en su cuerpo. Lo puede hacer porque ha
capturado una referencia a estas variables de la función de alrededor
y las usa en su propio cuerpo. Al capturar estas referencias las
variables `totalAcumulado` y `cantidad` no desaparecen cuando termina
la llamada a `construyeIncrementador`. Estas variables también estarán
disponibles la próxima vez que se llame la función `incrementador`.

Aquí hay un ejemplo de `construyeIncrementador` en acción:

```swift
let incrementaDiez = construyeIncrementador(incremento: 10)
```

Este ejemplo define una constante llamada `incrementaDiez` para
referenciar la función `incrementador` que devuelve
`construyeIncrementador`. Esta función añade 10 a la variable
`totalAcumulado` cada vez que se es llamada. Si llamamos a la función
más de una vez podemos comprobar su conducta en acción:

```swift
incrementaDiez()
// devuelve 10
incrementaDiez()
// devuelve 20
incrementaDiez()
// devuelve 30
```

Si creamos un segundo incrementador, tendrá sus propias referencias a
un variable `totalAcumulado` nueva, distinta de la anterior:


```swift
let incrementaSiete = construyeIncrementador(incremento: 7)
incrementaSiete()
// devuelve 7
```

Si llamamos a la función `incrementador` original (`incrementaDiez`)
vemos que sigue incrementando su propia variable `totalAcumulado` y
que no se ve afectada por la variable capturada por `incrementaSiete`:

```swift
incrementaDiez()
// devuelve 40
```

#### Las clausuras son tipos de referencia

En el ejemplo anterior, `incrementaSiete` e `incrementaDiez` son
constantes, pero las clausuras a las que estas constantes se refieren
pueden incrementar la variable `totalAcumulado` que han
capturado. Esto es porque funciones y clausuras son tipos referencia.

Siempre que asignamos una función o una clausura a una constante o una
variable, estamos realmente estableciendo que la constante o variable
es una referencia a la función o la clausura. En el ejemplo anterior,
es la elección de la clausura a la que referencia `incrementaDiez` la
que es constante, no los contenidos propios de la clausura.

Esto también significa que si asignamos una clausura a dos constantes
o variables distintas, ambas constantes o variables se referirán a la
misma clausura:

```swift
let tambienIncrementaDiez = incrementaDiez
tambienIncrementaDiez()
// devuelve 50
```

### 9. Funciones de orden superior

Una de las características funcionales que más hemos usado para
trabajar con listas en Scheme son las funciones de orden superior como
`map`, `filter` o `fold-right`. Swift tiene definidas funciones
equivalentes para trabajar con colecciones. Se denominan `map`,
`filter` y `reduce`. Todas ellas aceptan expresiones de clausura como
argumento.

#### Map

El método `map` se define en el protocolo
[`CollectionType`](https://developer.apple.com/library/ios/documentation/Swift/Reference/Swift_CollectionType_Protocol/index.html#//apple_ref/swift/intfm/CollectionType/s:FEsPs14CollectionType3mapurFzFzWx9Generator7Element_qd__GSaqd___)
y es adoptado por múltiples estructuras como `Array`, `Dictionary`,
`Set` o `String.CharacterView`.

El método `map` recibe como parámetro una función unaria `transform`
del tipo de los elementos de la colección y que devuelve otro elemento
(puede ser del mismo o de distinto tipo que los elementos de la
colección). Devuelve un array que contiene el resultado de aplicar
`transform` a cada elemento del array original.

Por ejemplo:

```swift
let numeros = [Int](0...5)
numeros.map {$0 * $0}
// devuelve [0, 1, 4, 9, 16, 25]
```

Otro ejemplo, en el que usamos `map` para implementar la función
`sumaParejas(parejas: [(Int, Int)]) -> [Int]` que devuelve recibe el
array `parejas` de tuplas de dos enteros y devuelve un array con el
resultado de sumar los dos elementos de cada pareja:

```swift
func sumaParejas(parejas: [(Int, Int)]) -> [Int] {
   return parejas.map({(pareja: (Int, Int)) -> Int in
                        return pareja.0 + pareja.1})
}
sumaParejas([(1, 1), (2, 2), (3, 3), (4, 4)])
// devuelve [2, 4, 6, 8]
```

Podemos usar en el cuerpo de la expresión de clausura de `map` una
variable capturada. Por ejemplo en la siguiente función
`incrementaValores(_:con:)` que suma `con` a todos los números de un
array que se le pasa por parámetro:

```swift
func incrementaValores(array: [Int], con: Int) -> [Int] {
   return array.map({(x: Int) -> Int in
                        return x + con})
}
incrementaValores([10, 20, 30], con: 5)
// devuelve [15, 25, 35]
```
La versión abreviada de la expresión de clausura es:

```swift
func incrementaValores(array: [Int], con inc: Int) -> [Int] {
   return array.map {$0 + inc}
}
incrementaValores([10, 20, 30], con: 5)
// devuelve [15, 25, 35]
```


#### Filter

La función `filter` 

```swift
let numeros = [Int](0...10)
numeros.filter {$0 % 2 == 0}
```


#### Reduce 

Similar al _fold_ de Scheme:

```swift
let numeros = [Int](0...10)
numeros.reduce(0, combine: +)
```

La función combina los elementos de la colección usando la función de
combinación que se pasa como parámetro. La función `combine` recibe
dos parámetros: el primero es el resultado de la combinación y el
segundo se coge de la colección. Por ejemplo:


```swift
let cadenas = ["Patatas", "Arroz", "Huevos"]
cadenas.reduce(0, combine: {(i: Int, c: String) -> Int in
                              c.characters.count + i })
// devuelve 18
```

La combinación se hace de izquierda a derecha:

```swift
let cadenas = ["Patatas", "Arroz", "Huevos"]
cadenas.reduce("", combine: +)
// devuelve "PatatasArrozHuevos"
```

### 10. Genéricos

Empecemos con un ejemplo sencillo. Supongamos la siguiente función
`intercambia(_:)` que recibe una tupla `(Int, String)` y devuelve una
tupla `(String, Int)` con los valores intercambiados.

```swift
func intercambia(tupla: (Int, String)) -> (String, Int) {
   let tuplaNueva = (tupla.1, tupla.0)
   return tuplaNueva
}

let tupla = (10, "Hola")
intercambia(tupla)
// devuelve ("Hola", 10)
```

La función es interesante, pero sólo recibe tuplas cuya primera
componente es un `Int` y su segunda componente es un
`String`. Supongamos que queremos hacer la misma función para
intercambiar elementos de una tupla `(Int, Int)`. Tendríamos que usar
el mismo código, pero cambiando los tipos:

```swift
func intercambia(tupla: (Int, Int)) -> (Int, Int) {
   let tuplaNueva = (tupla.1, tupla.0)
   return tuplaNueva
}

let tupla = (10, 20)
intercambia(tupla)
// devuelve (20, 10)
```

El código es el mismo, lo único distinto son los tipos. ¿Podríamos
**generalizar** las funciones anteriores para hacer que el código
pueda trabajar con cualquier tipo? La respuesta es sí, usando
**función genérica**:

```swift
func intercambia<A,B>(tupla: (A, B)) -> (B, A) {
   let tuplaNueva = (tupla.1, tupla.0)
   return tuplaNueva
}
```

El cuerpo de la función es idéntico a la función anterior. La
diferencia es que en la versión genérica se usan *placeholders* (los
símbolos `A` y `B`) en lugar de tipos concretos. Son tipos genéricos,
que se definen usando un identificador entre símbolos de `<` y
`>`. Los tipos reales que se van a usar en la función se determinan en
cada invocación a la función, dependiendo del tipo del parámetro que
se utiliza en la llamada:

```swift
let tupla = (10, "Hola")
intercambia(tupla)
// devuelve ("Hola", 10)
let tupla2 = (10, 20)
intercambia(tupla2)
// devuelve (10, 20)
let tupla3 = (true, 10.5)
intercambia(tupla3)
// devuelve (10.5, true)
```

En el primer ejemplo, los tipos `A` y `B` se infieren como `Int` y
`String`. En el segundo ejemplo como `Int` e `Int`. Y en el tercero
como `Bool` y `Double`.

Otro ejemplo. La siguiente función `miFilter` utiliza el tipo genérico
`A` como el tipo de los elementos del `ArraySlice` y el tipo del
predicado que se aplica a los elementos. El compilador determina el
tipo `A` cuando se invoca a la función con `ArraySlice` y un predicado
concreto.

```swift
func miFilter<A>(array: ArraySlice<A>, func f: (A) -> Bool) -> [A] {
   if array.isEmpty {
      return []
   } else {
      let primero = array[array.startIndex]
      let resto = array[array.startIndex+1..<array.endIndex]
      if f(primero) {
         return [primero] + miFilter(resto, func: f)
      } else {
         return miFilter(resto, func: f)
      }
   }
}

miFilter([1, 2, 3, 4, 5, 6], func: {$0 % 2 == 0})
// Devuelve [2, 4, 6]
```

Los tipos genéricos se pueden usar en la definición de todos los
elementos de Swift: funciones, enums, estructuras, clases, protocolos
o extensiones. Terminamos con un ejemplo en el que incluimos muchos
conceptos vistos en este tema. Se trata de la implementación en Swift
de listas al estilo Scheme, con las funciones `car`, `cdr` y `vacia`
usando un enum recursivo con un tipo genérico que permite generalizar
el tipo de elementos de la lista.

```swift
indirect enum Lista<T> {
     case Vacia
     case Cons(T, Lista<T>)
}

func car<T>(lista: Lista<T>?) -> T? {
   if let l = lista {
      switch l {
         case let .Cons(primero, _):
            return primero
         case .Vacia:
            return nil
      }
   } else {
      return nil
   }
}

func cdr<T>(lista: Lista<T>?) -> Lista<T>? {
   if let l = lista {
      switch l {
         case let .Cons(_, resto):
            return resto
         case .Vacia:
            return nil
      }
   } else {
      return nil
   }
}

func vacia<T>(lista: Lista<T>?) -> Bool {
   if let l = lista {
      switch l {
         case .Vacia:
            return true
         default:
            return false
      }
   } else {
      return false
   }
}

let lista : Lista = .Cons(20, .Cons(30, .Cons(40, .Vacia)))

print(car(lista)!) // Imprime 20
print(car(cdr(lista))!) // Imprime 30
print(car(cdr(cdr(lista)))!) // Imprime 40
print(vacia(cdr(cdr(cdr(lista))))) // Imprime true
```
