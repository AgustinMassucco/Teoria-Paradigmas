Recursividad y Evaluación diferida
===

<h1>Introducción</h1>

Una función puede llamarse a sí misma,y ese mecanismo recursivo es el que implementa una estructura de repetición en el paradigma funcional.

**Ejemplo: Factorial**

La definición del factorial es:
$n! = 1 × 2 × 3 × ... × (n − 1) × n$

Genéricamente:
$n! = { 1 → n = 0  \cup n=n×(n-1)! → n>0}
$

**Solución 1: Definición con guardas**

```haskell
factorial n
 | n == 0   =1
 | n > 0    = n * factorial (n - 1)
```
Aquí vemos que cualquier valor entero "matchea" con n (si quiero evaluar factorial 3, la variable n se unifica con el valor  3).

**Solución 2: Definición con pattern matching**

```haskell
factorial 0 = 1
factorial n = n * factorial (n - 1)
```

Aquí interviene el concepto de *pattern matching*: tratamos de hacer que encaje un determinado valor en una expresión:
 
 -`factorial 3` no matchea con `factorial 0`
 Pero 3 sí es unificable a n