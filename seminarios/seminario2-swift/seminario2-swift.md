## Seminario 2: Seminario de Swift

[Lenguajes y Paradigmas de Programación](https://moodle2015-16.ua.es/moodle/course/view.php?id=4802), curso 2016-17  
Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  

### Contenidos

- [1. El lenguaje de programación Swift](#1)
- [2. Ejecución de programas Swift](#2)
    - [2.1. Ejecución en IBM Swift Sandbox](#2-1)
    - [2.2. Máquina Vagrant con Swift](#2-2)
        - [2.1. Instalación de la máquina virtual Vagrant](#2-2-1)
        - [2.2. (Windows) Conexión ssh mediante PuTTY](#2-2-2)
        - [2.3. Directorio compartido entre el ordenador *host* y la máquina virtual](#2-2-3)
        - [2.4 Editor de código Atom](#2-2-4)
- [3. Un tour de Swift](#3)

### <a name="0"></a> Bibliografía y referencias

- Documentación sobre Swift
    - [The Swift Programming Language (ePub)](https://swift.org/documentation/TheSwiftProgrammingLanguage(Swift3.1).epub)
    - [The Swift Programming Language (html)](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html)
    - [Recursos Swift en Apple](https://developer.apple.com/swift/resources/)
    - [swift.org](https://swift.org)
- Swift Open Source
    - [Repositorio `swift` en GitHub](https://github.com/apple/swift): repositorio principal de Swift, que contiene el código fuente del compilador Swift, la biblioteca estándar y SourceKit.
    - [Repositorio `swift-evolution` en GitHub](https://github.com/apple/swift-evolution): documentos relacionados con la evolución continua de Swift, incluyendo objetivos de las próximas versiones y propuestas de cambios y extensiones de Swift.
- Vagrant
    - [Documentación de Vagrant](https://www.vagrantup.com/docs/)
- Atom
    - [Manual de Atom](https://atom.io/docs/v1.5.3/)

### <a name="1"></a> 1. El lenguaje de programación Swift

Swift
[(enlace a la Wikipedia)](https://en.wikipedia.org/wiki/Swift_(programming_language))
es un lenguaje de programación compilado, de propósito general y
multi-paradigma desarrollado por Apple. Swift se presentó en la
edición 2014 de la Conferencia de Desarrolladores de Apple
(WWDC). Durante el año 2014 evolucionó la versión 1.2 y en la WWDC
2015 se presentó Swift 2, una actualización importante del
lenguaje. Inicialmente fue un lenguaje propietario, pero el 3 de
diciembre de 2015 se hizo _open source_ bajo la licencia Apache 2.0,
para las plataformas Apple y Linux. En la actualidad se ha
estabilizado la versión 3 del lenguaje y se está desarrollando la
versión 4 que se presentará a finales de 2017.
 
La siguiente descripción se ha extraído del repositorio GitHub de
Swift:

> Swift es un lenguaje de programación de sistemas de alta
> eficiencia. Tiene una sintaxis limpia y moderna, ofrece acceso
> transparente a código y librerías existentes en C y Objective-C, y
> es seguro en el uso de memoria (*memory safe*).

> Aunque está inspirado en Objective-C y en muchos otros lenguajes,
> Swift no es en si mismo un lenguaje derivado de C. Como lenguaje
> completo e independiente, Swift proporciona características
> fundamentales como control de flujo, estructuras de datos y
> funciones, junto con construcciones de alto nivel como objetos,
> protocolos, clausuras y genéricos. Swift se apoya en módulos,
> eliminando la necesidad de cabeceras y la duplicación de código que
> éstas inducen.


### <a name="2"></a> 2. Ejecución de programas Swift

Presentamos dos posibles configuraciones para poder ejecutar programas
Swift en cualquier sistema operativo (Mac, Windows o Linux): 

- Ejecución on-line en un entorno de IBM (IBM Swift Sandbox).
- Ejecución local en una máquina virtual Vagrant.

#### <a name="2-1"></a> 2.1. Ejecución en IBM Swift Sandbox

IBM mantiene un entorno de edición y ejecución on-line de Swift con el
que es posible editar, salvar y ejecutar programas Swift:
[IBM Swift Sandbox](https://swift.sandbox.bluemix.net/). 

<img src="imagenes/bluemix.png" width="700px"/>


Es un entorno bastante completo, que tiene la ventaja de poder usarse
sin necesidad de realizar ninguna instalación en local. El
inconveniente obvio es que estamos a expensas de tener una conexión a
Internet y de que el entorno no esté caído.

#### <a name="2-2"></a> 2.2. Ejecución en entorno local

Vamos a utilizar una máquina virtual Vagrant con la versión Linux de
Swift para realizar las prácticas. De esta forma todos trabajaremos
con el mismo entorno, independientemente del sistema operativo que
tengamos.

#### <a name="2-2-1"></a> 2.2.1. Instalación de la máquina virtual Vagrant

Veamos los pasos para construir una máquina virtual Linux (Ubuntu
14.04) con Swift gestionada por Vagrant. Funcionan tanto para Windows
como para Mac OS X:

1. Instala VirtualBox
   [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads).
2. Instala Vagrant
   [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html).
3. Crea un directorio llamado `swift` en la ruta que desees.
4. Crea dentro de este directorio un fichero llamado `Vagrantfile` con
   el siguiente contenido. Este fichero define la configuración base
   de la máquina Vagrant, incluyendo el script de aprovisionamiento
   que instala paquetes de linux y descarga e instala Swift.

    **Fichero `Vagrantfile`**:

    ```
    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    SWIFT_VERSION = "swift-3.0.2-RELEASE"
    SWIFT_URL = "https://swift.org/builds/swift-3.0.2-release/ubuntu1404/swift-3.0.2-RELEASE/swift-3.0.2-RELEASE-ubuntu14.04.tar.gz"

    Vagrant.configure(2) do |config|

        config.vm.box = "ubuntu/trusty64"

        config.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
            vb.gui = false
            # Customize the amount of memory on the VM:
            vb.memory = "1024"
        end

        config.vm.provision "fix-no-tty", type: "shell" do |s|
            s.privileged = false
            s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
        end

        # Aprovisionamiento inline
        config.vm.provision "shell", inline: <<-SHELL
            sudo apt-get -y install -f clang libicu-dev
            wget -q #{SWIFT_URL}
            tar xzf #{SWIFT_VERSION}-ubuntu14.04.tar.gz
            sudo chown -R vagrant #{SWIFT_VERSION}-ubuntu14.04 
            sudo mv #{SWIFT_VERSION}-ubuntu14.04 /usr/local 
            echo "export PATH=/usr/local/#{SWIFT_VERSION}-ubuntu14.04/usr/bin:\"${PATH}\"" >> /home/vagrant/.bashrc
        SHELL
    end
    ```

5. Abre un terminal, muévete al directorio `swift` y lanza el commando
   `vagrant up`:

    ```
    $ cd swift
    $ vagrant up
    ```

    Este comando pone en marcha la máquina Vagrant y realiza el
    aprovisionamiento (instalación de los paquetes iniciales) si es la
    primera vez que lo lanzamos.

6. Una vez en marcha la máquina Vagrant y realizado su
   aprovisionamiento podemos logearnos en ella conectándonos a ella
   por `ssh`. Si tu sistema operativo es Windows deberás conectarte a
   la máquina usando PuTTY (ver siguiente apartado):

    ```
    $ vagrant ssh
    Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 3.13.0-113-generic x86_64)
    vagrant:~$
    ```

    Una vez en la máquina vagrant, lanzamos el intérprete de Swift y debe funcionar correctamente:
    
    ```
    vagrant:~$ swift
    Welcome to Swift version 3.0.2 (swift-3.0.2-RELEASE). Type :help for assistance.
    1> 
    ```

7. Una vez hayamos terminado de trabajar, podemos salir del intérprete
   de Swift y de vagrant de la siguiente forma:

    ```
    1> :quit
    vagrant:~$ exit
    $ vagrant halt
    ```

    El comando `vagrant halt` detiene la máquina virtual y la deja apagada hasta la siguiente vez que hagamos `vagrant up` 

#### <a name="2-2-2"></a> 2.2.2. (Windows) Conexión ssh mediante PuTTY

En Windows es conveniente instalar la utilidad
[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
para conectarse a la máquina Vagrant mediante ssh. Una vez en marcha
la máquina virtual (después del comando `vagrant up`), hay que
realizar una conexión ssh con los siguientes parámetros:

- Dirección IP: 127.0.0.1
- Puerto: 2222
- login: vagrant
- password: vagrant


#### <a name="2-2-3"></a> 2.2.3. Directorio compartido entre el ordenador *host* y la máquina virtual

En la máquina Vagrant se monta automáticamente el directorio del
ordenador anfitrión en el que se se encuentra el fichero `Vagrantfile`
(el que hemos llamado `swift`). Se monta en el directorio
`/vagrant`. Lo podemos comprobar moviéndonos a ese directorio en la
máquina vagrant y examinando su contenido:

```text
vagrant:~$ cd /vagrant
vagrant:~$ ls -l
-rw-r--r-- 1 vagrant vagrant  1035 Mar  6 10:21 Vagrantfile
```

De esta forma es muy cómodo editar los programas Swift en el ordenador
anfitrión en este directorio y ejecutarlos desde línea de comando en
la máquina Vagrant.  Recomendamos utilizar un editor de textos
orientado a la programación (Atom, Sublime, Xcode en el Mac, etc.). En
la siguiente sección podemos ver cómo instalar Atom.

Probamos a usar el directorio compartido:

1. Editamos en el directorio `swift` del ordenador anfitrión un
   programa llamado `holaMundo.swift`.

    **Fichero `holaMundo.swift`**:

    ```swift
    // Primer programa Swift

    var str = "Mensaje"
    str = "Hola mundo!"
    print(str)

    // Vamos a sumar dos números

    var firstNumber = 2
    var secondNumber = 3
    var totalSum = firstNumber + secondNumber
    firstNumber = firstNumber + 1
    secondNumber = secondNumber + 1
    totalSum = firstNumber + secondNumber
    print("El resultado de la suma es = \(totalSum)")
    ```

2. Arrancamos la máquina vagrant y nos conectamos a ella:

    ```
    $ vagrant up
    $ vagrant ssh
    ```

3. Lanzamos el programa desde la máquina vagrant, cambiando al directorio `/vagrant` (el directorio compartido) y ejecutando el comando swift:

    ```text
    $ cd /vagrant
    vagrant:~$ swift holaMundo.swift
    Hola mundo!
    El resultado de la suma es = 7
    ```

4. Prueba a cambiar cualquier cosa en el programa desde el editor en
   el ordenador anfitrión y a volver a ejecutar el programa desde la
   máquina vagrant. Verás que el directorio está realmente compartido
   y que el programa se ejecuta con las modificaciones que has
   introducido.

5. Cuando termines de trabajar con la máquina vagrant recuerda salir
   de ella y apagarla desde el anfitrión:

    ```
    vagrant:~$ exit
    $ vagrant halt
    ```

#### <a name="2-2-4"></a> 2.2.4 Editor de código Atom

Para editar código Swift puedes usar cualquier editor orientado a
programación. Aconsejamos [Atom](https://atom.io), un editor
desarrollado por GitHub con características muy interesantes. Puedes
instalarlo en cualquier plataforma.

Atom es un editor modular en el que se pueden instalar múltiples
extensiones desarrolladas por terceros. Para programar con swift es
conveniente instalar los siguientes paquetes (en **Preferencias >
Settings > Install Packages**):

- `language-swift`: proporciona coloreado de sintaxis de
  Swift
- `platformio-ide-terminal`: proporciona la posibilidad de abrir un
  terminal en la parte inferior de la ventana.

<img src="imagenes/atom.png" width="700px"/>

Puedes consultar los conceptos básicos de Atom, y el manual completo en [este enlace](http://flight-manual.atom.io/getting-started/sections/atom-basics/).

### <a name="3"></a> 3. Un tour de Swift

Aquí empieza el seminario de Swift. El texto que hay a continuación es
una traducción del documento de Apple
*[A Swift Tour](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/GuidedTour.html#//apple_ref/doc/uid/TP40014097-CH2-ID1)*
en el que se presenta una introducción rápida a los conceptos
fundamentales del lenguaje. En los temas siguientes de la asignatura
(Tema 5 - Programación Funcional con Swift y Tema 6 - Programación
Orientada a Objetos con Swift) profundizaremos en aspectos como
funciones, genéricos, clases o protocolos.

La tradición sugiere que el primer programa en un nuevo lenguaje
debería imprimir las palabras "Hello, world!" en la pantalla. En
Swift, esto puede hacerse con una única línea:

```swift
print("Hello, world!")
```

Si has escrito código en C o en Objective-C, esta sintaxis te parecerá
familiar. En Swift, esta línea de código es un programa completo. No
necesitas importar una biblioteca separada para funcionaliades como
entrada/salida o manejo de cadenas. El código escrito en el ámbito
global se usa como el punto de entrada del programa, por lo que no
necesitas una función `main()`. Tampoco tienes que escribir puntos y
comas al final de cada sentencia.

Este tour te da información suficiente para empezar a escribir código
in Swift enseñándote cómo conseguir una variedad de tareas de
programación. No te preocupes si no entiendes algo, todo lo que se
introduce en este tour se explica en detalle en el
[resto del libro](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID309).

#### Valores simples

Usa `let` para crear una constante y `var` para crear una variable. No
es necesario que se conozca en tiempo de compilación el valor de una
constante, pero debes asignarle un valor exactamente una vez. Esto
significa que puedes usar constantes para nombrar un valor que
determinas una vez pero que usas en muchos lugares.

```swift
var miVariable = 42
miVariable = 50
let miConstante = 42
```

Una constante o variable debe tener el mismo tipo que el valor que
quieres asignarle. Sin embargo, no siempre tienes que escribir el tipo
explícitamente. Cuando se proporciona un valor al crear una constante
o una variable el compilador infiere su tipo. En el ejemplo anterior,
el compilador infiere que `myVariable` es un entero porque su valor
inicial es un entero.

Si el valor inicial no proporciona información suficiente (o si no hay
valor inicial), especifica el tipo escribiéndolo después de la
variable, separándolo por dos puntos.

```swift
let implicitoInteger = 70
let implicitoDouble = 70.0
let explicitoDouble: Double = 70
```

> EXPERIMENTO  
> Crea una constante con el tipo explícito de `Float` y un valor de 4.

Los valores nunca se convierten implícitamente a otro tipo. Si
necesitas convertir un valor a un tipo diferente, construye
explícitamente una instancia del tipo deseado.

```swift
let etiqueta = "El ancho es "
let ancho = 94
let anchoEtiqueta = etiqueta + String(ancho)
```

> EXPERIMENTO  
> Intenta eliminar la conversión a `String` en la última línea. ¿Qué error obtienes?

Hay una forma aún más sencilla de incluir valores en cadenas: escribe
el valor entre paréntesis, y escribe una barra invertida (`\`) antes
de los paréntesis. Por ejemplo:

```swift
let manzanas = 3
let naranjas = 5
let resumenManzanas = "Tengo \(manzanas) manzanas."
let resumenFrutas = "Tengo \(manzanas + naranjas) frutas."
```

> EXPERIMENTO  
> Usa `\()` para incluir un cálculo en punto flotante en una cadena y
> para incluir el nombre de alguien en un saludo.

Crea arrays y diccionarios utilizando corchetes (`[]`), y accede a sus
elementos escribiendo el índice o la clave en los corchetes. Se
permite una coma después del último elemento.

```swift
var listaCompra = ["huevos", "agua", "tomates", "pant"]
listaCompra[1] = "botella de agua"

var trabajos = [
    "Malcolm": "Capitán",
    "Kaylee": "Mecánico",
]
trabajos["Jayne"] = "Relaciones públicas"
```

Para crear un array o diccionario vacío, usa la sintaxis de inicialización.

```swift
let arrayVacio = [String]()
let diccionarioVacio = [String: Float]()
```

Si el tipo de información puede ser inferido, puedes escribir un array
vacío como `[]` y un diccionario vacío como `[:]`; por ejemplo, cuando
estableces un nuevo valor para una variable o pasas un argumento a una
función.

```swift
listaCompra = []
trabajos = [:]
```

#### Tuplas

Una tupla agrupa varios valores en un único valor compuesto.

```swift
let http404Error = (404, "Not Found")
```

El tipo de la tupla es `(Int, String)`.

Para obtener los valores de la tupla podemos _descomponerla_. Si
queremos ignorar una parte podemos utilizar un subrrayado (`_`).

```swift
let (statusCode, statusMensaje) = http404Error
let (soloStatusCode, _) = http404Error
```

También podemos acceder por posición:

```swift
println("El código de estado es \(http404Error.0)")
```

#### Control de flujo

Usa `if` y `switch` para hacer condicionales y usa `for-in`, `for`,
`while` y `repeat-while` para hacer bucles. Los paréntesis alrededor
de las condiciones o de la variable del bucle son opcionales. Se
requieren llaves alrededor del cuerpo.

```swift
let puntuacionesIndividuales = [75, 43, 103, 87, 12]
var puntuacionEquipo = 0
for puntuacion in puntuacionesIndividuales {
    if puntuacion > 50 {
        puntuacionEquipo += 3
    } else {
        puntuacionEquipo += 1
    }
}
print(puntuacionEquipo)
```

En una sentencia `if`, el condicional debe ser una expresión booleana;
esto significa que código como `if score { ... }` es un error, no una
comparación implícita con cero.

Puedes usar `if` y `let` juntos para trabajar con valores que pueden
faltar. Estos valores se representan como opcionales. Un valor
opcional o bien contiene un valor o contiene `nil` para indicar que el
valor falta. Escribe una interrogación (`?`) después del tipo de un
valor para marcar el valor como opcional.

```swift
var cadenaOpcional: String? = "Hola"
print(cadenaOpcional == nil)

var nombreOpcional: String? = "John Appleseed"
var saludo = "Hello!"
if let nombre = nombreOpcional {
    saludo = "Hola, \(nombre)"
}
```

> EXPERIMENTO  
> Cambia `nombreOpcional` a `nil`. ¿Qué saludo obtienes? Añade una
> cláusula `else` que establezca un saludo diferente si
> `nombreOpcional` es `nil`.

Si el valor opcional es `nil`, el condicional es `false` y el código
en las llaves se salta. En otro caso, el valor opcional se desenvuelve
y se asigna a la constante después del `let`, lo que hace que el valor
desenvuelto esté disponible dentro del bloque de código.

Otra forma de manejar valores opcionales es proporcionar un valor por
defecto usando el operador `??`. Si falta el valor valor opcional, se
usa el valor por defecto en su lugar.

```swift
let nombrePila: String? = nil
let nombreCompleto: String = "John Appleseed"
let saludoInformal = "¿Qué tal, \(nombrePila ?? nombreCompleto)?"
```

Las sentencias `switch` permiten cualquier tipo de datos y una amplia
variedad de operaciones de comparación; no están limitados a enteros y
pruebas de igualdad.

```swift
let verdura = "pimiento rojo"
switch verdura {
    case "zanahoria":
        print("Buena para la vista.")
    case "lechuga", "tomates":
        print("Podrías hacer una buena ensalada.")
    default:
        print("Siempre puedes hacer una buena sopa.")
}
```

> EXPERIMENTO  
> Intenta eliminar el caso por defecto. ¿Qué error obtienes?

Date cuenta de cómo `let` puede usarse en un patrón para asignar a una
constante el valor que empareja esa parte del patrón.

Después de ejecutar el código dentro del caso que se empareja, el
programa sale de la sentencia `switch`. La ejecución no continua con
el siguiente caso, por lo que no hay necesidad de romper el `switch`
al final del código de cada caso.

Usa `for-in` para iterar sobre elementos en un diccionario
proporcionando una pareja de nombres para usar en cada pareja
clave-valor. Los diccionarios son colecciones desordenadas, por lo que
sus claves y valores se iteran en un orden arbitrario.

```swift
let numerosInteresantes = [
    "Primos": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Cuadrados": [1, 4, 9, 16, 25],
]
var mayor = 0
for (clase, numeros) in numerosInteresantes {
    for numero in numeros {
        if numero > mayor {
            mayor = numero
        }
    }
}
print(mayor)
```

> EXPERIMENTO  
> Añade otra variable para seguir qué clase de número es el mayor.

Usa `while` para repetir un bloque de código hasta que una condición
cambie. La condición de un bucle puede estar también al final,
asegurando que el bucle se ejecuta al menos una vez.

```swift
var n = 2
while n < 100 {
    n *= 2
}
print(n)
 
var m = 2
repeat {
    m *=  2
} while m < 100
print(m)
```

Puedes definir un índice en un bucle usando `..<` para construir un
rango de índices.

```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
```

Usa `..<` para construir un rango que omita su valor superior, y usa
`...` para construir un rango que incluya ambos valores.


#### Funciones y clausuras

Usa `func` para declarar una función. Usa `->` para separar los
nombres de los parámetros y sus tipos del tipo devuelto de la
función.

```swift
func saluda(nombre: String, dia: String) -> String {
    return "Hola \(nombre), hoy es \(dia)."
}
saluda(nombre: "Bob", dia: "Martes")
```

> EXPERIMENTO  
> Elimina el parámetro día. Añade un parámetro para incluir la comida
> de hoy en el saludo.

Por defecto, las funciones usan los nombres de los parámetros como
etiquetas de los argumentos. Es posible definir una etiqueta
escribiéndola antes del nombre del parámetro, o no usar etiqueta
escribiendo `_`:

```swift
func saluda(_ nombre: String, el dia: String) -> String {
    return "Hola \(nombre), hoy es \(dia)."
}
saluda("Bob", el: "Martes")
```

Las funciones pueden devolver cualquier tipo de dato, como tuplas.

```swift
func calculaEstadisticas(puntuaciones: [Int]) -> 
                        (min: Int, max: Int, sum: Int) {
    var min = puntuaciones[0]
    var max = puntuaciones[0]
    var sum = 0
    
    for puntuacion in puntuaciones {
        if puntuacion > max {
            max = puntuacion
        } else if puntuacion < min {
            min = puntuacion
        }
        sum += puntuacion
    }
    
    return (min, max, sum)
}
let estadisticas = calculaEstadisticas([5, 3, 100, 3, 9])
print(estadisticas.sum)
print(estadisticas.2)
```

Las funciones también pueden tener un número variable de argumentos,
agrupándose todos ellos en un array.

```swift
func sumaDe(numeros: Int...) -> Int {
    var suma = 0
    for numeros in numeros {
        suma += numeros
    }
    return suma
}
sumaDe()
sumaDe(42, 597, 12)
```

> EXPERIMENTO  
> Escribe una función que calcule la media de sus argumentos.

Las funciones pueden anidarse. Las funciones pueden acceder variables
declaradas en la función exterior. Puedes usar funciones anidadas para
organizar el código en una función que es larga o complicada.

```swift
func devuelveQuince() -> Int {
    var y = 10
    func suma() {
        y += 5
    }
    suma()
    return y
}
devuelveQuince()
```

Las funciones son un tipo de primera clase. Esto significa que una
función puede devolver otra función como resultado.

```swift
func construyeIncrementador() -> ((Int) -> Int) {
    func sumaUno(numero: Int) -> Int {
        return 1 + numero
    }
    return sumaUno
}
var incrementa = construyeIncrementador()
incrementa(7)
```

Podemos modificar el ejemplo `devuelveQuince` para que se devuelva una
versión modificada de la función `suma`. Llamamos a la función
`devuelveSuma`.

```swift
func devuelveSuma() -> (() -> Int) {
    var y = 10
    func suma() -> Int {
        y += 5
        return y
    }
    suma()
    return suma
}

let f = devuelveSuma()
f()
f()
```

Una función puede tomar otra función como uno de sus argumentos.

```swift
func cumpleCondicion(lista: [Int], condicion: (Int) -> Bool) -> Bool {
    for item in lista {
        if condicion(item) {
            return true
        }
    }
    return false
}
func menorQueDiez(numero: Int) -> Bool {
    return numero < 10
}
var numeros = [20, 19, 7, 12]
cumpleCondicion(numeros, condicion: menorQueDiez)
```

Las funciones son en la realidad un caso especial de clausuras:
bloques de código que pueden ser llamados después.  El código en la
clausura tiene acceso a cosas como variables y funciones que estaban
disponibles en el ámbito (*scope*) en el que se creó la clausura,
incluso si la clausura se está en un ámbito distinto cuando se
ejecuta; ya viste un ejemplo de esto con las funciones
anidadas. Puedes escribir una clausura rodeando el código con llaves
(`{}`). Usa `in` para separar los argumentos del cuerpo.

```swift
numeros.map({
    (numero: Int) -> Int in
    let resultado = 3 * numero
    return resultado
})
```

> EXPERIMENTO  
> Reescribe la clausura para que devuelva cero para todos los números
> impares.

Tienes bastantes opciones para escribir clausuras de forma más
concisa. Cuando ya se conoce el tipo de una clausura puedes omitir el
tipo de sus parámetros, el tipo devuelto o ambos. Las clausuras
escritas en una línea devuelven implícitamente el valor de su única
sentencia.

```swift
let numerosMapeados = numeros.map({ numero in 3 * numero })
print(numerosMapeados)
```

Puedes referirte a los parámetros por número en lugar de por nombre;
este enfoque es especialmente útil en clausuras muy cortas. Una
clausura pasada como último argumento puede aparecer inmediatamente
después de los paréntesis. Cuando una clausura es el único argumento
de una función, puedes omitir los paréntesis por completo.

```swift
let numerosOrdenados = numeros.sort { $0 > $1 }
print(numerosOrdenados)
```

#### Objetos y clases

Usa `class` seguido por el nombre de la clase para crear una
clase. Una declaración de una propiedad en una clase se escribe de la
misma forma que la declaración de una constante o una variable,
excepto que está en el contexto de una clase. De la misma forma, las
declaraciones de los métodos se escriben de la misma forma que las
funciones.

```swift
class Figura {
    var numeroDeLados = 0
    func descripcionSencilla() -> String {
        return "Una figura con \(numeroDeLados) lados."
    }
}
```

> EXPERIMENTO  
> Añade una propiedad constante con `let`, y añade otro método que
> tome un argumento.

Crea una instancia de una clase poniendo paréntesis después del nombre
de la clase. Usa la sintaxis de punto para acceder a las propiedades y
los métodos de la instancia.

```swift
var figura = Figura()
figura.numeroDeLados = 7
var descripcionFigura = figura.descripcionSencilla()
```

A esta versión de la clase `Figura` le falta algo importante: un
inicializador para preparar la clase cuando se crea una instancia. Usa
`init` para crear uno.

```swift
class FiguraConNombre {
    var numeroDeLados: Int = 0
    var nombre: String
    
    init(nombre: String) {
        self.nombre = nombre
    }
    
    func descripcionSencilla() -> String {
        return "Una figura con \(numeroDeLados) lados."
    }
}
```

Fíjate en cómo se utiliza `self` para distinguir la propiedad `nombre`
del argumento `nombre` al inicializador. Los argumentos al
inicializador se pasan como una llamada a una función cuando creas una
instancia de la clase. Cada propiedad necesita un valor asignado; ya
sea en su declaración (como `numeroDeLados`) o en el inicializador
(como `nombre`).

Usa `deinit` para crear un *desinicializador* si necesitas realizar
alguna limpieza antes de que el objeto sea eliminado.

Las subclases incluyen el nombre de su subclase después del nombre de
la clase, separado por una coma. No hay ningún requisito de que las
clases deban ser subclases de alguna clase raíz, por lo que puedes
omitir una superclase si así lo necesitas.

Los métodos en una subclase que sobreescriben la implementación de la
superclase se marcan con `override`; la sobreescritura de un método
por accidente, sin `override`, se detecta por el compilador como un
error. El compilador también detecta métodos con `override` que
realmente no sobreescriben ningún método de la superclase.

```swift
class Cuadrado: FiguraConNombre {
    var longitudLado: Double
    
    init(longitudLado: Double, nombre: String) {
        self.longitudLado = longitudLado
        super.init(nombre: nombre)
        numeroDeLados = 4
    }
    
    func area() ->  Double {
        return longitudLado * longitudLado
    }
    
    override func descripcionSencilla() -> String {
        return "Un cuadrado con lados de longitud \(longitudLado)."
    }
}
let test = Cuadrado(longitudLado: 5.2, nombre: "Mi cuadrado de prueba")
test.area()
test.descripcionSencilla()
```

> EXPERIMENTO  
> Construye otra subclase de `FiguraConNombre` llamada `Circulo` que
> tome un radio y un nombre como argumentos de su
> inicializador. Implementa un método `area()` y
> `descripcionSencilla()` en la clase `Circulo`.

Además de propiedades simples que se almacenan, las propiedades pueden
tener un *getter* y un *setter*.

```swift
class TrianguloEquilatero: FiguraConNombre {
    var longitudLado: Double = 0.0
    
    init(longitudLado: Double, nombre: String) {
        self.longitudLado = longitudLado
        super.init(nombre: nombre)
        numeroDeLados = 3
    }
    
    var perimetro: Double {
        get {
            return 3.0 * longitudLado
        }
        set {
            longitudLado = newValue / 3.0
        }
    }
    
    override func descripcionSencilla() -> String {
        return "Un triangulo equilátero con lados de longitud \(longitudLado)."
    }
}
var triangulo = TrianguloEquilatero(longitudLado: 3.1, nombre: "un triángulo")
print(triangulo.perimetro)
triangulo.perimetro = 9.9
print(triangulo.longitudLado)
```

En el *setter* de `perimetro`, el nuevo valor tiene el nombre
implícito `newValue`. Puedes proporcionar un nombre explícito en el
paréntesis después de `set`.

Date cuenta de que el inicializador de la clase `TrianguloEquilatero`
tiene tres pasos diferentes:

1. Establecer el valor de las propiedades que declara la subclase.
2. Llamar al inicializador de la superclase.
3. Cambiar el valor de las propiedades definidas por la
   superclase. Cualquier trabajo adicional que use métodos, *getters*
   o *setters* puede hacerse también en este punto.

Si no necesitas calcular la propiedad pero necesitas proporcionar
código que se ejecuta antes y después de establecer un nuevo valor,
usa `willSet` y `didSet`. El código que proporcionas se ejecuta cada
vez que el valor cambia fuera de un inicializador. Por ejemplo, la
siguiente clase se asegura de que la longitud del lado de su triángulo
siempre es la misma que la longitud del lado de su cuadrado.

```swift
class TrianguloYCuadrado {
    var triangulo: TrianguloEquilatero {
        willSet {
            cuadrado.longitudLado = newValue.longitudLado
        }
    }
    var cuadrado: Cuadrado {
        willSet {
            triangulo.longitudLado = newValue.longitudLado
        }
    }
    init(tamaño: Double, nombre: String) {
        cuadrado = Cuadrado(longitudLado: tamaño, nombre: nombre)
        triangulo = TrianguloEquilatero(longitudLado: tamaño, nombre: nombre)
    }
}
var trianguloYCuadrado = TrianguloYCuadrado(tamaño: 10, nombre: "Otra figura de prueba")
print(trianguloYCuadrado.cuadrado.longitudLado)
print(trianguloYCuadrado.triangulo.longitudLado)
trianguloYCuadrado.cuadrado = Cuadrado(longitudLado: 50, nombre: "Cuadrado mayor")
print(trianguloYCuadrado.triangulo.longitudLado)
```

Cuando trabajamos con valores opcionales, puedes escribir `?` antes de
operaciones como métodos, propiedades y subíndices. Si el valor antes
del `?` es `nil`, todo lo que hay después se ignora y el valor de la
expresión completa es `nil`. En otro caso, el valor opcional se
desenvuelve, y todo lo que hay después del `?` se realiza sobre el
valor desenvuelto. En ambos casos, el valor de la expresión completa
es un valor opcional.

```swift
let cuadradoOpcional: Cuadrado? = Cuadrado(longitudLado: 2.5, nombre: "Cuadrado opcional")
let longitudLado = cuadradoOpcional?.longitudLado
```

#### Enumeraciones y estructuras

Usa `enum` para crear una enumeración. Como las clases y otros tipos
con nombre, las enumeraciones pueden tener métodos asociados.

```swift
enum Valor: Int {
    case uno = 1
    case dos, tres, cuatro, cinco, seis, siete, ocho, nueve, diez
    case sota, caballo, rey
    func descripcionSencilla() -> String {
        switch self {
        case .uno:
            return "as"
        case .sota:
            return "sota"
        case .caballo:
            return "caballo"
        case .rey:
            return "rey"
        default:
            return String(self.rawValue)
        }
    }
}
let carta = Valor.uno
let valorBrutoCarta = carta.rawValue
```

> EXPERIMENTO  
> Escribe una función que compare dos valores `Valor` a través de una
> comparación de sus valores brutos.

Por defecto, Swift asigna los valores brutos comenzando en cero e
incrementándolos por uno cada vez, pero puedes cambiar esta conducta
especificando explícitamente los valores. En el ejemplo anterior, a
`As` se le da un valor bruto de `1` y el resto de los valores brutos
se asignan en orden. Puedes también usar cadenas o números en punto
flotante como valores brutos de una enumeración. Utiliza la propiedad
`rawValue` para acceder al valor bruto de una enumeración.

Usa el inicializador `init?(rawValue:)` para construir una instancia
de una enumeración a través de un valor bruto.

```swift
if let valorConvertido = Valor(rawValue: 3) {
    let descripcionTres = valorConvertido.descripcionSencilla()
}
```

Los valores *case* de una enumeración son valores reales, no una forma
nueva de escribir sus valores brutos. De hecho, en los casos en los
que no hay un valor bruto que tenga sentido, no tienes que
proporcionar uno.

```swift
enum Palo {
    case oros, bastos, copas, espadas
    func descripcionSencilla() -> String {
        switch self {
        case .oros:
            return "oros"
        case .bastos:
            return "bastos"
        case .copas:
            return "copas"
        case .espadas:
            return "espadas"
        }
    }
}
let copas = Palo.copas
let descripcionCopas = copas.descripcionSencilla()
```

> EXPERIMENTO  
> Añade un método `color()` a `Palo` que devuelva "agresivo" para
> *bastos* y *espadas* y devuelva "reflexivo" para *oros* y *copas*.

Date cuenta de las dos formas en las que nos referimos al caso `Copas`
de la enumeración anterior: cuando se asigna un valor a la constante
`copas`, nos referimos al caso de la enumeración `Palo.Copas` usando
su nombre completo porque la constante no tiene un tipo explícito
especificado. Dentro del `switch`, nos referimos al caso de la
enumeración con la forma abreviada `.Copas` porque ya se sabe que el
valor de `self` es un `Palo`. Puedes usar la forma abreviada en
cualquier momento en que el tipo del valor ya se conozca.

Una instancia de un caso de enumeración puede tener valores asociados
con la instancia. Instancias del mismo caso de enumeración pueden
tener asociados valores diferentes. Proporcionas los valores asociados
cuando creas la instancia. Los valores asociados y los valores brutos
son distintos: el valor bruto de un caso de enumeración es el mismo
para todas las instancias, mientras que proporcionas el valor asociado
cuando defines la enumeración.

Por ejemplo, considera el caso de realizar una petición a un servidor
de la hora de salir el sol y de la hora de ponerse el sol. El servidor
responde con la información o responde con alguna información de
error.

```swift
enum RespuestaServidor {
    case resultado(String, String)
    case error(String)
}
 
let exito = RespuestaServidor.resultado("6:00 am", "8:09 pm")
let fallo = RespuestaServidor.error("Sin queso.")
 
switch exito {
    case let .resultado(salidaSol, puestaSol):
        print("La salida del sol es a las \(salidaSol) y la puesta es a \(puestaSol).")
    case let .error(error):
        print("Fallo...  \(error)")
}
```

> EXPERIMENTO  
> Añade un tercer caso al `ServerResponse` y al switch.

Date cuenta de cómo la hora de salir el sol y de ponerse el sol se
extraen del `ServerResponse` como parte del emparejamiento entre el
valor y los casos switch.

Usa `struct` para crear una estructura. Las estructuras comparten
muchas características de las clases, incluyendo métodos e
inicializadores. Una de las diferencias más importantes entre
estructuras y clases es que las estructuras siempre se copian cuando
las pasas en tu código, mientras que las clases se pasan por
referencia.

```swift
struct Carta {
    var valor: Valor
    var palo: Palo
    func descripcionSencilla() -> String {
        return "El \(valor.descripcionSencilla()) de \(palo.descripcionSencilla())"
    }
}
let tresDeEspadas = Carta(valor: .tres, palo: .espadas)
let descripcionTresDeEspadas = tresDeEspadas.descripcionSencilla()
```

> EXPERIMENTO  
> Añade un método a `Carta` que cree un mazo completo de cartas, con
> una carta de cada combinación de valor y palo.

