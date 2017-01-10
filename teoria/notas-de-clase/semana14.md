
## Tema 6: Programación Orientada a Objetos con Swift

Notas de clase de la semana 14

### Contenidos

- **1 Introducción, historia y características**
- **2 Clases y estructuras**
- **3 Propiedades**
- **4 Métodos**
- **5 Herencia**
- **6 Inicialización**
- 7 Protocolos
- 8 Casting de tipos
- 9 Programación Orientada a Protocolos
- 10 Extensiones

----

### Bibliografía

- Swift Language Guide
    - [Classes and Structures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-ID82)
    - [Properties](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-ID254)
    - [Methods](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Methods.html#//apple_ref/doc/uid/TP40014097-CH15-ID234)
    - [Inheritance](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Inheritance.html#//apple_ref/doc/uid/TP40014097-CH17-ID193=)
    - [Initialization](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID203)


### <a name="1"></a> 1. Introducción, historia y características

#### Nacimiento

- La Programación Orientada a Objetos es un paradigma de programación que explota en los 80 pero nace a partir de ideas a finales de los 60 y 70
- Primer lenguaje con las ideas fundamentales de POO: Simula
- Smalltalk como lenguaje paradigmática de POO
- Alan Kay es el creador del término “Object-Oriented”
- Artículo de Alan Kay: [“The Early History of Smalltalk”](http://gagne.homedns.org/%7etgagne/contrib/EarlyHistoryST.html), ACM SIGPLAN, March 1993

#### Alan Kay

> “I invented the term Object-Oriented and I can tell you I did not have C++ in mind.”

> “Smalltalk is not only NOT its syntax or the class library, it is not even about classes. I'm sorry that I long ago coined the term objects for this topic because it gets many people to focus on the lesser idea. The big idea is messaging.”

> “Smalltalk's design–and existence–is due to the insight that everything we can describe can be represented by the recursive composition of a single kind of behavioral building block that hides its combination of state and process inside itself and can be dealt with only through the exchange of messages.”

#### ¿Interesados en Smalltalk?

Visitar:

- [http://www.squeak.org/](http://www.squeak.org/)
- [http://swiki.agro.uba.ar/small_land](http://swiki.agro.uba.ar/small_land)
- [http://www.squeakland.org](http://www.squeakland.org)

#### Lenguajes OO

- Smalltalk, Java, Scala, Ruby, Python, C#, C++, Swift, ...

#### Del paradigma imperativo al OO

- Programación procedural: estado abstracto (tipos de datos y barrera de abstracción) + funciones
- Siguiente paso: agrupar estado y funciones en una única entidad
- Los objetos son estas entidades


#### Características de la POO

- Objetos (creados/instanciados en tiempo de ejecución) y clases (plantillas estáticas/tiempo de compilación)
- Los objetos agrupan estado y conducta (métodos)
- Los métodos se invocan mediante mensajes
- *Dispatch dinámico*: cuando una operación es invocada sobre un objeto, el propio objeto determina qué código se ejecuta. Dos objetos con la misma interfaz pueden tener implementaciones distintas.
- Herencia: las clases se pueden definir utilizando otras clases como plantillas y modificando sus métodos y/o variables de instancia.

#### Clases y objetos

Objeto:

- Un objeto contiene un estado (propiedades, atributos o variables de instancia) y un conjunto de funciones (métodos) que se ejecutan en el ámbito del objeto e implementan las funcionalidades soportadas
- Al ejecutar un método, el objeto modifica su estado
- Pedimos a un objeto que ejecute un método

Clase:

- Una clase es la plantilla que sirve para definir los objetos
- En una clase se define los elementos que componen el objeto (sus atributos o campos) y sus métodos
- En algunos lenguajes se pueden definir también en las clases variables (variables de clase) compartidas por todos los objetos de esa clase

#### Lenguajes POO dinámicos vs. estáticos

Dos tendencias:

- Lenguajes **dinámicos**: muchas características del programa se obtienen en tiempo de ejecución
    - Mayor flexibilidad y generalidad del código
    - Dispatch dinámico
    - Reflexión (posibilidad de consultar características de la instancia (nombres de métodos, propiedades, etc.) en tiempo de ejecución)
    - Ejemplos: Smalltalk, Ruby, Python, JavaScript, Java (en menor medida)

- Lenguajes **estáticos**: la mayoría de características del programa se obtienen en tiempo de compilación
    - Mayor eficiencia
    - Se conoce a priori el tipo de la mayor parte de instancias del programa
    - Fuertemente tipeado
    - Ejemplos: C++, Swift


Vamos a detallar a continuación las características de Programación Orientada a Objetos de Swift.

### <a name="2"></a> 2. Clases y estructuras

En Swift se suele hablar de _instancias_ (un término más general) en lugar de _objetos_.

Las clases y las estructuras en Swift tienen muchas cosas en común. Ambos pueden:

- Definir **propiedades** y almacenar valores
- Definir **métodos** para proporcionar funcionalidad
- Definir inicializadores para configurar el estado inicial
- Ser extendidas para expandir su funcionalidad más allá de una implementación por defecto
- Ajustarse a un protocolo 

Las clases tienen características adicionales que no tienen las estructuras:

- Mediante la herencia una clase puede heredar las características de otra
- El casting de tipos permite comprobar e interpretar el tipo de una instancia de una clase en tiempo de ejecución
- Los deinicializadores permiten a una instancia de una clase liberar los recursos que ha asignado
- Mediante el conteo de referencias se permite que exista más de una referencia a una instancia de una clase

#### Definición

```swift
class UnaClase {
    // definición de clase
}
struct UnaEstructura {
    // definición de una estructura
}
```

#### Ejemplo 

```swift
struct Resolucion {
    var ancho = 0
    var alto = 0
}
class ModoVideo {
    var resolucion = Resolucion()
    var entrelazado = false
    var frameRate = 0.0
    var nombre: String?
}
```

#### Inicialización de instancias de clases y estructuras


```swift
let unaResolucion = Resolucion()
let unModoVideo = ModoVideo()
```


#### Acceso a propiedades

Se puede acceder y modificar las propiedades usando la _sintaxis de punto_:

```swift
print("El ancho de unaResolucion es \(unaResolucion.ancho)")
// Imprime "El ancho de unaResolucion es 0
print("El ancho de unModoVideo es \(unModoVideo.resolucion.ancho)")
// Imprime el ancho de unModoVideo es 0
unModoVideo.resolucion.ancho = 1280
print("El ancho de unModoVideo es ahora \(unModoVideo.resolucion.ancho)")
// Imprime "El ancho de unModoVideo es ahora 1280"
```

#### Inicialización de las estructuras por sus propiedades

Podemos inicializar **las estructuras (no las clases)** dando valores **a todas sus propiedades** en el inicializador:

```swift
let vga = Resolucion(ancho: 640, alto: 480)
```

#### Estructuras y enumeraciones son tipos valor

**DEFINICIÓN**: Un _tipo valor_ es un tipo cuyo valor se copia cuando se asigna a una variable o constante, o cuando se pasa a una función.

Todos los tipos básicos de Swift -enteros, números en punto flotante, cadenas, arrays y diccionarios- son tipos valor y se implementan como estructuras. **Las estructuras y las enumeraciones son tipos valor en Swift**.

Comprobación de que las estructuras son de tipo valor

```swift
let hd = Resolucion(ancho: 1920, alto: 1080)
var cine = hd
cine.ancho = 2048
print("cine tiene ahora \(cine.ancho) píxeles de ancho")
// Imprime "cine tiene ahora 2048 píxeles de ancho"
print("hd tiene todavía \(hd.ancho) píxeles de ancho")
// Imprime "hd tiene todavía 1920 píxeles de ancho"
```
Podemos comprobar que la propiedad se modifica en `cine`, pero que el valor del ancho en `hd` sigue siendo el mismo.

#### Las clases son tipos referencia

A diferencia de los tipos valor, los tipos de referencias no se copian cuando se asignan o se pasan a funciones. En su lugar se usa una referencia a la misma instancia existente.

Ejemplo:

```swift
let diezOchenta = ModoVideo()
diezOchenta.resolucion = hd
diezOchenta.entrelazado = true
diezOchenta.frameRate = 25.0
diezOchenta.nombre = "1080i"

let tambienDiezOchenta = diezOchenta
tambienDiezOchenta.frameRate = 30.0
print("La propiedad frameRate de diezOchenta es ahora \(diezOchenta.frameRate)")
// Imprime "La propiedad frameRate de diezOchenta es ahora 30.0"
```

Hay que hacer notar que `diezOchenta` y `tambienDiezOchenta` se declaran con `let` como constantes. Sin embargo, podemos modificar sus propiedades debido a que los valores que contienen `diezOchenta` y `tambienDiezOchenta` son constantes que no han cambiado. Esas variables no "almacenan" instancias de `ModoVideo`, sino que se "refieren" a una instancia de `ModoVideo`. Es la propiedad `frameRate` de la instancia subyacente la que se cambia, no los valores de las referencias constantes a la instancia de `ModoVideo`.

#### Operadores de identidad

A veces puede ser útil descubrir si dos constantes o variables se refieren exactamente a la misma instancia de una clase. Para permitir esto, Swift proporciona dos operadores de identidad:

- Idéntico a (`===`)
- No idéntico a (`!==`)

```swift
if diezOchenta === tambienDiezOchenta {
    print("diezOchenta y tambienDiezOchenta se refieren a la misma instancia de ModoVideo.")
}
// Imprime "diezOchenta y tambienDiezOchenta se refieren a la misma instancia de ModoVideo."
```

Estos operadores "idéntico a" no son los mismos que los de "igual a" (representado por dos signos iguales `==`):

- "Idéntico a" significa que dos constantes o variables de una clase se refieren exactamente a la misma instancia de la clase.
- "Igual a" significa que dos instancias se consideran "iguales" o "equivalentes" en su valor. Es responsabilidad del diseñador de la clase definir la implementación de estos operadores.

#### Criterios para usar estructuras y clases

Podemos usar tanto clases como estructuras para definir nuestros tipos de datos y utilizarlos como bloques de construcción del código de nuestros programas. Sin embargo, se utilizan para distintos tipos de tareas.

Como regla general, utilizaremos una estructura cuando se cumplen una o más de las siguientes condiciones:

- El principal objetivo de la estructura es encapsular unos pocos datos relativamente sencillos.
- Es razonable esperar que los valores encapsulados serán copiados, más que referenciados, cuando asignamos o pasamos una instancia de esa estructura.
- Todas las propiedades almacenadas en la estructura son a su vez tipos valor, que también se espera que sean copiados más que referenciados.
- La estructura no necesita heredar propiedades o conducta de otro tipo existente.

Ejemplos de buenos candidatos de estructuras incluyen:

- El tamaño de una forma geométrica, encapsulando por ejemplo las propiedades `ancho` y `alto` de tipo `Double`.
- Una forma de referirse a rangos dentro de una serie, encapsulando por ejemplo, una propiedad `comienzo` y otra `longitud`, ambos del tipo `Int`.
- Un punto en un sistema de coordenadas 3D, encapsulando quizás las propiedades `x`, `y` y `z`, todos ellos de tipo `Double`.

En el resto de casos, definiremos una clase y crearemos instancias de esa clase que tendrán que ser gestionadas y pasadas por referencia. En la práctica, esto representa que la mayoría de datos que construiremos en nuestros programas deberían clases, no estructuras. Aunque usaremos muchas de las estructuras estándar de Swift.

### <a name="3"></a> 3. Propiedades

Las _propiedades_ asocian valores con una clase, estructura o enumeración particular. 

- Las propiedades almacenadas (_stored properties_) almacenan valores constantes y variables como parte de una instancia
- Las propiedades calculadas (_computed properties_) calculan (en lugar de almacenar) un valor. 

Enumeraciones, clases y estructuras pueden contener propiedades:

- Enumeraciones: pueden contener sólo propiedades calculadas.
- Clases y estructuras: pueden contener propiedades almacenadas y calculadas.

Las propiedades calculadas y almacenadas se asocian habitualmente con instancias de un tipo particular. Sin embargo, las propiedades también pueden asociarse con el propio tipo. Estas propiedades se conocen como propiedades del tipo (_type properties_).

Además, en Swift es posible definir **observadores de propiedades** que monitoricen cambios en los valores de una propiedad, a los que podemos responder con acciones programadas. Los observadores de propiedades pueden añadirse tanto a propiedades almacenadas definidas por nosotros como a propiedades heredadas de la superclase.

#### Propiedades almacenadas

Pueden ser variables y constantes:

```swift
struct RangoLongitudFija {
    var primerValor: Int
    let longitud: Int
}
var rangoDeTresItemss = RangoLongitudFija(primerValor: 0, longitud: 3)
// el rango representa ahora valores enteros the range represents integer values 0, 1, and 2
rangoDeTresItemss.primerValor = 6
// el rango representa ahora valores enteros 6, 7 y 8
```

IMPORTANTE: Si creamos una instancia de una estructura y la asignamos a una constante, no podremos modificar las propiedades de la instancia, incluso si han sido declaradas como propiedades variables:

```swift
let rangoDeCuatroItems = RangoLongitudFija(primerValor: 0, longitud: 4)
// este rango representa valores enteros 0, 1, 2 y 3
rangoDeCuatroItems.primerValor = 6
// esto producirá un error, incluso aun siendo primerValor una propiedad variable
```

Esto no sucede así con las clases, que son _tipos referencia_. Si asignamos una instancia de un tipo referencia a una constante, puedes seguir cambiando las propiedades variables de esa instancia.

#### Propiedades calculadas

Además de las propiedades almacenadas, las clases, estructuras y enumeraciones pueden definir _propiedades calculadas_, que no almacenan realmente un valor. En su lugar, proporcionan un _getter_ y un opcional _setter_ que devuelven y modifican otras propiedades y valores de forma indirecta.

```swift
struct Punto {
    var x = 0.0, y = 0.0
}
struct Tamaño {
    var ancho = 0.0, alto = 0.0
}
struct Rectangulo {
    var origen = Punto()
    var tamaño = Tamaño()
    var centro: Punto {
        get {
            let centroX = origen.x + (tamaño.ancho / 2)
            let centroY = origen.y + (tamaño.alto / 2)
            return Punto(x: centroX, y: centroY)
        }
        set(centroNuevo) {
            origen.x = centroNuevo.x - (tamaño.ancho / 2)
            origen.y = centroNuevo.y - (tamaño.alto / 2)
        }
    }
}
var cuadrado = Rectangulo(origen: Punto(x: 0.0, y: 0.0),
                  tamaño: Tamaño(ancho: 10.0, alto: 10.0))
let centroCuadradoInicial = cuadrado.centro
cuadrado.centro = Punto(x: 15.0, y: 15.0)
print("cuadrado.origen está ahora en (\(cuadrado.origen.x), \(cuadrado.origen.y))")
// Prints "cuadrado.origen está ahora en (10.0, 10.0)"
```

La propiedad centro se actualiza al nuevo valor de `(15, 15)` lo que mueve el cuadrado arriba a la derecha, a la nueva posición mostrada por el cuadrado naranja en el diagrama de abajo. Al asignar el nuevo valor a la propiedad se llama al _setter_ del centro, lo que modifica los valores `x` e `y` de las propiedades almacenadas originales, y mueve el cuadrado a su nueva posición.

<img src="../tema06-programacion-orientada-objetos-swift/imagenes/computedProperties.png" width="300px"/>

Se puede definir una versión acortada del _setter_ usando la variable por defecto `newValue` que contiene el nuevo valor asignado en el _setter_:

```swift
struct Rectangulo {
    var origen = Punto()
    var tamaño = Tamaño()
    var centro: Punto {
        get {
            let centroX = origen.x + (tamaño.ancho / 2)
            let centroY = origen.y + (tamaño.alto / 2)
            return Punto(x: centroX, y: centroY)
        }
        set {
            origen.x = newValue.x - (tamaño.ancho / 2)
            origen.y = newValue.y - (tamaño.alto / 2)
        }
    }
}
```

#### Propiedades solo-lectura

Una propiedad calculada con un _getter_ y sin _setter_ se conoce como una propiedad calculada de solo-lectura. Es posible simplificar la declaración de una propiedad calculada de solo-lectura eliminando la palabra clave `get` y sus llaves:

```swift
struct Cuboide {
    var ancho = 0.0, alto = 0.0, profundo = 0.0
    var volumen: Double {
        return ancho * alto * profundo
    }
}
let cuatroPorCincoPorDos = Cuboide(ancho: 4.0, alto: 5.0, profundo: 2.0)
print("el volumen de cuatroPorCincoPorDos es \(cuatroPorCincoPorDos.volumen)")
// Imprime "el volumen de cuatroPorCincoPorDos es 40.0"
```

#### Observadores de propiedades

Los observadores de propiedades (_property observers_) observan y responden a cambios en el valor de una propiedad. Los observadores de propiedades se llaman cada vez que el valor de una propiedad es actualizado, incluso si el nuevo valor es el mismo que el valor actual de la propiedad.

Observadores:

- `willSet` es llamado justo antes de que el nuevo valor se almacena en la propiedad.
- `didSet` es llamado inmediatamente después de que el nuevo valor es almacenado en la propiedad.

Ejemplo:

```swift
class ContadorPasos {
    var totalPasos: Int = 0 {
        willSet(nuevoTotalPasos) {
            print("A punto de actualizar totoalPasos a \(nuevoTotalPasos)")
        }
        didSet {
            if totalPasos > oldValue  {
                print("Añadidos \(totalPasos - oldValue) pasos")
            }
        }
    }
}
let contadorPasos = ContadorPasos()
contadorPasos.totalPasos = 200
// Imprime: "A punto de actualizar totalPasos a 200"
// Imprime: "Añadidos 200 pasos"
contadorPasos.totalPasos = 360
// Imprime: "A punto de actualizar totalPasos a 360"
// Imprime: "Añadidos 160 pasos"
contadorPasos.totalPasos = 896
// Imprime: "A punto de actualizar totalPasos a 896"
// Imprime: "Añadidos 536 pasos"
```

#### Variables locales y globales

Las capacidades anteriores de propiedades calculadas y de observadores también están disponibles para variables globales y locales. 

```swift
var x = 10  {
   didSet {
      print("El nuevo valor: \(x) y el valor antiguo: \(oldValue)")
   }
}
var y = 2 * x
var z : Int {
   get {
      return x + y
   }
   set {
      x = newValue / 2
      y = newValue / 2
   }
}

print(z)
z = 100
print(x)
```

#### Propiedades del tipo

Las propiedades de las instancias son propiedades que pertenecen a una instancia de un tipo particular. Cada vez que creamos una nueva instancia de ese tipo, tiene su propio conjunto de valores de propiedades, separados de los de cualquier otra instancia.

Podemos definir también propiedades que pertenecen al tipo propiamente dicho, no a ninguna de las instancias de ese tipo. Sólo habrá una copia de estas propiedades, sea cual sea el número de instancias de ese tipo que creemos. Estos tipos de propiedades se llaman propiedades del tipo (_type propierties_).

Para definir una propiedad del tipo hay que usar la palabra clave `static`. Para propiedades de tipo calculadas de clases, podemos usar en su lugar la palabra clave `class` para permitir a las subclases que sobreescriban la implementación de la superclase. 

Ejemplo:

```swift

struct UnaEstructura {
    static var propiedadTipoAlmacenada = "Algún valor."
    static var propiedadTipoCalculada : Int {
        return 1
    }
}
enum UnaEnumeracion {
    static var propiedadTipoAlmacenada = "Algún valor."
    static var propiedadTipoCalculada: Int {
        return 6
    }
}
class UnaClase {
    static var propiedadTipoAlmacenada = "Algún valor."
    static var propiedadTipoCalculada: Int {
        return 27
    }
    class var propiedadTipoCalculadaSobreescribible: Int {
        return 107
    }
}
```

Las propiedades del tipo se consultan y actualizan usando también la sintaxis de punto, pero sobre _el tipo_, no sobre una instancia:

```swift
print(UnaEstructura.propiedadTipoAlmacenada)
// Imprime "Algún valor."
UnaEstructura.propiedadTipoAlmacenada = "Otro valor."
print(UnaEstructura.propiedadTipoAlmacenada)
// Imprime "Otro valor."
print(UnaEnumeracion.propiedadTipoCalculada)
// Imprime "6"
print(UnaClase.propiedadTipoCalculada)
// Imprime "27"
```

Un ejemplo:

```swift
struct Valor {
   var valor: Int = 0 {
      didSet {
         Valor.sumaValores += valor
      }
   }
   static var sumaValores = 0
}

var c1 = Valor()
var c2 = Valor()
var c3 = Valor()
c1.valor = 10
c2.valor = 20
c3.valor = 30
print("Suma de los cambios de valores: \(Valor.sumaValores)")
// Imprime 60
```

### <a name="4"></a> 4. Métodos

- Los _métodos_ son funciones que están asociadas a un tipo particular. 
- Las clases, estructuras y enumeraciones pueden definir todas ellas métodos de instancia

#### Métodos de instancia

Los métodos de instancia son funciones que pertenecen a instancias de una clase, estructura o enumeración. 

Un ejemplo de definición:

```swift
class Contador {
    var veces = 0
    func incrementa() {
        veces += 1
    }
    func incrementaEn(cantidad: Int) {
        veces += cantidad
    }
    func reset() {
        veces = 0
    }
}
```

Y un ejemplo de uso:

```swift
let contador = Contador()
// el valor inicial del contador es 0
contador.incrementa()
// el valor del contador es ahora 1
contador.incrementaEn(5)
// el valor del contador es ahora 6
contador.reset()
// el valor del contador es ahora 0
```

#### Nombres locales y externos de parámetros

Ya vimos que los parámetros de las funciones pueden tener un nombre interno y un nombre externo. Lo mismo sucede con los métodos, porque los métodos no son más que funciones asociadas con un tipo.

Los nombres de los métodos en Swift se refieren normalmente al primer parámetro usando una preposición como `con`, `en`, `a` o `por`, como hemos visto en el ejemplo anterior `incrementaEn(_:)`. El uso de la preposición permite que el método se lea como una frase.

Ejemplo:

```swift
class Contador {
    var valor: Int = 0
    func incrementaEn(cantidad: Int, numeroDeVeces: Int) {
        valor += cantidad * numeroDeVeces
    }
}
```

Podemos llamar al método de la siguiente forma:

```swift
let contador = Contador()
contador.incrementaEn(5, numeroDeVeces: 3)
// el valor del contador es ahora 15
```

Al igual que en las funciones, podemos definir explícitamente los nombres externos de los parámetros y usar el subrayado (`_`) para indicar que ese parámetro no tendrá nombre externo.

#### La propiedad `self`

Toda instancia de un tipo tiene una propiedad implícita llamada `self`, que es exactamente equivalente a la instancia misma. Podemos usar la propiedad `self` para referirnos a la instancia actual dentro de sus propios métodos de instancia.

Ejemplo:

```swift
func incrementa() {
    self.veces += 1
}
```

Es obligado usarlo es cuando el nombre de la propiedad coincide con el nombre de un parámetro:

```swift
struct Punto {
    var x = 0.0, y = 0.0
    func estaAlaDerechaDeX(x: Double) -> Bool {
        return self.x > x
    }
}
let unPunto = Punto(x: 4.0, y: 5.0)
if unPunto.estaAlaDerechaDeX(1.0) {
    print("Este punto está a la derecha de la línea donde x == 1.0")
}
// Imprime "Este punto está a la derecha de la línea donde x == 1.0"
```

### Modificación de tipos valor desde dentro de la instancia

Las estructuras y las enumeraciones son tipos valor. Por defecto, las propiedades de un tipo valor no pueden ser modificadas desde dentro de los métodos de instancia. 

Pero si usamos la palabra clave `mutating` en un método particular, podemos conseguir una conducta _mutadora_ para ese método. El método puede mutar (esto es, cambiar) sus propiedades desde dentro del método, así como asignar una instancia completamente nueva a su propiedad implícita `self`, con lo que esta nueva instancia reemplazará la existente cuando el método termine.

```swift
struct Punto {
    var x = 0.0, y = 0.0
    mutating func incrementaEnX(deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var unPunto = Punto(x: 1.0, y: 1.0)
unPunto.incrementaEnX(2.0, y: 3.0)
print("El punto está ahora en (\(unPunto.x), \(unPunto.y))")
// Imprime "El punto está ahora en (3.0, 4.0)"
```

Un `let` hace constante toda la estructura:

```swift
let puntoFijo = Punto(x: 3.0, y: 3.0)
puntoFijo.incrementaEnX(2.0, y: 3.0)
// esto provocará un error
```

#### Asignación a `self` en un método mutador

Los métodos mutadores pueden asignar una nueva instancia completamente nueva a la propiedad `self`:

```swift
struct Punto {
    var x = 0.0, y = 0.0
    mutating func incrementaEnX(deltaX: Double, y deltaY: Double) {
        self = Punto(x: x + deltaX, y: y + deltaY)
    }
}
```

El resutado final de llamar a esta versión alternativa será exactamente el mismo que llamar a la versión anterior (aunque con una pequeña penalización de eficiencia: este método es 1,3 veces más lento que el anterior en la versión 2.2 del compilador de Swift).

Los métodos mutadores de enueraciones pueden establecer el parámetro `self` implícito para que tenga un subtipo distinto de la misma enumeración:

```swift
enum InterruptorTriEstado {
    case Apagado, Medio, Alto
    mutating func siguiente() {
        switch self {
        case Apagado:
            self = Medio
        case Medio:
            self = Alto
        case Alto:
            self = Apagado
        }
    }
}
var luzHorno = InterruptorTriEstado.Medio
luzHorno.siguiente()
// luzHorno es ahora .Alto
luzHorno.siguiente()
// luzHorno es ahora .Apagado
```

#### Métodos del tipo

Es posible también definir métodos que se llaman en el propio tipo, llamados _métodos del tipo_ . Se define un método del tipo escribiendo la palabra clave `static` antes de la palabra clave `func` del método. Las clases también pueden usar la palabra clave `class`:

```swift
class UnaClase {
    class func unMetodoDelTipo() {
        print("Hola desde el tipo")
    }
}
UnaClase.unMetodoDelTipo()
```

Un ejemplo, en el que se define una estructura llamada `NivelTracker` que sigue el progreso de un jugador a través de los distintos niveles de un juego. Es un juego de un único jugador, pero puede almacenar la información para múltiples jugadores en un mismo dispositivo.

```swift
struct NivelTracker {
    static var nivelMasAltoDesbloqueado = 1
    static func desbloqueaNivel(nivel: Int) {
        if nivel > nivelMasAltoDesbloqueado {
           nivelMasAltoDesbloqueado = nivel }
    }
    static func nivelEstaDesbloqueado(nivel: Int) -> Bool {
        return nivel <= nivelMasAltoDesbloqueado
    }
    var nivelActual = 1
    mutating func avanzarAlNivel(nivel: Int) -> Bool {
        if NivelTracker.nivelEstaDesbloqueado(nivel) {
            nivelActual = nivel
            return true
        } else {
            return false
        }
    }
}
```

La estructura `NivelTracker` se usa junto con la clase `Jugador`:

```swift
class Jugador {
    var tracker = NivelTracker()
    let nombreJugador: String
    func nivelCompletado(nivel: Int) {
        NivelTracker.desbloqueaNivel(nivel + 1)
        tracker.avanzarAlNivel(nivel + 1)
    }
    init(nombre: String) {
        nombreJugador = nombre
    }
}
```

Podemos crear una instancia de la clase `Jugador` para un jugador nuevo, y comprobar qué pasa cuando el jugador completa el nivel uno:

```swift
var jugador = Jugador(nombre: "Lucía")
jugador.nivelCompletado(1)
print("el mayor nivel desbloqueado es ahora \(NivelTracker.nivelMasAltoDesbloqueado)")
// Imprime "el mayor nivel desbloqueado es ahora 2"
```

Si creamos un segundo jugador, que intenta moverse a un nivel que no ha sido desbloqueado por ningún jugador, el intento de establecer el nuevo nivel falla:

```swift
jugador = Jugador(nombre: "Anabel")
if jugador.tracker.avanzarAlNivel(6) {
    print("el jugador está ahora en el nivel 6")
} else {
    print("el nivel 6 no ha sido desbloqueado todavía")
}
// Imprime "el nivel 6 no ha sido desbloqueado todavía"
```

### <a name="5"></a> 5. Herencia

- Una clase puede _heredar_ métodos, propiedades y otras características de otra clase. Cuando una clase hereda de otra, la clase que hereda se denomina _subclase_, y la clase de la que se hereda se denomina su _superclase_. La herencia es una conducta fundamental que diferencia las clases de otros tipos en Swift.

- Las clases en Swift pueden llamar y acceder a métodos y propiedades que pertenecen a su superclase y pueden proporcionar sus propias versiones que sobreescriben esos métodos y propiedades. Para sobreescribir un método o una propiedad es necesario cumplir con la definición proporcionada por la superclase.

- Las clases también pueden añadir observadores a las propiedades heredadas para ser notificadas cuando cambia el valor de una propiedad. A cualquier propiedad se le puede añadir un observador de propiedad, independientemente de si es originalmente una propiedad almacenada o calculada.

#### Definición de una clase base

Una clase que no hereda de ninguna otra se denomina una _clase base_ (_base class_). A diferencia de otros lenguajes orientados a objetos, **las clases en Swift no heredan de una clase base universal**.

También a diferencia de otros lenguajes orientados a objetos **Swift no permite definir clases _abstractas_** que no permiten crear instancias. Cualquier clase en Swift puede ser instanciada.

Ejemplo:

```swift
class Vehiculo {
    var velocidadActual = 0.0
    var descripcion: String {
        return "viajando a \(velocidadActual) kilómetros por hora"
    }
    func hazRuido() {
        // no hace nada - un vehículo arbitrario no hace ruido necesariamente
    }
}
```
Creamos una instancia nueva de `Vehiculo` y accedemos a su descripción

```swift
let unVehiculo = Vehiculo()
print("Vehículo: \(unVehiculo.descripcion)")
// Vehículo: viajando a 0.0 kilómetros por hora
```

La clase `Vehiculo` define características comunes para un vehículo arbitrario, pero no es de mucha utilidad por si misma. Para hacerla más útil, tenemos que refinarla para describir tipos de vehículos más específicos.

#### Construcción de subclases

La construcción de una subclase (_subclassing_) es la acción de basar una nueva clase en una clase existente. La subclase hereda características de la clase existente, que después podemos refinar. También podemos añadir nuevas características a la subclase.

Sintaxis:

```swift
class UnaSubclase: UnaSuperClase {
    // definición de la subclase
}
```

Por ejemplo, podemos definir una subclase `Bicicleta`:

```swift
class Bicicleta: Vehiculo {
    var tieneCesta = false
}
```

Por defecto, cualquier instancia nueva de `Bicicleta` no tendrá una cesta. Podemos establecer la propiedad `tieneCesta` a `true` para una instancia particular de `Bicicleta` después de crearla:

```swift
let bicicleta = Bicicleta()
bicicleta.tieneCesta = true
```

Podemos también modificar la propiedad heredada `velocidadActual` y preguntar por la propiedad `descripcion`:

```swift
bicicleta.velocidadActual = 10.0
print("Bicicleta: \(bicicleta.descripcion)")
// Bicicleta: viajando a 10.0 kilómetros por hora
```

Podemos construir subclases a partir de otras subclases. El siguiente ejemplo crea una subclase de `Bicicleta` que representa una bicicleta de dos sillines (un "tandem"):

```swift
class Tandem: Bicicleta {
    var numeroActualDePasajeros = 0
}
```

`Tandem` hereda todas las propiedades y métodos de `Bicicleta`, que a su vez hereda todas sus propiedades y métodos de `Vehiculo`. La subclase `Tandem` también añade una nueva propiedad almacenada llamada `numeroActualDePasajeros`, con un valor por defecto de 0.

Si creamos una instancia de `Tandem` podremos trabajar con cualquiera de sus propiedades nuevas y heredadas, y preguntar a la descripción de solo lectura que hereda de `Vehiculo`:

```swift
let tandem = Tandem()
tandem.tieneCesta = true
tandem.numeroActualPasajeros = 2
tandem.velocidadActual = 18.0
print("Tandem: \(tandem.descripcion)")
// Tandem: viajando a 18.0 kilómetros por hora
```

#### Sobreescritura

- Una subclase puede proporcionar su propia implementación de un método de la instancia, método del tipo, propiedad de la instancia o propiedad del tipo que hereda de su superclase. Esto se conoce como _sobreescritura_ (_overriding_).

- Para sobreescribir una característica que sería de otra forma heredada debemos usar el prefijo `override`. 

- Cuando proporcionamos una sobreescritura puede ser útil acceder a los valores proporcionados por la clase padre. Para acceder a ellos podemos usar el prefijo `super`.

Ejemplo:

```swift
class Tren: Vehiculo {
    override func hazRuido() {
        print("Chuu Chuu")
    }
}
```

Si creamos una nueva instancia de `Tren` y llamamos al método `hazRuido` podemos comprobar que se llama a la versión del método de la subclase:

```swift
let tren = Tren()
tren.hazRuido()
// Imprime "Chuu Chuu"
```

- Podemos sobreescribir cualquier propiedad heredada del tipo o de la instancia y proporcionar nuestros propios _getters_ y _setters_ para esa propiedad, o añadir observadores de propiedades para observar cuando cambian los valores subyacentes de la propiedad.

- Podemos proporcionar un _getter_ (o _setter_, si es apropiado) para sobreescribir cualquier propiedad heredada, independientemente de si la propiedad heredada se implementa como una propiedad almacenada o calculada. 

Por ejemplo, podemos definir un coche con marchas:

```swift
class Coche: Vehiculo {
    var marcha = 1
    override var descripcion: String {
        return super.descripcion + " con la marcha \(marcha)"
    }
}
```

Podemos ver el funcionamiento en el siguiente ejemplo:

```swift
let coche = Coche()
coche.velocidadActual = 50.0
coche.marcha = 3
print("Coche: \(coche.descripcion)")
// Coche: viajando a 50.0 kilómetros por hora con la marcha 3
```

Por último, podemos añadir observadores a propiedades heredadas. Esto nos permite ser notificados cuando el valor de una propiedad heredada cambia, independientemente de si esa propiedad se ha implementado en la subclase o en la superclase.

Por ejemplo, la clase `CocheAutomatico` representa un coche con una caja de cambios automática, que selecciona automáticamente la marcha basándose en la velocidad actual:

```swift
class CocheAutomatico: Coche {
    override var velocidadActual: Double {
        didSet {
            marcha = min(Int(velocidadActual / 25.0) + 1, 5)
        }
    }
}
```

En cualquier momento que se modifica la propiedad `velocidadActual` de una instancia de `CocheAutomatico`, el observador `didSet` establece la propiedad `marcha` a un valor apropiado para la nueva velocidad. Un ejemplo de ejecución:

```swift
let automatico = CocheAutomatico()
automatico.velocidadActual = 100.0
print("CocheAutomatico: \(automatico.descripcion)")
// CocheAutomatico: viajando a 100.0 kilómetros por hora con la marcha 5
```

Por último, es posible prevenir un método o propiedad de ser sobreescrito declarándolo como _final_. Para ello, hay que escribir el modificador `final` antes del nombre de la palabra clave que introduce el método o la propiedad (como `final var`, `final func` o `final class`). También es posible marcar la clase completa como final, escribiendo el modificador antes de `class` (`final class`).


### <a name="6"></a> 6. Inicialización

- _Inicialización_ es el proceso de preparar para su uso una instancia de una clase, estructura o enumeración. Este proceso incluye la asignación de un valor inicial para cada propiedad almacenada y la ejecución de cualquier otra operación de inicialización que se necesite para que la nueva instancia esté lista para usarse.

- Para implementar este proceso de inicialización hay que definir _inicializadores_, que son como métodos especiales que pueden llamarse para crear una nueva instancia de un tipo particular. A diferencia de otros lenguajes, los inicializadores en Swift no devuelven un valor. Su papel principal es que las nuevas instancias del tipo estén correctamente inicializadas antes de poder ser usadas por primera vez.

- El proceso de inicialización de una instancia puede resultar un proceso complicado, sobre todo cuando se tienen relaciones de herencia y hay que especificar también cómo realizar la inicialización de la subclase utilizando la superclase. Por falta de tiempo no vamos a explicar todo el proceso completo. Recomendamos consultar la [documentación original de Swift](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID203).

#### Inicialización de propiedades almacenadas

Las clases y estructuras deben definir todas sus propiedades almacenadas a un valor inicial en el tiempo en la instancia se crea. Las propiedades almacenadas no pueden dejarse en un estado indeterminado. 

Podemos definir el valor inicial para una propiedad en un inicializador o asignándole un valor por defecto como parte de la definición de la propiedad.

Un _inicializador_, en su forma más simple, es como un método de la instancia sin parámetros, escrito con la palabra clave `init`:

```swift
init() {
    // realizar alguna inicialización aquí
}
```

Ejemplo:

```swift
struct Fahrenheit {
    var temperatura: Double
    init() {
        temperatura = 32.0
    }
}
var f = Fahrenheit()
print("La temperatura por defecto es \(f.temperatura)° Fahrenheit")
// Imprime "La temperatura por defecto es 32.0° Fahrenheit"
```

La implementación anterior es equivalente a la siguiente (que es preferible, por ser más clara):

```swift
struct Fahrenheit {
    var temperatura = 32.0
}
```

#### Customización de la inicialización

Es posible _customizar_ el proceso de inicialización con parámetros de entrada y tipos opcionales, o asignando propiedades constantes durante la inicialización.

Ejemplo:

```swift
struct Celsius {
    var temperaturaEnCelsius: Double
    init(desdeFahrenheit fahrenheit: Double) {
        temperaturaEnCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(desdeKelvin kelvin: Double) {
        temperaturaEnCelsius = kelvin - 273.15
    }
}

let puntoDeEbullicionDelAgua = Celsius(desdeFahrenheit: 212.0)
// puntoDeEbullicionDelAgua.temperaturaEnCelsius es 100.0
let puntoDeCongelacionDelAgua = Celsius(desdeKelvin: 273.15)
// puntoDeCongelacionDelAgua.temperaturaEnCelsius is 0.0
```

Vemos que dependiendo del nombre de parámetro proporcionado se escoge un inicializador u otro. 

En los inicializadores es obligatorio proporcionar los nombres de todos los parámetros:

```swift
struct Color {
    let rojo, verde, azul: Double
    init(rojo: Double, verde: Double, azul: Double) {
        self.rojo   = rojo
        self.verde = verde
        self.azul  = azul
    }
    init(blanco: Double) {
        rojo  = blanco
        verde = blanco
        azul  = blanco
    }
}
let magenta = Color(rojo: 1.0, verde: 0.0, azul: 1.0)
let medioGris = Color(blanco: 0.5)
```

Podemos evitar proporcionar nombres externos usando un subrayado. Ejemplo: 

```swift
struct Celsius {
   var temperaturaEnCelsius: Double
   init(desdeFahrenheit fahrenheit: Double) {
      temperaturaEnCelsius = (fahrenheit - 32.0) / 1.8
   }
   init(desdeKelvin kelvin: Double) {
      temperaturaEnCelsius = kelvin - 273.15
   }
   init(_ celsius: Double) {
      temperaturaEnCelsius = celsius
   }
}

let temperaturaCuerpo = Celsius(37.0)
// temperaturaCuerpo.temperaturaEnCelsius es 37.0
```

Por último, es posible dejar sin inicializar propiedades opcionales, ya que el valor que tomarían sería `nil`:

```swift
class PreguntaEncuesta {
    let texto: String
    var respuesta: String?
    init(texto: String) {
        self.texto = texto
    }
    func pregunta() {
        print(texto)
    }
}
let preguntaQueso = PreguntaEncuesta(texto: "¿Te gusta el queso?")
preguntaQueso.pregunta()
// Imprime "¿Te gusta el queso?
preguntaQueso.respuesta = "Sí, me gusta el queso."
```

En el ejemplo anterior se comprueba también que es posible inicializar constantes. Por ejemplo, la propiedad `text` está definida con un `let` y se inicializa en el inicializador.

Por último, es posible definir valores por defecto a los inicializadores que sean sobreescritos por otros inicializadores, así como invocar a otros inicializadores más básicos en otros:

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
    init() {}
    init(origen: Punto, tamaño: Tamaño) {
        self.origen = origen
        self.tamaño = tamaño
    }
    init(centro: Punto, tamaño: Tamaño) {
        let origenX = centro.x - (tamaño.ancho / 2)
        let origenY = centro.y - (tamaño.ancho / 2)
        self.init(origen: Punto(x: origenX, y: origenY), tamaño: tamaño)
    }
}
let basicRectangulo = Rectangulo()
// el origen de basicRectangulo es (0.0, 0.0) y su tamaño (0.0, 0.0)
let origenRectangulo = Rectangulo(origen: Punto(x: 2.0, y: 2.0),
                        tamaño: Tamaño(ancho: 5.0, alto: 5.0))
// el origne de origenRectangulo es (2.0, 2.0) y su tamaño (5.0, 5.0)
let centroRectangulo = Rectangulo(centro: Punto(x: 4.0, y: 4.0),
                        tamaño: Tamaño(ancho: 3.0, alto: 3.0))
// el origen de centroRectangulo es (2.5, 2.5) y su tamaño (3.0, 3.0)
```
