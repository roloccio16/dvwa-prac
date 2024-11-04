# Maquina DVWA

## Vulnerabilidades: Dificultad baja

### Fuerza bruta

Para realizar un ataque por fuerza bruta en esta página DVWA vamos
a necesitar por ejemplo Burp Suite, entraremos en el login, activaremos
el proxy de Burp que tenemos configurado y procederemos a interceptar el trafico
web de la página en cuestion, cogeremos la linea que interceptamos donde mandamos
la petición del login y la mandaremos al intruder, en este determinarenos
cuales son las palabras clave que queremos que Burp Suite cambie para ir probando
en este caso usuarios y contraseñas, tendremos que configurar dos diccionarios
uno para usernames y otro para passwords, elegiremos el método de ataque de
Cluster Bomb, es el más apropiado para este caso, ya que usa varios diccionarios
y probara todas las contraseñas para cada nombre de usuario, una vez todo esté
perfectamente configurado procedemos a lanzar el ataque, recibiremos el resultado
que esperabamos si nos logea en algun usuario, o si nos da un error que no sea el 200
que por lo menos nos indique que efectivamente hay un usuario aunque nos hayamos equivocado
de contraseña

### Inyeccion de Comandos

Para realizar esta vulnerabilidad nos ponen un input en el que nos piden una ip
a la que vamos a realizar un ping, solo tenemos que escribir la ip random que
queramos y a continuacion escribir el carácter ";", despues de esto puedes escribir
los comandos que quieras, desde un ls y un nano a un rm rf y borrar todo del tiron

### Subir archivo

En este apartado hemos procedido después de investigar un poco y ver que la página funciona con php, por lo tanto vamos a intentar subir un archivo php muy simple en el que simplemente introducimos la funcion "phpinfo()", parece que se lo traga y nos dice donde lo ha almacenado, simplemente cogemos la ip de la pagina y añadimos la ruta que nos da y ahi tenemos el endpoint donde se almacenan todos los archivos, viendo esto hay un monton de posibilidades asi que decidimos añadir un archivo mas, una terminal simple "shell.php",
subimos el archivo y podemos acceder a todo lo que hay dentro.
Hemos probado "powny-shell", "c99shell" y "pentestmokeyshell"

### CSRF Y XSS (STORED)

Hemos unido estas dos vulnerabilidades rompiendolas a la vez, CSRF consiste en que esta página por ejemplo el cambio de contraseña de los usuarios se realiza mediante la url, escribiendo en esta una serie de parámetros con la contraseña que quieres cambiar sacandolos del formulario en el que escribe el usuario.
En XSS(Stored) nos permite escribir un comentario con el nombre de quien lo ha publicado y todos los usuarios deberian de poder verlo, la caja de front donde nos ponen a escribir el comentario que queremos publicar tiene un número de caracteres limitados, pero parece que no es asi en el back despues de comprobar que si editamos la solicitud en el inspeccionar del navegador o en el mismo Burp Suite podemos enviar una cadena de texto mas larga sin que haya ningun problema.
asi que creamos un comentario y editando la solicitud que enviamos añadimos un a href que cuando un usuario aleatorio pulse en el o pase el raton por encima lo redirija a la url necesaria para cambiar su contraseña a la que nosotros hayamos querido, para ellos simplemente tenemos que copiar el codigo url que haria falta para cambiarsela y usar cyberchef por ejemplo para encodearlo de manera que la solicitud sea interpretable por el navegador, y listo.
