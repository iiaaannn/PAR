# Genericidad

- [Genericidad](#genericidad)
- [Técnicas de genericidad](#técnicas-de-genericidad)
  - [Tipado dinámico](#tipado-dinámico)
  - [Sobrecarga de subrutinas, métodos y operadores](#sobrecarga-de-subrutinas-métodos-y-operadores)
  - [Templates](#templates)
  - [Clase universal (O.O)](#clase-universal-oo)
  - [Clases parametrizadas (O.O)](#clases-parametrizadas-oo)
  - [Programación funcional](#programación-funcional)
  - [Variantes](#variantes)
  - [Metatipos y Metaclases](#metatipos-y-metaclases)
- [Sobrecarga](#sobrecarga)
- [Clase universal](#clase-universal)
- [Clases y métodos genéricos](#clases-y-métodos-genéricos)
- [Genericidad restringida](#genericidad-restringida)
- [Covarianza y contravarianza](#covarianza-y-contravarianza)
  - [Covarianza](#covarianza)
  - [Contravarianza](#contravarianza)

:point_right: ***Genericidad*** : serie de técnicas que permiten escribir algoritmos que puedan aplicarse a un amplio rango de datos (Int, String, Boolean...). Esto se consigue haciendo una <u>abstracción</u> del tipo de datos al que son aplicados y <u>parametrizando</u> el tipo de datos que intervienen. Además se busca mantener la <u>seguridad</u> del sistema de tipado.

Algunos sitios donde es aplicable la genericidad es en colas, pilas, listas, arrays, ficheros...

```java
static boolean existe(char [] v, char x) {
    int i = 0;
    while (i < v.length && v[i] != x) {
        i++;
    } 
    return i < v.length;
}
```

Este ejemplo sólo es aplicable para arrays de caracteres, no aplica genericidad.

# Técnicas de genericidad

## Tipado dinámico

Los tipos de datos se verifican en tiempo de ejecución. El problema es que es bastante lento y pueden detectarse errores tarde.

## Sobrecarga de subrutinas, métodos y operadores

Crea varias versiones de una función con el mismo nombre pero distintos parámetros. El problema es que se repite mucho código, interfiere con el polimorfismo y complica la resolución de métodos.

## Templates 

Generan código basada en moldes. El problema es que carecen de restricciones y no encaja bien el modelo orientado a objetos (<u>O.O</u>)

## Clase universal (O.O)

Usar una clase base de la que heredan todos. El problema es que requiere <u>casting</u>, que es propenso a errores y poco a elegantes

## Clases parametrizadas (O.O)

Definir clases con parámetros de tipo. El problema es que hay problemas de consistencia teórica (covarianza - contravarianza)

## Programación funcional

Se enfoca en el tipado algebraico.

## Variantes

Una variable de tipo <u>variante</u> es aquella que puede almacenar valores de distinas naturalezas pero **NO** al mismo tiempo. Puede existir un ***selector*** que "anote" 

## Metatipos y Metaclases

Una variable de tipo <u>meta-tipo</u> es aquella que puede almacenar referencias a tipos de datos. Una <u>meta-clase</u> es aquella clase cuyas instancias (objetos) son referencias a clases 

***

Respecto al ejemplo de antes, os dejo por aquí cómo sería el método "existe" pero genérico, que vale tanto para un array de caracteres, enteros, Strings... Este ejemplo usa la técnica de tipado dinámico :

```py
def existe(v, x):
    for i in v:
        if i == x:
            return True
    return False
```

# Sobrecarga

Podemos crear distintos métodos con el mismo nombre siempre que los parámetros sean diferentes, pero esto puede ocasionar una ***sobrecarga*** de métodos :

```java
static boolean existe(String [] v, String x) {
    ...
}

static boolean existe(Integer [] v, Integer x) {
    ...
}
...
// Esto es SEGURO, pero NO EFICIENTE
```

# Clase universal

Por lo tanto, una buena solución sería usar una ***clase universal***. Esta clase universal se llama **Object**, y todas las clases (String, Integer...) derivan de ella : 

```java
static boolean existe(Object [] v, Object x) {
    ...
}
// Esto es EFICIENTE, pero NO SEGURO
```

# Clases y métodos genéricos

Pero sin duda, la mejor opción es usar clases y métodos ***genéricos*** : 

```java
static <T> boolean existe(List <T> v, T x) {
    ...
}

List <String> v = Arrays.AsList("Perez","Sanchez", "Rodriguez");
List <Integer> w = Arrays.AsList(12,34,56);

System.out.println(existe(v,"Sanchez"));
System.out.println(existe(w,34));
System.out.println(existe(w,"Sanchez")); // Da error
// Esto es EFICIENTE y SEGURO
```

La sintaxis de `<T>` indica que el método es genérico. Es como decir : "En este método, la clase T no existe, es simplemente un nombre temporal que voy a usar. El primer parámetro `List <T> v` es una lista de 'esas cosas', y el segundo parámetro `T x` es exactamente una cosa de ese tipo"

Entonces, ¿cuándo se decide de qué tipo es T? pues en tiempo de compilación. Cuando llamamos al método existe() con la lista v (que es de Strings), el método detecta automaticamente que el tipo T va a ser un String, y reemplaza todas las T's por Strings, y lo mismo con la lista w de enteros.

- Nota : los <u>templates</u> en C++ van de la mano con los métodos genéricos en Java.

***

:point_right: ***contenedor*** : objeto que guarda otros objetos. La mejor forma de crearlos es mediante una **clase parametrizada (genérica)**. Según su <u>semántica</u>, podemos tener : 

- **procedencia (listas/colas)** : los elementos van uno detrás de otro. Importa el orden.
- **jerarquía (árboles)** : los elementos se organizan como un árbol genealógico.
- **vecindad (grafos)** : los elementos están conectados de forma compleja.

# Genericidad restringida

La genericidad restringida es simplemente ponerle unas reglas/límites a la etiqueta `<T>` : 

- `<T extends Class>` : sólo admite clases que sean o deriven de ***Class***
- `<?>` : representa una clase desconocida
- `<? extends Class>` : representa una clase desconocida pero que es o deriva de ***Class***
- `<? super Class>` : representa una clase desconocida pero que sea padre de ***Class***

# Covarianza y contravarianza

Vamos a entender la covarianza y la contravarianza de una forma muy simple : 

## Covarianza

Imaginemos que tenemos una caja de manzanas con la etiqueta "caja de frutas" : 

&#9989; **Acceder (sacar)** : esto podemos hacerlo porque si metemos la mano en una caja con la etiqueta "fruta", podemos estar seguros que lo que saquemos va a ser una manzana, pues recordemos que la caja, aunque tenga la etiqueta de "caja de frutas", es una caja de manzanas.
&#10060; **Añadir** : esto NO podemos hacerlo porque si alguien lee la etiqueta "fruta", puede intentar introducir una naranja, pero hay que recordar, ¡que la caja es sólo de manzanas!

```java
List <Manzana> manzanas = new List <Manzanas>;
List <Fruta> frutas = manzanas;

frutas[0]; // Correcto (siempre sacaremos una manzana)
frutas.push(new Naranja()); // INCORRECTO (hay manzanas, y si alguien intenta meter algo que no son manzanas, la caja se corrompe)
```

## Contravarianza

Ahora imaginemos que tenemos una caja de comida y la usamos para meter manzanas :

&#10060; **Acceder (sacar)** : esto NO podemos hacerlo porque si metemos la mano esperando sacar una manzana, podemos sacar perfectamente un chuletón, porque recordemos, la caja era de comida...
&#9989; **Añadir** : esto podemos hacerlo porque en una caja de comida perfectamente podemos añadir una manzana.

```java
List <Comida> comidas = new List <Comida>;
List <Manzanas> manzanas = comidas;

manzanas[0]; // INCORRECTO (esperamos sacar una manzana pero podemos sacar cualquier cosa)
manzanas.push(new Manzana()); // Correcto (podemos meter una manzana en una caja de comida)
```