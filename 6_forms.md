# Crear Formularios

Desafortunadamente, la entrada del usuario puede exponer su sitio a 
ataques maliciosos. Es importante verificar los datos enviados desde 
un formulario antes de aceptarlo. Aunque los elementos de formulario 
HTML5 validan la entrada del usuario en los navegadores modernos, 
aún debe verificar los datos en el servidor. La validación de HTML5 
ayuda a los usuarios legítimos a evitar enviar un formulario con 
errores, pero los usuarios malintencionados pueden eludir fácilmente 
las comprobaciones realizadas en el navegador. La validación del 
lado del servidor no es opcional, pero es esencial. Las soluciones 
PHP de este capítulo le muestran cómo filtrar o bloquear cualquier 
cosa sospechosa o peligrosa. Ninguna aplicación en línea es 
completamente a prueba de piratería, pero no se necesita mucho 
esfuerzo para mantener a raya a todos, excepto a los merodeadores 
más decididos. También es una buena idea conservar la entrada del 
usuario y volver a mostrarla si el formulario está incompleto o se 
descubren errores. 

Estas soluciones crean un script de procesamiento de correo completo 
que se puede reutilizar en diferentes formularios, por lo que es 
importante leerlas en secuencia.

El contenido de este capitulo incluye siguiente:

- Comprender cómo se transmite la entrada del usuario desde un 
formulario en línea
- Visualización de errores sin perder la entrada del usuario
- Validando la entrada del usuario
- Envío de información de usuario por correo electrónico

## Cómo PHP recopila información de un formulario

Aunque HTML contiene todas las etiquetas necesarias para construir un 
formulario, no proporciona ningún medio para procesar el formulario 
una ves este se envía. Para eso, se necesita una solución del lado del 
servidor, como PHP.

El sitio web de Japan Journey contiene un formulario de comentarios 
simple (ver Figura 6-1). Otros elementos, como botones de opción, 
casillas de verificación y menús desplegables, se agregarán más adelante.

Primero, echemos un vistazo al código HTML del formulario (está en 
`contact_01.php` en la carpeta ch06):


```html

<form method="post" action="">
    <p>
        <label for="name">Name:</label>
        <input name="name" id="name" type="text">
    </p>
    <p>
        <label for="email">Email:</label>
        <input name="email" id="email" type="text">
    </p>
    <p>
        <label for="comments">Comments:</label>
        <textarea name="comments" id="comments"></textarea>
    </p>
    <p>
        <input name="send" type="submit" value="Send message">
    </p>
</form>
```

Las dos primeras etiquetas _<input>_ y la etiqueta _<textarea>_ contienen 
atributos de nombre e identificación configurados con el mismo valor. 
La razón de esta duplicación es la accesibilidad. HTML usa el atributo 
_id_ para asociar el elemento _<label>_ con el elemento _<input>_ correcto. 
Los scripts de procesamiento de formularios, sin embargo, se basan en 
el atributo de _name_. Por lo tanto, aunque el atributo _id_ es opcional 
en el botón _submit_, debe usar el atributo _name_ para cada elemento 
del formulario que desee procesar.

El atributo _name_ de un elemento de entrada de formulario normalmente no 
debe contener espacios. Si desea combinar varias palabras, únalas con un 
guión bajo (PHP lo hará automáticamente si deja espacios). Debido a que el 
script desarrollado más adelante en este capítulo convierte los atributos 
_name_ en variables PHP, no use guiones ni ningún otro carácter que no sea 
válido en los nombres de las variables PHP.

Otras dos cosas a tener en cuenta son los atributos de _method_ y _action_
dentro de la etiqueta de apertura _<form>_. El atributo de _method_ determina 
cómo el formulario envía datos. Se puede configurar para _post_ y _get_. El 
atributo _action_ le dice al navegador dónde enviar los datos para su 
procesamiento cuando se hace clic en el botón _submit_. Si el valor se deja 
vacío, como aquí, la página intenta procesar el formulario en sí. Sin embargo, 
un atributo _action_ vacío no es válido en HTML5, por lo que será necesario corregirlo.

He evitado deliberadamente el uso de cualquiera de las nuevas funciones 
de formulario HTML5, como _type = "email"_ y el atributo _required_. Esto 
facilita la prueba de los scripts de validación del lado del servidor PHP. 
Después de la prueba, puede actualizar sus formularios para utilizar las 
funciones de validación de HTML5. La validación en el navegador es 
principalmente una cortesía hacia el usuario para evitar que se envíe 
información incompleta, por lo que es opcional. La validación del lado 
del servidor nunca debe omitirse.

## Diferencia entre Post y Get


De lo contrario, la carpeta ch06 contiene un conjunto completo de archivos 
para el sitio Japan Journey con todo el código del Capítulo 5 incorporado. 
Copie _contact_01.php_ a la raíz del sitio y cámbielo al nombre _contact.php_. 
También copie _footer.php_, _menu.php_ y _title.php_ de la carpeta _ch06 / includes_
a la carpeta _includes_ en la raíz del sitio.

1. Localice la etiqueta de apertura _<form>_ en _contact.php_ y cambie el valor 
del atributo del método de _post_ para obtener, así:

```html
<form method="get" action="">
```

2. Guarde _contact.php_ y cargue la página en un navegador. Escriba su nombre, 
dirección de correo electrónico y un mensaje corto en el formulario, luego haga 
clic en _Enviar mensaje_.


3. Busque en la barra de direcciones del navegador. Debería ver el contenido 
del formulario adjunto al final de la URL.  Si divide la URL, se verá así:

```
http://localhost/phpsols-4e/contact.php
?name=David
&email=david%40example.com
&comments=Greetings%21+%3A-%29
&send=Send+message
```

Los datos enviados por el formulario se han agregado a la URL básica 
como una cadena de consulta que comienza con un signo de interrogación. 
El valor de cada campo y el botón de envío se identifica mediante el 
atributo _name_ del elemento de formulario, seguido de un signo igual y 
los datos enviados. Los datos de cada elemento de entrada están separados 
por un signo comercial (&). Las URL no pueden contener espacios o ciertos 
caracteres (como un signo de exclamación o un emoticón), por lo que el 
navegador reemplaza los espacios con + y codifica otros caracteres como 
valores hexadecimales, un proceso conocido como codificación de URL (para 
obtener una lista completa de valores, consulte 
www.degraeve.com/reference/urlencoding.php).
 
4. Vuelva al código de _contact.php_ y cambie el método a _post_, así:

```html
<form method="post" action="">
```

Guarde _contact.php_ y vuelva a cargar la página en su navegador. Escribe 
otro mensaje y haz clic en _Enviar mensaje_. Su mensaje debería desaparecer, 
pero no sucede nada. No se ha perdido, pero todavía no ha hecho nada para 
procesarlo.
 
6. En _contact.php_, agregue el siguiente código inmediatamente debajo de 
la etiqueta de cierre _</form>_:

```html
<pre>
<?php if ($_POST) { print_r($_POST); } ?>
</pre>
```

Esto muestra el contenido del array superglobal `_$ _POST_` si se han 
enviado datos _post_. Como se explicó en el Capítulo 4, la función 
_print_r ()_ le permite inspeccionar el contenido de los array; las 
etiquetas _<pre>_ simplemente facilitan la lectura de la salida.



&_____________________________________________________________________
