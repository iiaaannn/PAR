# Orientación a eventos

En este paradigma, el flujo del programa viene determinado por ***eventos*** (acción del usuario, activación de sensores...)

Básicamente son un conjunto de bloques de código que son invocados al producirse un evento

# Arquitectura de orientación a eventos 

En este paradigma, existe una entidad externa llamado ***controlador***. Esta entidad se encarga de detectar eventos, determinar los programas interesados en esos eventos y <u>enviar</u> mensajes a esos programas/invocar al código encargado de <u>manejar</u> esos eventos.

Algunas arquitecturas típicas son las GUIs, Entornos Web, Sistemas empotrados...

# Paso de mensajes

Cada evento es traducido en un **mensaje** por el controlador y contiene la información más relevante del evento. Cada aplicación tiene asociada una ***cola de mensajes***, por lo tanto, cada vez que surge un evento, este se añade a la cola de eventos.

Por lo tanto podríamos decir que una aplicación consiste en un ***bucle de mensajes*** donde se accede a la cola de mensajes y se ejecuta el código necesario para tratar ese mensaje. Pueden añadirse mensajes a la cola mientras se trata otro mensaje, y se finaliza el bucle (aplicación) cuando se recibe el ***mensaje de finalización***

:point_right: ***Multitarea cooperativa*** : si al acceder a una cola, ésta está vacía, el controlador transfiere el control a otra cola que si tiene tareas pendientes (una aplicación puede enviarle mensajes al controlador para obtener información/comunicarse con otras aplicaciones)

# Manejadores de eventos

La aplicación **registra** pares (evento, manejador). Cuando ocurre cierto evento, le dice al controlador : "Si pasa este evento, ejecuta este manejador"

:point_right: ***manejador*** : subrutina/método que recibe como parámetros datos importantes sobre el evento y contiene el código que manejará ese evento.

El bucle de mensajes sigue existiendo, pero no le vemos, está oculto. Este bucle de mensajes se usa para ***frameworks*** (AWT y Swing para Java, wxPython...)

# Técnicas para Programación Orientada a Eventos (P.O.E)

## Callbacks 

Un callback es guardar la referencia de una función en una variable para poder invocar estas funciones mediante variables.

:point_right: ***tipo procedural*** : un tipo procedural es un tipo de datos que contiene referencias a funciones con una determinada cabecera (parámetros y resultado). Esto es lo que hace posible que lenguajes que no son puramente orientados a objetos puedan usar callbacks.

## Orientación a Objetos

Un objeto puede encapsular datos (<u>atributos</u>) y código (<u>métodos</u>). Esto es bastante útil porque en vez de enviar una función con sus respectivos parámetros, envías directamente el objeto (que ya sabe lo que tiene que hacer con los datos que tiene) y señalas uno de sus métodos como el manejador, usando herencia y polimorfismo.