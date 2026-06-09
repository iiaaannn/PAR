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
  - [Tipado algebraico](#tipado-algebraico)
  - [Tipos recursivos y paramétricos](#tipos-recursivos-y-paramétricos)
    - [Tipos paramétricos](#tipos-paramétricos)
    - [Tipos recursivos](#tipos-recursivos)
    - [Tipos predefinidos](#tipos-predefinidos-1)
- [Listas](#listas)
  - [Funciones predefinidas para listas](#funciones-predefinidas-para-listas)
  - [Funciones anónimas](#funciones-anónimas)
  - [Listas enumeradas (rangos)](#listas-enumeradas-rangos)
- [MAP](#map)
- [FILTER](#filter)
- [FOLDER](#folder)
- [ZIP](#zip)
  - [ZipWith](#zipwith)
- [Compresión de listas](#compresión-de-listas)

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

En resumidas cuentas, la evaluación diferida hace que una expresión no se evalúe hasta que sea estrictamente necesario.

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

- Nota : la cláusula `otherwise` significa "en cualquier otro caso". Esto se usa cuando no se cumple ninguna de las condiciones anteriores (es como el else)

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

Haskell tiene un sistema de tipado **estricto** y **seguro** (todo valor pertenece a un tipo de datos, toda función pertenece a un tipo de datos...) Los errores se detectan en tiempo de compilación.

Haskell tiene <u>inferencia de tipos</u>, es decir, no es necesario (pero si conveniente) declarar el tipo de ningún elemento, pues el compilador puede averiguarlo

## Tipado algebraico

Haskell usa un sistema de tipado **algebraico**. Esto significa que todo valor proviene de un <u>constructor de datos</u> (que son funciones) : 
- **No evaluables (Formas normales)** :  podemos pensar en ellas como etiquetas que identifican valores 
- **Funciones sin parámetros** : valor constante
- **Funciones con parámetros** : encapsula varios datos en parámetros (como un registro) (se puede usar ***concordancia de patrones*** para acceder a esos parámetros/campos del registro)

Podemos definir varios constructores para un mismo tipo de dato.

Los tipos de datos pueden ser **recursivos**.

Para crear los tipos, se usa la siguiente sintaxis :

```hs
data Tipo = Constructor { parámetros } | Constructor2 { parámetros2 } | ...
```

Los paréntesis indica que no es obligatorio. Pongo un ejemplo : 

```hs
data Genero = Mujer | Hombre | Otro String
Otro "Perro" -- valor de tipo "Otro"
```

Las funciones "Hombre" y "Mujer" son funciones sin argumento que construyen un valor de tipo "Genero"

Para acceder a los datos usamos concordancia de patrones, como en este ejemplo :

```hs
data Clima = Soleado | Lloviendo

queHacer :: Clima -> String
queHacer dia = case dia of
  | Soleado   -> "Ir al parque"
  | Lloviendo -> "Quedarse en casa leyendo"
```

- Nota : la cláusula `case` permite utilizar concordancia de patrones en expresiones.

## Tipos recursivos y paramétricos

### Tipos paramétricos

Esto permite que en vez de crear una lista con enteros, otra con Strings... (`ListInt`, `ListString`...) creamos una única lista que sea genérica, así : 

```hs
data List a = ...
```

Esto significa que "aquí va un tipo de dato, el que tú quieras" (es como el `List <T>` de Java)

### Tipos recursivos

Es un tipo que para definirse se llama <u>a sí mismo</u>. Aquí un ejemplo de una lista con tres elementos : 

```hs
Nodo 1 (Nodo 2 (Nodo 3))
```

### Tipos predefinidos

Todos los tipos predefinidos en clase tienen el mismo esquema en su definición :

```hs
data () = () -- Tipo nulo
data Bool = False | True -- Booleanos
data Char = .. |'a'|'b'|'c' .. -- Caracteres
data Int = .. |-1| 0| 1| 2 .. -- Enteros
data Ordering = LT | EQ | GT -- Res. ordenación
data [a] = [] | a : [a] -- Listas
data (a,b) = (a,b) -- Tuplas
data Maybe a = Nothing | Just a -- Nulificables
data Either a b = Left a | Right b -- Alternativa
type String = [Char] -- Ejemplo de tipo sinónimo
```

# Listas

Las listas son las estructuras de datos básicas en Haskell. Además de almacenar datos, son elementos fundamentales en el diseño de algoritmos.

Una lista es un par `x : xs` donde `x` es el primer elemento de la lista y `xs` es la sublista con el resto de elementos.

- Si la lista es de un solo elemento, podemos tener `x : z` ó `xs : []`

En Haskell una lista puede estar definida por una expresión, y por el mecanismo de **evaluación diferida** (es decir, que hace únicamente lo estrictamente necesario) podemos definir/usar listas infinitas así : 

```hs
numerosNaturales = [..1] -- lista con numeros desde el 1 hasta el infinito
```

## Funciones predefinidas para listas

- Nota : el símbolo `-` significa algo como "sé que aquí hay un elemento, pero no lo quiero para nada, deséchalo"
- `Head` : accede al primer elemento de la lista
```hs
head :: [a] -> a
head (x:_) = x
head [1, 2, 3, 4, 5] = 1
```

- `Last` : accede al último elemento de la lista

```hs
last [a] -> a
last [x] = x
last (_:xs) = last xs
last [1, 2, 3, 4, 5] = 5
```

- `Tail` : me devuelve la lista quitando el primer elemento

```hs
tail :: [a] -> a
tail (_:xs) = xs
tail [1, 2, 3, 4, 5] = [2, 3, 4, 5]
```

- `Init` : me devuelve la lista quitando el último elemento

```hs
init :: [a] -> a
init [x] = []
init (x:xs) = x : init xs
init [1, 2, 3, 4, 5] = [1, 2, 3, 4]
```

- `Length` : devuelve la longitud de la lista

```hs
Length :: [a] -> Int
length [] = 0
length (_:xs) = 1 + length xs
length [1, 2, 3, 4, 5] = 5
```

- `!!` : me devuelve el elemento i-ésimo

```hs
(!!) :: [a] -> Int -> a
(x:_xs) !! 0 = x
(_:xs) !! n = xs !! (n-1)
[1, 2, 3, 4, 5] !! 2 = 3
```

- `(++)` : concatena listas

```hs
(++) :: [a] -> [a] -> [a]
[] ++ ys = ys
(x:xs) ++ ys = x:(xx ++ ys)
[1, 2] ++ [3, 4, 5] = [1, 2, 3, 4, 5]
```

- `Take` : coge los n primeros elementos

```hs
take :: Int -> [a] -> [a]
take n _ | n <= 0 = []
take _ [] = []
take n (x:xs) = x : take (n-1) xs
take 3 [1, 2, 3, 4, 5] = [1, 2, 3]
```

Análogamente, la función `drop` elimina los n primeros elementos

- `TakeWhile` : coge TODOS los elementos hasta que se cumpla la condición

```hs
takeWhile :: (a -> Bool) -> [a] -> [a]
takeWhile p [] = []
takeWhile p (x:xs)
  | p x = x : takeWhile p xs
  | otherwise = []
takeWhile (< 4) [1,2,3,4,5] = [1, 2, 3]
```

Análogamente, la función `dropWhile` elimina todos los elementos hasta donde se cumple la condición

- `reverse` : invierte una lista

```hs
reverse :: [a] -> [a]
reverse [1, 2, 3, 4, 5] = [5, 4, 3, 2, 1]
```

- `span` : devuelve una tupla de dos listas. La primera la lista está formada por elementos que cumplan el predicado impuesto, y la segunda por el resto resto de elementos.

```hs
span :: (a -> Bool) -> [a] -> ([a], [a])
span (< 3) [1, 2, 3, 4, 5] = ([1, 2], [3, 4, 5])
```

## Funciones anónimas

Son funciones de la forma : 

```hs
(\(param {,param}) -> expresion)
```

y de forma más simplificada : 


```hs
\parámetros -> expresión
```

Si por ejemplo quisiéramos hacer una función que sume 1 a un número dado, lo haríamos así : 

```hs
sumar1 :: Int -> Int
sumar1 x = x + 1
```

Y su forma anónima sería : 

```hs
\x -> x + 1
```

## Listas enumeradas (rangos)

Todo tipo de datos que pertenezca a la clase `Enum`, tiene definidas las funciones `enumFrom`, ... Para generar rangos de valores. Existe una sintaxis especial que nos permite hacer esto : 

```
[1..10] -> [1,2,3,4,5,6,7,8,9,10]
['a'..'z'] -> "abcdefghijklmnopqrstuvwxyz"
[1,3..10] -> [1,3,5,7,9]
[1..] -> [1,2,3,4...] (lista infinita)
```

# MAP

Aplica una misma operación (unario) que transforma una lista de elementos de tipo a en otra de elementos de tipo b

```hs
map :: (a -> b) -> [a] -> [b]
map (+1) [1, 2, 3, 4, 5] = [2, 3, 4, 5, 6]
```

# FILTER

Recibe un predicado y una lista de valores. El resultado es una lista con los elementos de la primera lista que cumplan ese predicado (el resto los descarta)

```hs
filter :: (a -> Bool) -> [a] -> [a]
filter (< 3) [1, 2, 3, 4, 5] = [1, 2]
```

# FOLDER

A partir de un valor inicial, y una operación, se va aplicando la operación al valor inicial con el primer elemento de la lista, el resultado con el segundo, ...

- **Versión izquierada** (`foldl`)

```hs
foldl :: (b -> a -> b) -> b -> [a] -> b
foldl (-) 0 [1, 2, 3, 4, 5] = -15 -- ((((0 - 1) - 2) - 3) - 4) - 5 = -15
```

- **Versión derecha** (`foldr`)

```hs
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr (-) 0 [1, 2, 3, 4, 5] = 3 -- 1 - (2 - (3 - (4 - (5 -0)))) = 3
```

# ZIP

Toma el primer elemento de una lista y el primero de la otra y los junta en una tupla. Luego coge los segundos elementos y los junta en otra tupla...

```hs
zip :: [a] -> [b] -> [(a,b)]
zip [1, 2, 3] ['a', 'b', 'c'] = [(1,'a'), (2,'b'), (3,'c')]
```

Y si las listas no son de igual longitud : 

```hs
zip [1, 2, 3] ['a', 'b'] = [(1,'a'), (2,'b')] -- Haskell se detiene cuando acaba la lista más corta
```

Análogamente `unzip` toma una lista de tuplas y las separa en dos listas diferentes

Y además, podemos combinar tres elementos en una tupla con `zip3`

## ZipWith

`ZipWith` funciona de forma parecida a Zip, pero en vez de crear una lista de tuplas, crea una lista de elementos que son el resultado de aplicar una operación (la que nosotros le pongamos)

```hs
zipWith :: (a -> b -> c) -> [a] -> [b] -> [c]
zipWith (+) [1, 2, 3] [4, 5, 6] = [5, 7, 9] -- [1 + 4, 2 + 5, 3 + 6]
```

# Compresión de listas

```hs
[expr | {var <- lista, condición}]
```

Esto crea una lista formada por elementos obtenidos de evaluar la **expresión** (expr). La expresión se evalua para todos los elementos de los **generadores** (es decir, todos los elementos de la **lista**) salvo los filtrados por la **condición**.

Con esto podemos hacer cosas como estas : 

$$\{5x ~ |  ~ x \in \{1,2,3,4,5,6,7\} \}$$

que en Haskell sería : 

```hs
[5 * x | x <- [1..7]] = [5,10,15,20,25,30,35]
```

Y también podemos añadir restricciones : 

$$\{5x ~ | ~ x \in \{1,2,3,4,5,6,7\} \land x > 3\}$$

```hs
[5 * x | x <- [1..7], x > 3] = [20,25,30,35]
```
