# Informe de Laboratorio: 
[Trabajando con Máquinas Virtuales](https://www.cloudskillsboost.google/paths/499/course_templates/50/labs/485541)

## Introducción

En este laboratorio, configura un servidor de Minecraft en una instancia de Compute Engine. Por ejemplo, utiliza una máquina e2-medium con un disco de arranque de 10 GB, 2 CPU virtuales y 4 GB de RAM, ejecutando Debian Linux. Adicionalmente, adjunta una unidad SSD de 50 GB para asegurar suficiente espacio para los datos del juego. Este servidor de Minecraft puede soportar hasta 50 jugadores simultáneamente.

## Objetivos

Durante el laboratorio, se hacen las siguientes tareas:

1. Personalizar un servidor de aplicaciones.
2. Instalar y configurar el software necesario.
3. Configurar el acceso a la red.
4. Programar copias de seguridad regulares.

### Configuración de Qwiklabs

Para cada laboratorio, se usa una ventana de incógnito para iniciar sesión en Qwiklabs, finaliza el lab en el tiempo marcado.

### Creación de la Máquina Virtual

Define una VM utilizando opciones avanzadas en la Consola de Cloud. Creando una instancia con el nombre "mc-server", especifica región, zona y configura un disco de arranque con Debian GNU/Linux. Adjunta un nuevo disco SSD persistente de 50 GB para el almacenamiento de datos del juego y configura la red para incluir una IP externa estática llamada "mc-server-ip".

### Preparación del Disco de Datos

Crea un directorio y formate el disco adjunto para luego montarlo en la instancia. 

### Instalación y Ejecución de la Aplicación

Instala el Entorno de Ejecución de Java (JRE) en su versión sin cabeza, optimizada para reducir el uso de recursos. Luego, descarga e instala el archivo JAR del servidor de Minecraft y acepta los términos del EULA para que el servidor pudiera funcionar correctamente. Utiliza la herramienta `screen` para mantener el servidor de Minecraft ejecutándose continuamente, incluso después de cerrar la sesión SSH.

### Permitir Tráfico de Clientes

Configura una regla de firewall en la Consola de Cloud para permitir las conexiones en el puerto TCP 25565, que es el puerto predeterminado del servidor de Minecraft. Esto aseguró que los jugadores pudieran acceder al servidor desde sus clientes de Minecraft.

### Programación de Copias de Seguridad Regulares

Para asegurar la integridad de los datos del juego, crea un script de copia de seguridad que guarda los datos del mundo del servidor en un bucket de Cloud Storage. Este script se ejecuta automáticamente cada 4 horas utilizando cron, lo que garantiza que las copias de seguridad se realicen de manera regular sin intervención manual.

### Mantenimiento del Servidor

Realiza mantenimiento en el servidor deteniendo el servidor de Minecraft y apagando la VM. Automatiza este proceso utilizando scripts de inicio y apagado almacenados en Cloud Storage. Estos scripts montan el disco de Minecraft y inician el servidor en una sesión de `screen` al arrancar la VM, y detienen el servidor de Minecraft antes de apagar la VM.

### Revisión

En este laboratorio, configuras una máquina virtual personalizada instalando y configurando un servidor de Minecraft. Adjunta y prepara un disco SSD de alto rendimiento y reserva una IP externa estática. Verifica la disponibilidad del servidor de juego en línea, configura un sistema de copias de seguridad automático, y automatiza el mantenimiento del servidor mediante scripts de inicio y apagado. Este laboratorio nos proporcionó una experiencia completa en la configuración y gestión de un servidor de aplicaciones en la nube.

--

Este informe resume las actividades clave y los conocimientos adquiridos en el laboratorio sobre la creación de máquinas virtuales, destacando el proceso y los resultados de cada tarea en Google Cloud Platform en la ruta: [Cloud Engineer Learning Path](https://www.cloudskillsboost.google/paths/11)