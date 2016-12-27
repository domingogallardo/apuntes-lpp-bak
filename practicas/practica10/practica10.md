## Práctica 10: Programación Funcional en Swift

Para entregar la práctica debes subir a Moodle el fichero `practica10.swift` con una cabecera inicial con tu nombre y apellidos, y las soluciones de cada ejercicio separadas por comentarios. Cada solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Una visualización por pantalla de todos los ejemplos incluidos en el enunciado que **demuestren qué hace la función**.


#### Ejercicio 1

a) Implementa en Swift una **función recursiva** `sumaParejas`

```swift
func sumaParejas(parejas: ArraySlice<(Int, Int)>) -> Int 
```

que recibe un `ArraySlice` de tuplas de dos enteros y devuelve un entero resultante de sumar todos los números.

Ejemplo:

```swift
let parejas = [(1, 1), (2, 2), (3, 3)]
print("La suma de las parejas: \(parejas) es \(sumaParejas(ArraySlice(parejas)))")
// Imprime: La suma de las parejas: [(1, 1), (2, 2), (3, 3)] es 12
```

b) Implementa en Swift una **función recursiva** `sumaParejasArray`

```swift
func sumaParejasArray(parejas: ArraySlice<(Int, Int)>) -> [Int]
```

que, como antes, recibe un `ArraySlice` de tuplas de dos enteros pero que esta vez devuelve el array con las sumas de cada pareja.

**Pista**: puedes usar el operador `+` para concatenar dos arrays.

Ejemplo:

```swift
sumaParejasArray([(1, 1), (2, 2), (3, 3)])
// devuelve [2, 4, 6])
```

#### Ejercicio 2

Implementa en Swift la **función recursiva** `aplica3D(_:funcion:coord:)` que reciba un `ArraySlice` de coordenadas 3D en forma de tupla, una función unaria `funcion` y un enumerado `coord` que puede tener los valores `Coord.X`, `Coord.Y` o `Coord.Z`. Deberá aplicar la función a los elementos correspondientes a esa coordenada y devolver las nuevas coordenadas.

Ejemplo:

```swift
func suma2(x: Int) -> Int {
   return x + 2
}
aplica3D([(1,2,3), (4,5,6), (7,8,9), (10,11,12)], funcion: suma2, coord: Coord.Y)
// devuelve [(1,4,3), (4,7,6), (7,10,9), (10,13,12)]
```

### Ejercicio 3

Implementa en Swift la **función recursiva** `compruebaParejas(_:funcion)` que reciba un `ArraySlice` enteros y una función que recibe un entero y devuelve un entero, y devuelva un array de tuplas. Este array debe de contener las tupla formadas por aquellos números contiguos del primer array que cumplan que el número es el resultado de aplicar la función al número situado en la posición anterior.

Ejemplo:

```swift
func cuadrado(x: Int) -> Int {
   return x * x
}
compruebaParejas([2, 4, 16, 5, 10, 100, 105], funcion: cuadrado)
// devuelve [(2,4), (4,16), (10,100)]
```

### Ejercicio 4

Implementa nuevas versiones de las funciones `union` e `interseccion` de la práctica anterior que trabajaban con intervalos, en las que se contemple la posibilidad de intervalos vacíos.

Para ello debes definir el tipo `Intervalo` como un **enumerado** que puede como valores asociados una tupla de enteros en el caso de enumeración `Limites` o la constante `Vacio`. Definidos de esta forma, los intervalos se inicializarán así:

```swift
let intervalo1 = Intervalo.Limites(10, 20)
let intervalo2 = Intervalo.Limites(15, 30)
let intervalo3 = Intervalo.Limites(25, 30)
let intervalo4 = Intervalo.Vacio
```

Ejemplo de llamadas a las funciones:

```swift
union(intervalo1, intervalo2) // devuelve Intervalo.Limites(10, 30)
union(intervalo1, intervalo4) // devuelve Intervalo.Limites(10, 20)
interseccion(intervalo1, intervalo2) // devuelve Intervalo.Limites(15, 20)
interseccion(intervalo1, intervalo3) // devuelve Intervalo.Vacio
```

### Ejercicio 5

Implementa en Swift un tipo enumerado recursivo que permita construir árboles binarios de enteros. El enumerado debe tener un caso en el que guardar tres valores (un `Int`, y dos árboles binarios: el hijo izquierdo y el hijo derecho) y otro caso constante: un árbol binario vacío. Llamaremos al tipo `BinaryTree` y a los casos `Node` y `Empty`.

Impleméntalo de forma que el siguiente ejemplo funcione correctamente:

```swift
let arbol: BinaryTree = .Node(8, .Node(2, .Empty, .Empty), .Node(12, .Empty, .Empty))
```

Implementa también la función `suma(_:)` que reciba una instancia de árbol binario y devuelva la suma de todos sus nodos:

```swift
print(suma(arbol))
// Imprime: 22
```

----

Lenguajes y Paradigmas de Programación, curso 2015-16  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  





