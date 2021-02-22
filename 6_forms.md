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
    
&____________________________________________________________________
