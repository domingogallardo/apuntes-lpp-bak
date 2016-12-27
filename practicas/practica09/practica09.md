## Práctica 9: Introducción a Swift

Para entregar la práctica debes subir a Moodle el fichero `practica09.swift` con una cabecera inicial con tu nombre y apellidos, y las soluciones de cada ejercicio separadas por comentarios. Cada solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Una visualización por pantalla de todos los ejemplos incluidos en el enunciado que **demuestren qué hace la función**.


#### Ejercicio 1

Implementa en Swift la función `ordenado(array: [Int]) -> Bool` que recibe como argumento un array de `Int` y devuelve `true` si los números del array están ordenados de forma creciente y `false` en caso contrario. Suponemos arrays de 1 o más elementos.

Ejemplo:

```swift
let aOrdenado = [10, 20, 30]
let aDesordenado = [20, 10, 30]

print("¿El array \(aOrdenado) está ordenado?: \(ordenado(aOrdenado))")
// Imprime: ¿El array [10, 20, 30] está ordenado?: true
print("¿El array \(aDesordenado) está ordenado?: \(ordenado(aDesordenado))")
// Imprime: ¿El array [20, 10, 30] está ordenado?: false
```


#### Ejercicio 2

Implementa las funciones 

- `union(intervalo: (Int, Int), con intervalo2: (Int, Int)) -> (Int, Int)` 
- `interseccion(intervalo:(Int, Int), con intervalo2: (Int, Int)) -> (Int, Int)?`

que devuelvan el resultado de unir e intersectar dos intervalos de números representados por tuplas `(Int, Int)`. El primer `Int` de la tupla representa el límite inferior del intervalo y el segundo su límite superior (ver ejemplos en la práctica 2 de la asignatura). En el caso de la intersección, si los intervalos no intersectan se devolverá `nil`.

Ejemplo:

```swift
let intervalo1 = (4, 10)
let intervalo2 = (3, 8)

print("La unión de \(intervalo1) y \(intervalo2) es \(union(intervalo1, con: intervalo2))")
// Imprime: La unión de (4, 10) y (3, 8) es (3, 10)

let intervalo3 = (8, 15)
let intervalo4 = (12, 20)

print("La intersección de \(intervalo1) y \(intervalo3) es \(interseccion(intervalo1, con: intervalo3))")
// Imprime: La intersección de (4, 10) y (8, 15) es Optional((8, 10))
print("La intersección de \(intervalo1) y \(intervalo4) es \(interseccion(intervalo1, con: intervalo4))")
// Imprime: La intersección de (4, 10) y (12, 20) es nil
```

#### Ejercicio 3

Implementa la función `buscaValores` que recibe un array de enteros y un diccionario de enteros y cadenas y devuelve un array de cadenas. Los números del array son claves del diccionario que pasamos como segundo parámetro. La función debe devolver el array de cadenas correspondientes a los números que hay en el array de enteros.

Ejemplo de invocación:

```swift
buscaValores([1,2,2,1,3], [1: "patatas", 2: "huevos", 3: "leche"])
// devuelve ["patatas", "huevos", "huevos", "patatas", "leche"]
```

#### Ejercicio 4

Supongamos que queremos calcular estadísticas sobre respuestas de una encuesta. Las respuestas se definen con el siguiente `enum`:

```swift
enum Respuesta: Int {
   case Nada = 0, Regular, Medio, Bastante, Todo
}
```

Implementa la función `estadisticas(respuestas: [Respuesta]) -> (Respuesta, Double)` que recibe un array de respuestas de la encuesta y devuelve la respuesta más frecuente y la media numérica de las respuestas, suponiendo que el valor de `Respuesta.Nada` es `0` y el de `Respuesta.Todo` es 4.

Ejemplo:

```swift
let contestaciones = [Respuesta.Nada, Respuesta.Nada, Respuesta.Todo, Respuesta.Medio, Respuesta.Regular]
print(estadisticas(contestaciones))
// Imprime (Respuesta.Nada, 1.4)
```

#### Ejercicio 5

Implementa la función `cuentaOcurrencias(array1: [String], _ array2: [String]) -> [(String, Int, Int)]` que recibe dos arrays de cadenas y devuelve una lista de tuplas de tipo `(String, Int, Int)`. Las tuplas representan el número de veces que aparece la cadena en la primera y la segunda lista.

Ejemplo:

```swift
print(cuentaOcurrencias(["pera", "melón", "pera", "manzana", "orégano"], 
                        ["perejil", "pera", "hierbabuena", "orégano", "perejil"]))
// Imprime: [("pera", 2, 1),
//           ("melón", 1, 0),
//           ("manzana", 1, 0),
//           ("orégano", 1, 1),
//           ("perejil", 0, 2),
//           ("hierbabuena", 0, 1)]
```

----

Lenguajes y Paradigmas de Programación, curso 2015-16  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  





