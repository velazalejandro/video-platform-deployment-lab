# video-platform-deployment-lab

## Proyecto: Despliegue y hardening de plataforma de video sobre máquina Debian

## Descripción
Proyecto de laboratorio personal basado en una prueba técnica de administración de sistemas. El objetivo consistió en desplegar una plataforma de vídeo open source sobre Debian, configurar el entorno de ejecución y aplicar medidas de seguridad y administración propias de un entorno productivo.

## Tecnologías
- Debian Linux
- Docker
- Git
- GitHub
- Bash
- SSH

## Pasos realizados
- Acceso y administración remota mediante SSH
- Gestión de usuarios y permisos
- Despliegue inicial de la aplicación
- Configuración de servicios necesarios
- Resolución de incidencias durante la instalación
- Documentación del proceso
- Plataforma no completamente operativa al finalizar la prueba

## Lecciones aprendidas
- Gestión de despliegues complejos sobre Linux
- Resolución de dependencias
- Administración de servicios
- Diagnóstico de errores
- Documentación técnica

## Pasos seguidos
1. Acceso al servidor:
Mediante los datos técnicos que se piden, primeramente vamos a conectarnos a la máquina virtual.
- Comprobamos que la IP xx.xx.xxx.xxx nos da conectividad y responde.
- Abrimos la consola de comandos (CMD) y escribimos la orden “ping xx.xx.xxx.xxx”.
- Resultado: nos hace el ping correctamente con lo cual responde.
Lo siguiente que realizaremos es conectarnos a la máquina mediante conexión SSH remota.
Abrimos la consola de PowerShell como administrador y escribimos el siguiente comando:
- ssh administrador@xx.xx.xxx.xxx y su contraseña. Nos deja acceder correctamente.
  
<img width="814" height="189" alt="image" src="https://github.com/user-attachments/assets/9027f327-1c96-4379-a401-23b440602e5a" />


2. Actualización del sistema y paquetes:

Para mantener el sistema bien actualizado y los paquetes, ejecutamos los siguientes comandos:
- sudo apt update
- sudo apt upgrade –y

<img width="643" height="403" alt="image" src="https://github.com/user-attachments/assets/84058798-2535-4372-9a90-26509659bc5f" />


3. Configuración de usuario y pruebas:

Creamos un nuevo usuario mediante buenas prácticas para realizar pruebas mediante la siguiente orden: (para no utilizar siempre el administrador en todo).
- sudo adduser alejandro

El nuevo usuario lo añadimos al grupo sudo:
- sudo usermod -aG sudo alejandro
  
Probamos a entrar con el usuario mediante:
- ssh alejandro@xx.xx.xxx.xxx + la contraseña a introducir

<img width="535" height="379" alt="image" src="https://github.com/user-attachments/assets/1a155f9a-e673-465e-92e1-a7e38038b4f0" />


4. Crear estructura de carpetas de despliegue
- sudo mkdir -p /opt/pumukit
- sudo mkdir -p /opt/pumukit/data
- sudo mkdir -p /opt/pumukit/backups

Se utiliza /opt/pumukit como directorio principal siguiendo la práctica habitual de alojar aplicaciones de terceros bajo /opt.
La estructura se diseñó para separar:
- Configuración
- Datos persistentes
- Logs
- Ficheros de despliegue

5. Instalar Docker

Los elementos importantes que consta el proyecto de Pumukit son los archivos de Dockerfile y Docker Compose, ya que el proyecto está construido con Docker principalmente. La documentación y el repositorio de Pumukit contiene contenedores, así que instalamos Docker Engine y Docker Compose Plugin
Añadimos la clave oficial de Docker:
- sudo install -m 0755 -d /etc/apt/keyrings curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \ sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
- sudo chmod a+r /etc/apt/keyrings/docker.gpg
  
Añadimos el repositorio oficial de Docker:

Añadimos la clave GPG oficial y el repositorio de Docker a tus fuentes de apt.

- sudo mkdir -p /etc/apt/keyrings
- curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
- echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Instalamos los paquetes base:

- sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

<img width="689" height="314" alt="image" src="https://github.com/user-attachments/assets/b5674745-0892-4f5c-a95d-ac9ed4567148" />

Instalamos Docker:

- sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

<img width="815" height="433" alt="image" src="https://github.com/user-attachments/assets/1db1e0fa-7b02-4ce9-9d3e-7b081c28df5c" />

Comprobamos que Docker funciona correctamente:

- sudo docker run hello-world

<img width="667" height="453" alt="image" src="https://github.com/user-attachments/assets/2df51cd7-1c34-4f30-ac80-dab46e78b597" />


6. Clonación del repositorio de Pumukit
   
Repositorio oficial: Pumukit GitHub

- cd /opt sudo git clone https://github.com/pumukit/Pumukit.git
- cd pumukit
- ls -la

<img width="808" height="306" alt="image" src="https://github.com/user-attachments/assets/e3858bf9-2052-4ed9-be64-a7c6a638e3d1" />

7. Desplegar dependencias

Instalamos nginx para configurar el proxy:

-sudo apt install -y nginx

<img width="577" height="432" alt="image" src="https://github.com/user-attachments/assets/2d583104-f439-4456-b9b9-991aebbdf669" />

Comprobamos que está activo:

- systemctl status nginx

<img width="640" height="257" alt="image" src="https://github.com/user-attachments/assets/64d55b56-7805-41e6-9fd6-a7ee9f49fab3" />

Vemos dentro del directorio Pumukit todo el contenido del repositorio:

- README.md
- documentación de instalación
- ficheros Docker/Compose

<img width="525" height="502" alt="image" src="https://github.com/user-attachments/assets/71510a92-c91f-4fd4-a504-752db0101044" />

Comprobamos las 100 primera líneas del README:

-head -100 README.md

<img width="801" height="328" alt="image" src="https://github.com/user-attachments/assets/7786d11d-3a84-4bb5-8e05-2ca8d412a33a" />

Comprobamos qué servicios define Compose:

- docker compose config --services

<img width="809" height="100" alt="image" src="https://github.com/user-attachments/assets/06854146-a0ea-4dc3-ab41-f65f0a85c7e6" />

El proyecto trae un Makefile que encapsula todos los comandos Docker necesarios.
Comprobamos si existe:

- cd /opt/Pumukit
- ls -la Makefile

<img width="653" height="47" alt="image" src="https://github.com/user-attachments/assets/dad188c8-f5c8-4d5b-9136-21592646b3ab" />

Instalamos make

<img width="533" height="295" alt="image" src="https://github.com/user-attachments/assets/14dcd774-0328-42e4-9a73-4c871c6ae819" />

Si existe y tiene la tarea up ejecutamos lo siguiente:

- sudo make up

<img width="815" height="563" alt="image" src="https://github.com/user-attachments/assets/6b72395b-271d-4790-86a5-57e095abd68a" />

Nos da un error en la ejecución.
El despliegue falla durante la construcción debido a una incompatibilidad entre la versión actual de Composer y las dependencias declaradas por Pumukit.
El problema está en la siguiente línea:

- && composer update --prefer-dist --no-scripts --no-progress --classmap-authoritative --no-interaction

El error indica que Composer está bloqueando api-platform/core por temas de seguridad.

Comprobamos si estamos usando la rama principal (master) más reciente:

<img width="673" height="68" alt="image" src="https://github.com/user-attachments/assets/22d72112-a210-49be-84cb-c3d1c57b2e2d" />

Rama master correcta

<img width="815" height="129" alt="image" src="https://github.com/user-attachments/assets/63657971-2752-45c6-8931-8efed0cbb949" />

Vemos que existe el composer.lock

Solución: sustituimos en el dockerfile el “composer update” por “composer install”. Y una vez realizado el cambio volvemos a ejecutar el comando make up para que despliegue todo y se reproduzca el entorno de nuevo.

Editamos el Dockerfile mediante el editor de texto nano:

<img width="805" height="656" alt="image" src="https://github.com/user-attachments/assets/7555ac82-fd85-484f-94c8-15d0833bc55b" />

Sustitumos composer update por composer install. Guardamos y cerramos.

Lo siguiente es lanzar make up.

<img width="444" height="89" alt="image" src="https://github.com/user-attachments/assets/89ee4e9a-d52b-45ed-aee2-09f42024a4f9" />

Vemos que los contenedores de pumukit están activos correctamente.

Después de lanzar correctamente y que funciona make up, comprobamos los contenedores que se han levantado:

- docker ps

<img width="813" height="80" alt="image" src="https://github.com/user-attachments/assets/47e30dc6-da31-4de8-916a-b95237a8522a" />

Vemos los 4 contenedores levantados todos con su puerto correspondiente, su estado y su ID. Funcionan 3 correctamente.
Falta solucionar el contenedor de pumukit-proxy.

Vamos a revisar los logs del contenedor ejecutando la orden:

- docker logs pumukit-proxy-1

Comprobamos las redes de docker:

- docker network ls

<img width="731" height="154" alt="image" src="https://github.com/user-attachments/assets/82573009-f27d-45e5-b6ca-43a444ebc35d" />

Revisamos el docker-compose para ver el tema de la configuración de red del proxy.
Ejecutamos el siguiente comando para comprobar los logs del apartado del proxy y que nos saque las últimas 20 líneas:

- docker compose logs proxy --tail 20

Revisamos

<img width="814" height="328" alt="image" src="https://github.com/user-attachments/assets/3c5f7a7c-eea4-49db-be1d-f6bb376e0129" />

Ejecutamos docker ps y vemos como están los 4 contenedores levantados correctamente. El proxy tiene el puerto 80 y el puerto 443 de https.

<img width="814" height="145" alt="image" src="https://github.com/user-attachments/assets/e3bf81cb-ffae-439c-a3bc-f2978249f919" />

Con lo cual el principal problema era que el contenedor pumukit-proxy-1 estaba reiniciándose continuamente y la web no era accesible.

Los logs mostraban:

- nginx: [emerg] host not found in upstream "php" in /etc/nginx/conf.d/default.conf:30

La línea afectada era:

- fastcgi_pass php:9000;

Durante el despliegue se detectó una incidencia en el servicio reverse proxy (Nginx), que impedía el arranque correcto de la plataforma. Se realizó un proceso de troubleshooting analizando logs de Docker, conectividad entre contenedores y resolución DNS interna. Tras verificar la red Docker y la resolución del servicio PHP mediante herramientas de inspección (docker inspect, getent hosts, docker compose logs), el servicio proxy quedó operativo y fue posible continuar con la validación del entorno.
Intentamos acceder por vía web con la IP indicada. Nos da un error 502 del Gateway.

<img width="1277" height="155" alt="image" src="https://github.com/user-attachments/assets/09a5294c-4dd4-4804-a1d1-7865df87dd6b" />


Falta que php atienda las peticiones correctamente. Seguimos revisando el tema de logs del contenedor de php:

- docker logs pumukit-php-1 –tail 100

<img width="681" height="508" alt="image" src="https://github.com/user-attachments/assets/c444cc8f-797a-41a1-be57-032c47dec225" />

Comprobamos con ss –lntp los puertos de la máquina virtual que están en escucha.

<img width="680" height="139" alt="image" src="https://github.com/user-attachments/assets/df414ec4-e08c-4e89-8e7b-23595a7b1081" />

Hemos encontrado la causa exacta del 502 Bad Gateway:

/usr/local/etc/php-fpm.d/www.conf:listen = 127.0.0.1:9000

<img width="676" height="204" alt="image" src="https://github.com/user-attachments/assets/6d9339fd-b7bc-4728-880a-0b29e95f5709" />


PHP-FPM está escuchando únicamente en la interfaz local del contenedor (localhost).

Nginx está en otro contenedor y trata de conectar a php:9000, pero como PHP-FPM sólo escucha en 127.0.0.1, rechaza la conexión.

Comprobamos el contenido del contenedor PHP:

- cat /usr/local/etc/php-fpm.d/www.conf | grep listen

<img width="682" height="61" alt="image" src="https://github.com/user-attachments/assets/a0307565-ddbe-4f53-929b-97536b2171d7" />

<img width="496" height="178" alt="image" src="https://github.com/user-attachments/assets/98d82957-6bb7-41e2-93df-c9c9e3f1483c" />

www.conf contiene listen = 127.0.0.1:9000

docker.conf contiene listen = 9000

Comprobamos qué configuración está usando PHP-FPM:

Nos situamos dentro del contenedor PHP con docker exec -it pumukit-php-1 sh y ejecutamos:

- php-fpm -i | grep listen
- grep listen /usr/local/etc/php-fpm.d/www.conf

<img width="818" height="220" alt="image" src="https://github.com/user-attachments/assets/062e8683-7e44-4d6b-a15a-6768bffd851a" />

PHP-FPM está arrancado

PHP-FPM escucha en 0.0.0.0:9000

Comprobamos la configuración de red de los contenedores PHP y proxy:

<img width="681" height="523" alt="image" src="https://github.com/user-attachments/assets/8252f108-ac55-4456-a359-b8388dcc6d00" />

Accedemos con el dominio vía web y aparece lo siguiente:

<img width="678" height="316" alt="image" src="https://github.com/user-attachments/assets/d9d0c040-56b8-4a4d-9c85-d91937af7f77" />

Error relacionado con Symfony.
No tengo experiencia previa con Symfony. Conseguí desplegar la infraestructura Docker y hacer que la aplicación respondiera por web. El estado final obtenido fue un error HTTP 400 relacionado con Symfony.

8. Configuración de almacenamiento
Los vídeos son el elemento principal.
Montamos directorios desde la ruta del directorio opt:
- /opt/pumukit/data/media
- /opt/pumukit/data/db
- /opt/pumukit/data/search

## Resumen final + Qué faltó por completar

Se ha conseguido desplegar la infraestructura Docker completa de Pumukit, incluyendo los servicios MongoDB, Redis, PHP y Nginx. Durante el proceso se resolvieron varias incidencias relacionadas con dependencias Composer, construcción de imágenes Docker, comunicación entre contenedores y configuración del servicio proxy. Finalmente la aplicación quedó accesible vía web y alcanzó la fase de ejecución de Symfony. El estado final obtenido fue un error HTTP 400 relacionado con la gestión de sesión de la aplicación, indicando que la infraestructura y la comunicación entre servicios ya estaban operativas. Aunque no dispongo de experiencia previa con Symfony, se realizó un proceso completo de despliegue, análisis y resolución de incidencias siguiendo buenas prácticas de administración de sistemas.

