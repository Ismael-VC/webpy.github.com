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

TODO: ¿Mover el próximo párrafo para agregar la instalación?

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
            return "¡Hola, Web!"
            
La clase `hola` tiene una función llamada `GET` (OBTENER) que retorna “¡Hola, Web!”. ¿Por qué `GET`? 

Cuando abres una página web, tu navegador solicita el contenido de dicha página. Esta petición es llamada el método `GET`. Web.py usa la misma terminología. La string que retorna tu método `GET` es mostrada en tu navegador.

Aunque el código de tu primera página está escrito, aun no puede ser abierto en un navegador. Procedamos a mapear una dirección web (URL) con tu clase. Inserta el siguiente código después de la declaración de importación:

    urls = (
      '/', 'hola')

Esto le indica a web.py que mapee la raíz de tu página web (como http://webpy.org/) a tu clase de Python llamada `hola`.

Después crea un instancia de una aplicación web.py. Esta instancia será el mediador entre tu clase y la web. Manejara las peticiones de tu navegador y servirá tus páginas.  (En resumen: hará todas las cosas de las que tu realmente no quieres preocuparte.) Usa el siguiente código:

    app = web.application(urls, globals())

Nota que `web.application()` es llamada con dos argumentos. Tu mapeado de URL (`urls`) y tu namespace global el cual contiene tu clase `hola` (`globals()`).

Para finalizar tu aplicación web.py inserta el siguiente código al final del código:

    if __name__ == "__main__":
        app.run()

`app.run()` inicia la aplicación web para servir las paginas solicitadas.


