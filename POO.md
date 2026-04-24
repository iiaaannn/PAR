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

## Herencia