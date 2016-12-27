
## Tema 5: Programación Funcional con Swift

### Contenidos

- **1 Introducción**
- **2 Funciones**
- **3 Tipos función**
- **4 Recursión**
- **5 Tipos**
- **6 Opcionales**
- 7 Inmutabilidad
- 8 Clausuras
- 9 Funciones de orden superior
- 10 Genéricos

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

Repaso de conceptos básicos de programación funcional.

Programación Funcional:

> La Programación Funcional es un paradigma de programación que trata la computación como la evaluación de funciones matemáticas y que evita cambios de estado y datos mutables.

Funciones matemáticas:

> Las funciones matemáticas tienen la característica de que cuando les das el mismo argumento te devolverán el mismo resultado.

Funciones como tipos de primera clase:

> Las funciones son objetos de primera clase del lenguaje, similares a enteros o _strings_. Podemos pasar funciones como argumentos o devolver funciones como resultados.

### <a name="2"></a> 2. Funciones

#### Definición de una función en Swift

Para definir una función en Swift se debe usar la palabra `func`, definir el nombre de la función, sus parámetros y el tipo de vuelto. El valor devuelto por la función se debe devolver usando la palabra `return`.

Código de la función `diHola(_:)`:

```swift
func diHola(nombrePersona: String) -> String {
    let saludo = "Hola, " + nombrePersona + "!"
    return saludo
}
```

Para invocar a la función `diHola(_:)`:

```
print(diHola("Ana"))
print(diHola("Pedro"))
// Imprime "Hola, Ana!"
// Imprime "Hola, Pedro!"
```

#### Nombres de parámetros externos e internos

Al llamar a una función con más de un parámetro hay que etiquetar los argumentos después del primero con el correspondiente nombre del parámetro.

Funciones `diHolaOtraVez(_:)` y `diHola(_:yaSaludado:)`:


```swift
func diHolaOtraVez(nombrePersona: String) -> String {
    return "Hola otra vez, " + nombrePersona + "!"
}

func diHola(nombrePersona: String, yaSaludado: Bool) -> String {
    if yaSaludado {
        return diHolaOtraVez(nombrePersona)
    } else {
        return diHola(nombrePersona)
    }
}

print(diHola("Tim", yaSaludado: true))
// Imprime "Hola otra vez, Tim!"
```

Los parámetros tienen un nombre externo y otro interno.

Cuando se llama a la función es obligatorio etiquetar a los argumentos con el nombre externo.


Por ejemplo, la siguiente función `concatena(palabra:con:)`: 

```swift
func concatena(palabra str1: String, con str2: String) -> String {
    return str1+str2
}

print(concatena(palabra:"Hola", con:"adios"))
```

Es posible definir un nombre externo "vacío" usando el símbolo `_` en la definición del nombre externo.

Por defecto, si no se especifica ningún nombre externo, el nombre externo del primer argumento es vacío y el del resto de argumentos el mismo que el interno.

```swift
func max(x:Int, _ y: Int) -> Int {
   if x > y {
      return x
   } else {
      return y
   }
}

print(max(10,3))

func divide(x:Double, entre y: Double) -> Double {
   return x / y
}

print(divide(30, entre:4))
```

El perfil de la función está formado por el nombre de la función, los nombres externos de los parámetros y el tipo devuelto por la función. En la documentación de las funciones usaremos el nombre y los parámetros externos separados por dos puntos. Por ejemplo, las funciones anteriores son `max(_:_:)` y `divide(_:entre:)`.

#### Parámetros y valores devueltos

Es posible definir funciones sin parámetros:

```swift
func diHolaMundo() -> String {
    return "hola, mundo"
}
print(diHolaMundo())
// Imprime "hola, mundo"
```

Podemos definir funciones sin valor devuelto. Por ejemplo, la siguiente función `sayGoodbye(_:)`. No hay que escribir flecha con el tipo devuelto. Cuidado, no sería propiamente programación funcional.

```swift
func sayGoodbye(personName: String) {
    print("Goodbye, \(personName)!")
}
sayGoodbye("Dave")
// Prints "Goodbye, Dave!"
```

Es posible devolver múltiples valores, construyendo una tupla. Por ejemplo, la siguiente función `minMax(_:)` busca el número más pequeño y más grande de un array de enteros.

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

Los valores de la tupla devuelta se etiquetan y se puede acceder por esos nombres cuando se consulta el valor devuelto por la función:

```swift
let limites = minMax([8, -6, 2, 109, 3, 71])
print("min es \(limites.min) y max es \(limites.max)")
// Imprime "min es -6 y max es 109"
```

Si hay algún caso en el que la función pueda devolver "no hay valor" para los elementos de la tupla podemos usar un tipo _opcional_ para indicar que el valor devuelto puede ser `nil`. Para ello se escribe un símbolo de interrogación tras el paréntesis. Por ejemplo, `(Int, Int)?` o `(String, Int, Bool)?`.

Podemos modificar la función anterior para considerar el caso en que pasemos un array sin elementos:

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

Tendremos entonces que llamar a la función comprobando si el valor devuelto es distinto de `nil`:

```swift
if let limites = minMax([8, -6, 2, 109, 3, 71]) {
    print("min es \(limites.min) y max es \(limites.max)")
}
// Imprime "min es -6 y max es 109"
```

### <a name="3"></a> 3. Tipos función 

En Swift las funciones son objetos de primera clase y podemos asignarlas a variables, pasarlas como parámetro o devolverlas como resultado de otra función. Al ser un lenguaje fuertemente tipado, las variables, parámetros o resultados deben ser objetos de tipo función.

Cada función tiene un tipo específico, definido por el tipo de sus parámetros y el tipo del valor devuelto.

```swift
func sumaDosInts(a: Int, _ b: Int) -> Int {
    return a + b
}
func multiplicaDosInts(a: Int, _ b: Int) -> Int {
    return a * b
}
```

El tipo de estas funciones es `(Int, Int) -> Int`, que se puede leer como:

"Un tipo función que tiene dos parámetros, ambos de tipo `Int` y que devuelve un valor de tipo `Int`".

En Swift se puede usar un tipo función de la misma forma que cualquier otro tipo:

```swift
var funcionMatematica: (Int, Int) -> Int = sumaDosInts
print("Resultado: \(funcionMatematica(2, 3))")
funcionMatematica = multiplicaDosInts
print("Resultado: \(funcionMatematica(2, 3))")
```

#### Funciones que reciben otras funciones

Podemos usar un tipo función en parámetros de otras funciones:

```swift
func printResultado(funcionMatematica: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Resultado: \(funcionMatematica(a, b))")
}
printResultado(sumaDosInts, 3, 5)
// Prints "Resultado: 8"
```

La función `printResultado(_:_:_:)` toma como primer parámetro otra función que recibe dos `Int` y devuelve un `Int`, y como segundo y tercer parámetro dos `Int`.

Veamos otro ejemplo, que ya vimos en Scheme. Supongamos que queremos calcular el sumatorio desde `a` hasta `b` en el que aplicamos una función `f` a cada número que sumamos:

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

func identidad(x: Int) -> Int {
   return x
}

func doble(x: Int) -> Int {
   return x + x
}

func cuadrado(x: Int) -> Int {
    return x * x
}

print(sumatorio(desde: 0, hasta: 10, func: identidad)) // Imprime 55
print(sumatorio(desde: 0, hasta: 10, func: doble)) // Imprime 110
print(sumatorio(desde: 0, hasta: 10, func: cuadrado)) // Imprime 385
```


#### Funciones que devuelven otras funciones

Por último, veamos un ejemplo de funciones que devuelven otras funciones y que se declaran de forma anidada:


```swift
func eligeFuncionPaso(menorQueCero: Bool) -> (Int) -> Int {
    func pasoAdelante(x: Int) -> Int { return x + 1 }
    func pasoAtras(x: Int) -> Int { return x - 1 }
    return menorQueCero ? pasoAdelante : pasoAtras
}


var valorActual = -4
let acercarseACero = eligeFuncionPaso(valorActual < 0)
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
func sumaHasta(x: Int) -> Int {
  if x == 0 {
    return 0
  } else {
    return x + sumaHasta(x - 1)
  }
}

print(sumaHasta(5))
```

Los arrays en Swift no funcionan exactamente como las listas de Scheme (no son listas de parejas), pero podríamos obtener el primer elemento y el resto de la siguiente forma.


```swift
let a = [1,2,3,4,5,6]
let primero = a[0]
let resto = a[1..<a.endIndex]]
```

En `resto` se guardará un `ArraySlice`. Es una vista de un rango de elementos del array, en este caso el que va de 1 hasta el último. Un `ArraySlice` puede situarse sobre cualquier zona de un array. Por ejemplo:

```swift
let b = a[2...3]
// b: ArraySlice<Int> = 2 values {
//  [2] = 3
//  [3] = 4
//}
```

Este rango va a ir cambiando conforme vayamos moviéndonos por el `ArraySlice`. Iremos avanzando el índice de comienzo y dejando fijo el índice final. De forma que, en general, usaremos las siguientes expresiones para obtener el primero y el resto:

```
let primero = valores[valores.startIndex]
let resto = valores[valores.startIndex+1..<valores.endIndex]
```

Con estas expresiones podemos definir una función recursiva que recorre un `ArraySlice`:

```swift
func sumaValores(valores: ArraySlice<Int>) -> Int {
  if valores.isEmpty {
    return 0
  } else {
    let primero = valores[valores.startIndex]
    let resto = valores[valores.startIndex+1..<valores.endIndex]
    return primero + sumaValores(resto)
  }
}
print(sumaValores([1,2,3,4,5,6,7,8]))
// 36
```

Veremos que las colecciones en Swift implementan funciones de orden superior como `map`, `filter`, etc.

Veremos también más adelante otras funciones recursivas cuando definamos árboles en Swift.


### <a name="5"></a> 5. Tipos

Swift es un lenguaje fuertemente tipado, a diferencia de Scheme. Muchos otros lenguajes de programación funcional, como Haskell o Clojure también lo son. 

Entre las ventajas del uso de tipos está la detección de errores en los programas en tiempo de compilación o las ayudas del entorno de desarrollo para autocompletar código. Entre los inconvenientes se encuentra la necesidad de ser más estrictos a la hora de definir los parámetros y los valores devueltos por las funciones, lo que impide la flexibilidad de Scheme.

Se utilizan tipos para definir los posibles valores de:

- variables
- parámetros de funciones
- valores devueltos por funciones

Las definiciones de tipos van precedidas de dos puntos en las variables y parámetros, o de una flecha (`->`) en la definición de los tipos de los valores devueltos por una función:

```swift
let valorDouble : Double = 3.0
let unaCadena: String = "Hola"

func calculaEstadisticas(valores: Array<Int>) -> (min: Int, max: Int, media: Int) {
   ...
}
```

En Swift existen dos clases de tipos: tipos con nombre y tipos compuestos. 

#### Tipos con nombre

Un tipo con nombre es un tipo al que se le puede dar un nombre determinado cuando se define. 

Definimos un tipo al definir:

- nombres de clases
- nombres de estructuras
- nombres de enumeraciones
- nombres de protocolos 

Por ejemplo, instancias de una clase definida por el usuario llamada `MyClass` tienen el tipo `MyClass`. Además de los tipos definidos por el usuario, la biblioteca estándar de Swift tiene un gran número de tipos predefinidos. A diferencia de otros lenguajes, estos tipos no son parte del propio lenguaje sino que se definen en su mayoría como estructuras implementadas en esta biblioteca estándar. Por ejemplo, arrays, diccionarios o incluso los tipos más básicos como `String` o `Int` están construidos en esa biblioteca.

#### Tipos compuestos

Los tipos compuestos son tipos sin nombre. En Swift se definen dos: tuplas y tipos función. Un tipo compuesto puede tener tipos con nombre y otros tipos compuestos. Por ejemplo la tupla `(Int, (Int, Int))` contiene dos elementos: el primero es el tipo con nombre `Int` y el segundo el tipo compuesto que define la tupla `(Int, Int)`. Los tipos función los hemos visto previamente.

```swift

let tupla: (Int, Int, String) = (2, 3, "Hola")
let otraTupla: (Int, Int, String) = (5, 8, "Adios")

func sumaTupla(t1: (Int, Int), con t2: (Int, Int)) -> (Int, Int) {
  return (t1.0 + t2.0, t1.1 + t2.1)
}

print(sumaTupla((tupla.0, tupla.1),
                 con: (otraTupla.0, otraTupla.1)))

// Imprime (7, 11)
```

#### Enumeraciones

Las enumeraciones definen un tipo con un valor restringido de posibles valores. Utilizan un caso base, que hay que dec

```swift
enum Direccion {
    case Norte
    case Sur
    case Este
    case Oeste
}
```

Cualquier variable del tipo `Direccion` solo puede tener uno de los cuatro valores definidos. Se obtiene el valor escribiendo el nombre de la enumeración, un punto y el valor definido. Si el tipo de enumeración se puede inferir no es necesario escribirlo.

```swift
var direccionActual = Direccion.Norte
if hemosGirado {
   direccionActual = .Sur
}
```

En sentencias switch:

```swift
direccionActual = .Sur
switch direccionAIr {
case .Norte:
    print("Nos vamos al norte")
case .Sur:
    print("Cuidado con los pinguinos")
case .Este:
    print("Donde nace el sol")
case .Oeste:
    print("Donde el cielo es azul")
}
// Imprime "Cuidado con los pinguinos"
```

Otro ejemplo:

```swift
enum Planeta {
    case Mercurio, Venus, Tierra, Marte, Jupiter, Saturno, Urano, Neptuno
}
```

#### Valores brutos de enumeraciones

Es posible asignar a las constantes del enumerado un valor concreto de un tipo subyacente:

```swift
enum CaracterControlASCII: Character {
    case Tab = "\t"
    case LineFeed = "\n"
    case CarriageReturn = "\r"
}
```

Se puede devolver el valor bruto de la siguiente forma:

```
let nuevaLinea = CaracterControlASCII.LineFeed.rawValue
```

También se puede hacer de forma implícita cuando el tipo subyacente es `Int`, dando un valor a la primera constante:

```swift
enum Planeta: Int {
    case Mercurio=1, Venus, Tierra, Marte, Jupiter, Saturno, Urano, Neptuno
}
let posicionTierra = Planeta.Tierra.rawValue
// posicionTierra es 3
```

Por último, se puede definir como tipo subyacente `String` y los valores brutos de las constantes serán sus nombres convertidos a cadenas:

```swift
enum Direccion: String {
    case Norte, Sur, Este, Oeste
}
let direccionAtardecer = Direccion.Oeste.rawValue
// direccionAtardecer es "Oeste"
```

Cuando se definen valores brutos es posible inicializar el enumerado de una forma similar a una estructura o una clase pasando el valor bruto. Devuelve el valor enumerado correspondiente o `nil` (un opcional):

```swift
let posiblePlaneta = Planeta(rawValue: 7)
// posiblePlaneta es de tipo Planeta? y es igual a Planeta.Urano
```

#### Valores asociados a instancias de enumeraciones

En otros lenguajes de programación se llaman _uniones etiquetadas_ o _variantes_. Permiten asociar valores de otro tipo a las opciones del enumerado.

Repasemos un primer ejemplo, recogido del seminario de Scheme.

Una instancia de un caso de enumeración puede tener valores asociados con la instancia. Instancias del mismo caso de enumeración pueden tener asociados valores diferentes. Se proporciona el valor asociado cuando se crea la instancia. Los valores asociados y los valores brutos son distintos: el valor bruto de un caso de enumeración es el mismo para todas las instancias, mientras que el valor asociado se proporciona cuando se define el valor concreto de la enumeración.

Por ejemplo, podemos definir un enumerado con el que formalizar una respuesta de un servidor. Puede ser de dos tipos o `Resultado`, en cuyo caso va acompañado de dos `String` o `Error`, en el que el valor asociado es un único `String`. Por ejemplo, podríamos usarlo para devolver la petición sobre la hora de salir y ponerse el sol. También podríamos dar un mensaje de error:

```swift
enum RespuestaServidor {
    case Resultado(String, String)
    case Error(String)
}
 
let exito = RespuestaServidor.Resultado("6:00 am", "8:09 pm")
let fallo = RespuestaServidor.Error("La base de datos no responde")
 
switch exito {
    case let .Resultado(salidaSol, puestaSol):
        print("La salida del sol es a las \(salidaSol) y la puesta es a \(puestaSol).")
    case let .Error(error):
        print("Fallo...  \(error)")
}
```

Otro ejemplo, en el que usamos un enum para definir posibles valores de un código de barras, en el que incluimos dos posibles tipos de código de barras: el código de barras lineal (denominado UPCA) y el código QR:

```swift
enum CodigoBarras {
    case UPCA(Int, Int, Int, Int)
    case QRCode(String)
}
```

Se lee de la siguiente forma: “Definimos un tipo enumerado llamado `CodigoBarras`, que puede tomar como valor un `UPCA` (código de barras lineal) con un valor asociado de tipo `(Int, Int, Int, Int)` (los 4 números que hay en los códigos de barras lineales) o un valor `QR` con valor asociado de tipo `String`". Esta definición no proporciona valores concretos de `Int` o `String`, sino que define el _tipo_ de valores asociados que las constantes y variables pueden almacenar cuando son de tipo `CodigoBarras.UPCA` o `CodigoBarras.QR`.

```swift
var codigoBarrasProducto = Barcode.UPCA(8, 85909, 51226, 3)
codigoBarrasProducto = .QRCode("ABCDEFGHIJKLMNOP")

switch codigoBarrasProducto {
case let .UPCA(sistemaNumeracion, fabricante, producto, control):
    print("UPC-A: \(sistemaNumeracion), \(fabricante), \(producto), \(control).")
case let .QRCode(codigoProducto):
    print("Código QR: \(codigoProducto).")
}
// Imprime  "Código QR : ABCDEFGHIJKLMNOP."
```

#### Enumeraciones recursivas

Es posible combinar las características de las enumeraciones con valor con la recursión para crear enumeraciones recursivas. Hay que preceder la palabra clave `enum` con `indirect`:

```swift
indirect enum ExpresionAritmetica {
    case Numero(Int)
    case Suma(ExpresionAritmetica, ExpresionAritmetica)
    case Multiplicacion(ExpresionAritmetica, ExpresionAritmetica)
}

let cinco = ExpresionAritmetica.Numero(5)
let cuatro = ExpresionAritmetica.Numero(4)
let suma = ExpresionAritmetica.Suma(cinco, cuatro)
let producto = ExpresionAritmetica.Multiplicacion(suma, ExpresionAritmetica.Numero(2))
```

Es muy cómodo manejar enumeraciones recursivas de forma recursiva:

```swift
func evalua(expresion: ExpresionAritmetica) -> Int {
    switch expresion {
    case let .Numero(valor):
        return valor
    case let .Suma(izquierda, derecha):
        return evalua(izquierda) + evalua(derecha)
    case let .Multiplicacion(izquierda, derecha):
        return evalua(izquierda) * evalua(derecha)
    }
}
```

#### Typealias

En Swift se define la palabra clave `typealias` para darle un nombre asignado a cualquier otro tipo. Ambos tipos son iguales a todos los efectos (es únicamente azúcar sintáctico).

```swift
typealias Resultado = (Int, Int)

enum Quiniela {
    case Uno, Equis, Dos
}

func resultado(resultado: Resultado) -> Quiniela {
  switch resultado {
    case let (goles1, goles2) where goles1 < goles2:
      return .Dos
    case let (goles1, goles2) where goles1 > goles2:
      return .Uno
    default:
      return .Equis
  }
}

resultado((1,3))
// Imprime Dos
resultado((2,2))
// Imprime Equis
```

### <a name="6"></a> 6. Opcionales

En Swift el valor nulo se representa con `nil` (equivalente a `null` en Java). No podemos asignar `nil` a una variable de un tipo dado:

```swift
// La siguiente línea daría un error en tiempo de compilación
// let cadena: String = nil
```

Los tipos opcionales de Swift permiten asignar a variables o bien un valor propio del tipo o bien `nil`, de forma que podemos expresar situaciones en las que:

- Hay un valor y es igual que _x_

o

- No hay ningún valor

Podemos definir como opcional variables, parámetros o valores devueltos por funciones.

Por ejemplo, el tipo `Int` de Swift tiene un inicializador que intenta convertir un valor `String` a un valor `Int`. Sin embargo, no toda cadena puede convertirse a un número. Por ejemplo, la cadena `"123"` se debería convertir al número 123, pero la cadena `"Hola, mundo"` no tiene un valor numérico al que convertirse.

El siguiente ejemplo muestra la forma correcta de usar el inicializador:

```swift
let posibleNumero = "123"
let numeroConvertido = Int(posibleNumero)
// numeroConvertido es de tipo "Int?", o "Int opcional"
```

Debido a que el inicializador puede fallar, devuelve un `Int` _opcional_, en lugar de un `Int`. Un `Int` opcional se escribe como `Int?`.

Para definir una variable como sin valor debemos asignarle el valor especial `nil`:

```swift
var codigoRespuestaServidor: Int? = 404
// codigoRespuestaServidor contine un valor Int de 404
codigoRespuestaServidor = nil
// codigoRespuestaServidor ahora no contiene ningún valor
```

Una variable opcional sin asignar ningún valor se inicializa automáticamente a `nil`:

```swift
var respuestaEncuesta: String?
// respuestaEncuesta es inicializado automáticamente a nil
```

#### Sentencias `if` y _desenvoltura forzosa_

Se puede usar un `if` para comprobar si un valor opcional es distinto de `nil`:

```swift
if numeroConvertido != nil {
    print("numeroConvertido contiene algún valor entero.")
}
// Imprime "numeroConvertido contiene algún valor entero."
```

Una vez que estamos seguros de que el opcional contiene un valor, debemos acceder a él usando un signo de exclamación (`!`). Quiere decir "Sé que hay este opcional tiene un valor concreto; por favor úsalo". Esto se conoce como _desenvoltura forzosa_ (_forced unwrapping_) del valor opcional:

```swift
if numeroConvertido != nil {
    print("numeroConvertido tiene un valor entero de \(numeroConvertido!).")
}
// Imprime "numeroConvertido tiene un valor entero de 123."
```

#### Ligado opcional

Es posible comprobar si un opcional tiene valor y asignar su valor a otra variable al mismo tiempo con una construcción llamada _ligado opcional_ (_optional binding_):

```swift
if let numeroVerdadero = Int(posibleNumero) {
    print("\"\(posibleNumero)\" tiene un valor entero de \(numeroVerdadero)")
} else {
    print("\"\(posibleNumero)\" no ha podido convertirse en un entero")
}
// Imprime ""123" tiene un valor entero de 123"
```

Podemos leer el código anterior de la siguiente forma: "Si el `Int` opcional devuelto por `Int(posibleNumero)` contiene un valor, define la constante `numeroVerdadero` con el valor contenido en el opcional".

Es posible incluir varios ligados opcionales y añadir una cláusula `where` que comprueba una condición en el caso que existan valores:

```swift
if let primerNumero = Int("4"), segundoNumero = Int("42") where primerNumero < segundoNumero {
    print("\(primerNumero) < \(segundoNumero)")
}
// Imprime "4 < 42"
```

#### Opcionales implícitamente desenvueltos

Algunas veces está claro a partir de la estructura del programa que un opcional siempre va a tener valor. En estos casos es útil eliminar la necesidad de comprobar y desenvolver el valor cada vez que se necesita. Estos valores se declaran como _opcionales implícitamente desenvueltos_ (_implicitly unwrapped optionals_). Se definen escribiendo un signo de admiración después del tipo (`String!`) en lugar del signo de interrogación (`String?`).

El valor de un opcional declarado de esta forma se desenvuelve automáticamente cada vez que se usa, sin necesidad del signo de admiración.

```swift
let posibleCadena: String? = "Una cadena opcional."
let cadenaExtraida: String = posibleCadena! // requiere un signo de admiración

let supuestaCadena: String! = "Una cadena opcional implícitamente desenvuelta" // implicitly unwrapped optional
let cadenaImplicita: String = supuestaCadena // no se necesita el signo de admiración
```

Es posible usar un opcional implícitamente desenvuelto como un opcional normal, para comprobar si tiene valor:

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

