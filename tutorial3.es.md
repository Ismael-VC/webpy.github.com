---
layout: default
title: web.py 0.3 tutorial
---

# web.py 0.3 tutorial

Este es un trabajo en progreso.

## TODO: app.internalerror = web.debugerror

## TODO: '$user' vs. '$:user' y '$var user:$user' vs. '$var user:$user\'

## TODO: Múltiples botones de envío.

## TODO: Mostrar/Esconder el código completo al final de las secciones. 

TODO: ¿Mover el próximo párrafo para instalar?

Para crear un sitio web con web.py deberá saber el lenguaje de programación Python y tenerlo instalado. Las instrucciones de instalación pueden ser encontradas en (http://python.org/](http://python.org/). Si no sabes si Python está instalado en tu sistema, abre una terminal y teclea  `python`. Un buen recurso para aprender Python  es el [tutorial] oficial (http://docs.python.org/tut/tut.html). Si eres nuevo en la programación en general, [Think Python] (http://www.greenteapress.com/thinkpython/) es un maravilloso libro para comprender los conceptos clave de la programación.

## TOC

TODO: ...

## Prerrequisitos 

Este tutorial asume que tanto Python como web.py están instalados en tu sistema. Si este no es el caso, por favor siga las [instrucciones de instalación] (http://webpy.org/install) antes de continuar.

Además, conocimiento básico de HTML es necesario para entender algunos ejemplos.

## Hola Web en web.py

Abre tu editor de texto favorito y crea un nuevo archivo `hola.py`. En este archivo definirás el contenido y la lógica de tu aplicación web así como sus direcciones web (URLs).

Antes de que seas capaz de usar las herramientas que web.py provee, necesitas importar el modulo web.py con el código siguiente:

    import web
    
En web.py las páginas web son mapeadas a clases de Python. Creemos el código para la primera página que llamaremos `hola`:

    class hola:
        def GET(self):
            return "Hola, Web!"
