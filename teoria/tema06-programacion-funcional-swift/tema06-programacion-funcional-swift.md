
## Tema 6: Programación Funcional con Swift

### Contenidos

- [1. Introducción](#1)
- [2. Funciones](#2)
- [3. Tipos función](#3)
- [4. Recursión](#4)
- [5. Tipos](#5)
- [6. Opcionales](#6)
- [7. Inmutabilidad](#7)
- [8. Clausuras](#8)
- [9. Funciones de orden superior](#9)
- [10. Genéricos](#10)

----

### Bibliografía

- Swift Language Guide
    - [The Basics](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309)
    - [Collection Types](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-ID105)
    - [Functions](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html#//apple_ref/doc/uid/TP40014097-CH10-ID158)
    - [Closures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID94)
    - [Enumerations](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID145)
- [Biblioteca estándar de Swift](https://developer.apple.com/library/ios/documentation/General/Reference/SwiftStandardLibraryReference/)


### <a name="1"></a> 1. Introducción

Te recomendamos que leas los primeros apartados del
[seminario de Swift](https://github.com/domingogallardo/apuntes-lpp/blob/master/seminarios/seminario2-swift/seminario2-swift.md),
en el que se introduce el lenguaje y se explica cómo ejecutar
programas en este lenguaje:

- [El lenguaje de programación Swift](https://github.com/domingogallardo/apuntes-lpp/blob/master/seminarios/seminario2-swift/seminario2-swift.md#-1-el-lenguaje-de-programación-swift)
- [Ejecución de programas Swift en IBM Swift Sandbox](https://github.com/domingogallardo/apuntes-lpp/blob/master/seminarios/seminario2-swift/seminario2-swift.md#-21-ejecución-en-ibm-swift-sandbox)
- [Valores simples (primer apartado de Un tour de Swift)](https://github.com/domingogallardo/apuntes-lpp/blob/master/seminarios/seminario2-swift/seminario2-swift.md#valores-simples)

De esta forma podrás probar los distintos ejemplos que presentamos en
este tema.

Swift es un lenguaje principalmente imperativo, pero en su diseño se
han introducido conceptos modernos de programación funcional,
extraídos de lenguajes como Rust o Haskell. 

Como dice su creador [Chris Lattner](http://nondot.org/sabre/):

> El lenguaje Swift es el resultado de un esfuerzo incansable de un
> equipo de expertos en lenguajes, gurús de documentación, ninjas de
> optimización de compiladores [..]. Por supuesto, también se
> benefició enormemente de las experiencias ganadas en muchos otros
> lenguajes, tomando ideas de Objective-C, Rust, Haskell, Ruby,
> Python, C#, CLU, y demasiados otros para ser enumerados.

Vamos a repasar en este tema cómo se implementan en Swift conceptos
principalmente funcionales como:

- Valores inmutables
- Funciones como objetos de primera clase y clasuras
- Funciones de orden superior


Repasamos rápidamente algunos conceptos básicos de programación
funcional, vistos en los primeros temas de la asignatura.

Programación Funcional:

> La Programación Funcional es un paradigma de programación que trata
> la computación como la evaluación de funciones matemáticas y que
> evita cambios de estado y datos mutables.

Funciones matemáticas o puras:

> Las funciones matemáticas tienen la característica de que cuando las
> invocas con el mismo argumento siempre te devolverán el mismo
> resultado.

Funciones como objetos de primera clase:

> En programación funcional, las funciones son objetos de primera
> clase del lenguaje, similares a enteros o _strings_. Podemos pasar
> funciones como argumentos en las denominadas _funciones de orden
> superior_ o devolver funciones creadas en tiempo de ejecución
> (clausuras).

### <a name="2"></a> 2. Funciones

#### Definición de una función en Swift

Para definir una función en Swift se debe usar la palabra `func`,
definir el nombre de la función, sus parámetros y el tipo de
vuelto. El valor devuelto por la función se debe devolver usando la
palabra `return`.

Código de la función `saluda(nombre:)`:

```swift
func saluda(nombre: String) -> String {
    let saludo = "Hola, " + nombre + "!"
    return saludo
}
```

Para invocar a la función `saluda(nombre:)`:

```swift
print(saluda(nombre:"Ana"))
print(saluda(nombre:"Pedro"))
// Imprime "Hola, Ana!"
// Imprime "Hola, Pedro!"
```

#### Etiquetas de argumentos y nombres de parámetros

Cada parámetro de una función tiene una etiqueta del argumento y un
nombre de parámetro. La etiqueta del argumento se usa cuando se llama a
la función y el nombre del parámetro se usa internamente en su
implementación. Por defecto, los parámetros usan su nombre de
parámetro como etiqueta del argumento:

```swift
func unaFuncion(primerNombreParametro: Int, segundoNombreParametro: Int) {
    // En el cuerpo de la función, primerNombreParametro y
    // segundoNombreParametro se refieren a los valores de los
    // argumentos del primer y el segundo parámetro
}
unaFuncion(primerNombreParametro: 1, segundoNombreParametro: 2)
```

Es posible hacer distintos la etiqueta del argumento del nombre del
parámetro:

```swift
func saluda(nombre: String, de ciudad: String) -> String {
    return "Hola \(nombre)! Me alegro de que hayas podido visitarnos desde \(ciudad)."
}
print(saluda(nombre: "Bill", de: "Cupertino"))
// Imprime "Hola Bill! Me alegro de que hayas podido visitarnos desde Cupertino."
```

Otro ejemplo, la siguiente función `concatena(palabra:con:)`: 

```swift
func concatena(palabra str1: String, con str2: String) -> String {
    return str1+str2
}

print(concatena(palabra:"Hola", con:"adios"))
```

Si no se quiere una etiqueta del argumento para un parámetro, se puede
escribir un subrayado (`_`) en lugar de una etiqueta del argumento
explícita para ese parámetro:

```swift
func max(_ x:Int, _ y: Int) -> Int {
   if x > y {
      return x
   } else {
      return y
   }
}

print(max(10,3))

func divide(_ x:Double, entre y: Double) -> Double {
   return x / y
}

print(divide(30, entre:4))
```

El perfil de la función está formado por el nombre de la función, las
etiquetas de los argumentos y el tipo devuelto por la función. En la
documentación de las funciones usaremos las etiquetas separadas por
dos puntos. Por ejemplo, las funciones anteriores son `max(_:_:)` y
`divide(_:entre:)`.

#### Parámetros y valores devueltos

Es posible definir funciones sin parámetros:

```swift
func diHolaMundo() -> String {
    return "hola, mundo"
}
print(diHolaMundo())
// Imprime "hola, mundo"
```

Podemos definir funciones sin valor devuelto. Por ejemplo, la
siguiente función `diAdios(nombre:)`. No hay que escribir flecha con el
tipo devuelto. Cuidado, no sería propiamente programación funcional.

```swift
func diAdios(nombre: String) {
    print("Adiós, \(nombre)!")
}
diAdios(nombre:"Dave")
// Imprime "Adiós, Dave!"
```

Es posible devolver múltiples valores, construyendo una tupla. Por
ejemplo, la siguiente función `minMax(array:)` busca el número más pequeño
y más grande de un array de enteros.

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var minActual = array[0]
    var maxActual = array[0]
    for valor in array[1..<array.count] {
        if valor < minActual {
            minActual = valor
        } else if valor > maxActual {
            maxActual = valor
        }
    }
    return (minActual, maxActual)
}
```

Los valores de la tupla devuelta se etiquetan y se puede acceder por
esos nombres cuando se consulta el valor devuelto por la función:

```swift
let limites = minMax(array: [8, -6, 2, 109, 3, 71])
print("min es \(limites.min) y max es \(limites.max)")
// Imprime "min es -6 y max es 109"
```

Si hay algún caso en el que la función pueda devolver "no hay valor"
para los elementos de la tupla podemos usar un tipo _opcional_ para
indicar que el valor devuelto puede ser `nil` (hablaremos más adelante
de los tipos opcionales). Para ello se escribe un símbolo de
interrogación tras el paréntesis. Por ejemplo, `(Int, Int)?` o
`(String, Int, Bool)?`.

Podemos modificar la función anterior para considerar el caso en que
pasemos un array sin elementos:

```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var minActual = array[0]
    var maxActual = array[0]
    for valor in array[1..<array.count] {
        if valor < minActual {
            minActual = valor
        } else if valor > maxActual {
            maxActual = valor
        }
    }
    return (minActual, maxActual)
}
```

Tendremos entonces que llamar a la función comprobando si el valor
devuelto es distinto de `nil`:

```swift
if let limites = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min es \(limites.min) y max es \(limites.max)")
}
// Imprime "min es -6 y max es 109"
```

### <a name="3"></a> 3. Tipos función 

En Swift las funciones son objetos de primera clase y podemos
asignarlas a variables, pasarlas como parámetro o devolverlas como
resultado de otra función. Al ser un lenguaje fuertemente tipado, las
variables, parámetros o resultados deben ser objetos de tipo función.

Cada función tiene un tipo específico, definido por el tipo de sus
parámetros y el tipo del valor devuelto.

```swift
func sumaDosInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplicaDosInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}
```

El tipo de estas funciones es `(Int, Int) -> Int`, que se puede leer como:

"Un tipo función que tiene dos parámetros, ambos de tipo `Int` y que
devuelve un valor de tipo `Int`".

En Swift se puede usar un tipo función de la misma forma que cualquier
otro tipo:

```swift
var funcionMatematica: (Int, Int) -> Int = sumaDosInts
print("Resultado: \(funcionMatematica(2, 3))")
funcionMatematica = multiplicaDosInts
print("Resultado: \(funcionMatematica(2, 3))")
```

#### Funciones que reciben otras funciones

Podemos usar un tipo función en parámetros de otras funciones:

```swift
func printResultado(funcion: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Resultado: \(funcionMatematica(a, b))")
}
printResultado(funcion: sumaDosInts, 3, 5)
// Prints "Resultado: 8"
```

La función `printResultado(funcion:_:_:)` toma como primer parámetro otra
función que recibe dos `Int` y devuelve un `Int`, y como segundo y
tercer parámetro dos `Int`.

Veamos otro ejemplo, que ya vimos en Scheme. Supongamos que queremos
calcular el sumatorio desde `a` hasta `b` en el que aplicamos una
función `f` a cada número que sumamos:

```text
sumatorio(a, b, f) = f(a) + f(a+1) + f(a+2) + ... + f(b)
```

Recordamos que se resuelve con la siguiente recursión:

```text
sumatorio(a, b, f) = f(a) + sumatorio(a+1, b, f)
sumatorio(a, b, f) = 0 si a > b
```

Veamos cómo se implementa en Swift: 

```swift
func sumatorio(desde a: Int, hasta b: Int, func f: (Int) -> Int) -> Int {
   if a > b { 
      return 0 
   } else {
      return f(a) + sumatorio(desde: a + 1, hasta: b, func: f)
   }
}

func identidad(_ x: Int) -> Int {
   return x
}

func doble(_ x: Int) -> Int {
   return x + x
}

func cuadrado(_ x: Int) -> Int {
    return x * x
}

print(sumatorio(desde: 0, hasta: 10, func: identidad)) // Imprime 55
print(sumatorio(desde: 0, hasta: 10, func: doble)) // Imprime 110
print(sumatorio(desde: 0, hasta: 10, func: cuadrado)) // Imprime 385
```


#### Funciones que devuelven otras funciones

Por último, veamos un ejemplo de funciones que devuelven otras
funciones y que se declaran de forma anidada:

```swift
func eligeFuncionPaso(menorQueCero: Bool) -> (Int) -> Int {
    func pasoAdelante(x: Int) -> Int { return x + 1 }
    func pasoAtras(x: Int) -> Int { return x - 1 }
    return menorQueCero ? pasoAdelante : pasoAtras
}

var valorActual = -4
let acercarseACero = eligeFuncionPaso(menorQueCero: valorActual < 0)
// acercarseACero ahora se refiere a la función anidada pasoAdelante
while valorActual != 0 {
    print("\(valorActual)... ")
    valorActual = acercarseACero(valorActual)
}
print("cero!")
// -4...
// -3...
// -2...
// -1...
// cero!
```

### <a name="4"></a> 4. Recursión

Ejemplos de funciones recursivas en Swift.

Recursión pura:

```swift
func suma(hasta x: Int) -> Int {
  if x == 0 {
    return 0
  } else {
    return x + suma(hasta: x - 1)
  }
}

print(suma(hasta: 5))
```

Los arrays en Swift no funcionan exactamente como las listas de Scheme
(no son listas de parejas), pero podríamos obtener el primer elemento
y el resto de la siguiente forma.

```swift
let a = [10, 20, 30, 40, 50, 60]
let primero = a[0]
let resto = a[1..<a.endIndex]
```

En `resto` se guarda un `ArraySlice`. Es una vista de un rango de
elementos del array, en este caso el que va desde la posición 1 hasta
la 5 (la posición inicial de un array es la 0).

Un `ArraySlice` puede situarse sobre cualquier zona de un array. Por
ejemplo:

```swift
let b = a[2...3]
// b: ArraySlice<Int> = 2 values {
//  [2] = 30
//  [3] = 40
//}
```

Podemos acceder al primer elemento de un `ArraySlice` usando su
atributo `startIndex`:

```swift
print(b.startIndex) // => 2
print(b[b.startIndex]) // => 3
```

Para trabajar con funciones recursivas de una forma similar a cómo lo
hacíamos con las listas de Scheme vamos a ir cambiando el rango
conforme nos movemos por el `ArraySlice`. Iremos obteniendo el primer
elemento de la colección y avanzando el índice de comienzo, de forma
similar a como hacíamos con las funciones _car_ y _cdr_ (o _primero_ y
_resto_):

```
let primero = valores[valores.startIndex]
let resto = valores[valores.startIndex+1..<valores.endIndex]
```

De esta forma podemos definir una función recursiva que recorre un
`ArraySlice`:

```swift
func sumaValores(valores: ArraySlice<Int>) -> Int {
  if valores.isEmpty {
    return 0
  } else {
    let primero = valores[valores.startIndex]
    let resto = valores[valores.startIndex+1..<valores.endIndex]
    return primero + sumaValores(valores: resto)
  }
}
print(sumaValores(valores: [1,2,3,4,5,6,7,8]))
// 36
```

Veremos que las colecciones en Swift implementan funciones de orden
superior como `map`, `filter`, etc.

Veremos también más adelante otras funciones recursivas cuando
definamos árboles en Swift.

### <a name="5"></a> 5. Tipos

Swift es un lenguaje fuertemente tipeado, a diferencia de
Scheme. Muchos otros lenguajes de programación funcional, como Haskell
o Clojure también lo son.

Entre las ventajas del uso de tipos está la detección de errores en
los programas en tiempo de compilación o las ayudas del entorno de
desarrollo para autocompletar código. Entre los inconvenientes se
encuentra la necesidad de ser más estrictos a la hora de definir los
parámetros y los valores devueltos por las funciones, lo que impide la
flexibilidad de Scheme.

Se utilizan tipos para definir los posibles valores de:

- variables
- parámetros de funciones
- valores devueltos por funciones

Las definiciones de tipos van precedidas de dos puntos en las
variables y parámetros, o de una flecha (`->`) en la definición de los
tipos de los valores devueltos por una función:

```swift
let valorDouble : Double = 3.0
let unaCadena: String = "Hola"

func calculaEstadisticas(valores: Array<Int>) -> (min: Int, max: Int, media: Int) {
   ...
}
```

En Swift existen dos clases de tipos: tipos con nombre y tipos compuestos. 

#### Tipos con nombre

Un tipo con nombre es un tipo al que se le puede dar un nombre
determinado cuando se define.

Definimos un tipo al definir:

- nombres de clases
- nombres de estructuras
- nombres de enumeraciones
- nombres de protocolos 

Por ejemplo, instancias de una clase definida por el usuario llamada
`MiClase` tienen el tipo `MiClase`. Además de los tipos definidos por
el usuario, la biblioteca estándar de Swift tiene un gran número de
tipos predefinidos. A diferencia de otros lenguajes, estos tipos no
son parte del propio lenguaje sino que se definen en su mayoría como
estructuras implementadas en esta biblioteca estándar. Por ejemplo,
arrays, diccionarios o incluso los tipos más básicos como `String` o
`Int` están construidos en esa biblioteca.

#### Tipos compuestos

Los tipos compuestos son tipos sin nombre. En Swift se definen dos:
tuplas y tipos función. Un tipo compuesto puede tener tipos con nombre
y otros tipos compuestos. Por ejemplo la tupla `(Int, (Int, Int))`
contiene dos elementos: el primero es el tipo con nombre `Int` y el
segundo el tipo compuesto que define la tupla `(Int, Int)`. Los tipos
función los hemos visto previamente.

```swift

let tupla: (Int, Int, String) = (2, 3, "Hola")
let otraTupla: (Int, Int, String) = (5, 8, "Adios")

func sumaTupla(tupla t1: (Int, Int), con t2: (Int, Int)) -> (Int, Int) {
  return (t1.0 + t2.0, t1.1 + t2.1)
}

print(sumaTupla(tupla: (tupla.0, tupla.1),
                con: (otraTupla.0, otraTupla.1)))

// Imprime (7, 11)
```

#### Enumeraciones

Las enumeraciones definen un tipo con un valor restringido de posibles
valores:

```swift
enum Direccion {
    case norte
    case sur
    case este
    case oeste
}
```

Cualquier variable del tipo `Direccion` solo puede tener uno de los
cuatro valores definidos. Se obtiene el valor escribiendo el nombre de
la enumeración, un punto y el valor definido. Si el tipo de
enumeración se puede inferir no es necesario escribirlo.

```swift
let hemosGirado = true
var direccionActual = Direccion.norte
if hemosGirado {
   direccionActual = .sur
}
```

En sentencias switch:

```swift
let direccionAIr = Direccion.sur
switch direccionAIr {
case .norte:
   print("Nos vamos al norte")
case .sur:
   print("Cuidado con los pinguinos")
case .este:
    print("Donde nace el sol")
case .oeste:
    print("Donde el cielo es azul")
}
// Imprime "Cuidado con los pinguinos"
```

Otro ejemplo:

```swift
enum Planeta {
    case mercurio, venus, tierra, marte, jupiter, saturno, urano, neptuno
}
```

#### Valores brutos de enumeraciones

Es posible asignar a las constantes del enumerado un valor concreto de
un tipo subyacente:

```swift
enum CaracterControlASCII: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

Se puede devolver el valor bruto de la siguiente forma:

```
let nuevaLinea = CaracterControlASCII.LineFeed.rawValue
```

También se puede hacer de forma implícita cuando el tipo subyacente es
`Int`, dando un valor a la primera constante:

```swift
enum Planeta: Int {
    case mercurio=1, venus, tierra, marte, jupiter, saturno, urano, neptuno
}
let posicionTierra = Planeta.tierra.rawValue
// posicionTierra es 3
```

Por último, se puede definir como tipo subyacente `String` y los
valores brutos de las constantes serán sus nombres convertidos a
cadenas:

```swift
enum Direccion: String {
    case norte, sur, este, oeste
}
let direccionAtardecer = Direccion.oeste.rawValue
// direccionAtardecer es "oeste"
```

Cuando se definen valores brutos es posible inicializar el enumerado
de una forma similar a una estructura o una clase pasando el valor
bruto. Devuelve el valor enumerado correspondiente o `nil` (un
opcional):

```swift
let posiblePlaneta = Planeta(rawValue: 7)
// posiblePlaneta es de tipo Planeta? y es igual a Planeta.urano
```

#### Valores asociados a instancias de enumeraciones

En otros lenguajes de programación se llaman _uniones etiquetadas_ o
_variantes_. Permiten asociar valores de otro tipo a las opciones del
enumerado.

Una instancia de un caso de enumeración puede tener valores asociados
con la instancia. Instancias del mismo caso de enumeración pueden
tener asociados valores diferentes. Se proporciona el valor asociado
cuando se crea la instancia. Los valores asociados y los valores
brutos son distintos: el valor bruto de un caso de enumeración es el
mismo para todas las instancias, mientras que el valor asociado se
proporciona cuando se define el valor concreto de la enumeración.

Veamos un ejemplo, en el que usamos un enum para definir posibles
valores de un código de barras, en el que incluimos dos posibles tipos
de código de barras: el código de barras lineal (denominado UPC) y el
código QR:

```swift
enum CodigoBarras {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

Se lee de la siguiente forma: “Definimos un tipo enumerado llamado
`CodigoBarras`, que puede tomar como valor un `upc` (código de barras
lineal) con un valor asociado de tipo `(Int, Int, Int, Int)` (los 4
números que hay en los códigos de barras lineales) o un valor `qrCode`
con valor asociado de tipo `String`". Esta definición no proporciona
valores concretos de `Int` o `String`, sino que define el _tipo_ de
valores asociados que las constantes y variables pueden almacenar
cuando son de tipo `CodigoBarras.upc` o `CodigoBarras.qrCode`.

```swift
var codigoBarrasProducto = CodigoBarras.upc(8, 85909, 51226, 3)
codigoBarrasProducto = .qrCode("ABCDEFGHIJKLMNOP")

switch codigoBarrasProducto {
case let .upc(sistemaNumeracion, fabricante, producto, control):
   print("UPC: \(sistemaNumeracion), \(fabricante), \(producto), \(control).")
case let .qrCode(codigoProducto):
   print("Código QR: \(codigoProducto).")
}
// Imprime  "Código QR : ABCDEFGHIJKLMNOP."
```

#### Enumeraciones recursivas

Es posible combinar las características de las enumeraciones con valor
con la recursión para crear enumeraciones recursivas. Hay que preceder
la palabra clave `enum` con `indirect`:

```swift
indirect enum ExpresionAritmetica {
    case numero(Int)
    case suma(ExpresionAritmetica, ExpresionAritmetica)
    case multiplicacion(ExpresionAritmetica, ExpresionAritmetica)
}

let cinco = ExpresionAritmetica.numero(5)
let cuatro = ExpresionAritmetica.numero(4)
let suma = ExpresionAritmetica.suma(cinco, cuatro)
let producto = ExpresionAritmetica.multiplicacion(suma, ExpresionAritmetica.numero(2))
```

Es muy cómodo manejar enumeraciones recursivas de forma recursiva:

```swift
func evalua(expresion: ExpresionAritmetica) -> Int {
    switch expresion {
    case let .numero(valor):
        return valor
    case let .suma(izquierda, derecha):
        return evalua(expresion: izquierda) + evalua(expresion: derecha)
    case let .multiplicacion(izquierda, derecha):
        return evalua(expresion: izquierda) * evalua(expresion: derecha)
    }
}

print(evalua(expresion: producto))
// Imprime 18
```

#### Typealias

En Swift se define la palabra clave `typealias` para darle un nombre
asignado a cualquier otro tipo. Ambos tipos son iguales a todos los
efectos (es únicamente azúcar sintáctico).

```swift
typealias Resultado = (Int, Int)

enum Quiniela {
    case uno, equis, dos
}

func quiniela(resultado: Resultado) -> Quiniela {
  switch resultado {
    case let (goles1, goles2) where goles1 < goles2:
      return .dos
    case let (goles1, goles2) where goles1 > goles2:
      return .uno
    default:
      return .equis
  }
}

print(quiniela(resultado: (1,3)))
// Imprime Dos
print(quiniela(resultado: (2,2)))
// Imprime Equis
```

### <a name="6"></a> 6. Opcionales

En Swift el valor nulo se representa con `nil` (equivalente a `null`
en Java). No podemos asignar `nil` a una variable de un tipo dado:

```swift
// La siguiente línea daría un error en tiempo de compilación
// let cadena: String = nil
```

Los tipos opcionales de Swift permiten asignar a variables o bien un
valor propio del tipo o bien `nil`, de forma que podemos expresar
situaciones en las que:

- Hay un valor y es igual que _x_

o

- No hay ningún valor

Podemos definir como opcional variables, parámetros o valores
devueltos por funciones.

Por ejemplo, el tipo `Int` de Swift tiene un inicializador que intenta
convertir un valor `String` a un valor `Int`. Sin embargo, no toda
cadena puede convertirse a un número. Por ejemplo, la cadena `"123"`
se debería convertir al número 123, pero la cadena `"Hola, mundo"` no
tiene un valor numérico al que convertirse.

El siguiente ejemplo muestra la forma correcta de usar el inicializador:

```swift
var posibleNumero = "123"
let numeroConvertido = Int(posibleNumero)
posibleNumero = "Hola mundo"
let conversionErronea = Int(posibleNumero)
// numeroConvertido y conversionErronea son de tipo "Int?", o "Int opcional"
// El primero contiene un número y el segundo nil
```

Debido a que el inicializador puede fallar, devuelve un `Int`
_opcional_, en lugar de un `Int`. Un `Int` opcional se escribe como
`Int?`.

Para definir una variable como sin valor debemos asignarle el valor
especial `nil`:

```swift
var codigoRespuestaServidor: Int? = 404
// codigoRespuestaServidor contine un valor Int de 404
codigoRespuestaServidor = nil
// codigoRespuestaServidor ahora no contiene ningún valor
```

Una variable opcional sin asignar ningún valor se inicializa
automáticamente a `nil`:

```swift
var respuestaEncuesta: String?
// respuestaEncuesta es inicializado automáticamente a nil
```

#### Sentencias `if` y _desenvoltura forzosa_

Se puede usar un `if` para comprobar si un valor opcional es distinto
de `nil`:

```swift
if numeroConvertido != nil {
    print("numeroConvertido contiene algún valor entero.")
}
// Imprime "numeroConvertido contiene algún valor entero."
```

Si estamos seguros de que el opcional contiene un valor,
podemos acceder a él usando un signo de exclamación (`!`). Quiere
decir "Sé que hay este opcional tiene un valor concreto; por favor
úsalo". Esto se conoce como _desenvoltura forzosa_ (_forced
unwrapping_) del valor opcional:

```swift
if numeroConvertido != nil {
    print("numeroConvertido tiene un valor entero de \(numeroConvertido!).")
}
// Imprime "numeroConvertido tiene un valor entero de 123."
```

#### Ligado opcional

Es posible comprobar si un opcional tiene valor y asignar su valor a
otra variable al mismo tiempo con una construcción llamada _ligado
opcional_ (_optional binding_):

```swift
if let numeroVerdadero = Int(posibleNumero) {
    print("\"\(posibleNumero)\" tiene un valor entero de \(numeroVerdadero)")
} else {
    print("\"\(posibleNumero)\" no ha podido convertirse en un entero")
}
// Imprime ""123" tiene un valor entero de 123"
```

Podemos leer el código anterior de la siguiente forma: "Si el `Int`
opcional devuelto por `Int(posibleNumero)` contiene un valor, define
la constante `numeroVerdadero` con el valor contenido en el opcional".

#### Opcionales implícitamente desenvueltos

Algunas veces está claro a partir de la estructura del programa que un
opcional siempre va a tener valor. En estos casos es útil eliminar la
necesidad de comprobar y desenvolver el valor cada vez que se
necesita. Estos valores se declaran como _opcionales implícitamente
desenvueltos_ (_implicitly unwrapped optionals_). Se definen
escribiendo un signo de admiración después del tipo (`String!`) en
lugar del signo de interrogación (`String?`).

El valor de un opcional declarado de esta forma se desenvuelve
automáticamente cada vez que se usa, sin necesidad del signo de
admiración.

```swift
let posibleCadena: String? = "Una cadena opcional."
let cadenaExtraida: String = posibleCadena! // requiere un signo de admiración

let supuestaCadena: String! = "Una cadena opcional implícitamente desenvuelta" // implicitly unwrapped optional
let cadenaImplicita: String = supuestaCadena // no se necesita el signo de admiración
```

Es posible usar un opcional implícitamente desenvuelto como un
opcional normal, para comprobar si tiene valor:

```swift
if supuestaCadena != nil {
    print(supuestaCadena)
}
// Imprime "Una cadena opcional implícitamente desenvuelta"
```

Y también se puede utilizar en un ligado opcional:

```swift
if let cadenaDefinitiva = supuestaCadena {
    print(cadenaDefinitiva)
}
// Imprime "Una cadena opcional implícitamente desenvuelta"
```

### <a name="7"></a> 7. Inmutabilidad

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

### <a name="8"></a> 8. Expresiones de clausuras

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

Si se usa estos argumentos abreviados, se puede omitir la definición de la lista de los argumentos:

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

### <a name="9"></a> 9. Funciones de orden superior

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

### <a name="10"></a> 10. Genéricos

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

<!-- ¿Cambiamos lo de antes por un árbol?:

class Node {
  var value: String
  var children: [Node] = []
  weak var parent: Node?
 
  init(value: String) {
    self.value = value
  }
 
  func add(child: Node) {
    children.append(child)
    child.parent = self
  }
}

https://www.raywenderlich.com/138190/swift-algorithm-club-swift-tree-data-structure

--> 


----

Lenguajes y Paradigmas de Programación, curso 2015–16  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Domingo Gallardo
