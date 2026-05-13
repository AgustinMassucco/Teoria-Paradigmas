# Introducción

--- 

Haskell está basado en el cálculo lambda, que es un sistema de reglas de transformación o reductor de expresiones que diseñó Alonzo Church.

$\lambda x:x*x$ es una expresión lambda que denota la función cuadrado.

En Haskell se codifica así:

```haskell
\x -> x * x
```

Y también podemos definirla como:

```haskell
cuadrado = \x -> x * x
```

La barra invertida (\\) es el símbolo que remite a la letra griego lambda $\lambda$ que es el ícono de la programación funcional. Luego de los parámetros que se separan por espacios, la flecha `->` termina de definir el cuerpo de la función.

Se evalúa de esta manera:

```haskell
> (\x -> x* x) 2
4
```

Otro ejemplo:

```haskell
> (\x y -> x + y) 2 3
5
```

Las expresiones lambdas permiten definir funciones *anónimas* que se usan en un contexto limitado. Al no tener nombre, es una variante menos expresiva que una función cuadrado o suma, que además se puede utilizar en diferentes contextos.

## Definición local de una función 

Otra forma de escribir lo mismo

```haskell
sumar 2 3 where sumar x y = x + y
```

Si quiero saber el número de raíces de una ecuación cuadrática:

```haskell
numeroDeRaices a b c
    | discriminante > 0 = 2
    | discriminante == 0 = 1
    | discriminante < 0 = 0
where discriminante = b * b - 4 * a * c
```

Defino una expresión en un solo lugar. Tiene un nombre explícito, lo que ayuda a su comprensión y la ventaja de no tener que definirse por "afuera", aunque también sirve solamente en el contexto local de una función.

## Uso efectivo de lambdas

### Lambdas con filter y map

Las lambdas son útiles cuando queremos trabajar con funciones de orden superior y no tenemos necesidad de reutilizar una expresión en otro contexto

```haskell
λ filter (\cliente -> edad cliente > 40) clientes
```

en muchos casos podemos resolver el mismo problema con composición y aplicación parcial

```haskell
λ filter ((>40).edad) clientes
```

y **ciertamente es la opción que vamos a preferir en la cursada**, ya que no solo es más conciso el código sino que demuestra más entendimiento de los conceptos del paradigma.

Algunos contraejemplos en donde el uso de funciones anónimas nos facilita resolver ciertos requerimientos.

```haskell
λ filter ((\unaEdad -> unaEdad < 20 || unaEdad > 60) . edad) clientes
```

Otro ejemplo puede ser una función anónima ad-hoc que permite sumar los elementos de una tupla:

```haskell
λ map (\(a,b) -> a + b) [(1, 2), (3, 5), (6, 3), (2, 6), (2, 5)]
[3, 8, 9, 8, 7]
```

#### Consecuencias de las lambdas

Si un empleado es una estructura definida de la siguiente manera

```haskell
data Empleado = Empleado {
 nombre :: String,
 sueldo :: Float,
 cantidadHijos :: Int,
 sector :: String
} deriving (Show)
```

y dada una lista de empleados

```haskell
type Nomina = [Empleado]

empleados :: Nomina
empleados = [ Empleado "mara" 17000.0 1 "contaduria",
 Empleado "gerardo" 15000.0 2 "ventas", .
```

Podemos saber cuál es el monto total que paga la empresa en sueldos, haciendo una consulta por consola

```haskell
λ foldl (\total empleado -> total + sueldo empleado) 0.0 empleados
```

También podemos saber cuántos hijos tienen los empleados en total:

```haskell
λ foldl (\total empleado -> total + cantidadHijos empleado) 0 empleados
```

pag 6