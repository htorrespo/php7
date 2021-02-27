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
siguiente dirección en la barra de direcciones:Abra dos pestañas en su navegador. En el primero, ingrese la 
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


### Using Port-Based Virtual Hosts

The configuration used next will create two virtual hosts that listen on different ports, for instance port 8080 and port 8181. When anything other than the default port number for the HTTP protocol, port 80, is used, the port is required to be appended to the URL with a colon (:). Here’s an instance:
192.168.1.100:8080

Using a different port than the default one makes the URL a bit more complicated, but that is not really a problem when you implement a DDNS service (see Chapter  4) that hides this complexity from the user.

In the sites-available directory, create a new configuration file.
$ cd /etc/apache2/sites-available
$ sudo gedit example2.conf

In the file example2.conf, enter the following directives to direct the server to include port numbers 8080 and 8181 in the list of the listening ports:
Listen 8080
Listen 8181

To create a vhost that listens on port 8080 on any IP address of the server, you can use the asterisk notation in the VirtualHost container as <VirtualHost *:8080>. Similarly, for the vhost that listens on port 8181, also on any IP address of the server, use the <VirtualHost *:8181> container. The complete listing of example2.conf is as follows:
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

Save the new configuration file and enable it using the a2ensite command . Type the following:
$ sudo a2ensite example2.conf

or simply type the following:
$ sudo a2ensite example2

To enable the new Apache configuration, you also need to reload the web server.
$ sudo service apache2 force-reload

Sometimes the same configuration rules apply to different sites, causing unpredictable results. If you have to work on sites with conflicting rules, then you have to disable one of the sites. To disable, for instance, example1, use the a2dissite (apache2 disable site) command, as shown here:
$ sudo a2dissite example1

To have two new port numbers that the server will listen to, you need to set up ufw to permit connections for those port numbers. At the Linux command line, type the following:
$ sudo ufw allow 8080
$ sudo ufw allow 8181

Create at the document root the two directory indexes, index3.html and index4.html, and refer to them in the example2.conf file. These indexes are for the vhosts that listen on port 8080 and on port 8181, respectively.
$ cd /var/www/html
$ sudo gedit index3.html

In the gedit window that appears, enter the following HTML source code:
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

To test the new vhosts, open two tabs in your browser. On the first tab, enter the following in the address bar:
127.0.0.1:8080

or enter the following:
192.168.1.100:8080

The directory index index3.html is displayed (Figure 1-20).
Open image in new windowFigure 1-20
Figure 1-20

Testing the first port-based vhost

On the second tab, enter one of the following in the address bar:
127.0.0.1:8181
192.168.1.100:8181

The directory index index4.html is displayed (Figure 1-21).
Open image in new windowFigure 1-21
Figure 1-21

Testing the second port-based vhost

In the following section, you will create another pair of vhosts that serve requests for specific domain names in the Host field of the client's HTTP request.


### Using Name-Based Virtual Hosts
The third approach for running multiple sites is to use name-based vhosts, where each vhost responds to the client’s request if the client provides a host HTTP header value that matches its ServerName directive . You will not use the Domain Name System for your web server until Chapter  4, so for now you can only try the name-based virtual hosts locally, from the same computer as the server. To provide names to your computer, edit the /etc/hosts file as indicated in Figure 1-22. Use the following command at the terminal:
$ sudo gedit /etc/hosts
Open image in new windowFigure 1-22
Figure 1-22

Adding two more host names to /etc/hosts

As you can see, you have already used one entry from this file, localhost , which corresponds to IP address 127.0.0.1. Enter two more entries in /etc/hosts, both resolving to the web server’s static private IP address.
192.168.1.100      webserver.com
192.168.1.100      myserver.com

Save the /etc/hosts file and try one of those names, for instance webserver.com, in your browser’s address bar. The domain name resolves to the IP address 192.168.1.100, and with sites of the example1 configuration disabled, the vhost that dispatches this request is the default one, 000-default.conf . The web page downloaded is therefore the one indicated in Figure 1-23.
Open image in new windowFigure 1-23
Figure 1-23

With no name-based vhost configured so far, the default vhost serves the webserver.com request

Create two name-based vhosts, each one responsible for one of the previous names. Use gedit to create the configuration file of the two vhosts.
$ sudo gedit example3.conf

In example3.conf, enter the following directives:
<VirtualHost *:80>
    ServerName webserver.com
    DocumentRoot "/var/www/html/test1"
</VirtualHost>
<VirtualHost *:80>
    ServerName myserver.com
    DocumentRoot "/var/www/html/test2"
</VirtualHost>

The first vhost listens to the default HTTP port, which is port 80. It is triggered when it receives an HTTP request with the value of the Host header set to webserver.com. The second vhost also listens to port 80. It waits for HTTP requests with the value of the Host header set to myserver.com.

The two sites use two different document roots, /var/www/html/test1 and /var/www/html/test2, and since no directory indexes are defined with the DirectoryIndex directive, the default one for Apache, index.html, will be used. Create next a file named index.html in each directory root. At the Linux terminal, enter the following:
$ cd /var/www/html/test1
$ sudo gedit index.html

In the gedit window, insert the following HTML code and save the file:
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

Create a file called index.html in the second document root. At the Linux terminal, enter the following:
$ cd /var/www/html/test2
$ sudo gedit index.html

In the gedit window, enter the following source code and save the file:
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

Enable the new configuration using the following:
$ sudo a2ensite example3

You also have to reload the web server to enable the new sites.
$ sudo service apache2 force-reload

In a browser on the computer that hosts the web server, open two tabs. In the first tab, enter the following in the address bar:
webserver.com

The web page index.html, located in /var/www/html/test1, is displayed in the browser’s window, as displayed in Figure 1-24.
Open image in new windowFigure 1-24
Figure 1-24

Testing the first name-based vhost

Use the following URL on the second tab:
myserver.com

The web page index.html, located in document root /var/www/html/test1, is displayed in the browser’s window, as viewed in Figure 1-25.
Open image in new windowFigure 1-25
Figure 1-25

Testing the second name-based vhost

In Chapter  9 you’ll test the ServerName directive with a globally valid domain name like httpsserver.eu and see how to obtain and apply such a domain name.


### Inspecting the Overall Virtual Host Configuration

Each configuration enabled with the a2ensite command adds a symbolic link of itself to the directory /etc/apache2/sites-enabled. Use the following:
$ ls /etc/apache2/sites-enabled

As displayed in Figure 1-26, the ls command lists three configuration files: 000-default.conf, example2.conf, and example3.conf. The configuration example1.conf is not included in this directory because it was disabled with the a2dissite command. Use the following to view the details of the overall configuration:
$ sudo apachectl -S
Open image in new windowFigure 1-26
Figure 1-26

Listing the enabled sites and details of the Apache configuration

Figure 1-26 displays also the output of the apachectl -S command, which provides an overview of the vhost configuration.

 
 ## Reading Apache Log Files

To get an idea of the number of visitors and see some of the connection details, such as the client IP address and the time, you can read the log files of Apache. For the vhosts examples of this chapter, the directives related to log files are used in the configuration files as follows:
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined

To locate APACHE_LOG_DIR, use the following at the Linux terminal:
$ grep APACHE_LOG_DIR /etc/apache2/envvars

The Linux terminal responds with the following output:
export APACHE_LOG_DIR=/var/log/apache2$SUFFIX

With an empty value assigned for the variable $SUFFIX in the file envvars, the Apache log directory resolves to /var/log/apache2.

Change the working directory to /var/log/apache2, where the file access.log is used to list the visitor details and the file error.log is used to report any errors.
$ cd /var/log/apache2

Move to another computer of your LAN and make an HTTP request to the Apache server. For instance, at the browser’s address bar, type the following:
192.168.1.100

Read the access.log file using more, less, tail, or a similar command:
$ tail –n 1 access.log

The previous Linux shell command prints one line from the end, which corresponds to the last record of the access log. You can also use the –f argument to continuously print the last visitor’s details of the access log:
$ tail –fn 1 access.log

The details about the new connection are displayed next, including the IP address of the computer used by the client, the date and time the request was issued, the URL used, the operating system, and the browser:
192.168.1.2 - - [14/Jun/2018:16:09:23 +0300] "GET /favicon.ico HTTP/1.1" 404 504 "http://192.168.1.100/" "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36”











