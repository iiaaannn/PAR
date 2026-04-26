Antes de todo me gustaría aclarar que Python sujeta la programación orientada a objetos con pinzas. ¿A qué me refiero con esto? pues, si bien Python nos puede servir para entender las bases de este paradigma, si indagamos un poco más a fondo, vemos que se desmonta por todos lados (por ejemplo en la encapsulación, lenguajes como Java no darían problema alguno, pero en Python podemos llegar a "hackear" la encapsulación)

- [Clases y objetos](#clases-y-objetos)
- [Métodos de POO](#métodos-de-poo)
  - [Encapsulación](#encapsulación)
  - [Herencia](#herencia)
    - [Herencia múltiple](#herencia-múltiple)
    - [Sistemas de tipado](#sistemas-de-tipado)
  - [Polimorfismo](#polimorfismo)
  - [Ligadura dinámica](#ligadura-dinámica)
- [Estructura dinámica](#estructura-dinámica)
- [Objetos - Métodos](#objetos---métodos)
  - [Ámbito](#ámbito)
  - [Tipos de métodos](#tipos-de-métodos)
- [Gestión de memoria](#gestión-de-memoria)


# Clases y objetos

* **clase** : una clase es el molde que define cómo será un objeto (pertenece a la ***parte estática***, pues se definen antes de la compilación)
* **objeto** : un objeto es una instancia real creada a partir de una clase (pertenece a la ***parte dinámica***, pues los objetos se crean y se destruyen en tiempo de ejecución)

```py
class coche:
    def __init__(self, marca, color):
        self.marca = marca
        self.color = color

    def arrancar(self):
        print("El coche está en marcha")

coche1 = coche("Toyota", "rojo")
coche2 = coche("Audi", "azul")
```

Como se puede observar en este ejemplo, la clase "coche" sirve como molde (es decir, todos los coches tendrán una marca y un color (esto se conoce como ***atributos*** $\rightarrow{}$ definen el estado del objeto) y todos los coches arrancan (esto son los ***métodos*** $\rightarrow{}$ código que se encarga de modelar el comportamiento de los objetos) y a su vez, existen unas instancias de esa misma clase (objetos) que son coche1 y coche2. coche1 es de la marca Toyota y es de color rojo mientras que el coche2 es de la marca Audi y será de color azul, y ambos coches podrán arrancar.

**Paradigma modular** $\rightarrow{}$ se basa en la *descomposición funcional* : dividir las aplicaciones en funciones/tareas que se van llamando en tiempo de ejecución.

**Paradigma orientado a objetos** $\rightarrow{}$ se basa en la *descomposición basada en objetos* : dividir las aplicaciones en objetos que se van llamando entre sí mediante el paso de mensajes.

# Métodos de POO

## Encapsulación

La encapsulación nos permite definir niveles de acceso y/o visibilidad para atributos y métodos:

* :red_circle: **private** : sólo accesibles por la propia clase
* :yellow_circle: **protected** : sólo accesibles por la propia clase y las clases que deriven de ella (herencia)
* :green_circle: **public** : accesibles a todas las clases

```py
class Cuenta:
    def __init__(self, saldo, pin, nombre):
        self.__saldo = saldo # atributo "private"
        self._pin = pin # atributo "protected"
        self.nombre = nombre # atributo "public"

    def depositar(self, cantidad):
        self.__saldo += cantidad

    def ver_saldo(self):
        return self.__saldo
    
cartera = Cuenta(100, 123456, "Pedro")
print(cartera.__saldo) # AtributeError
```

Este código nos suelta un AtributeError porque el atributo __saldo es privado, y aunque estamos accediendo a él correctamente, la encapsulación nos lo "oculta" para que no podamos verlo directamente.

La filosofía de la programación orientada a objetos no permite modificar directamente los atributos, sino que debe realizarse mediante métodos : 
```py
cartera.saldo += 100 (MAL)
cartera.depositar(100) (BIEN)
```
## Herencia

Existe Agregación y herencia : 

* :point_right: **Agregación** : los objetos de la clase A tienen objetos de la clase B

```py
class Motor:
    pass

class Coche:
    def __init__(self):
        self.motor = Motor()
```

como podemos ver, cada Coche tiene un motor, pero cada Coche no es un motor como tal.

* :point_right: **Herencia** : en la herencia, la clase B hereda/se deriva de la clase A

```py
class Animal:
    def comer(self):
        print("Está comiendo")

class Perro(Animal):
    def ladrar(self):
        print("Guau")
```

En este ejemplo, un Perro es una especie de Animal, que puede ladrar (método propio de un Perro), pero como es un Animal, también puede comer (método propio de un Animal)

```py
p = Perro()

p.comer() # aunque no esté definido este método dentro de la clase, podemos
          # utilizar el método comer() por pertenecer a la clase "Animal"
p.ladrar()
```

### Herencia múltiple

Como su propio nombre indica, podemos crear una clase que derive de varias clases a la vez : 

```py
class Volador:
    def volar(self):
        print("Puede volar")

class Nadador:
    def nadar(self):
        print("Puede nadar")

class Pato(Volador, Nadador):
    pass

p = Pato()

p.volar()
p.nadar()
```

Pero aparace un **problema**, ¿qué ocurre si dos clases tienen un método con el mismo nombre? : 

```py
class A:
    def saludar(self):
        print("Hola desde A")

class B:
    def saludar(self):
        print("Hola desde B")

class C(A, B):
    pass
```

Si llamamos al método saludar(), se ejecutará el de la clase A o el de la clase B?

Bien, pues Python usa algo llamado ***MRO*** (Method Resolution Order), y significa que primero busca en la clase de la que primero herede y luego la segunda. Como la clase C se definde de la forma : 

```py
class C(A, B)
```

C deriva primero de A y luego de B, por lo tanto si ejecutamos el método saludar, se ejecuta el método saludar de la clase A.

### Sistemas de tipado

Un sistema de tipado son el conjunto de reglas que definen qué es cada cosa y qué podemos hacer con ella.

* **jerarquía de subtipos** : la herencia permite crear clases a partir de otras ya existentes. Un ejemplo sería la clase Vehículo (**Clase Base (Padre)**) y otras derivadas serían el Coche o la Moto (**Clase Derivada (Hijo)**)

* **Polimorfismo** : toda clase que deriva de otra se considera un ***subtipo*** de su clase base ("es un" ...) La <u>conversión implícita</u> usa subtipos como valores de la clase base.

* **Clase universal** : en muchos lenguajes de programación, aunque no esté explícita, existe una "***clase madre***" de la que derivan todas las clases. Suele llamarse <u>Object</u>/<u>TObject</u>...

* **Interface** : es una clase totalmente abstracta. No tiene atributos y los métodos no ejecutan ninguna operación, sólo pone el nombre del método y lo que devuelve.

* **Herencia simple** : en la herencia simple, TODA clase debe tener un único padre directo. Esto permite tener una relación de tipo árbol mucho más limpia.

## Polimorfismo

El polimorfismo permite que distintos objetos respondan al mismo método pero cada uno a su manera :

```py
class Perro:
    def hablar(self):
        return "Guau"

class Gato:
    def hablar(self):
        return "Miau"

perro = Perro()
gato = Gato()

perro.hablar() # 'Guau'
gato.hablar() # 'Miau'
```

Como se puede observar, tanto el objeto perro como gato, ambos hacen referencia al mismo nombre del método, pero se comportan de manera diferente.

## Ligadura dinámica

La ligadura dinámica permite conocer qué método ejecutar en concreto según el objeto.

```py
class Perro:
    def hacer_sonido(self):
        print("Guau")

class Gato:
    def hacer_sonido(self):
        print("Miau")

class Vaca:
    def hacer_sonido(self):
        print("Muuu")


def reproducir_sonido(animal):
    animal.hacer_sonido()

animales = [Perro(), Gato(), Vaca()]

for i in animales:
    reproducir_sonido(i)
```

Aquí la función reproducir_sonido() no sabe que tipo de animal es, entonces, ¿cómo sabe qué sonido hace cada animal? Pues aquí es cuando entra en juego la ligadura dinámica. Para cada objeto en tiempo de ejecución, busca el método hacer_sonido() y lo ejecuta.

Existen más métodos de POO, pero no son muy importantes.

# Estructura dinámica

Un **objeto** es una instancia de una clase y se crean en <u>tiempo de ejecución</u>. Para crear un objeto existen diferentes mecanismos : 

* con operadores (Java, C++...)

```java
Coche coche1 = new Coche()
```

* como si fueran funciones (Python)

```py
coche1 = Coche()
```

Estas variables (coche1) NO son el coche como tal, sino ***referencias/punteros*** a esos objetos (direcciones de memoria). 

**Polimorfismo** $\rightarrow{}$ si una variable hace referencia a una clase de tipo A, puede hacer referencia también a cualquier clase que derive de A, pero NO puede hacer referencia a cualquier otra (por ejemplo una clase/subclase de tipo B)

# Objetos - Métodos

Una de las operaciones más básicas entre objetos es el <u>envío de mensajes</u> para pedir la invocación de un <u>método</u>. Existen diferentes formas de reflejar este hecho : 

* "Paso de mensajes" (Smalltalk, Objective-C)

```Smalltalk
Objeto mensaje: parámetros
```

* Llamada de subrutinas con el operador de acceso a campos de registros "." (la gran mayoría)

```py
Objeto.metodo(parámetros)
```

## Ámbito 

El ámbito de una función determina qué valores puede tocar y cuáles no..

El ***ámbito local*** de un método está formado por : 

* sus parámetros

* variables locales

* variable/parámetro predefinido que representa al objeto sobre el que se ha invocado el método (atributos) (`self` en Python, `this` en C++, Java...)

* todos los atributos y métodos de la clase y las clases derivadas (salvo los atributos privados) El acceso es directo o via la varible que representa el objeto

* suele existir un mecanismo para acceder a la versión `base` de aquellos métodos que han sido redefinidos por los hijos (objeto/función `super` en Java/Python, objeto `base` en C#...)

## Tipos de métodos

* :point_right: **finales** : métodos que no se pueden redefinir en clases derivadas (`final` en Java, por defecto en C++/C#)

* :point_right: **virtuales** : métodos que se pueden redefinir en clases (`virtual` en C++/C#, por defecto en Java, Python)

* :point_right: **abstractos** : métodos para los que la clase no proporciona implementación (lo implementan las interfaces) (`abstract` en Java/C#, 0 en C++)
 
* :point_right: **estáticos** : métodos que puedes usar sin necesidad de crear una instancia de la clase (objeto) y llamar al método desde ese objeto (`static` en Java/C#/C++, `@static_method` en Python) No pueden acceder a atributos ni invocar métodos NO estáticos. Para invocarlos usamos la sintaxis `clase.metodo(params)`. Java, C++, C#, Python permiten atributos estáticos (que pueden accederse mediante métodos estáticos)

* :point_right: **constructores/destructores** : se encargan de inicializar y liberar recursos del objeto respectivamente

# Gestión de memoria

Existen dos formas de gestionar la memoria : 

* :point_right: **gestión manual** : el programador debe encargarse de destruir los objetos al finalizar su uso. Puede haber problemas de "agujeros de memoria" si no los destruimos, problemas de acceso a objetos ya destruidos...

* :point_right: **gestión automática (garbaje collection)** : el entorno tiene un proceso encargado de detectar cuando un objeto no es accesible y lo destruye. Lenguajes como Java, C++, Python... implementan este sistema.