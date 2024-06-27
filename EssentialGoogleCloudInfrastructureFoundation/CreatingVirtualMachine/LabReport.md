### Memoria del Laboratorio: Google Cloud Platform

[Creación de Máquinas Virtuales](https://www.cloudskillsboost.google/paths/499/course_templates/50/labs/485532)


## Introducción
En este laboratorio se explora diversas opciones para crear y configurar instancias de Máquinas Virtuales (VM) utilizando Google Cloud Platform (GCP). El objetivo fue comprender los diferentes tipos de VM disponibles y sus respectivas configuraciones.

## Objetivos
El laboratorio consistió en varias tareas:
1. Crear una máquina virtual de utilidad.
2. Crear una máquina virtual con Windows.
3. Crear una máquina virtual personalizada.

Cada tarea implicó la configuración de VM con parámetros específicos como sistemas operativos, tipos de máquinas y opciones de redes.

#### Tarea 1: Crear una Máquina Virtual de Utilidad
**Pasos Realizados:**
- Se creó una instancia de VM con un nombre según las especificaciones.
- Se seleccionó la configuración de máquina serie E2 con diferentes tipos de máquinas para observar las diferencias de costos.
- Se configuró el disco de arranque según las especificaciones del Lab.
- Se configuraron opciones de redes, asegurando que no hubiera dirección IPv4 externa para ahorrar costos.
- Se exploraron los detalles de la VM, incluida la plataforma de CPU y las políticas de disponibilidad.

**Observaciones:**
- Se destacó la importancia de seleccionar tipos de máquinas adecuados según los requisitos de carga de trabajo y las implicaciones de costos.
- Se exploraron las políticas de disponibilidad que afectan el comportamiento de la VM durante el mantenimiento y las interrupciones.

#### Tarea 2: Crear una Máquina Virtual con Windows
**Pasos Realizados:**
- Se creó una instancia de VM de Windows utilizando la serie E2 y un tipo de máquina especificado.
- Se configuró el disco de arranque con Windows Server 2016 Datacenter Core.
- Se habilitó el tráfico HTTP y HTTPS mediante reglas de firewall.
- Se estableció una contraseña para el acceso RDP, aunque la conexión real no se realizó en este laboratorio.

**Observaciones:**
- Se resaltó la diferencia en el método de conexión (RDP) en comparación con SSH para las VM de Windows.
- Se señaló el requisito de un cliente RDP en las máquinas locales para conectar a los escritorios de las VM de Windows.

#### Tarea 3: Crear una Máquina Virtual Personalizada
**Pasos Realizados:**
- Se creó una instancia de VM personalizada con configuraciones específicas, incluidos núcleos (vCPUs) y memoria.
- Se configuró el disco de arranque con Debian GNU/Linux 10 (buster).

**Observaciones:**
- Se exploraron comandos para obtener información del sistema (por ejemplo, uso de memoria, detalles de CPU) a través de la terminal SSH.
- Se demostró la flexibilidad de crear VM adaptadas a necesidades específicas de recursos.

#### Tarea 4: Revisión
En este laboratorio, se crearon y configuraron con éxito varias instancias de VM con características diversas:
- Una VM de utilidad para tareas administrativas básicas.
- Una VM de Windows configurada para acceso RDP.
- Una VM personalizada adaptada a necesidades específicas de recursos.

#### Conclusión
Este laboratorio proporcionó una valiosa experiencia práctica en la navegación de la Consola de Google Cloud para crear y gestionar instancias de máquinas virtuales. Comprender las diferentes configuraciones y capacidades de las VM es crucial para optimizar el uso de recursos y satisfacer eficientemente los requisitos de las aplicaciones.

---

Este informe resume las actividades clave y los conocimientos adquiridos en laboratorios destacando el proceso y los resultados de cada tarea en Google Cloud Platform en la ruta: [Cloud Engineer Learning Path](https://www.cloudskillsboost.google/paths/11)