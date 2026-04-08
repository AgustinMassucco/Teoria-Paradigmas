## Composición 
Matemáticamente, podemos aplicar dos funciones, $g(f(x))=(g \circ f)x$, generando una **nueva funcion**, que va de $a → g(f(a))$ como restriccion, la iamgen de $f$ tiene que coincidir con el dominio de $g$.

#### Implementacion
El mismo concepto se da en Haskell, donde la notación es
```haskell
g(f x) = (g . f ) x
```
que es una nueva función que resulta de aplicar `f` primero, y luego `g` en ese orden

###### Ejemplo
$4 \times x = 2 \times (2 \times x)$
```haskell
cuadruple numero = (doble . doble) numero
```
Como el tipo de doble es
```haskell
doble :: Numero -> Numero 
```
Se puede componer consigo mismo, dado que su imagen coincide con su dominio.
```haskell
cuadruple = doble . doble
```
Tambien podriamos haber pedido en la consola que calcule el cuadruple de un numero:
``` haskell
ghc> (doble . doble) 12
```

###### Ejemplo
*Saber si la longitud del nombre de una persona es una cantidad par*
```haskell
nombrePar :: String -> Bool
nombrePar = even . length
---
ghc> nombrePar "Laura"
False
```

###### Ejemplo
*Saber si una persona es mayor de edad*

¿Como modelo ua persona? Me interesa mantener solo el nombre y la edad, es decir un `String` y un `Numero`. Para esto existe **tupla**, un tipo de dato compuesto que agrupa informacion relacionada: `(String,Int)`.

  - Existe la funcio `snd`, que dada una tupla de dos elementos devuelve el segundo.

Pero `snd` no es representativa del dominio que estamos modelando, etnocnes vamos a construir una nueva función que mejore la expresividad de nuestra solución:
  
```haskell
edad = snd
```
Acabamos de escribir un sinónimo para `snd`, sin volver a definir la función. Si el operador `=` represenra la igualdad matematica, solamente estamos diciendo a Haskell que cuando necesite reducir 
`edad ("laura",41)`, esta expresión será equivalente a hacer `snd ("laura",41)`
```haskell
ghc> edad ("laura",41)
41
ghc>:t edad ("laura",41)
edad ("laura",41) :: Num b => b
```
Como no hay ninguna función que dado un número me diga si es considerable como mayoria de edad. La construimos:
```haskell
mayorDeEdad :: Integer -> Bool
mayorDeEdad edad = edad > 18
```
Ahora si, podemos componer edad y mayorDeEdad:

```haskell
esMayorEdad :: (String, Integer) -> Bool
esMayorEdad = mayoDeEdad . edad
```
Podemos definir que una ``Persona` es un `(String, Int)`, esto facilita la lectura del tipo de la función que acabamos de construir:

```haskell
type Persona = (String, Integer)
esMayorEdad :: Persona -> Bool
laura = ("Laura",41)
---
ghc> esMayorEdad laura
True
```
#### Acoplamiento de funciones
Aparece un cambio en la forma de modelar una persona, nos piden que además del nombre y la edad, tambien deberíamos conocer el domiciolio. Sabemos que se modela con un `String` ¿que debemos cambiar en nuestra solución?
```haskell
type Persona = (String, Integer, String)

laura :: Persona
laura = ("Laura",41,"Medrano 951 CABA")

edad :: Persona -> Integer
edad (_,e,_) = e
```
- La definición de `Persona` debe aceptar un `String` adicional para el domiciolio
- También la definicion de la expresion laura, que agrega el domiciolio
- La funcion edad utiliza **patter matching**, para deolver la edad en la tupla de 3 elementos que conforma la persona.
Por otra parte, veamos que estas definiciones **no cambiaron**:
```haskell
mayorDeEdad :: Integer -> Bool
mayorDeEdad edad = edad > 18

esMayorEdad :: Persona -> Bool
esMayorEdad = mayorDeEdad . edad
```

#### Composición de más de dos funciones

Pare resolver este requerimiento: "*Saber si una persona no es mayor de edad*", partiendo de lo que ya sabemos, podemos hacer una consulta directamente por consola:
```haskell
ghc> (not . esMayorEdad) laura
False
```
`not` es una función que niega el valor de verdad de un booleano.

```haskell
ghc>:t not
not :: Bool -> Bool
```
Como recibe un `Bool`, se puede componer con cualquier función que devuelva un `Bool`:
```haskell
esMenorEdad = not . mayorDeEdad . edad
---
ghc> esMenorEdad laura
False
```
###### Comienza con p
*Queremos saber si una palabra comienza con p*

La función recibe
- una palabra (un `String`)
- y devuelve un `Bool`

Para obtener la primera letra, tenemos la funcion `head` que toma el primer elemento de una lista
Para saber si una letra es 'p' nos convendria tener una función a la que la pasemoa un `Char` ynos diga si es 'p'. Por suerte existe esa funcion, si aprovechamos el operador `==` y lo aplicamso parcialmente con el caracte 'p':

```haskell
esP :: Char -> Bool
esP = ('p' ==)
```
Esa función nos dice, dado un carácter, si ese carácter es una p.
No es necesaria escribr la funcion esP, la contruimos directamente:
```haskell
ghc> (('p'==) . head) "palabra"
True
```

La nueva función toma una lsita de caracteres y devuelve un `Bool`:

```haskell
ghc>:t (('p' ==) . head)
(('p' ==) . head) :: [Char] -> Bool
```

###### Costo del estacionamiento
*"El costo de estacionamiento es de $50 la hora, con un mínimo de 2 horas"*, esto significa que:

- Si estamos 1 hora, nos cobrarán por 2 horas
- Si estamos 3 horas, nos cobrarán por 3 horas

Independientemente de eso, siempre nos cobran $50 la hora

¿Que funciones necesitamos?

- La primera que calcula el tiempo total que nos cobran: deberiamos considerar el máximo entre 2 y el tiempo qe dejamos el auto. `mas 2` (max aplicada parcialmente)
- Por otra parte, si el costo es siempre $50 la hora, podemos aplicar parcialmente el operador de multiplicacion.

```haskell
costoEstacionamiento :: Integer -> Integer
costoEstacionamiento = (*50) . max 2
---
ghc> costoEstacionamiento 1
100
ghc> costoEstacionamiento 5
250
```

Esta definicion 
```haskell
costoEstacionamiento horas = ((* 50) . max 2 horas)
```
**es incorrecta**, no se puede componer `max 2 horas` con `(* 50)` ya que `max 2 horas` se reduce a un valor entero, que **no es una función**