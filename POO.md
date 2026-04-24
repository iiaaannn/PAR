Antes de todo me gustaría aclarar que Python sujeta la programación orientada a objetos con pinzas. ¿A qué me refiero con esto? pues, si bien Python nos puede servir para entender las bases de este paradigma, si indagamos un poco más a fondo, vemos que se desmonta por todos lados (por ejemplo en la encapsulación, lenguajes como Java no darían problema alguno, pero en Python podemos llegar a "hackear" la encapsulación)

- [Clases y objetos](#clases-y-objetos)
- [Métodos de POO](#métodos-de-poo)
  - [Encapsulación](#encapsulación)
  - [Herencia](#herencia)


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

* <font color="F54927">**private**</font> : sólo accesibles por la propia clase
* <font color="F54927">**protected**</font> : sólo accesibles por la propia clase y las clases que deriven de ella (herencia)
* <font color="F54927">**public**</font> : accesibles a todas las clases

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

* <font color="BDFFD7">**Agregación**</font> : los objetos de la clase A tienen objetos de la clase B

```py
class Motor:
    pass

class Coche:
    def __init__(self):
        self.motor = Motor()
```

como podemos ver, cada Coche tiene un motor, pero cada Coche no es un motor como tal.

* <font color="BDFFD7">**Herencia**</font> : en la herencia, la clase B hereda/se deriva de la clase A

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

Pero aparace un <font color="F54927">**problema**</font>, ¿qué ocurre si dos clases tienen un método con el mismo nombre? : 

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

C deriva primero de A y luego de B, por lo tanto si ejecutamos el mçetodo saludar, se ejecuta el método saludar de la clase A.