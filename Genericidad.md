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

Respecto al ejemplo de antes, os dejo por aquí cómo sería el método "existe" pero genérico, que vale tanto para un array de caracteres, enteros, Strings... Este ejemplo usa la técnica de tipado dinámico :

```py
def existe(v, x):
    for i in v:
        if i == x:
            return True
    return False
```

