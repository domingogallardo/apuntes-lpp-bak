
## Tema 6: Programación Orientada a Objetos con Swift (2)

Notas de clase de la semana 15

### Contenidos

- 1 Introducción, historia y características
- 2 Clases y estructuras
- 3 Propiedades
- 4 Métodos
- 5 Herencia
- 6 Inicialización
- **7 Protocolos**
- **8 Casting de tipos**
- **9 Extensiones**
- **10 Funciones operador**

----

### Bibliografía

- Swift Language Guide
    - [Protocolos](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)
    - [Casting de tipos](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TypeCasting.html#//apple_ref/doc/uid/TP40014097-CH22-ID338)
    - [Extensiones](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html#//apple_ref/doc/uid/TP40014097-CH24-ID151)
    - [Funciones operador](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-ID28)


### 7. Protocolos

Un _protocolo_ (_protocol_) define un esquema de métodos, propiedades
y otros requisitos que encajan en una tarea particular o un trozo de
funcionalidad. El protocolo puede luego ser _adoptado_ (_adopted_) por
una clase, estructura o enumarción para proporcionar una
implementación real de esos requisitos. Cualquier tipo que satisface
los requerimientos de un protocolo se dice que _se ajusta_ (_conform_)
a ese protocolo.

#### Sintaxis

Los protocolos se definen de forma similar a las clases, estructuras y enumeraciones:

```swift
protocol UnProtocolo {
    // definición del protocolo
}
```

Para definir un tipo que se ajusta a un protocolo particular se debe
poner el nombre del protocolo tras el nombre del tipo, separado por
dos puntos. Podemos listar más de un protocolo, y se separan por
comas:

```swift
struct UnStruct: PrimerProtocolo, OtroProtocolo {
    // definición del struct
}
```

Si una clase tiene una superclase, se escribe el nombre de la
superclase antes los protocolos, seguido por una coma:

```swift
class UnaClase: UnaSuperClase, PrimerProtocolo, OtroProto {
    // definición de la clase
}
```

#### Requisitos de propiedades

Un protocolo puede requerir a cualquier tipo que se ajuste a él que
proporcione una propiedad de instancia o de tipo con un nombre y tipo
particular. El protocolo no especifica si la propiedad es una
propiedad calculada o almacenada, sólo especifica el nombre y el tipo
de la propiedad requerida. El protocolo también especifica si la
propiedad debe ser de lectura y escritura o sólo de lectura.

Si un protocolo requiere que una propiedad sea de lectura y escritura,
el requisito no puede ser satisfecho por una propiedad constante
almacenada o por una propiedad calculada de sólo lectura. Si el
protocolo sólo requiere que la propiedad sea de lectura, el requisito
puede ser satisfecho por cualquier tipo de propiedad, y es válido que
la propiedad sea también de escritura si es útil para nuestro propio
código.

Los requisitos de la propiedad se declaran siempre como propiedades
variables, precedido por la palabra clave `var`. Las propiedades de
lectura y escritura se indican escribiendo `{ get set }` después de la
declaración de su tipo, y las propiedades de sólo lectura se indican
escribiendo `{ get }`.


```swift
protocol UnProtocolo {
    var debeSerEscribible: Int { get set }
    var noTienePorQueSerEscribible: Int { get }
}
```

Para definir una propiedad de tipo hay que precederla en el protocolo
con la palabra clave `static`:

```swift
protocol OtroProtocolo {
    static var unaPropiedadDeTipo: Int { get set }
}
```

Veamos un ejemplo. Definimos el protocolo `TieneNombre` en el que se
requiere que cualquier clase que se ajuste a él debe tener una
propiedad de instancia de lectura de tipo `String` que se llame
`nombreCompleto`:

```swift
protocol TieneNombre {
    var nombreCompleto: String { get }
}
```

Un ejemplo de una sencilla estructura que adopta el protocolo:

```swift
struct Persona: TieneNombre {
    var edad: Int
    var nombreCompleto: String
}

let john = Persona(nombreCompleto: "John Appleseed")
// john.nombreCompleto es "John Appleseed"
```

Este ejemplo define una estructure llamada `Persona`, que representa
una persona con una edad y un nombre específico. En la primera línea
se declara que se adopta el protocolo `TieneNombre`. Cada instancia de
`Persona` tiene la propiedad almacenada llamada `nombreCompleto`, que
es de tipo `String`. Esto cumple el único requisito del protocolo
`TieneNombre`, y signifca que `Persona` se ajusta correctamente al
protocolo (Swift informa de un error en tiempo de compilación si un
requisito de un protocolo no se cumple).

Otro ejemplo de una clase más compleja, que también adopta el
protocolo:

```swift
class NaveEstelar: TieneNombre {
    var prefijo: String?
    var nombre: String
    init(nombre: String, prefijo: String? = nil) {
        self.nombre = nombre
        self.prefijo = prefijo
    }
    var nombreCompleto: String {
        return (prefijo != nil ? prefijo! + " " : "") + nombre
    }
}
var ncc1701 = NaveEstelar(nombre: "Enterprise", prefijo: "USS")
// ncc1701.nombreCompleto es "USS Enterprise"
```

Esta clase implementa el requisito de la propiedad `nombreCompleto`
como una propiedad calculada de solo lectura para una nave
estelar. Cada instancia de `NavaEstelar` almacena un nombre
obligatorio y un prefijo opcional. La propiedad `nombreCompleto` usa
el valor del prefijo si existe, y la añade al comienzo del nombre para
crear un nombre completo de la nave estelar.

#### Requisitos de métodos

Los protocolos pueden requerir que los tipos que se ajusten a ellos
implementen métodos de instancia y de tipos específicos. Estos métodos
se escriben como parte de la definición del protocolo de la misma
forma que los métodos normales, pero sin sus cuerpos. Los métodos del
tipo en el protocolo deben indicarse con la palabra clave `static`:

```swift
protocol UnProtocolo {
    static func unMetodoDelTipo()
}
```

Por ejemplo:

```swift
protocol GeneradorNumerosAleatorios {
    func random() -> Double
}
```

Este protocolo, `GeneradorNumerosAleatorios`, requiere que cualquier
tipo que se ajuste a él tenga un método de instancia llamado `random`,
que devuelve un valor `Double` cada vez que se llama. Aunque no está
especificado en el protocolo, se asume que este valor será un número
entre 0.0 y 1.0 (sin incluirlo). El protocolo
`GeneradorNumerosAleatorios` no hace ninguna suposición sobre cómo
será generado cada número aleatorio, simplemente requiere al generador
que proporcione una forma estándar de generarlo.

Una implementación de una clase que adopta el protocolo:

```swift
class GeneradorLinealCongruente: GeneradorNumerosAleatorios {
    var ultimoRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        ultimoRandom = ((ultimoRandom * a + c) % m)
        return ultimoRandom / m
    }
}
let generador = GeneradorLinealCongruente()
print("Un número aleatorio: \(generador.random())")
// Imprime "Un número aleatorio: 0.37464991998171"
print("Y otro: \(generador.random())")
// Imprime "Y otro: 0.729023776863283"
```

#### Requisito de método `mutating`

Si definimos un protocolo con un requisito de método de instancia que
pretenda mutar las instancias del tipo que adopte el protocolo, se
debe marcar el método con la palabra `mutating`. Esto permite a las
estructuras y enumeraciones que adopten el protocolo definir ese
método como `mutating`. No es necesario hacerlo con las clases, porque
la palabra `mutating` solo es necesaria en estructuras y
enumeraciones.

Un ejemplo:

```swift
protocol Conmutable {
    mutating func conmutar()
}

enum Interruptor: Conmutable {
    case Apagado, Encendido
    mutating func conmutar() {
        switch self {
        case Apagado:
            self = Encendido
        case Encendido:
            self = Apagado
        }
    }
}

var interruptorLampara = Interruptor.Apagado
interruptorLampara.conmutar()
// interruptorLampara es ahora igual a .Encendido
```

#### Protocolos como tipos

Los protocolos no implementan realmente ninguna funcionalidad por
ellos mismos. Sin embargo, cualquier protocolo que definamos se
convierte automáticamente en un tipo con todas sus propiedades que
podemos usar en nuestro código.

Podemos entonces usar el protocolo en cualquier sitio donde permitamos
otros tipos, incluyendo:

- El tipo de un parámetro de una función, método o inicializador o de
  sus valores devueltos.
- El tipo de una constante, variable o propiedad
- El tipo de los ítems de un array, diccionario u otro contenedor

```swift
class Dado {
    let caras: Int
    let generador: GeneradorNumerosAleatorios
    init(caras: Int, generador: GeneradorNumerosAleatorios) {
        self.caras = caras
        self.generador = generador
    }
    func tirar() -> Int {
        return Int(generador.random() * Double(caras)) + 1
    }
}
```

Este ejemplo define una nueva clase llamada `Dado`, que representa un
dado de _n_ caras que se puede usar en un juego de tablero. Las
instancias de dados tienen una propiedad llamada `caras`, que
representa cuántas caras tienen, y una propiedad llamada `generador`,
que proporciona un generador a partir del cual crear valores de
tiradas.

La propiedad generador es del tipo
`GeneradorNumerosAleatorios`. Podemos asignarle una instancia de
cualquier tipo que adopte el protocolo `GeneradorNumerosAleatorios`.

`Dado` tiene también un inicializador, para configurar sus estado
inicial. El inicializador tiene un parámetro llamado `generador`, que
también es del tipo `GeneradorNumerosAleatorios`. Podemos pasarle un
valor de cualquier instancia que se ajuste a este tipo. Y también
proporciona un método de instancia llamado `tirar`, que devuelve un
valor entero entre 1 y el número de caras del dado. Este método llama
al método `random()` del generador para crear un nuevo número
aleatorio entre 0.0 y 1.0 y usa este número aleatorio para crear un
valor de tirada que esté dentro del rango correcto. Debido a que
sabemos que el generador se ajusta al protocolo
`GeneradorNumerosAleatorios` tenemos la garantía de que va a existir
un método `random()` al que llamar.

Un ejemplo de uso del código:

```swift
var d6 = Dado(caras: 6, generador: GeneradorLinealCongruente())
for _ in 1...5 {
    print("La tirada del dado es \(d6.tirar())")
}
// La tirada del dado es 3
// La tirada del dado es 5
// La tirada del dado es 4
// La tirada del dado es 5
// La tirada del dado es 4
```

#### Colecciones de tipos protocolo

Como hemos comentado anteriormente, un protocolo puede usarse como el
tipo que se almacena un una colección (array, diccionario,
etc.). Veamos un ejemplo:

```swift
var peterParker = Persona(edad: 24, nombreCompleto: "Peter Parker")
var ncc1701 = NaveEstelar(nombre: "Enterprise", prefijo: "USS")

let cosasConNombre: [TieneNombre] = [peterParker, ncc1701]

for cosa in cosasConNombre {
   print(cosa.nombreCompleto)
}
// Peter Parker
// USS Enterprise
```

Hay que hacer notar que la constante `cosa` que itera sobre los
elementos del array es de tipo `TieneNombre`], no es de tipo `Persona`
ni de tipo `NaveEstelar`, incluso aunque las instancias que hay tras
de escena son do esos tipos. Por ser del tipo `TieneNombre` sabemos
que tiene una propiedad `nombreCompleto` que podemos usar sobre la
variable iteradora.


### 8. Casting de tipos

El _casting_ de tipos es una forma de comprobar el tipo de una
instancia o de tratar esa instancia como de una superclase distinta o
conseguir una subclase de algún otro sitio en la propia jerarquía de
clase. La forma de implementarlo es utilizando los operadores `is` y
`as`. Estos operadores proporcionan una forma simple y expresiva de
comprobar el tipo de un valor o transformar un valor en uno de otro
tipo. También se puede usar el _casting_ de tipos para comprobar si un
tipo se ajusta a un protocolo.

#### Una jerarquía de clases para el casting de tipos

Vamos a comenzar construyendo una jerarquía de clases y subclases con
las que trabajar. Utilizaremos el _casting_ de tipos para comprobar el
tipo de una instancia particular de una clase y para convertir esa
instancia en otra clase dentro de la misma jerarquía.

En el primer fragmento de código definimos una clase nueva llamada
`MediaItem`. Esta clase proporciona la funcionalidad básica de
cualquier tipo de ítem que aparece en una biblioteca de medios
digitales. Específicamente, declara una propiedad `nombre` de tipo
`String` y un inicializador `init nombre` (suponemos que todos los
ítems, incluyendo películas y canciones, tendrán un nombre).

```swift
class MediaItem {
    var nombre: String
    init(nombre: String) {
        self.nombre = nombre
    }
}
```

El siguiente fragmento define dos subclases de `MediaItem`. La primera
subclase, `Pelicula`, encapsula información adicional sobre una
película. Añade una propiedad `director` a la clase base `MediaItem`,
con su correspondiente inicializador. La segunda subclase, `Cancion`,
añade una propiedad `artista` y un inicializador a la clase base:

```swift
class Pelicula: MediaItem {
    var director: String
    init(nombre: String, director: String) {
        self.director = director
        super.init(nombre: nombre)
    }
}

class Cancion: MediaItem {
    var artista: String
    init(nombre: String, artista: String) {
        self.artista = artista
        super.init(nombre: nombre)
    }
}
```

Por último, creamos un array constante llamado `biblioteca`, que
contienen dos instancias de `Pelicula` y tres instancias de
`Cancion`. El tipo del array se infiere a partir de la
inicialización. El compilador de Swift es capaz de deducir que
`Pelicula` y `Cancion` tienen una superclase común `MediaItem`, por lo
que infiriere `[MediaItem]` como el tipo del array:

```swift
let biblioteca = [
    Pelicula(nombre: "El Señor de los Anillos", director: "Peter Jackson"),
    Cancion(nombre: "Child in Time", artista: "Deep Purple"),
    Pelicula(nombre: "El Puente de los Espías", director: "Steven Spielberg"),
    Cancion(nombre: "I Wish You Were Here", artista: "Pink Floyd"),
    Cancion(nombre: "Yellow", artista: "Coldplay")
]
```

Los ítems almacenados en la biblioteca son todavía instancias de
`Pelicula` y `Cancion`. Sin embargo, si iteramos sobre los contenidos
de este array, los ítems que recibiremos tendrán el tipo `MediaItem` y
no `Pelicula` o `Cancion`. Para trabajar con ellos como su tipo
nativo, debemos chequear su tipo, y hacer un _downcast_ a su tipo
concreto.

#### Comprobación del tipo

Podemos usar el _operador de comprobación_ (_check operator_) `is`
para comprobar si una instancia es de un cierto tipo subclase. El
operador de comprobación devuelve `true` si la instancia es del tipo
de la subclase y `false` si no.

Lo podemos comprobar en el siguiente ejemplo, en el que contamos las
instancias de películas y canciones en el array `biblioteca`:

```swift
var contadorPeliculas = 0
var contadorCanciones = 0

for item in biblioteca {
    if item is Pelicula {
        contadorPeliculas += 1
    } else if item is Cancion {
        contadorCanciones += 1
    }
}

print("La biblioteca contiene \(contadorCanciones) películas y \(contadorPeliculas) canciones")
// Imprime "La biblioteca contiene 3 películas y 2 canciones"
```

El ejemplo itera por todos los ítems del array `biblioteca`. En cada
paso, el bucle `for-in` guarda en la constante `item` el siguiente
`MediaItem` del array.

La instrucción `item is Pelicula` devuelve `true` si el `MediaItem`
actual es una instancia de `Pelicula` y `false` en otro caso. De forma
similar, `item is Cancion` comprueba si el ítem es una instancia de
`Cancion`. Al final del bucle `for-in`, los valores de
`contadorPeliculas` y `contadorCanciones` contendrán una cuenta de
cuantas instancias `MediaItem` de cada tipo se han encontrado.


#### Downcasting

Una constante o variable de un cierto tipo de clase puede referirse
(contener) a una instancia de una subclase. Cuando creemos que sucede
esto, podemos intentar hacer un _downcast_ al tipo de la subclase con
un operador de _cast_ (`as?` o `as!`). Como el _downcast_ puede
fallar, se utilizan estas las versiones anteriores. La versión
condicional, `as?`, devuelve un valor opcional del tipo al que estamos
intentando hacer el _downcasting_. La versión forzosa, `as!`, intenta
el _downcast_ y fuerza la desenvoltura del resultado en un única
acción compuesta.

Debemos usar la versión condicional (`as?`) cuando no estamos seguros
si el _downcast_ tendrá éxito. Se devolverá un valor opcional y el
valor será `nil` si no es posible hacer el _downcast_. Esto permitirá
comprobar si ha habido un _downcast_ con éxito.

La otra versión (`as!`) se usa sólo cuando estamos seguros de que el
_downcast_ tendrá éxito. Esta versión del operador lanzará un error en
tiempo de ejecución si intentamos hacer un _downcast_ a un tipo
incorrecto.

El siguiente ejemplo itera sobre cada `MediaIyem` en `biblioteca`, e
imprime una descripción apropiada para cada ítem. Para hacerlo,
necesita acceder a cada ítem como una `Pelicula` o `Cancion` y no sólo
como una `MediaItem`. Esto es necesario para poder acceder a la
propiedad `director` o `artista` de una instancia de `Pelicula` o
`Cancion`.

En este ejemplo, cada ítem en el array podría ser un `Pelicula` o
podría ser una `Cancion`. No sabemos por anticipado la clase verdadera
de cada ítem, por lo que es apropiado usar la versión condicional
(`as?`) para comprobar el _downcast_ cada vez a lo largo del bucle:

```swift
for item in biblioteca {
    if let pelicula = item as? Pelicula {
        print("Película: \(pelicula.nombre), dir. \(pelicula.director)")
    } else if let cancion = item as? Cancion {
        print("Cancion: \(cancion.nombre), de \(cancion.artista)")
    }
}

// Película: El Señor de los Anillos, dir. Peter Jackson
// Cancion: Child in Time, de Deep Purple
// Película: El Puente de los Espías, dir. Steven Spielberg
// Cancion: I Wish You Were Here, de Pink Floyd
// Cancion: Yellow, de Coldplay
```

El ejemplo comienza intentando hacer `downcast` del ítem a una
`Pelicula`. Debido a que es una instancia de `MediaItme`, es posible
que sea un `Pelicula` o una `Cancion`, o incluso el tipo base
`MediaItem`. Debido a esta incertidumbre, debemos usar la versión
`as?` para devolver un valor opcional. El resultado será una "Pelicula
opcional". Podemos desenvolver el valor `Pelicula` usando un `if let`
como vimos en el apartado de opcionales. Si tiene éxito el
_downcasting_, las propiedades de la película se pueden usar para
imprimir una descripción de la película llamando a los
correspondientes métodos de la clase `Pelicula`. Igual con `Cancion`.


#### _Casting_ para `Any` 

El tipo `Any` puede representar una instancia de cualquier tipo,
incluyendo tipos función:

```swift
var array = [Any]()

array.append(0)
array.append(0.0)
array.append(42)
array.append(3.14159)
array.append("hola")
array.append((3.0, 5.0))
array.append(Pelicula(nombre: "Ghostbusters", director: "Ivan Reitman"))
array.append({ (name: String) -> String in "Hola, \(name)" })
```

Las array contiene dos valores `Int`, dos valores `Double`, un valor
`String`, una tupla del tipo `(Double, Double)`, la película
"Ghostbusters", y una clausura que toma un `String` y devuelve otro
`String`.

Puedes usar los operadores `is` y `as` en una sentencia `switch` para
descubrir en tiempo de ejecución el tipo específico de una constante o
variable de la que sólo se sabe que es de tipo `Any`:

```swift
for item in array {
    switch item {
    case 0 as Int:
        print("cero como un Int")
    case 0 as Double:
        print("cero como un Double")
    case let someInt as Int:
        print("un valor entero de \(someInt)")
    case let unDouble as Double where unDouble > 0:
        print("a valor positivo de \(unDouble)")
    case is Double:
        print("algún otro valor double que no quier imprimir")
    case let someString as String:
        print("una cadena con valor de \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("un punto (x, y) en \(x), \(y)")
    case let pelicula as Pelicula:
        print("una película: \(pelicula.nombre), dir. \(pelicula.director)")
    case let stringConverter as String -> String:
        print(stringConverter("Michael"))
    default:
        print("alguna otra cosa")
    }
}

// cero como un Int
// cero como un Double
// un valor entero de 42
// a valor positivo de 3.14159
// una cadena con valor de "hola"
// un punto (x, y) en 3.0, 5.0
// una película: Ghostbusters, dir. Ivan Reitman
// Hola, Michael
```

#### Comprobación de ajustarse a un protocolo

Podemos usar también los operadores anteriores `is` y `as` (y `as?` y
`as!`) para comprobar si una instancia se ajusta a un protocolo y para
hacer un _cast_ a un protocolo específico.

Veamos un ejemplo. Definimos el protocolo `TieneArea` con el único
requisito de una propiedad de lectura llamada `area` de tipo `Double`:

```swift
protocol TieneArea {
    var area: Double { get }
}
```

Definimos dos clases `Circulo` y `Pais` que se ajustan ambos al protocolo: 

```swift
class Circulo: TieneArea {
    let pi = 3.1415927
    var radio: Double
    var area: Double { return pi * radio * radio }
    init(radio: Double) { self.radio = radio }
}

class Pais: TieneArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

La clase `Circulo` implementa el requisito como una propiedad
calculada, basada en la propiedad almacenada `radio`. La clase `Pais`
implementa el requisito directamente como una propiedad
almacenada. Ambas clases se ajustan correctamente al protocolo
`TieneArea`.

Definimos una clase `Animal` que no se ajusta al protocolo:

```swift
class Animal {
    var patas: Int
    init(patas: Int) { self.patas = patas }
}
```

Las clases `Circulo`, `Pais` y `Animal` no tienen ninguna clase base
compartida. Sin embargo, todas son clases, por lo que las instancias
de los tres tipos pueden usarse para inicializar un array que almacena
valores de tipo `Any`:

```swift
let objetos: [Any] = [
    Circulo(radio: 2.0),
    Pais(area: 243_610),
    Animal(patas: 4)
]
```

Y ahora podemos iterar sobre el array de objetos, comprobando para
cada ítem si la instancia se ajusta al protocolo `TieneArea`:

```swift
for objecto in objetos {
    if let objetoConArea = objecto as? TieneArea {
        print("El área es \(objetoConArea.area)")
    } else {
        print("Algo que no tiene un área")
    }
}

// El área es 12.5663708
// El área es 243610.0
// Algo que no tiene un área
```

Cuando un objeto en el array se ajusta al protocolo `TieneArea`, el
valor opcional devuelto por el operador `as?` se desenvuelve con un
ligado opcional en una constante llamada `objetoConArea`. Esta
constante tiene el tipo `TieneArea`, por lo que su propiedad `area`
podrá ser accedida e impresa.

Hay que notar que los objetos subyacentes no cambian en el proceso de
_casting_. Siguen siendo un `Circulo`, un `Pais` y un `Animal`. Sin
embargo, en el momento en se almacenan en la constante
`objetoConArea`, sólo se sabe que son del tipo `TieneArea`, por lo que
sólo podremos acceder a su propiedad `area`.


### 9. Extensiones

Las _extensiones_ añaden nueva funcionalidad a una clase, estructura,
enumeración o protocolo. Esto incluye la posibilidad de extender tipos
para los que no tenemos acceso al código fuente original (esto se
conoce como _modelado retroactivo_).

Entre otras cosas, las extensiones pueden: 

- Añadir propiedades calculadas de instancia y de tipo
- Definir métodos de instancia y de tipo
- Proporcionar nuevos inicializadores
- Hacer que un tipo existente se ajuste a un protocolo



#### Sintaxis

Para declarar una extensión hay que usar la palabra clave `extension`:

```swift
extension UnTipo {
    // nueva funcionalidad para añadir a UnTipo
}
```

#### Propiedades calculadas

Las extensiones pueden añadir propiedades calculadas de instancias y
de tipos. Como primer ejemplo, vamos a añadir a la clase persona la
propiedad calcula `mayorEdad`, un `Bool` que indica si la edad de la
persona es mayor o igual de 18:

```swift
extension Persona {
   var mayorEdad: Bool {
      return edad >= 18
   }
}
```

Una vez definida esta extensión, hemos ampliado la clase con esta
nueva propiedad, sin tocar el código de la clase. Podemos preguntar si
una persona es mayor de edad:

```swift
var p = Persona(edad: 15, nombreCompleto: "Lucía")
p.mayorEdad // false
```

En este otro ejemplo añadimos cinco propiedades de instancia
calculadas al tipo de Swift Double, para proporcionar un soporte
básico para trabajar con unidades de distancia:

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let unaPulgada = 25.4.mm
print("Una pulgada es \(unaPulgada) metros")
// Una pulgada es 0.0254 metros
let tresPies = 3.ft
print("Tres pies son \(tresPies) metros")
// Tres pies son 0.914399970739201 metros
```

Estas propiedades calculadas expresan que un valor `Double` debería
considerarse como una cierta unidad de longitud. Aunque se implementan
como propiedades calculadas, los nombres de las propiedades pueden
añadirse a un literal en punto flotante con la sintaxis del punto,
como una forma de usar el valor del literal para ejecutar conversiones
de distancia.

En este ejemplo, un valor `Double` de 1.0 se considera que representa
"un metro". Esto por lo que la propiedad calculada `m` devuelve
`self` - la expresión `1.m` devuelve el valor `Double` de 1.0.

Otras unidades requieren alguna conversión para expresarse como un
valor medido en metros. Un kilómetro es 1,000 metros, por lo que la
propiedad calculada `km` multiplica el valor por 1_000.00 para
conventirlo en un número expresado en metros. De forma similar, hay
3.28084 pies en un metro, por lo que la propiedad calculada `ft`
divide el valor `Double` subyacente por 3.28084, para conventirlo de
pies a metros.

Estas propieades son propiedades calculadas de solo lectura, por lo
que se expresan sin la palabra clave `get`, por brevedad. Sus valores
devueltos son de tipo `Double`, y pueden usarse en cálculos
matemáticos en cualquier sitio que se acepte un `Double`:

```Swift
let unMaraton = 42.km + 195.m
print("Un maratón tiene una longitud de \(unMaraton) metros")
// Un maratón tiene una longitud de 42195.0 metros
```


#### Inicializadores

Las extensiones pueden añadir nuevos inicializadores a tipos
existentes. Esto nos permite extender otros tipos para aceptar
nuestros propios tipos como parámetros de la inicialización, o para
proporcionar opciones adicionales que no estaban incluidos en la
implementación original del tipo.

El siguiente ejemplo define una estructura `Rectangulo` que representa
un rectángulo geométrico. Este ejemplo también define dos estructuras
auxiliares llamadas `Tamaño` y `Punto`, que proporcionan valores por
defecto de 0.0 a todas sus propiedades:

```swift
struct Tamaño {
    var ancho = 0.0, alto = 0.0
}
struct Punto {
    var x = 0.0, y = 0.0
}
struct Rectangulo {
    var origen = Punto()
    var tamaño = Tamaño()
}
```

Debido a que la estructura `Rectangulo` proporciona valores por
defecto para todas sus propiedades, tiene un inicializador por defecto
que puede utilizarse para crear nuevas instancias. También podemos
inicializarlo asignando todas sus propieades:

```swift
let rectanguloPorDefecto = Rectangulo()
let rectanguloInicializado = Rectangulo(origen: Punto(x: 2.0, y: 2.0),
                                tamaño: Tamaño(ancho: 5.0, alto: 5.0))
```

Podemos extender la estructura `Rectangulo` para proporcionar un
inicializador adicional que toma un punto específico del centro y un
tamaño:

```swift
extension Rectangulo {init(centro: Punto, tamaño: Tamaño) {
                                        let origenX = centro.x - (tamaño.ancho / 2)
                                        let origenY = centro.y - (tamaño.alto / 2)
                                        self.init(origen: Punto(x: origenX, y: origenY), tamaño: tamaño)
                                    }
 }
```

Este nuevo inicializador empieza por calcular un punto de origen
basado en el centro propuesto y en el tamaño. El inicializador llama
después al inicializador automático de la estructura
`init(origen:tamaño:)`, que almacena los nuevos valores de las
propiedades:

```swift
 let rectanguloCentro = Rectangulo(centro: Punto(x: 4.0, y: 4.0),
                           tamaño: Tamaño(ancho: 3.0, alto: 3.0))
 // el origen del rectanguloCentro es is (2.5, 2.5) y su tamaño es (3.0, 3.0)
```

#### Métodos

Las extensiones pueden añadir nuevos métodos de instancia y nuevos
métodos del tipo. El siguiente ejemplo añade un nuevo método de
instancia llamado `repeticiones` al tipo `Int`:

```swift
extension Int {
    func repeticiones(tarea: () -> Void) {
        for _ in 0..<self {
            tarea()
        }
    }
}
```

El método `repeticiones(_:)` toma un único argumento de tipo `() ->
Void`, que indica una función que no tiene parámetros y no devuelve
ningún valor. Después de definir esta extensión, podemos llamar al
método `repeticiones(_:)` en cualquier número entero para ejecutar una
tarea un cierto número de veces:


```swift
3.repeticiones({
   print("¡Hola!")
})
// ¡Hola!
// ¡Hola!
// ¡Hola!
```

Usando clausuras por la cola podemos hacer la llamada más concisa:

```swift
3.repeticiones {
   print("¡Adios!")
}
// ¡Adios!
// ¡Adios!
// ¡Adios!
```

#### Métodos de instancia mutadores

Los métodos de instancia añadidos con una extensión también pueden
modificar (o mutar) la propia instancia. Los métodos de las
estructuras y los enumerados que modifican `self` o sus propiedades
deben marcarse como `mutating`.

Por ejemplo: 

```swift
extension Int {
    mutating func cuadrado() {
        self = self * self
    }
}
var unInt = 3
unInt.cuadrado()
// unInt es ahora 9
```


#### Ajustar un tipo a un protocolo mediante una extensión


Una extensión puede extender un tipo existente para hacer que se
ajuste a uno o más protocolos. En este caso, los nombres de los
protocolos se escriben exactamente de la misma forma que en una clase
o estructura:


```swift
extension UnTipo: UnProtocolo, OtroProtocolo {
    // implementación de los requisitos del protocolo
}
```

Las instancias del tipo adoptarán automáticamente el protocolo y se
ajustarán al protocolo con las propiedades y métodos añadidos por la
extensión.

Por ejemplo, este protocolo, llamado `RepresentableComoTexto` puede
ser implementado por cualquier tipo que tenga una forma de
representarse como texto. Esto puede ser una descripción de si mismo o
una versión de texto de su estado actual:

```swift
protocol RepresentableComoTexto {
    var descripcionTextual: String { get }
}
```

La clase `Dado` que vimos anteriormente puede extenderse para
ajustarse al protocolo:

```swift
extension Dado: RepresentableComoTexto {
    var descripcionTextual: String {
        return "Un dado de \(caras) caras"
    }
}
```

Esta extensión adopta el nuevo protocolo exactamente como si el `Dado`
lo hubiera proporcionado en su implementación original. El nombre del
protocolo se indica tras el nombre del tipo, separado por dos puntos,
y se proporciona una implementación entre llaves.

Cualquier instancia de un dado se puede tratar ahora como `RepresentableComoTexto`:

```swift
let d12 = Dado(caras: 12, generador: GeneradorLinealCongruente())
print(d12.descripcionTextual)
// Un dado de 12 caras
```

De forma similar, un `Rectangulo` también puede extenderse para
adoptar y ajustarse al protocolo:

```swift
extension Rectangulo: RepresentableComoTexto {
   var descripcionTextual: String {
      return "Un rectángulo situado en (\(origen.x), \(origen.y))"
   }
}
let rectanguloInicializado = Rectangulo(origen: Punto(x: 2.0, y: 2.0),
                                tamaño: Tamaño(ancho: 5.0, alto: 5.0))

print(rectanguloInicializado.descripcionTextual)
// Un rectángulo situado en (2.0, 2.0)
```

#### Declaración de la adopción de un protocolo con una extensión

Si un tipo ya se ajusta a todos los requisitos de un protocolo, pero
todavía no se ha declarado que se ajusta al protocolo, podemos hacer
que lo adopte con una extensión vacía:

```swift
struct Hamster {
    var nombre: String
    var descripcionTextual: String {
        return "Un hamster llamado \(nombre)"
    }
}
extension Hamster: RepresentableComoTexto {}
```

Las instancias de `Hamster` pueden ahora usarse en cualquier sitio que
se requiera un tipo `RepresentableComoTexto`:

```swift
let simonElHamster = Hamster(nombre: "Simon")
let algoRepresentableComoTexto: RepresentableComoTexto = simonElHamster
print(algoRepresentableComoTexto.descripcionTextual)
// Un hamster llamado Simon
```

### 10. Funciones operadoras

Las clases y las estructuras pueden proporcionar sus propias
implementaciones de operadores existentes. Esto se conoce como
_sobrecarga_ de los operadores existentes.

En el siguiente ejemplo se muestra cómo implementar el operador de
suma (`+`) para una estructura. El operador suma es un operador
binario (tiene dos operandos) e infijo (aparece junto entre los dos
operandos). Definimos una estructura `Vector2D` para un vector de
posición de dos dimensiones:


```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}
func + (izquierdo: Vector2D, derecho: Vector2D) -> Vector2D {
    return Vector2D(x: izquierdo.x + derecho.x, y: izquierdo.y + derecho.y)
}
```

La función operador se define como una función global con un un nombre
de función que empareja con el operador a sobrecargar (`+`). Debido a
que la suma aritmética se define como un operador binario, esta
función operador toma dos parámetros de entrada de tipo `Vector2D` y
devuelve un único valor de salida, también de tipo `Vector2D`.

En esta implementación, llamamos a los parámetros de entrada
`izquierdo` y `derecho` para representar las instancias de `Vector2D`
que estarán a la izquierda y a la derecha del operador `+`. La función
devuelve una nueva instancia de `Vector2D`, cuyas propiedades `x` e
`y` se inicializan con la suma de las propiedades `x` e `y` de las
instancias de `Vector2D` que se están sumando.

La función se define globalmente, más que como un método en la
estructura `Vector2D`, para que pueda usarse como un operador infijo
entre instancias existentes de `Vector2D`:


```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let otroVector = Vector2D(x: 2.0, y: 4.0)
let vectorSuma = vector + otroVector
// vectorSuma es una instancia de Vector2D con valores de (5.0, 5.0)
```

#### Operadores prefijos y postfijos

El ejemplo anterior demuestra una implementación propia de un operador
binario infijo. Las clases y las estructuras pueden también
proporcionar implementaciones de los operadores unarios estándar. Los
operadores unarios operan sobre un único objetivo. Son prefijos se
preceden el objetivo (como `-a`) y postfijos si siguen su objetivo
(como en `b!`).

Para implementar un operador unario prefijo o postfijo se debe
escribir el modificador `prefix` o `postfix` antes de la palabra clave
`func` en la declaración de la función operador:

```swift
prefix func - (vector: Vector2D) -> Vector2D {
    return Vector2D(x: -vector.x, y: -vector.y)
}
```

El ejemplo anterior implementa el operador unario negación (`-a`) para
instancias de `Vector2D`.

Por ejemplo:

```swift
let positivo = Vector2D(x: 3.0, y: 4.0)
let negativo = -positivo
// negativo es una instancia de Vector2D con valores de (-3.0, -4.0)
let tambienPositivo = -negativo
// tambienPositivo es una instancia de Vector2D con valores de (3.0, 4.0)
```

#### Operadores de equivalencia

Las clases y estructuras construidas no tienen una implementación por
defecto de los operadores "igual a" (`==`) y "no igual a" (`!=`). No
es posible para Swift adivinar qué valores serán "iguales" en nuestras
clases, porque el significado de "igual" depende del papel que esos
tipos juegan en nuestro código.

Para poder usar los operadores de igualdad en nuestros propios tipos,
debemos proporcionar una implementación de la misma forma que para
otros operadores infijos:

```swift
func == (izquierdo: Vector2D, derecho: Vector2D) -> Bool {
    return (izquierdo.x == derecho.x) && (izquierdo.y == derecho.y)
}
func != (izquierdo: Vector2D, derecho: Vector2D) -> Bool {
    return !(izquierdo == derecho)
}
```

En el ejemplo anterior se implementa un operador "igual a" (`==`) que
comprueba si dos instancias de `Vector2D` tienen valores
equivalentes. En el contexto del `Vector2D` tiene sentido considerar
"igual" como "ambas instancias tienen los mismos valores x e y", por
lo que esta es la lógica usada por la implementación. También se
implementa el operador "no igual a" (`!=`), que simplemente devuelve
el inversa del resultado del operador "igual a".

Ahora podemos usar estos operadores para chequear si dos instancias de
`Vector2D` son equivalentes:

```swift
let dosTres = Vector2D(x: 2.0, y: 3.0)
let otroDosTres = Vector2D(x: 2.0, y: 3.0)
if dosTres == otroDosTres {
    print("Los dos vectores son equivalentes.")
}
// Los dos vectores son equivalentes
```
