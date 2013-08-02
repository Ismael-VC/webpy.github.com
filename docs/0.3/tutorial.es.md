---
layout: default
title: Tutorial
---

# Tutorial

Otros lenguajes : [chinese 简体中文 ](/docs/0.3/tutorial.zh-cn) | [français](/docs/0.3/tutorial.fr) | [Bahasa Indonesia](/docs/0.3/tutorial.id) | [Español](/docs/0.3/tutorial.es) | ...

## Sumario

* [Comenzando](#starting)
* [Manejo de URLs](#urlhandling)
* [GET y POST: la diferencia](#getpost)
* [Iniciar el servidor](#start)
* [Plantillas](#templating)
* [Forms](#forms)
* [Databasing](#databasing)
* [Developing](#developing)
* [What next?](#whatnext)

<a name="starting"> </a>
## Comenzando

Así que ya sabe Python y quiere hacer una página web. web.py le provee el código necesario para hacerlo fácilmente.

Si usted quiere hacer todo el tutorial, deberá tener ya instalado Python, web.py, flup, psycopg2 y Postgres (o una base de datos equivalente y su controlador de Python). (Mire la <a href="/install">instalación</a> para más detalles.)

Comencemos.

<a name="urlhandling"> </a>
## Manejo de URLs

La parte más importante  de cualquier página web es la estructura de su URL. Sus URLs no son solo las cosas que los visitantes ven y mandan por email a sus amigos, también proveen un modelo mental de cómo funciona su página. En lugares populares como [del.icio.us](http://del.icio.us/), las URLs son incluso parte de la interfaz de usuario. Web.py hace fácil el crear grandiosas URLs.

Para empezar con su nueva aplicación web.py, abra un nuevo archivo de texto (llamémoslo `code.py`) y escriba:

    import web

Esto importara el modulo web.py.

Ahora necesitamos especificar a web.py la estructura de nuestra URL. Empecemos con algo simple:

    urls = (
      '/', 'index'
    )

La primera parte es una [expresión regular](http://osteele.com/tools/rework/) que coincide con la URL, como `\`, `/help/faq`, `/item/(\d+)`, etc. (ej.: `\d+` coincide con una secuencia de digitos). El paréntesis indica que el patrón que coincide sea guardado para ser usado mas tarde. La segunda parte es el nombre de la clase a la cual se le envía la petición, como `ìndex`, `view`, `welcomes.hello` (lo cual consigue la clase `hello` del modulo `welcomes`), o `get_\1`. `\1` es reemplazado por la primera coincidencia de su expresión regular; las coincidencias sobrantes son pasadas a su función.

Esta linea dice que queremos la URL `\` (ej.: la página principal) para ser manejada por la clase llamada `index`.

<a name="getpost"> </a>
## GET y POST: la diferencia


Ahora necesitamos escribir la clase `ìndex`. Mientras que la mayoría de la gente no lo nota con tan solo navegar por ahí, su navegador usa un lenguaje conocido como HTTP para comunicarse con la World Wide Web. Los detalles no son importantes, pero la idea básica es que los visitantes le soliciten a servidores web realizar ciertas funciones (como `GET`o `POST`) en URLs (como `/` o `/foo?f=1`).

`GET` es el método con el que la mayoría esta familiarizado, el que se usa para solicitar el texto de una pagina web. Cuando usted escribe `harvard.edu` dentro de su navegador web, este literalmente pregunta al servidor web de Harvard obtener `/` (`GET /`). El segundo método mas famoso, `POST`, es usado frecuentemente cuando se envían ciertas clases de formularios, como una solicitud para comprar algo. Uno usa `POST` siempre que el acto de enviar una petición _haga algo_ (como cargar a tu tarjeta de credito y procesar una orden). Esto es esencial, porque las URLs de `GET` pueden ser enviadas e indexadas por motores de búsqueda, lo cual seguramente quieres para la mayoría de tus paginas pero definitivamente _no_ para cosas como procesar ordenes (imagina si Google intentara comprar todo en tu sitio!).

En nuestro código de web.py, haremos la distinción entre ambas explicita:

    class index:
        def GET(self):
            return "Hello, world!"

Ahora esta función `GET` sera llamada por web.py cada vez que alguien haga una petición `GET`para `/`.

Después necesitamos crear una aplicación especificando las urls y una manera de decirle a web.py que empiece a servir paginas web:

    if __name__ == "__main__":
        app = web.application(urls, globals())
        app.run()

Primero le decimos a web.py que cree una aplicación con las URLs listadas arriba, buscando las clases en el espacio de nombres global de este archivo.
Y finalmente nos aseguramos que web.py sirva la aplicación que creamos arriba.

Dese cuenta, aunque he estado hablando mucho, realmente solo tenemos cinco o seis lineas de código. Eso es todo lo que necesitas para hacer una aplicación web.py completa.

Para facilitar el acceso, aquí esta como debe verse su código:

    import web

    urls = (
        '/', 'index'
    )

    class index:
        def GET(self):
            return "Hello, world!"

    if __name__ == "__main__":
        app = web.application(urls, globals())
        app.run()

<a name="start"> </a>
## Iniciar el servidor

Si accede a la linea de comandos e ingresa:

    $ python code.py
    http://0.0.0.0:8080/

Ahora usted tiene un a aplicación web.py corriendo un servidor web real en su computadora. Visite la URL y deberá ver "Hello, world!" (Puede añadir una IP dirección/puerto después de "code.py" para controlar en donde lanzara web.py el servidor. También puede indicarle si correr un servidor `fastcgi` o `scgi`.)

**Nota:** Usted puede especificar el numero de puerto en la linea de comandos si usted no puede o quiere usar el predeterminado, así:

    $ python code.py 1234


<a name="templating"> </a>
## Plantillas

Writing HTML from inside Python can get cumbersome; it's much more fun to write Python from inside HTML. Luckily, web.py makes that pretty easy.

Let's make a new directory for our templates (we'll call it `templates`). Inside, make a new file whose name ends with HTML (we'll call it `index.html`). Now, inside, you can just write normal HTML:

    <em>Hello</em>, world!

Or you can use web.py's templating language to add code to your HTML:

    $def with (name)

    $if name:
        I just wanted to say <em>hello</em> to $name.
    $else:
        <em>Hello</em>, world!

As you can see, the templates look a lot like Python files except for the `def with` statement at the top (saying what the template gets called with) and the `$`s placed in front of any code.  Currently, template.py requires the `$def` statement to be the first line of the file.  Also, note that web.py automatically escapes any variables used here, so that if for some reason `name` is set to a value containing some HTML, it will get properly escaped and appear as plain text. If you want to turn this off, write `$:name` instead of `$name`.

Now go back to `code.py`. Under the first line, add:

    render = web.template.render('templates/')

This tells web.py to look for templates in your templates directory. Then change `index.GET` to:

    name = 'Bob'
    return render.index(name)

('index' is the name of the template and 'name' is the argument passed to it)

Visit your site and it should say hello to Bob.

But let's say we want to let people enter their own name in. Replace the two lines we added above with:

    i = web.input(name=None)
    return render.index(i.name)

Visit `/` and it should say hello to the world. Visit `/?name=Joe` and it should say hello to Joe.

Of course, having that `?` in the URL is kind of ugly. Instead, change your URL line at the top to:

    '/(.*)', 'index'

and change the definition of `index.GET` to:

    def GET(self, name):
        return render.index(name)

and delete the line setting name. Now visit `/Joe` and it should say hello to Joe.

If you wish to learn more about web.py templates, visit the <a href="/docs/0.3/templetor">templetor page</a>.

<a name="forms"> </a>
## Forms

The form module of web.py allows the ability to generate html forms, get user input, and validate it before processing it or adding it to a database.
If you want to learn more about using the module forms web.py, see the [Documentation](/docs/0.3) or direct link to [Form Library](/form)

<a name="databasing"> </a>
## Databasing

**Note:** Before you can start using a database, make sure you have the appropriate database library installed.  For MySQL databases, use [MySQLdb](http://sourceforge.net/project/showfiles.php?group_id=22307) and for Postgres use [psycopg2](http://initd.org/pub/software/psycopg/).

First you need to create a database object.

    db = web.database(dbn='postgres', user='username', pw='password', db='dbname')

(Adjust these -- especially `username`, `password`, and `dbname` -- for your setup. MySQL users will also want to change `dbn` definition to `mysql`.)

That's all you need to do -- web.py will automatically handle connecting and disconnecting from the database.

Using your database engines admin interface, create a simple table in your database:

    CREATE TABLE todo (
      id serial primary key,
      title text,
      created timestamp default now(),
      done boolean default 'f'    );

And an initial row:

    INSERT INTO todo (title) VALUES ('Learn web.py');

Return to editing `code.py` and change `index.GET` to the following, replacing the entire function:

    def GET(self):
        todos = db.select('todo')
        return render.index(todos)

and change back the URL handler to take just `/` as in:

    '/', 'index',

Edit and replace the entire contents of `index.html` so that it reads:

    $def with (todos)
    <ul>
    $for todo in todos:
        <li id="t$todo.id">$todo.title</li>
    </ul>

Visit your site again and you should see your one todo item: "Learn web.py". Congratulations! You've made a full application that reads from the database. Now let's let it write to the database as well.

At the end of `index.html`, add:

    <form method="post" action="add">
    <p><input type="text" name="title" /> <input type="submit" value="Add" /></p>
    </form>

And change your URLs list to read:

    '/', 'index',
    '/add', 'add'

(You've got to be very careful about those commas.  If you omit them, Python adds the strings together and sees `'/index/addadd'` instead of your list of URLs!)

Now add another class:

    class add:
        def POST(self):
            i = web.input()
            n = db.insert('todo', title=i.title)
            raise web.seeother('/')

(Notice how we're using `POST` for this?)

`web.input` gives you access to any variables the user submitted through a form.

Note: In order to access data from multiple identically-named items, in a list format (e.g.: a series of check-boxes all with the attribute name="name") use:

    post_data=web.input(name=[])

`db.insert` inserts values into the database table `todo` and gives you back the ID of the new row. `seeother` redirects users to that URL.

Some quick additional notes: `db.update` works just like `db.insert` except instead of returning the ID it takes it (or a string `WHERE` clause) after the table name.

`web.input`, `db.query`, and other functions in web.py return "Storage objects", which are just like dictionaries except you can do `d.foo` in addition to `d['foo']`. This really cleans up some code.

You can find the full details on these and all the web.py functions in [the documentation](/docs/0.3).

<a name="developing"> </a>
## Developing

web.py also has a few tools to help us with debugging. When running with the built-in webserver, it starts the application in debug mode. In debug mode any changes to code and templates are automatically reloaded and error messages will have more helpful information.

The debug is not enabled when the application is run in a real webserver. If you want to disable the debug mode, you can do so by adding the following line before creating your application/templates.

    web.config.debug = False

<a name="whatnext"> </a>
## What Next ?

This ends the tutorial for now. Take a look at the [cookbook](/cookbook/) and the [code examples](/src/)  for lots more cool stuff you can do with web.py.
Also don't forget about the [api reference](/docs/0.3/api)
