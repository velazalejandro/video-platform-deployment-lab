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



## Qué faltó por completar
