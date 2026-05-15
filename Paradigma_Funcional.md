# Paradigma funcional

- [Paradigma funcional](#paradigma-funcional)
- [Características](#características)
  - [Funciones](#funciones)
- [Problemas](#problemas)
- [Lenguaje Haskell](#lenguaje-haskell)
  - [Elementos básicos de Haskell](#elementos-básicos-de-haskell)
    - [Tipos predefinidos](#tipos-predefinidos)
      - [Tipos simples](#tipos-simples)
      - [Tipos compuestos](#tipos-compuestos)
    - [Tipos funcionales](#tipos-funcionales)
    - [Declaraciones de tipos](#declaraciones-de-tipos)
  - [Definición de funciones](#definición-de-funciones)
    - [Evaluación directa](#evaluación-directa)
    - [Evaluación diferida/perezosa](#evaluación-diferidaperezosa)
    - [Aplicación de funciones](#aplicación-de-funciones)

# Características

En el paradigma funcional <u>NO</u> existe la operación de asignación. Las "variables" almacenan definiciones o referencias a expresiones. Está basado en el modelo matemático de ***función*** (la operación fundamental es la aplicación de una función a una serie de argumentos) y entonces un programa consiste en una serie de definiciones (funciones, tipos de datos...). Las estructuras básicas de control básicas (y generalmente únicas) son la **composición** y la **recursión**.

## Funciones

En los lenguajes funcionales, las funciones son de ***primer orden*** (high-order) (significa que no es sólo un bloque de código, es un valor más, como un número o una letra). Podemos pasarlas como <u>parámetros</u> a otras funciones, ser <u>devueltas</u> por funciones, <u>combinarse</u> con otras funciones, un <u>tipo de datos</u> asociado.

En haskell, las funciones están **currificadas** (es decir, tienen como <u>máximo</u> una entrada y una <u>única</u> salida). Una función con dos parámetros, se ejecutaría de forma <u>parcial</u> $\rightarrow{}$ `suma(1, 2)` se ejecutaría primero la función con el parámetro `1` devolviendo una función con el 1 guardado y volviendo a llamar a `suma()` pero con el parámetro `2`. Además los valores se obtienen mediante **constructores de datos** que pueden verse como <u>funciones congeladas</u>.
```haskell
data Punto = CrearPunto Float Float 
``` 

Aquí CrearPunto es el constructor (que está congelado, pues no cambia ningún parámetro, y forma una estructura que mantiene unidos a esos parámetros), y podría verse como una función que recibe como parámetro dos `Float` y devuelve un `Punto`. 

# Problemas 

Pueden surgir problemas como : 

- :point_right: Interacción con el mundo exterior : al hacer operaciones de entrada/salida (I/O) es inevitable producir efectos laterales. Hay que evitar que esto ocurra, y para eso se proponen dos soluciones : 
  - **Abandonar la pureza** : permite que existan funciones que tengan efectos laterales.
  - **Introducir el mundo exterior como entidad** : el problema es que se pueden generar mundos paralelos.
  - &#9989; **Solución de haskell (*mónadas*)** : los <u>mónadas</u> permiten encapsular acciones impuras, y por lo tanto, las funciones que las usen, se siguen viendo como puras hacia el exterior.
    - **mónada IO (IO $\alpha$)** : representa una acción que devuelve $\alpha$ ($\alpha$ puede ser un Int, un String...)
      - **putStrLn :: String $\rightarrow{}$ IO ()** : al ser evaluada, muestra una cadena por pantalla.
      - **getLine :: IO String** : pide una cadena al usuario.
- :point_right: Eficiencia : si no podemos modificar datos, hay que crear duplicados que sí incorporen modificación (más código, menos eficiente) Esto resulta un problema, pero no es tan grave como se piensa, pues aunque los elementos sean ***inmutables*** (es decir, cada vez que añadimos una variable a una lista por ejemplo, no se modifica la lista, sino que se crea una nueva lista con todos los elementos anteriores mas el nuevo), existen ***estructuras enlazadas***, podemos ***reciclar estructuras***... que hacen que los programas no sean tan ineficientes como se piensa.
- :point_right: complejidad de programación : como no existen estados externos, se debe enviar a cada función todos los datos necesarios, lo que complica la programación.
- :point_right: sistemas de tipado : resulta difícil incorporar un sistema de tipado a la programación funcional.

# Lenguaje Haskell

Es un lenguaje <u>funcional puro</u> :
- tipado algebraico con inferencia de tipos
- tipado estricto y seguro
- funciones currificadas
- concordancia de patrones
- evaluación perezosa/diferida
- listas infinitas
- I/O y estilo pseudo-imperativo mediante mónadas

## Elementos básicos de Haskell

### Tipos predefinidos

#### Tipos simples

- `Int` $\rightarrow$ entero
- `Integer` $\rightarrow$ enteros de tamaño arbitrario
- `Double` $\rightarrow{}$ números reales
- `Char` $\rightarrow{}$ caracteres
- `Bool` $\rightarrow{}$ valores literales (***True*** y ***False***)

#### Tipos compuestos

- `Listas` $\rightarrow$ **[a]** : contienen elementos de tipo `a` (todos los elementos deben ser del mismo tipo) Un ejemplo : `[1,2,3]`
- `Tuplas` $\rightarrow$ **(a,b...)** : pueden contener elementos de distinto tipo. Un ejemplo : `('x', True, 2)`
- `String` $\rightarrow$ en Haskell, las cadenas de caracteres son simplemente una <u>lista de caracteres</u>. Un ejemplo : `"Hola" = ['H', 'o', 'l', 'a']`

### Tipos funcionales

El operador `->` es un constructor de tipos funcionales. Si por ejemplo tenemos `a -> b` indica que son funciones que reciben un parámetro de tipo `a` y devuelven un resultado de tipo `b`. 

Este operador es <u>asociativa a la derecha</u>, por lo tanto : `a -> b -> c ≡ a -> (b -> c)` quiere decir que es una función que recibe como parámetro un tipo `a` y devuelve otra función que toma como parámetro el tipo `b` y por último devuelve un valor de tipo `c` (también se puede ver como una función que toma como dos parámetros de tipo `a` y `b`y devuelve un tipo `c`)

### Declaraciones de tipos

El operador `::` se traduce como ***pertenecer al tipo***. Básicamente es como decir de qué tipo de dato va a ser un nombre. Por ejemplo : 

```hs
edad :: Int
edad = 18
```

Alguna cosilla importante : 

- `raiz :: Double -> Double` : esto indica que el identificador raíz hace referencia a una función que recibe un valor real (Double) y devuelve un valor real.

## Definición de funciones

En haskell, una **función** es la expresión por la que se puede sustituir una <u>aplicación</u> (una aplicación es la llamada de la función) de la función a un parámetro al evaluar una expresión en la que aparezca. 

En resumidas cuentas, si tenemos : 

```hs
triplicar x = x + x + x
10 + triplicar 2
```

cuando haskell ve `triplicar 2`, lo que hace no es evaluar esa expresión, sino sustiturilo por su definición (`2 + 2 + 2`), por lo tanto quedaría así : `10 + 2 + 2 + 2`

Existen dos tipos de formas de evaluar una función : 

### Evaluación directa

```hs
sumarDiez :: Int -> Int
sumarDiez x = x + 10
```

Al hacer `sumarDiez(2 + 3)` primero resuelve lo que hay dentro del paréntesis `2 + 3 = 5` y después llama a la función con el 5 $\rightarrow$ `sumarDiez 5`

### Evaluación diferida/perezosa

Para el mismo ejemplo de antes, lo que hace el ordenador es llamar a la función directamente : `sumarDiez (2 + 3) = (2 + 3) + 10`, y ahora que tiene la expresión completa, la evalúa (lo suma todo) $\rightarrow$ `(2 + 3) + 10 = 5 + 10 = 15`

Existen expresiones (***Formas normales***) que <u>NO</u> se evalúan. Estas son datos y funciones constructoras.

### Aplicación de funciones

En haskell, la llamada a una función se hace mediante mera **yuxtaposición** (pasando los parámetros uno junto a otro) : 

```hs
max :: Int -> Int -> Int
max x y = if x > y then x else y

max 3 5 -- BIEN
max(3, 5) -- MAL (se entiende que se le pasa una tupla a la función y eso está mal)
```

Llamar a una función tiene **precedencia máxima** (es decir, es más importante que cualquier otra cosa)