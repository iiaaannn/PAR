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
  - [Secciones](#secciones)
  - [Concordancia de patrones (Pattern Matching)](#concordancia-de-patrones-pattern-matching)
  - [Subexpresiones](#subexpresiones)
  - [Operadores y funciones](#operadores-y-funciones)
  - [Genericidad restringida](#genericidad-restringida)
- [Sistemas de tipado](#sistemas-de-tipado)

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

En Haskell no existe distinción entre operadores y funciones, es decir : `a + b` $\equiv$ `suma a b`

Todo operador puede operar como una función, basta con escribirlo entre paréntesis $\rightarrow$ `(+) a b`

Y además, toda función <u>binaria</u> puede funcionar como un operador, escribiéndolo entre acentos $\rightarrow$ `3 'max' 7`

## Secciones

Como hemos visto en las funciones, no aceptan realmente dos parámetros, aceptan uno y devuelven una función con ese parámetro para después usar el otro.

```hs
max :: Int -> (Int -> Int) -- paréntesis implícitos
max x y = if x > y then x else y
```

```hs
mayorQue10 :: Int -> Int
mayorQue10 y = max 10
```

La función `mayorQue10` toma la función predefinida `max`, y devuelve el propio número si es mayor que 10 o 10 si es menor que 10.

Por lo tanto, con operadores podemos hacer lo mismo. Esto es lo que se denomina una ***sección*** :

```hs
(+1) n -- n + 1
(+(-1)) n -- n - 1
(*2) n -- n * 2
(1/) n -- 1 / n
(^2) n -- n ^ 2
(<3) n -- n < 3
...
```

## Concordancia de patrones (Pattern Matching)

Esta técnica consiste en que en vez de escribir funciones usando una única definición gigante como esta :

```hs
fact n = if n > 1 then 1 else n * fact(n-1)
```

se usan varias definiciones y en vez de indicando parámetros, ***patrones*** :

```hs
fact 0 = 1
fact n = n*fact(n-1)
```

## Subexpresiones

La cláusula `where` permite que el código sea más fácil de leer y evita repetir cosas. 

Imagina que quieres sacar las dos soluciones de una ecuación de segundo grado : 

```hs
eq2grad a b c = ( (-b + sqrt (b^2 - 4*a*c)) / (2*a) , 
                  (-b - sqrt (b^2 - 4*a*c)) / (2*a) )
```

El problema de esto es que haskell tiene que calcular la raíz cuadrada dos veces seguidas (y es una pérdida de tiempo)

```hs
eq2grad a b c = ( (-b + disc) / den , 
                  (-b - disc) / den )
  where
    disc = sqrt (b^2 - 4*a*c)
    den  = 2 * a
```

como se observa en el código, `where` permite definir una única vez la operación del discriminante, y poder usarla tantas veces como queramos.

Además, nos permite definir variables y hacerlas depender de otras definidas anteriormente.

## Operadores y funciones

- `+ - *` $\rightarrow$ aplica la operación aritmética correspondiente
- `a ^ b` $\rightarrow$ aplica la operación de potencia. Se usa para elevar a números enteros ($2^2$, $2^3$, ...)
- `a ** b` $\rightarrow$ aplica la operación de potencia. Se usa para elevar a números reales ($2^{\frac{1}{3}}$, $2^{\frac{1}{2}}$, ...)
- `div mod` $\rightarrow$ devuelven el cociente y el resto de una división respectivamente
- `/` $\rightarrow$ aplica la división entre números reales ($\frac{2.5}{3.2}$, $\frac{1}{5}$, ...)
- `&& || not` $\rightarrow$ aplican la operación de AND, OR, NOT booleano respectivamente
- `== /=` $\rightarrow$ es como el `== !=` de otros lenguajes (igual/distinto)
- `< <= > >=` $\rightarrow$ devuelve True si es menor, menor o igual, mayor, mayor o igual respectivamente.
- `f . g` $\rightarrow$ aplica la composición de funciones ($f(g(x))$)
- `f $ g` $\rightarrow$ aplica g y lo que salga se le aplica a f
- `pred, succ` $\rightarrow$ devuelve el elemento anterior/siguiente respectivamente (`succ 5` $\rightarrow$ `6`, `pred 6` $\rightarrow$ `5`)
- `show` $\rightarrow$ traduce a String el valor (`show 5` $\rightarrow$ `"5"`, `show True` $\rightarrow$ `"True"`)

## Genericidad restringida

Además de tipos de datos, existen ***clases*** de tipos. Una clase es un conjunto de tipos para los que se garantiza que existe una serie de funciones

```hs
class Dibujable a where
    dibujar :: a -> String
```

Aquí, cualquier tipo a que quieran ser <u>dibujables</u>, debe saber usar la función "dibujar"

Si un tipo pertenece a una clase, se dice que es una ***instancia*** de la clase, y por lo tanto sabe usar las funciones definidas en la clase.

```hs
class (Eq a, Show a) => Num a where
  (+), (-), (*) :: a -> a -> a
  -- ...
```

lo que acabamos de crear aquí es una clase <u>Num</u> que "hereda" de <u>Eq</u> y <u>Show</u>. Por lo tanto, los datos que sean de tipo **Num**, deben ser también de tipo **Eq** y **Show**, por lo tanto podrán usar las funciones definidas en estas dos últimas además de las funciones definidas en la clase Num (suma, resta, multiplicación...)

En POO los objetos llevan consigo los métodos, mientras que en Haskell es el sistema el que guarda una especie de tabla con funciones y selecciona la más adecuada.

El programador puede definir nuevas clases y declarar tipos como pertenecientes a esas clases

# Sistemas de tipado