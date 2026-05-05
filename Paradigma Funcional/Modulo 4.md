Recursividad y Evaluación diferida
===

<h1>Introducción</h1>

Una función puede llamarse a sí misma,y ese mecanismo recursivo es el que implementa una estructura de repetición en el paradigma funcional.

**Ejemplo: Factorial**:

La definición del factorial es:
$n! = 1 × 2 × 3 × ... × (n − 1) × n$

Genéricamente:
$n! = { 1 → n = 0  \cup n=n×(n-1)! → n>0}
$

**Solución 1: Definición con guardas**:

```haskell
factorial n
 | n == 0   =1
 | n > 0    = n * factorial (n - 1)
```
Aquí vemos que cualquier valor entero "matchea" con n (si quiero evaluar factorial 3, la variable n se unifica con el valor  3).

**Solución 2: Definición con pattern matching**:

```haskell
factorial 0 = 1
factorial n = n * factorial (n - 1)
```

Aquí interviene el concepto de *pattern matching*: tratamos de hacer que encaje un determinado valor en una expresión:
 
 -`factorial 3` no matchea con `factorial 0`
 Pero 3 sí es unificable a n

**Recursividad e inducción**:

El concepto de recursividad viene atado al de inducción: verifico P(0), y defino P(N + 1) en base a P(N). Volviendo al ejemplo del factorial, primero definimos el caso base, que es el que corta la recursividad: para 0, el factorial es 1:

`factorial 0 = 1`

El factorial de un número positivo mayor a 0 se calcula como n multiplicado por el factorial de (n -1), y aquí tenemos el caso recursivo:

`factorial n = n * factorial (n - 1)`

Todo algoritmo recursivo debe tener:

- Un caso base para cortar la recursividad
- Un caso recursivo para que verdaderamente exista recursividad

**Segundo ejemplo: Fibonacci**:

```haskell
fibonacci 0 = 1
fibonacci 1 = 1
fibonacci n = fibonacci (n - 1) + fibonacci (n - 2)
```

<h3>Evaluación diferida</h3>

Haskell y muchos otros lenguajes del paradigma funcional trabajan con el concepto de **evaluación diferida o perezosa** (*lazy evaluation*), de manera de evaluar los argumentos a medida que los va necesitando.

**Ventajas de la evaluación diferida**:

- Con la evaluación diferida sólo se evalúa aquello que realmente se necesita.
  - Como corolario, puedo trabajar con estructuras potencialmente infinitas mientras asegure que el algoritmo converge.
