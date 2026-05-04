# Paradigma funcional

- [Paradigma funcional](#paradigma-funcional)
- [Características](#características)
  - [Funciones](#funciones)
- [Problemas](#problemas)

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

- :point_right: Interacción con el mundo exterior : al hacer operaciones de entrada/salida (I/O) es inevitable producir efectos laterales.
- :point_right: Eficiencia : si no podemos modificar datos, hay que crear duplicados que sí incorporen modificación (más código, menos eficiente)
- :point_right: complejidad de programación : 