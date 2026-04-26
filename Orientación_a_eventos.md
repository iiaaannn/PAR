# Orientación a eventos

En este paradigma, el flujo del programa viene determinado por ***eventos*** (acción del usuario, activación de sensores...)

Básicamente son un conjunto de bloques de código que son invocados al producirse un evento

# Arquitectura de orientación a eventos 

En este paradigma, existe una entidad externa llamado ***controlador***. Esta entidad se encarga de detectar eventos, determinar los programas interesados en esos eventos y <u>enviar</u> mensajes a esos programas/invocar al código encargado de <u>manejar</u> esos eventos.

Algunas arquitecturas típicas son las GUIs, Entornos Web, Sistemas empotrados...