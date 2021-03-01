<!-- https://link-springer-com.ezproxy.unal.edu.co/chapter/10.1007/978-1-4842-4463-0_1 -->

# Servidor web Apache

La mayoría de la gente está familiarizada con el uso de un navegador 
web. Incluso cuando utilizan un navegador nuevo por primera vez, son 
capaces de visitar una página web y navegar a otros sitios 
inmediatamente. Esto se debe a que los navegadores suelen incluir las 
mismas funciones prácticas. Por ejemplo, todos los navegadores incluyen 
una barra de direcciones en la parte superior de la ventana, una opción 
de historial, una carpeta de favoritos y algunos botones de navegación.

Los servidores web, por otro lado, rara vez se parecen entre sí. Sin 
embargo, los servidores comparten algunos puntos en común. En este 
libro, utilizará dos servidores web diferentes y conocerá las 
características comunes de ambos, así como las diferencias. En este 
capítulo, utilizará el servidor web Apache y en el Capítulo 5 utilizará 
otro servidor web popular, Lighttpd.

## Empezando con Apache

El servidor web Apache es uno de los servidores web más populares en 
la actualidad. Esto se debe a la facilidad de uso de las funciones 
administrativas y a su flexibilidad basada en su diseño modular. 
Apache obtuvo su nombre en 1995 cuando comenzó el Proyecto Apache. 
El Proyecto Apache fue una colaboración de muchos programadores y, 
por lo tanto, el código fuente requirió muchos parches (correcciones 
de software). Mediante módulos de software, Apache ofrece funciones 
adicionales que los administradores pueden instalar. Debido a estos 
componentes agregados, Apache no es por defecto una pieza de software 
enorme y monolítica.

Apache fue uno de los primeros servidores en admitir hosts virtuales 
(`vhosts`). Esta noción, también conocida como servidores virtuales 
en otros servidores web como Cherokee, denota la capacidad de un solo 
servidor web para ejecutar varios sitios simultáneamente que se 
diferencian por la dirección IP, por el número de puerto o por el 
nombre de dominio utilizado en el solicitud de cliente.

Instalación y prueba de Apache

El programa Apache se conoce actualmente como apache2, y puede 
descargarlo e instalarlo desde la terminal de Linux usando el 
siguiente comando:

```
$ sudo apt-get install apache2
```

El proceso apache2 comienza a ejecutarse. Utilice el comando `ps` (estado 
del proceso) del terminal de Linux para ver los procesos de apache2.

```
$ ps xa | grep apache2
```

Más de un proceso apache2 se crea instantáneamente para estar listo 
para atender más solicitudes de clientes. La Figura 1-1 muestra los 
ID de proceso (PID) para los procesos de Apache devueltos en este 
momento por el comando `ps`.

Abrir imagen en una ventana nueva Figura 1-1

Figura 1-1 La salida del comando ps enumera los PID de apache2

El servidor web está en funcionamiento y listo para enviar la primera 
página. En la barra de direcciones de su navegador, ingrese la dirección 
IP de bucle invertido, que es la dirección IP que posee su PC, incluso 
si no hay una tarjeta de red instalada, o su nombre de host 
correspondiente. Aquí hay unos ejemplos:

```
127.0.0.1
http://127.0.0.1
localhost
```

Apache responde proporcionando el índice del directorio, ya que no se 
especificó una página web real en la URL.

¡Pista!

Un conjunto de páginas predeterminado para un directorio específico se 
denomina índice de directorio para este directorio. Además, el índice 
de directorio de la raíz del documento (es decir, el directorio base de 
un sitio web) es el índice de directorio del sitio.

El contenido del índice de directorio incluye una breve introducción a 
las opciones de configuración de Apache. La Figura 1-2 muestra la página 
predeterminada de Apache para Ubuntu.

Abrir imagen en una ventana nueva Figura 1-2

Figura 1-2 El índice del directorio de Apache para Ubuntu

Como dice el título de la primera sección, "¡Funciona!" La mala noticia 
es que hasta ahora funciona solo para usuarios que usan su propia PC y 
están interesados solo en la configuración de Apache. Hay buenas 
noticias. Siguiendo los pasos del libro, puede personalizar su servidor 
web con sus preferencias y puede hacer que su sitio esté disponible 
para todo Internet.

### Adding New Directories and Web Pages

Al leer el contenido de la página web, puede averiguar la raíz del 
documento y el nombre de archivo del índice del directorio. El 
siguiente texto:

"Debería reemplazar este archivo (ubicado en` /var/www/html/index.html`)"

indica que esta ruta es la raíz del documento:

```
/var/www/html/
```

e indica que este archivo es el índice del directorio:

`index.html`

Desde la terminal de Linux, cambie el directorio de trabajo actual a 
`/var/www/html`, como se muestra aquí:

```
$ cd /var/www/html
```

Cree una copia de `index.html` para usar su propio índice de directorio.

```
$ sudo cp index.html index_OLD.html
```

Utilice un editor de texto de Linux como gedit para editar el 
`index.html` original, como se muestra aquí:

```
$ sudo gedit index.html
```

En la ventana de gedit, ingrese su propio código fuente HTML, por 
ejemplo:

```
<!DOCTYPE html>
<html>
<head>
<title>Testing Apache</title>
</head>
<style>
p {
   font-size:24px;
   text-align:center;
   color:blue;
}
body{
background-color:yellow;
}
</style>
</head>
<body>
<p>Hello World!</p>
</body>
</html>
```

Haga clic en el botón Guardar en la ventana de gedit. Luego ingrese la 
dirección IP de loopback en la barra de direcciones del navegador. Como 
se muestra en la Figura 1-3, el servidor Apache muestra su propia 
página web.

Abrir imagen en una ventana nueva Figura 1-3

Figura 1-3 Un índice de directorio personalizado que imprime 
"¡Hola mundo!"

Para solicitar otra página web, por ejemplo `index_OLD.html`, puede 
proporcionar la URL del archivo de la página web al incluir la ruta de 
la página web a partir de la raíz del documento. Por ejemplo, ingrese 
uno de los siguientes en la barra de direcciones de su navegador:

```
127.0.0.1/index_OLD.html
localhost/index_OLD.html
```

Debido a que `index_OLD.html` está en la raíz del documento, la ruta 
incluye solo el nombre de archivo de la página web. Al usar esta URL, 
la página web predeterminada de Apache para Ubuntu se muestra nuevamente, 
como se muestra en la Figura 1-4.

Abrir imagen en una ventana nueva Figura 1-4

Figura 1-4 Descarga de una página web específica incluyendo su ruta en 
la URL

Comience a incluir subdirectorios en la raíz del documento para 
organizar mejor su sitio web. En la terminal de Linux, cambie el 
directorio de trabajo a la raíz del documento.

```
$ cd /var/www/html/
```

Cree un nuevo directorio de prueba. En el nuevo directorio, cree 
`test.html`, otro archivo HTML.

```
$ sudo mkdir test
$ sudo gedit test/test.html
```

En la ventana de gedit, ingrese algún código fuente HTML para 
`test.html` y haga clic en el botón Guardar. Luego ingrese la ruta del 
nombre del archivo de la página web, excluyendo la raíz del documento, 
en la barra de direcciones de su navegador. La ruta del nombre de 
archivo para `test.html` es la siguiente:


```
/var/www/html/test/test.html
```

Puede omitir la parte raíz del documento ya que en el servidor web 
cualquier referencia de archivo comienza desde la raíz del documento; 
por lo tanto, la URL de `test.html` es la siguiente:

```
localhost/test/test.html
or 127.0.0.1/test/test.html
```

La página web se carga en su navegador, como se muestra en la Figura 1-5.

Abrir imagen en una nueva ventana Figura 1-5

Figura 1-5 Prueba de una página web ubicada en el subdirectorio raíz 
del documento

### Probar de su sitio web desde otra computadora en su LAN

Como otra prueba, puede ver si Apache se conecta a otras computadoras 
en la misma red de área local (LAN) que su PC. Una conexión entre dos 
estaciones de trabajo en la misma LAN ocurre a través de la capa de 
Internet del protocolo TCP / IP entre sus direcciones IP privadas. Las 
direcciones IP privadas son válidas solo internamente en la LAN. Esto 
contrasta con las direcciones IP públicas que son globalmente accesibles 
a todo Internet. En pocas palabras, todas las estaciones de trabajo en 
una LAN poseen direcciones IP privadas y están representadas en Internet 
con la dirección IP pública del enrutador LAN (que también incluye una 
dirección IP privada para la comunicación interna con las estaciones de 
trabajo LAN).

En la terminal de Linux, use el comando `ifconfig` para averiguar la 
dirección IP privada de su computadora.

```
$ ifconfig
```

También puede utilizar el comando `hostname`.

```
$ hostname -I
```

La dirección IP privada de mi computadora utilizada en ese momento era 
192.168.1.5, como se muestra en la Figura 1-6.

Abrir imagen en una ventana nueva Figura 1-6

Figura 1-6 `ifconfig` devuelve la dirección IP privada

Utilice la dirección IP privada devuelta por ifconfig en la barra de 
direcciones del navegador local. La página web se carga como antes 
(con la dirección de bucle invertido), como se muestra en la Figura 1-7.

Abrir imagen en una ventana nueva Figura 1-7

Figura 1-7 Prueba del servidor web con una dirección IP privada

A continuación, intente con la misma dirección IP desde otra computadora 
de su LAN. La Figura 1-8 muestra el navegador utilizado en un sistema 
Windows 7 en la misma LAN.

Abrir imagen en una ventana nueva Figura 1-8

Figura 1-8 Prueba del servidor web desde otra PC de su LAN


## Proporcionar una dirección IP privada estática al servidor web

El siguiente paso para la configuración del servidor web es configurar 
una dirección IP privada estática. Esta es una dirección IP privada 
que permanece igual cada vez que se reinicia la computadora que aloja 
su servidor web. Este paso es opcional para probar su servidor desde 
su LAN, pero es esencial para que el servidor esté disponible para 
toda Internet. Para proporcionar una dirección IP privada estática, la 
conexión de red de la computadora que aloja el servidor web debe 
configurarse correctamente.

La configuración más sencilla para una estación de trabajo en una LAN 
es obtener su dirección IP con el Protocolo de configuración dinámica 
de host (DHCP). Cuando una PC está configurada con la opción DHCP, al 
inicio, el enrutador le asigna una dirección IP, una máscara de red, 
la dirección IP de la puerta de enlace predeterminada (el enrutador 
mismo) y el servidor DNS utilizado para realizar las traducciones entre 
nombres de dominio y direcciones IP. El inconveniente de DHCP es que su 
PC obtiene la siguiente dirección IP disponible de un grupo de direcciones 
IP, que puede ser una dirección diferente a la obtenida la vez anterior 
(una dirección dinámica).

Su otra opción es utilizar una dirección IP privada estática. Cada vez 
que inicia su PC, su dirección IP sigue siendo la misma. Una dirección 
IP estática es obligatoria para que un servidor esté disponible para 
conexiones a través de Internet. Para implementar una dirección IP 
estática, debe configurar manualmente la información previamente 
recuperada por DHCP. Ejecute primero el comando route desde su terminal 
para encontrar la puerta de enlace predeterminada (el enrutador) en su 
LAN, como se muestra en la Figura 1-9.

```
$ route –n
```

Abrir imagen en una ventana nueva Figura 1-9

Figura 1-9 El comando route devuelve la dirección IP de la puerta de 
enlace predeterminada

La dirección IP de la puerta de enlace (el enrutador) es por lo tanto 
192.168.1.1.

Ejecute `ifconfig` nuevamente para recopilar información sobre la 
dirección IP actual y la máscara.

Como vio en la Figura 1-6 anteriormente en este capítulo, la dirección 
IP del servidor web es actualmente 192.168.1.5 con una máscara de red 
de 255.255.255.0. La parte distinta de cero de la máscara de red 
describe la parte de la dirección IP que corresponde a la red, mientras 
que el cero describe la parte que corresponde a la computadora en esa 
red. Al aplicar un AND lógico a la dirección IP y su máscara de red, 
se obtiene la dirección de red.

Por lo tanto, la LAN en este ejemplo tiene la dirección IP 
192.168.1.0. Esto deja el último byte para representar la computadora 
en la red. Por lo tanto, este es el byte que cambiará para elegir la 
dirección IP estática para el servidor web. Puede seleccionar cualquier 
valor no utilizado de 1 a 254. Por ejemplo, para el valor 100, obtiene 
la dirección IP 192.168.1.100.

Con la información recopilada, haga clic en el icono Red cableada en su 
escritorio y luego haga clic en Siguiente. Alternativamente, seleccione 
Configuración del sistema ➤ Red ➤ Cableado o Configuración 
➤ Preferencias ➤ Conexiones de red según su versión de Ubuntu. En la 
ventana Conexiones de red que aparece, como se muestra en la Figura 
1-10, haga doble clic en “Conexión cableada 1” (o la opción con un 
nombre similar en la lista de Ethernet).

Abrir imagen en una ventana nueva Figura 1-10

Figura 1-10 La ventana Conexiones de red

La ventana "Editar conexión cableada 1" que aparece a continuación 
proporciona las pestañas Configuración de IPv4 y Configuración de IPv6 
para configurar direcciones IPv4 e IPv6, respectivamente (Figura 1-11).

Abrir imagen en una ventana nueva Figura 1-11

Figura 1-11 La ventana "Editar conexión cableada 1"

En la pestaña Configuración de IPv4, el método seleccionado actualmente 
es Automático (DHCP). Anule la selección de esta opción y seleccione 
Manual en la lista de métodos (Figura 1-12).

Abrir imagen en una ventana nueva Figura 1-12

Figura 1-12 Aplicación del método manual para configurar los ajustes 
de IPv4

Haga clic en el botón Agregar para completar los campos obligatorios 
para configurar manualmente los parámetros de conexión. En Dirección, 
ingrese su dirección IP preferida. Para la LAN dada con la dirección IP 
192.168.1.0, puede ingresar, por ejemplo, 192.168.1.100 como la nueva 
dirección IP estática para el servidor web. Ingrese también el valor de 
la máscara de red, 255.255.255.0, para la opción Máscara de red, e 
ingrese la dirección IP del enrutador, 192.168.1.1 en este ejemplo, 
para la opción Puerta de enlace. También debe proporcionar la dirección 
(o direcciones) IP de un servidor DNS en el campo "Servidores DNS". Si 
ingresa más de una dirección IP, sepárelas con comas. Para el servidor 
DNS, puede usar la dirección IP del servidor DNS sugerida por su 
proveedor de servicios de Internet (ISP) o usar la dirección IP de un 
servidor DNS disponible gratuitamente como el 8.8.8.8 de Google.

Haga clic en el botón Guardar para confirmar la nueva configuración. 
Para activar la nueva dirección IP, haga clic en el icono de Network 
Manager y seleccione Desconectar; luego haga clic nuevamente y 
seleccione "Conexión cableada 1" o ingrese lo siguiente en la terminal 
de Linux:

```
$ sudo service network-manager restart
```

Puede probar ahora la dirección IP estática de su servidor web 
ingresándola en la barra de direcciones de un navegador y ejecutándola 
desde otra PC en su LAN, como se indica en la Figura 1-13.

Abrir imagen en una ventana nueva Figura 1-13

Figura 1-13 Prueba del servidor web utilizando su dirección IP estática

En la siguiente sección, puede configurar su firewall de Linux para 
bloquear las conexiones para todos los números de puerto, excepto los 
establecidos por usted.

## Using the Linux Firewall

Debería poder ver su página web en otra PC en su LAN sin problemas. 
Sin embargo, si un firewall está habilitado en su sistema, recibirá 
el mensaje de error "No se puede acceder a este sitio" o uno similar 
en su navegador. (Un firewall es una aplicación que verifica las 
conexiones a su sistema y las permite o prohíbe según las reglas que 
usted establezca).

En el sistema que usé en la sección anterior, el firewall de Ubuntu 
Linux preinstalado (ufw) estaba deshabilitado de forma predeterminada. 
Para deshabilitarlo, puede usar este comando:

```
$ sudo ufw disable
```

Ahora intente conectarse y vea si recibe el mensaje de error. Luego 
puede restaurarlo usando el siguiente comando:

```
$ sudo ufw enable
```

Para permitir que el servidor Apache reciba conexiones mientras hay un 
firewall activado, puede especificar las reglas apropiadas. Apache, 
tratando de facilitar su trabajo, ya estableció las reglas básicas para 
usted cuando se instaló en su sistema. En la línea de comando, ingrese 
lo siguiente:

```
$ sudo ufw app list
```

La salida del comando incluye los perfiles de ufw para Apache que 
se muestran en la Figura 1-14.

Abrir imagen en una ventana nueva Figura 1-14

Figura 1-14 Listado de perfiles ufw

- Profile Apache es para el puerto HTTP 80 predeterminado.

- Profile Apache Full es para los puertos 80 y 443.

- Perfil Apache Secure es para el puerto HTTPS 443 predeterminado.

Para permitir el tráfico entrante para el perfil Apache Full, ingrese 
lo siguiente en la terminal de Linux:

```
$ sudo ufw allow 'Apache Full'
```

Para verificar esto, ingrese lo siguiente:

```
$ sudo ufw status
```

La salida del comando se muestra en la Figura 1-15.

Abrir imagen en una ventana nueva Figura 1-15

Figura 1-15 Visualización del estado del firewall ufw con el conjunto 
de perfiles Apache Full

Una forma aún mejor de verificar esto es conectarse al servidor web 
Apache desde otra PC en su LAN. La página web debería cargarse como de 
costumbre.

## Managing the Apache Process

Para aplicar nuevas reglas de configuración mientras se ejecuta Apache 
y también para iniciar y detener el proceso de Apache (requerido, por 
ejemplo, cuando desea probar otros servidores web), debe ejecutar 
algunos comandos desde la terminal de Linux.

Para detener su servidor web, ingrese lo siguiente en la línea de 
comando:

```
$ sudo systemctl stop apache2
```

O usar:

```
$ sudo service apache2 stop
```

Para iniciar el servidor web Apache cuando ya está detenido, ingrese 
lo siguiente:

```
$sudo systemctl start apache2
```

O usar:

```
$ sudo service apache2 start
```

Realice el siguiente experimento para implementar los comandos anteriores. 
Primero ejecute lo siguiente para ver los procesos de apache2:

```
$ ps xa | grep apache2
```

Luego use el comando `systemctl stop` y ejecute el comando `ps` 
nuevamente. Verá que los procesos de `apache2` han sido "eliminados". 
También puede probar una solicitud desde su navegador. Verá que 
aparece el mensaje "No se puede acceder al sitio", como se ve en 
la Figura 1-16.

Abrir imagen en una ventana nueva Figura 1-16

Figura 1-16 Intentando descargar una página web con el servidor 
Apache detenido

Utilice el comando systemctl start para restaurar el servidor 
Apache.

La Figura 1-17 muestra los comandos anteriores.

Abrir imagen en una ventana nueva Figura 1-17

Figura 1-17 Visualización de Apache PID cuando el servidor se 
ejecuta y la ausencia de Apache PID cuando el servidor se detiene

Para detener y luego iniciar el servidor Apache nuevamente, ingrese 
lo siguiente: Luego use el comando systemctl stop y ejecute el comando 
`ps` nuevamente. Verá que los procesos de `apache2` han sido 
"eliminados". También puede probar una solicitud desde su navegador. 
Verá que aparece el mensaje "No se puede acceder al sitio", como se ve 
en la Figura 1-16.

Abrir imagen en una ventana nueva Figura 1-16

Figura 1-16 Intentando descargar una página web con el servidor 
Apache detenido

Utilice el comando `systemctl start` para restaurar el servidor 
Apache.

La Figura 1-17 muestra los comandos anteriores.

Abrir imagen en una ventana nueva Figura 1-17

Figura 1-17 Visualización de Apache PID cuando el servidor se ejecuta 
y la ausencia de Apache PID cuando el servidor se detiene

Para detener y luego iniciar el servidor Apache nuevamente, ingrese 
lo siguiente:

```
$ sudo systemctl restart apache2
```

Para recargar Apache (es decir, para mantener el proceso en ejecución 
y simplemente actualizar la configuración), ingrese lo siguiente:

```
$ sudo systemctl reload apache2
```

o puede ingresar lo siguiente:

```
$ sudo service apache2 force-reload
```

que es igual a lo siguiente:

```
$ sudo service apache2 reload
```

El servidor Apache se inicia de forma predeterminada cuando se inicia 
el sistema. Para deshabilitar esa configuración, ingrese lo siguiente:

```
$ sudo systemctl disable apache2
```
Para que Apache se inicie al iniciar el sistema, ingrese lo siguiente:

```
$ sudo systemctl enable apache2
```

## Working with Virtual Hosts

Los hosts virtuales (`vhosts`) o los servidores virtuales permiten que 
un servidor web albergue varios sitios simultáneamente. Los `vhosts` 
pertenecen a una de las siguientes categorías:

- Hosts virtuales basados en IP, donde cada `vhost` está dedicado a 
un sitio que hace uso de una dirección IP específica de las direcciones 
IP del servidor web.

- Hosts virtuales basados en puertos, donde cada vhost escucha en un 
- número de puerto diferente para cada sitio que aloja

- Hosts virtuales basados en nombre, donde cada `vhost` está dedicado 
a un sitio con un nombre de dominio específico

Una configuración de Apache puede mezclar todas estas categorías y 
también incluir un host predeterminado.

El archivo de configuración inicial de Apache se llama `000-default.conf`
 y se encuentra en el directorio `/etc/apache2/sites-available`. Apache 
requiere un nuevo archivo de configuración para tener una extensión de 
archivo `.conf`. En la terminal de Linux, cambie el directorio de 
trabajo a `sites-available`, que es el directorio de Apache que 
contiene los archivos de configuración.

```
$ cd /etc/apache2/sites-available
```

Abra el archivo de configuración predeterminado `000-default.conf` con 
el comando `cat`, como se muestra aquí:

```
$ cat 000-default.conf
```

El archivo se abre en una nueva ventana. El contenido del archivo 
es el siguiente:

```
<VirtualHost *:80>
      # The ServerName directive sets the request scheme, hostname and port that
      # the server uses to identify itself. This is used when creating
      # redirection URLs. In the context of virtual hosts, the ServerName
      # specifies what hostname must appear in the request's Host: header to 
      match this virtual host. For the default virtual host (this file) this 
      value is not decisive as it is used as a last resort host regardless.
      # However, you must set it for any further virtual host explicitly.
      #ServerName www.example.com
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/html
      # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
      # error, crit, alert, emerg.
      # It is also possible to configure the loglevel for particular
      # modules, e.g.
      #LogLevel info ssl:warn
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
      # For most configuration files from conf-available/, which are
      # enabled or disabled at a global level, it is possible to
      # include a line for only one particular virtual host. For example the
      # following line enables the CGI configuration for this host only
      # after it has been globally disabled with "a2disconf".
      #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

# vim: sintaxis=apache ts=4 sw=4 sts=4 sr noet

Cada archivo de configuración de Apache incluye tres tipos de entradas.

- Directivas que definen el comportamiento del servidor web

-Contenedores, como `VirtualHos`t, que definen bloques de directivas 
para un `vhost`

- Comentarios, comenzando con un hash (`#`), que brindan alguna ayuda 
en el uso de las directivas

El contenedor `<VirtualHost*:80>` se empareja con `</VirtualHost>` como 
una etiqueta HTML de inicio y finalización. El par `VirtualHost` incluye 
todas las directivas para el `vhost` específico y también define la 
dirección IP y el número de puerto al que el `vhost` dado debe responder. 
En este archivo de configuración, el asterisco (`*`) corresponde a cualquier 
dirección IP y 80 corresponde al puerto 80, que es el puerto 
predeterminado para el protocolo HTTP. Ya probó esta configuración en 
las secciones anteriores cuando se usó una de las siguientes direcciones 
IP en la barra de direcciones de su navegador para descargar el índice 
del directorio:

```
127.0.0.1
192.168.1.100
```

Ambas direcciones tuvieron éxito porque la configuración aceptó 
cualquier dirección IP válida para este servidor. En la siguiente 
sección, creará dos `vhosts` diferentes, cada uno con una dirección 
IP diferente.


### Uso de hosts virtuales basados en IP

En esta sección, creará un nuevo archivo de configuración con gedit 
para implementar dos `vhosts` basados en IP utilizando las directivas 
contenidas entre los contenedores de `start/end` de `VirtualHost`. Un 
`vhost` será responsable de la dirección IP de `loopback`, `127.0.0.1`, 
y el otro será responsable de la dirección IP privada del servidor web, 
`192.168.1.100`, ambos escuchando en el puerto `80`. El par de dirección 
IP y número de puerto para cada `vhost` se indica en el contenedor 
`VirtualHost`. Por ejemplo, `<VirtualHost 127.0.0.1:80>` indica la 
dirección IP de loopback 127.0.0.1 y 80 es el número de puerto 
predeterminado para el protocolo HTTP. Puede proporcionar un índice de 
directorio diferente para cada `vhost`, como `index1.html` para el 
primero e `index2.html` para el segundo, utilizando la directiva 
`DirectoryIndex`. Puede dejar las otras directivas con sus valores 
predeterminados del archivo `000-default.conf`.

En la terminal de Linux, cambie el directorio de trabajo a 
`sites-available` y use gedit para crear el archivo `example1.conf`.

```
$ cd /etc/apache2/sites-available
$ sudo gedit example1.conf
```

Ingrese las siguientes reglas de configuración y guarde el archivo:

```
# 1st vhost
<VirtualHost 127.0.0.1:80>
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/html
      DirectoryIndex index1.html
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
# 2nd vhost
<VirtualHost 192.168.1.100:80>
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/html
      DirectoryIndex index2.html
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```


A continuación, debe habilitar la nueva configuración mediante el 
comando `a2ensite` (apache2 enable site). En la terminal de Linux, 
ingrese lo siguiente:

```
$ sudo a2ensite example1.conf
```

O simplemente ingrese esto:

```
$ sudo a2ensite example1
```
Para habilitar la nueva configuración de Apache, también necesita 
volver a cargar el servidor web.

```
$ sudo service apache2 force-reload
```

Cree dos páginas web en el directorio raíz del documento que servirán 
como índices de directorio para los dos `vhosts`.

```
$ cd /var/www/html
$ sudo gedit index1.html
```


Ingrese el siguiente código fuente HTML:

```
<!DOCTYPE html>
<html>
<head>
<title>Testing the Apache Web Server</title>
<style>
   body {
        background-color:lightblue;
        }
   p {
   background-color:red;
   font-size:20px;
   }
</style>
</head>
<body>
<p>Hello from 127.0.0.1</p>
</body>
</html>
```

En `/var/www/html`, cree una página web llamada `index2.html` para 
utilizarla como índice de directorio para el segundo `vhost`. Utilice
 el siguiente código fuente HTML:

```
<!DOCTYPE html>
<html>
<head>
<title>Testing the Apache Web Server</title>
<style>
   body {
        background-color:red;
        }
   p {
   background-color:lightblue;
   font-size:20px;
   }
</style>
</head>
<body>
<p>Hello from 192.168.1.100</p>
</body>
</html>
```

Abra dos pestañas en su navegador. En el primero, ingrese la 
siguiente dirección en la barra de direcciones:Abra dos 
pestañas en su navegador. En el primero, ingrese la 
siguiente dirección en la barra de direcciones:

```
127.0.0.1
```

En la segunda pestaña, ingrese la siguiente dirección en la barra 
de direcciones:

```
192.168.1.100
```

Con la configuración actual, Apache muestra contenido diferente para 
cada solicitud. La primera solicitud, con la dirección IP 127.0.0.1, 
se resuelve en `index1.html` (Figura 1-18).

Abrir imagen en una ventana nueva Figura 1-18

Figura 1-18 Prueba del primer `vhost` basado en IP

La segunda solicitud con la dirección IP 192.168.1.100 se resuelve 
en `index2.html` (Figura 1-19).

Abrir imagen en una ventana nueva Figura 1-19

Figura 1-19 Prueba del segundo `vhost` basado en IP

A continuación, verá un ejemplo similar, esta vez utilizando hosts 
virtuales basados en puertos.


### Uso de hosts virtuales basados en puertos

La configuración utilizada a continuación creará dos hosts virtuales 
que escuchan en diferentes puertos, por ejemplo, el puerto 8080 y el 
puerto 8181. Cuando se usa cualquier otro número de puerto que no sea 
el predeterminado para el protocolo HTTP, el puerto 80, es necesario 
agregar el puerto a la URL con dos puntos (`:`). Aquí hay una instancia:

```
192.168.1.100:8080
```

El uso de un puerto diferente al predeterminado hace que la URL sea un 
poco más complicada, pero eso no es realmente un problema cuando se 
implementa un servicio DDNS (ver Capítulo 4) que oculta esta complejidad 
al usuario.

En el directorio de sitios disponibles, cree un nuevo archivo de 
configuración.

```
$ cd /etc/apache2/sites-available
$ sudo gedit example2.conf
```
En el archivo `example2.conf`, ingrese las siguientes directivas para 
indicar al servidor que incluya los números de puerto 8080 y 8181 en la 
lista de puertos de escucha:

```
Listen 8080
Listen 8181
```

Para crear un `vhost` que escuche en el puerto 8080 en cualquier 
dirección IP del servidor, puede usar la notación de asterisco en el 
contenedor `VirtualHost` como `<VirtualHost *:8080>`. De manera similar, 
para el `vhost` que escucha en el puerto 8181, también en cualquier 
dirección IP del servidor, use el contenedor `<VirtualHost *:8181>`. 
La lista completa de `example2.conf` es la siguiente:

```
Listen 8080
Listen 8181
# 1st vhost
<VirtualHost *:8080>
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/html
      DirectoryIndex index3.html
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
# 2nd vhost
<VirtualHost *:8181>
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/html
      DirectoryIndex index4.html
      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Guarde el nuevo archivo de configuración y habilítelo con el comando 
`a2ensite`. Escriba lo siguiente:

```
$ sudo a2ensite example2.conf
```

o simplemente escriba lo siguiente:

```
$ sudo a2ensite example2
```

TPara habilitar la nueva configuración de Apache, también debe volver 
a cargar el servidor web.

```
$ sudo service apache2 force-reload
```

A veces, las mismas reglas de configuración se aplican a diferentes 
sitios, lo que genera resultados impredecibles. Si tiene que trabajar 
en sitios con reglas en conflicto, debe desactivar uno de los sitios. 
Para deshabilitar, por ejemplo, `example1`, use el comando `a2dissite`
(apache2 disable site), como se muestra aquí:

```
$ sudo a2dissite example1
```
Para tener dos números de puerto nuevos que escuchará el servidor, debe 
configurar `ufw` para permitir conexiones para esos números de puerto. 
En la línea de comandos de Linux, escriba lo siguiente:

```
$ sudo ufw allow 8080
$ sudo ufw allow 8181
```

Cree en la raíz del documento los dos índices de directorio, 
`index3.html` e `index4.html`, y consúltelos en el archivo 
`example2.conf`. Estos índices son para los `vhosts` que escuchan en 
el puerto 8080 y en el puerto 8181, respectivamente.

```
$ cd /var/www/html
$ sudo gedit index3.html
```
In the gedit window that appears, enter the following HTML source code:

```
<!DOCTYPE html>
<html>
<head>
<title>Testing the Apache Web Server</title>
<style>
   body {
        background-color:red;
        }
   p {
   background-color:lightblue;
   font-size:20px;
   }
</style>
</head>
<body>
<p>Hello from 8080</p>
</body>
</html>

Next, in the document root directory , add the following HTML source code to index4.html:
<!DOCTYPE html>
<html>
<head>
<title>Testing the Apache Web Server</title>
<style>
   body {
        background-color:lightblue;
        }
   p {
   background-color:red;
   font-size:20px;
   }
</style>
</head>
<body>
<p>Hello from 8181</p>
</body>
</html>
```
Para probar los nuevos vhosts, abra dos pestañas en su navegador. En 
la primera pestaña, ingrese lo siguiente en la barra de direcciones:

```
127.0.0.1:8080
```

o ingrese lo siguiente:

```
192.168.1.100:8080
```
Se muestra el índice de directorio `index3.html` (Figura 1-20).

Abrir imagen en una ventana nueva Figura 1-20

Figura 1-20 Prueba del primer `vhost` basado en puerto

En la segunda pestaña, ingrese uno de los siguientes en la barra de 
direcciones:

```
127.0.0.1:8181
192.168.1.100:8181
```

Se muestra el índice de directorio `index4.html` (Figura 1-21).

Abrir imagen en una ventana nueva Figura 1-21

Figura 1-21 Prueba del segundo `vhost` basado en puerto

En la siguiente sección, creará otro par de `vhosts` que sirven 
solicitudes de nombres de dominio específicos en el campo `Host` de 
la solicitud HTTP del cliente.


### Uso de hosts virtuales basados en nombres

El tercer enfoque para ejecutar varios sitios es utilizar `vhosts` 
basados en nombres, donde cada `vhost` responde a la solicitud del 
cliente si el cliente proporciona un valor de encabezado HTTP de host 
que coincida con su directiva `ServerName`. No utilizará el Sistema de 
nombres de dominio para su servidor web hasta el Capítulo 4, por lo que 
por ahora solo puede probar los hosts virtuales basados en nombres 
localmente, desde la misma computadora que el servidor. Para 
proporcionar nombres a su computadora, edite el archivo `/etc/hosts` c
omo se indica en la Figura 1-22. Utilice el siguiente comando en la 
terminal:

```
$ sudo gedit /etc/hosts
```

Abrir imagen en una ventana nueva Figura 1-22

Figura 1-22 Agregar dos nombres de host más a `/etc/hosts`

Como puede ver, ya ha utilizado una entrada de este archivo, `localhost`, 
que corresponde a la dirección IP 127.0.0.1. Ingrese dos entradas más en 
`/etc/hosts`, ambas resueltas en la dirección IP privada estática del 
servidor web.

```
192.168.1.100      webserver.com
192.168.1.100      myserver.com
```

Guarde el archivo `/etc/hosts` y pruebe uno de esos nombres, por 
ejemplo `webserver.com`, en la barra de direcciones de su navegador. 
El nombre de dominio se resuelve en la dirección IP 192.168.1.100, y 
con los sitios de la configuración `example1` deshabilitados, el `vhost` 
que envía esta solicitud es el predeterminado, `000-default.conf`. La 
página web descargada es, por tanto, la indicada en la Figura 1-23.

Abrir imagen en una ventana nueva Figura 1-23

Figura 1-23 Sin ningún `vhost` basado en nombre configurado hasta ahora, 
el `vhost` predeterminado atiende la solicitud `webserver.com`

Cree dos `vhosts` basados en nombres, cada uno responsable de uno de los 
nombres anteriores. Utilice gedit para crear el archivo de configuración 
de los dos `vhost`s.

```
$ sudo gedit example3.conf
```

En `example3.conf`, ingrese las siguientes directivas:

```
<VirtualHost *:80>
    ServerName webserver.com
    DocumentRoot "/var/www/html/test1"
</VirtualHost>
<VirtualHost *:80>
    ServerName myserver.com
    DocumentRoot "/var/www/html/test2"
</VirtualHost>
```
El primer `vhost` escucha el puerto HTTP predeterminado, que es el 
puerto 80. Se activa cuando recibe una solicitud HTTP con el valor del 
encabezado del host establecido en `webserver.com`. El segundo `vhost` 
también escucha el puerto 80. Espera las solicitudes HTTP con el valor 
del encabezado del Host establecido en `myserver.com`.

Los dos sitios usan dos raíces de documentos diferentes, 
`/var/www/html/test1` y `/var/www/html/test2`, y dado que no se definen 
índices de directorio con la directiva `DirectoryIndex`, el predeterminado 
para Apache, `index.html`, lo hará ser usado. A continuación, cree un 
archivo llamado `index.html` en la raíz de cada directorio. En la 
terminal de Linux, ingrese lo siguiente:

```
$ cd /var/www/html/test1
$ sudo gedit index.html
```
En la ventana de gedit, inserte el siguiente código HTML y guarde 
el archivo:

```
<!DOCTYPE html>
<html>
<head>
<title>Testing the Apache Web Server</title>
<style>
   body {
        background-color:red;
        }
   p {
   background-color:lightblue;
   font-size:20px;
   }
</style>
</head>
<body>
<p>Hello from webserver.com</p>
</body>
</html>
```

Cree un archivo llamado `index.html` en la segunda raíz del documento. En 
la terminal de Linux, ingrese lo siguiente: Cree un archivo llamado 
`index.html` en la segunda raíz del documento. En la terminal de Linux, 
ingrese lo siguiente:

```
$ cd /var/www/html/test2
$ sudo gedit index.html
```

En la ventana de gedit, ingrese el siguiente código fuente y guarde el 
archivo:

```
<!DOCTYPE html>
<html>
<head>
<title>Testing the Apache Web Server</title>
<style>
   body {
        background-color:lightblue;
        }
   p {
   background-color:red;
   font-size:20px;
   }
</style>
</head>
<body>
<p>Hello from myserver.com</p>
</body>
</html>
```

Habilite la nueva configuración usando lo siguiente:

```
$ sudo a2ensite example3
```

También debe volver a cargar el servidor web para habilitar los 
nuevos sitios.

```
$ sudo service apache2 force-reload
```

En un navegador de la computadora que aloja el servidor web, abra dos 
pestañas. En la primera pestaña, ingrese lo siguiente en la barra de 
direcciones:

```
webserver.com
```

La página web `index.html`, ubicada en `/var/www/html/test1`, se 
muestra en la ventana del navegador, como se muestra en la Figura 1-24.

Abrir imagen en una ventana nueva Figura 1-24

Figura 1-24 Prueba del primer vhost basado en nombre

Utilice la siguiente URL en la segunda pestaña:

```
myserver.com
```

La página web `index.html`, ubicada en el documento root 
`/var/www/html/test1`, se muestra en la ventana del navegador, como se 
ve en la Figura 1-25.

Abrir imagen en una ventana nueva Figura 1-25

Figura 1-25 Prueba del segundo vhost basado en el nombre

En el Capítulo 9, probará la directiva `ServerName` con un nombre de 
dominio globalmente válido como `httpsserver.eu` y verá cómo obtener y 
aplicar dicho nombre de dominio.


### Inspección de la configuración general del host virtual

Cada configuración habilitada con el comando `a2ensite` agrega un enlace 
simbólico de sí mismo al directorio `/etc/apache2/sites-enabled`. Utilice 
lo siguiente:

```
$ ls /etc/apache2/sites-enabled
```

Como se muestra en la Figura 1-26, el comando `ls` enumera tres archivos 
de configuración: `000-default.conf`, `example2.conf` y `example3.conf`. 
La configuración `example1.conf` no está incluida en este directorio 
porque fue deshabilitada con el comando `a2dissite`. Utilice lo siguiente 
para ver los detalles de la configuración general:

```
$ sudo apachectl -S
```

Abrir imagen en una ventana nueva Figura 1-26

Figura 1-26 Listado de los sitios habilitados y detalles de la 
configuración de Apache

La Figura 1-26 muestra también la salida del comando `apachectl -S`, 
que proporciona una descripción general de la configuración de `vhost`.

 
 ## Lectura de archivos de registro de Apache

Para tener una idea del número de visitantes y ver algunos de los 
detalles de la conexión, como la dirección IP del cliente y la hora, 
puede leer los archivos de registro de Apache. Para los ejemplos de 
`vhosts` de este capítulo, las directivas relacionadas con los archivos 
de registro se utilizan en los archivos de configuración de la 
siguiente manera:

```
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
```

Para ubicar `APACHE_LOG_DIR`, use lo siguiente en la terminal de 
Linux:

```
$ grep APACHE_LOG_DIR /etc/apache2/envvars
```

El terminal de Linux responde con el siguiente resultado:

```
export APACHE_LOG_DIR=/var/log/apache2$SUFFIX
```

Con un valor vacío asignado para la variable `$SUFFIX` en el archivo 
`envvars`, el directorio de registro de Apache se resuelve en 
`/var/log/apache2`.

Cambie el directorio de trabajo a `/var/log/apache2`, donde el archivo 
`access.log` se usa para listar los detalles del visitante y el archivo 
`error.log` se usa para informar cualquier error.

```
$ cd /var/log/apache2
```

Muévase a otra computadora de su LAN y realice una solicitud HTTP al 
servidor Apache. Por ejemplo, en la barra de direcciones del navegador, 
escriba lo siguiente:

```
192.168.1.100
```

Lea el archivo `access.log` con `more`, `less`, `tail` o un comando 
similar:

```
$ tail –n 1 access.log
```

El comando de shell de Linux anterior imprime una línea desde el final, 
que corresponde al último registro del registro de acceso. También 
puede usar el argumento `–f` para imprimir continuamente los detalles 
del último visitante del registro de acceso:

```
$ tail –fn 1 access.log
```
Los detalles sobre la nueva conexión se muestran a continuación, 
incluida la dirección IP de la computadora utilizada por el cliente, 
la fecha y hora en que se emitió la solicitud, la URL utilizada, el 
sistema operativo y el navegador:

```
192.168.1.2 - - [14/Jun/2018:16:09:23 +0300] 
"GET /favicon.ico HTTP/1.1" 404 504 
"http://192.168.1.100/" 
"Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36”
```
