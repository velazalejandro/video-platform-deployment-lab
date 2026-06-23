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









## Qué faltó por completar
