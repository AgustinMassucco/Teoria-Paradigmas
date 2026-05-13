Listas
===

Una lista es una seria de elementos del mismo tipo. En esa serie puede no haber elementos, en ese caso la lista es vacía.

##### Formas de generar una lista

La forma más sencilla de generar una lista es enumerar los elementos encerrados entre corchetes y separar cada elementos mediante una coma

```haskell
[1,2,3]
["hola","mundo"]
```

Los strings son literales especiales que encierran entre comillas dobles:

```
"Hola" ==> lista de caracateres ≣ ['H', 'o', 'l', 'a']
```

En una lista de Haskell todos los elementos deben ser homogéneos, de lo contrario se generará un mensaje de error.

En el caso de las listas numéricas, se puede generar una lista de enteros especificando cota inferior y superior:

```haskell
ghc>[1..10]
[1,2,3,4,5,6,7,8,9,10]
```

También podemos definir una serie numérica de enteros a partir del primer número, el siguiente y la cota superior:

```haskell
ghc>[1,3..16]
[1,3,5,7,9,11,13,15]
```

El rango puede ser ascendente o descendente (dependiendo del segundo número y las cotas inferior o superior)

```haskell
ghc>[8,7..-2]
[8,7,6,5,4,3,2,1,0,-1,-2]
```

E incluso podemos definir una lista infinita:

```haskell
ghc>[1..]
[1,2,3,4,5,6,7,..^C para cortar la evaluación
```

##### Pattern matching sobre listas

Existen varios patrones para trabajar las listas:
|Pattern|Denota|
| --    | --   |
|[ ] | Lista vacía, no se puede separar en cabeza y cola|
|(x:xs)|El operador: separa cabeza y cola de una lista. Lista que tiene al menos un elemento, donde la cabeza es un elemento x, y la cola es una lista. Para [1,2,3] la cabeza es 1, la cola es la lista [2,3].|
(x:y:ys)|Lista de al menos 2 elementos. [1,2,3] entonces x = 1, y = 2, ys = [3]|
|[x]|Lista de exactamente un elemento. para [1] x es 1|
|[x,y]|Lista de exactamente dos elementos. Para [1,2] x = 1, y = 2.|

##### Funciones que aplican sobre listas

###### Head: cabeza de una lista
La función head devuelve la cabeza de una lista.

```haskell
ghc>head [1,7,9]
1
```

Implementamos la función aplicando pattern matching:

```haskell
head (x:xs) = x
```

Como xs no nos interesa utilizarlo en la función, podemos utilizar la variable anónima (que se denota con e símbolo guion bajo o underscore):

```haskell
head (x:_) = x

ghc>:t head
head :: [a] -> a
```

###### Tail: cola de una lista

De la misma manera tenemos la función que devuelve la cola de una lista:

```haskell
tail (_:xs) = xs

ghc>:t tail
tail :: [a] -> [a]
```

###### Otras funciones

|Función|Objetivo|Ejemplo de uso|
|-|-|-|
|length|Devuelve la longitud de una lista| `ghc> length[2..4]  3`|
|sum| Suma los elementos de una lista de números|`ghc> sum [4,9,1] 14`|
|(++)|Concatena dos listas|`ghc> [3..5] ++ [4,6]  [3,4,5,4,6]`|
|take|Toma los primeros n elementos de la lista|`ghc> take 3 [4..11]  [4,5,6]`|
|drop|Devuelve la lista sin los primeros n elementos|`ghc> 2 "Ciencia" "Encia"`|
|(!!)|Devuelve el elemento que está en la posición n (donde 0 es la primer posición)|`ghc> [1..10]!! 3  4`|
|reverse|Devuelve una lista con los elementos en orden inverso|`ghc> reverse "Anita"  atinA`|

## Tuplas

Las tuplas permiten representar un tipo de dato compuesto, pero con elementos que pueden ser de distinto tipo. El número de elementos es fijo (siempre el mismo)

```haskell
(23,02,1973) => tupla de 3 elementos para representar una fecha
("Juan",23) => tupla de 2 elementos para representar una persona
```

Una tupla es un tipo de dato compuesto.

###### Pattern matching sobre tuplas

|Pattern|Denota|
|   -   |   -  |
|(x,y) | Tupla de dos elementos, x e y pueden tomar valores de cualquier tipo|
|(x,y,z)|Tupla de tres elementos x,y,z, pueden tomar valores de cualquier tipo|

###### Comparación entre tuplas y listas

- Las listas requieren que todos los elementos sean homogéneos: no podemos mezclar en una misma lista números y strings. Las tuplas pueden ser heterogéneas.
- El número de elementos de una lista es variable, puede ser infinito. En una tupla el número de elementos es fijo.
- La lista es un tipo de dato recursivo, la tupla no, aunque ambos son compuestos.

###### Funciones que aplican sobre tuplas

```haskell
fst (a,_) = a
snd (_,b) = b

fst :: (a,b) -> a
snd :: (a,b) -> b
```

###### Sinónimos de tipo

El requerimiento que nos pide es "modelar la suma entre números complejos". La primera decisión que tomamos es que usaremos una tupla para representar un número complejo.

Definimos entonces un sinónimo de tipo, para decir que existen los números complejos, de la siguiente manera.

```haskell
type Complejo = (Float, Float)
```

###### Definición de una función con tuplas

Escribimos el tipo que tiene la función que suma complejos

```haskell
sumarComplejos :: Complejo -> Complejo -> Complejo

sumarComplejos (real1, imaginario1) (real2, imaginario2) = (real1 + real2, imaginario1 + imaginario2)
```

#### Tipos propios

Se pueden definir nuevos tipos de datos que se agregan a los ya existentes en el lenguaje, mediante la expresión data. Si queremos modelar varias personas que tienen nombre y edad, lo hacemos de la siguiente manera.

```haskell
data Persona = Persona String int
```

###### Pattern matching sobre data

Para poder reconocer un tipo de dato propio usamos el constructor:

```haskell
Persona "Santiago" 32 
```

El primer parametro (String que representa el nomre) se asocia a "Santiago" y el segundo (int que representa la edad) a 32

Ahora podemos definir las funciones que permite conocer el nombre y la edad:

```haskell
nombre (Persona _nombre _edad) = _nombre
edad (Persona _nombre _edad) = _edad
```

###### Cómo definir una persona con record syntax

Si ahora 

```haskell
data Persona = Persona String Int String String (Int,Int,Int) Bool Float
```
se vuelve bastante menos expresivo. Podemos utilizar la notacion *record syntax* que consiste en definir persona de la siguiente manera:

```haskell
data Persona = Persona{
    nombre :: String,
    edad :: Int,
    domicilio :: String,
    telefono :: String,
    fechaNacimiento :: (Int,Int,Int),
    buenaPersona :: Bool
    plata :: Float
}
```

Esto nos permite modelar a una persona de forma mucho más expresiva

```haskell
juan = Persona {
    nombre = "Juan",
    edad = 29,
    domicilio = "Ayacucho 554",
    telefono = "45232598",
    fechaNacimiento = (17,7,1988),
    buenaPersona = True,
    plata = 30.0
}
```

De esta manera también podemos alternar el orden en el que definimos lo valores de Juan.

###### Funciones "adquiridas" con record syntax

Al definir una Persona con la record syntax ,adquirimos funciones que permite preguntar por las propiedades de una persona:

```haskell
ghc> telefono juan
"45232598"
ghc> domicilio juan
"Ayacucho 554"
```

Las definiciones de la función telefono o domicilio están implícitas por la definición del tipo de dato Persona:

```haskell
ghc>:t edad
edad :: Persona -> Int
```

###### Mostrar personas en la consola

Si intentamos visualizar a Juan en la consola, aparece un mensaje de error:

```haskell
ghc> juan
<interactive>:14:1:
 No instance for (Show Persona) arising from a use of ‘print’
 In a stmt of an interactive GHCi command: print it

```

Una persona no se puede puede mostrar si no se define como instancia de Show. Para solucionar este error, basta con incorporar:

```haskell
data Persona = Persona {
 nombre :: String,
 edad :: Int,
 domicilio :: String,
 telefono :: String,
 fechaNacimiento :: (Int, Int, Int),
 buenaPersona :: Bool,
 plata :: Float
} deriving (Show)

```
Ahora sí, podemos evaluar en la consola a Juan:

```haskell
ghc> juan
Persona {nombre = "Juan", edad = 29, domicilio = "Ayacucho 554",
telefono = "45232598", fechaNacimiento = (17,7,1988), buenaPersona =
True, plata = 30.0}
```
