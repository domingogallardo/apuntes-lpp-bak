## Práctica 11: Programación funcional en Swift: clausuras y funciones de orden superior

Para entregar la práctica debes subir a Moodle el fichero
`practica11.swift` con una cabecera inicial con tu nombre y apellidos,
y las soluciones de cada ejercicio separadas por comentarios. Cada
solución debe incluir:

- La **definición de las funciones** que resuelven el ejercicio.
- Una visualización por pantalla de todos los ejemplos incluidos en el
  enunciado que **demuestren qué hace la función**.

## Ejercicios

### Ejercicio 1

Implementa en Swift utilizando funciones de orden superior la función
`ocurrenciasPalabrasLongitud(cadena:String, longitud:Int)` que recibe un
String que representa una frase y un entero que especifica una
longitud de palabra, y que devuelva un array de tuplas en donde cada
tupla contiene una palabra de la longitud especificada contenida en la
frase y el número de apariciones de esa palabra en dicha frase.

**AYUDA:**

Para construir un array con las palabras que aparecen en la frase,
puedes usar la siguiente sentencia:

```swift
let arrayPalabras = frase.characters.split{$0 == " "}.map(String.init)
```

Ejemplo:

```swift
let frase = "vamos a contar las ocurrencias de las palabras con una determinada longitud utilizando funciones de orden superior predefinidas en swift y expresiones de clausuras"

ocurrenciasPalabrasLongitud(cadena:frase, longitud:2) -> [("de", 3), ("en", 1)]
ocurrenciasPalabrasLongitud(cadena:frase, longitud:3) -> [("las", 2), ("con", 1), ("una", 1)]
ocurrenciasPalabrasLongitud(cadena:frase, longitud:7) -> []
ocurrenciasPalabrasLongitud(cadena:frase, longitud:11) -> [("ocurrencias", 1), ("determinada", 1), ("expresiones", 1)]
```


### Ejercicio 2

Implementa en Swift la función `imprimirListadosNotas(alumnos:)` que
recibe un array de tuplas, en donde cada tupla contiene información de
la evaluación de un alumno de LPP (nombreAlumno, notaParcial1,
notaParcial2, añosMatriculacion) y que debe imprimir por pantalla los
siguientes listados: 

- listado 1: array ordenado por nombre del alumno (orden alfabético
creciente) 
- listado 2: array ordenado por la nota del parcial 1 (orden
decreciente de nota) 
- listado 3: array ordenado por la nota del parcial 2 (orden creciente
de nota) 
- listado 4: array ordenado por nota del parcial 2 y año de
matriculación (orden decreciente de nota y año) 
- listado 5: array ordenado por nota que necesita obtener en el
parcial 3 para aprobar la asignatura en la convocatoria de Junio
(orden decreciente de nota necesaria)
 
**AYUDA:**

Para que los listados se muestren formateados con espacios, puedes usar la siguiente
función (para ello también debes incluir el import que se indica)
 
```swift
import Foundation
 
func imprimirListadoAlumnos(_ alumnos: [(String, Double, Double, Int)]) {
    print("Alumno   Parcial1   Parcial2   Años")
    for alu in alumnos {
        print(String(format:"%-10s",
                     OpaquePointer(alu.0.cString(using:String.Encoding.utf8)!)),
              terminator:"")

        print(String(format:"%5.2f      %5.2f    %3d",alu.1,alu.2,alu.3))
    }
}
```

 
Ejemplo:

```swift
let listaAlumnos = [("Pepe", 8.45, 3.75, 1), 
                    ("Maria", 9.1, 7.5, 1), 
                    ("Jose", 8.0, 6.65, 1),
                    ("Carmen", 6.25, 1.2, 2), 
                    ("Felipe", 5.65, 0.25, 3), 
                    ("Carla", 6.25, 1.25, 2), 
                    ("Luis", 6.75, 0.25, 2), 
                    ("Loli", 3.0, 1.25, 3)]
imprimirListadosNotas(listaAlumnos)
```
 
Algunos de los listados que se deben mostrar serían los siguientes:

```txt
LISTADO ORIGINAL
Alumno   Parcial1   Parcial2   Años
Pepe       8.45       3.75      1
Maria      9.10       7.50      1
Jose       8.00       6.65      1
Carmen     6.25       1.20      2
Felipe     5.65       0.25      3
Carla      6.25       1.25      2
Luis       6.75       0.25      2
Loli       3.00       1.25      3
 
LISTADO ORDENADO por Parcial1
Alumno   Parcial1   Parcial2   Años
Maria      9.10       7.50      1
Pepe       8.45       3.75      1
Jose       8.00       6.65      1
Luis       6.75       0.25      2
Carmen     6.25       1.20      2
Carla      6.25       1.25      2
Felipe     5.65       0.25      3
Loli       3.00       1.25      3

LISTADO ORDENADO por Nota para aprobar en Junio
Alumno   Parcial1   Parcial2   Años
Maria      9.10       7.50      1
Jose       8.00       6.65      1
Pepe       8.45       3.75      1
Carla      6.25       1.25      2
Carmen     6.25       1.20      2
Luis       6.75       0.25      2
Felipe     5.65       0.25      3
Loli       3.00       1.25      3
```


### Ejercicio 3

Dado el array listaAlumnos del ejercicio anterior, rellena los huecos
en las siguientes expresiones para que se obtenga los datos
requeridos. Deberás utilizar las funciones de orden superior (`filter`,
`map`, `reduce`).

```swift
let listaAlumnos = [("Pepe", 8.45, 3.75, 1), 
                    ("Maria", 9.1, 7.5, 1), 
                    ("Jose", 8.0, 6.65, 1),
                    ("Carmen", 6.25, 1.2, 2), 
                    ("Felipe", 5.65, 0.25, 3), 
                    ("Carla", 6.25, 1.25, 2), 
                    ("Luis", 6.75, 0.25, 2), 
                    ("Loli", 3.0, 1.25, 3)]
```

A) Eliminar los alumnos que tienen la nota media suspensa

```swift
print(listaAlumnos.filter ________________________________ )
// Resultado:
// [("Pepe", 8.4499999999999993, 3.75, 1), ("Maria", 9.0999999999999996, 7.5, 1), ("Jose", 8.0, 6.6500000000000004, 1)]
```

B) Número de alumnos que han aprobado primer parcial y suspendido el segundo

```swift
print(listaAlumnos.filter ________________________________ )
// Resultado: 5
```

C) Nota media de cada alumno

```swift
print(listaAlumnos._______________________________ )
// Resultado:
[6.0999999999999996, 8.3000000000000007, 7.3250000000000002, 3.7250000000000001, 2.9500000000000002, 3.75, 3.5, 2.125]
```

D) Sumar una convocatoria a los que tienen una nota media < 5


```swift
print(listaAlumnos._____  {
// 
// Aquí va una expresión de clausura de varias líneas
//
// 
})
// Resultado: [("Pepe", 8.4499999999999993, 3.75, 1), ("Maria", 9.0999999999999996, 7.5, 1), ("Jose", 8.0,
// 6.6500000000000004, 1), ("Carmen", 6.25, 1.2, 3), ("Felipe", 5.6500000000000004, 0.25, 4), ("
// Carla", 6.25, 1.25, 3), ("Luis", 6.75, 0.25, 3), ("Loli", 3.0, 1.25, 4)]
```

E) Nota total media de los dos parciales

```swift
print(listaAlumnos.reduce(0) __________________________________ )
// Resultado: 4.721875
```

F) Nota media de todos los alumnos en forma de tupla `(media_p1, media_p2)`

```swift
var tupla = listaAlumnos._____________________________________ )
tupla = (tupla.0 / Double(listaAlumnos.count), tupla.1 / Double(listaAlumnos.count))
print(tupla)
// Resultado: (6.6812499999999995, 2.7624999999999997)
```

### Ejercicio 4

Queremos escribir la función `calcular(exp: String, sobre:
[(Double, Double)])` que recibe una cadena que codifica una expresión
sobre una tupla y una lista de tuplas y devuelve el resultado de aplicar
esa expresión matemática sobre cada una de las tuplas de la lista.

Por ejemplo:

```swift

let tuplas = [(1.0, 2.5), (10.8, 3.3), (-1.0, 12.0), (-3.4, 4.0)]

print(calcular(exp: "$1 * 2.0", sobre: tuplas)!)
// [5.0, 6.5999999999999996, 24.0, 8.0]
print(calcular(exp: "$0 - 5.0", sobre: tuplas)!)
// [-4.0, 5.8000000000000007, -6.0, -8.4000000000000004]
print(calcular(exp: "$0 + $1", sobre: tuplas)!)
// [3.5, 14.100000000000001, 11.0, 0.60000000000000009]
```

- En el primer ejemplo la expresión indica que hay que multiplicar el
  segundo número de cada tupla por 2.0.
- En el segundo ejemplo la expresión indica que hay que restar al
  primer número de cada tupla 5.0.
- En el tercer ejemplo la expresión indica que hay que sumar el primer
  y el segundo elemento de cada tupla.

La expresión codificada tendrá siempre dos operandos y una
operación. Los operandos pueden ser las expresiones `$0` y `$1` para
referirse al primer y segundo elemento de cada tupla o números
decimales. La operación puede ser `+`, `-`, `*` y `/` para referirse a
las operaciones suma, resta, multiplicación y división.

Vamos a realizar el ejercicio en tres partes. Primero hay que
implementar una función que analiza la cadena y devuelve una tupla de
enumerados; después una función convertirá la tupla de enumerados en
una clausura y por último la función principal que usa las
anteriores y aplica la clausura al array de tuplas. Vamos a detallar
cada una de las partes.

a) En primer lugar debes implementar la función `parse(exp:)` que analice la cadena y
la transforme en una tupla de dos operandos y una operación, siendo
éstos los siguientes tipos enumerados:

```swift
enum Operando {
   case primero
   case segundo
   case valor(Double)
}

enum Operacion {
   case suma
   case resta
   case mult
   case div
}
```

La función `parse(exp:)` tiene el siguiente perfil:

```swift
func parse(exp: String) -> (op1: Operando, op2: Operando, op: Operacion)?
```

Tendrás que definir **funciones auxiliares** para implementar esta
función. 

**AYUDA**: Puedes usar la pista del ejercicio 1 para dividir la cadena
y separar los operandos.

Ejemplos:

```swift
parse("$0 + $1") 
// Devuelve opcional con la tupla (Operando.primero, Operando.segundo, Operacion.suma)
parse("5.0 * $1")
// Devuelve opcional con la tupla (Operando.valor(5.0), Operando.segundo, Operacion.mult)
parse("Hola")
// Devuelve nil
```

b) En segundo lugar debes implementar la función
`construyeFunc(op1:op2:op:)` que recibe los enumerados que representan
la expresión matemática y devuelve una clausura que aplica la
expresión matemática a una tupla.

La función tiene el siguiente perfil:

```swift
func construyeFunc(op1: Operando, op2: Operando, op: Operacion) -> ((Double, Double)) -> Double
```
La clausura que se devuelve También es conveniente que definas **funciones auxiliares**.

**Nota**: La clausura que se devuelve puede ser una clausura que
captura los parámetros `op1`, `op2` y `op` y que los usa en su
cuerpo. De esta forma podemos hacer una clausura genérica que llama a
las funciones auxiliares y evitamos tener que escribir un número
enorme de clasuras concretas con todas las posibles combinaciones de
tuplas y operadores.

c) Por último debes implementar la función `calcular(exp:sobre:)` con
el siguiente perfil:

```swift
func calcular(exp str: String, sobre tuplas: Array<(Double, Double)>) -> [Double]? 
```

----

Lenguajes y Paradigmas de Programación, curso 2016-17  
© Departamento Ciencia de la Computación e Inteligencia Artificial, Universidad de Alicante  
Antonio Botía, Domingo Gallardo, Cristina Pomares  
