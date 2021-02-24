
En este capítulo, conectará el servidor Apache a un motor PHP para 
crear sitios que acepten datos específicos del cliente de formularios 
HTML, que se procesan, y luego los resultados se envían de vuelta 
al navegador del cliente, proporcionando así contenido dinámico. Se 
le presentará PHP y luego comparará la programación del lado del cliente 
en JavaScript con la programación del lado del servidor en PHP. Por 
último, creará un sitio basado en PHP que valida los códigos de función 
que se pueden ingresar en línea en un sitio web a cambio de premios.


## El motor PHP

Un servidor web habilitado para PHP puede recibir una solicitud de un 
archivo PHP principalmente de dos formas.

- Utilizando la URL del archivo PHP en la barra de direcciones del 
navegador web

- Al enviar un formulario HTML, donde el atributo de acción se establece 
en la URL del archivo PHP

Al recibir la solicitud de un archivo PHP, el servidor web localiza el 
archivo y lo pasa al motor PHP para su procesamiento. El motor PHP es 
un intérprete de PHP que está asignado actualmente al servidor web. Lo 
que el usuario realmente ve en el navegador no es el archivo PHP en sí, 
sino la salida de todos los comandos PHP, enviados por el servidor web. 
Por ejemplo, el comando echo PHP es el que se usa para enviar mensajes. 
Aquí tienes un ejemplo:

```
echo "Hello world!";
```

Las etiquetas PHP de inicio/fin (`<?` y `?>`) Se utilizan como delimitadores 
de un programa PHP.


```
<?php
echo "Hello world!";
?>
```

&____________________________________________________________________
