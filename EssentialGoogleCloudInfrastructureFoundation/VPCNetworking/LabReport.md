# Reporte de Memoria

[VPC Networking](https://www.cloudskillsboost.google/paths/499/course_templates/50/labs/485519)


## Introducción

Google Cloud Virtual Private Cloud (VPC) proporciona funcionalidades de red para instancias de máquinas virtuales (VM) en Compute Engine, contenedores en Kubernetes Engine y el entorno flexible de App Engine. Sin una red VPC, no se pueden crear instancias de VM, contenedores o aplicaciones en App Engine. Por lo tanto, cada proyecto de Google Cloud tiene una red predeterminada para comenzar.

Una red VPC se puede comparar con una red física, excepto que está virtualizada dentro de Google Cloud. Una red VPC es un recurso global que consiste en una lista de subredes virtuales regionales (subnets) en centros de datos, todos conectados por una red de área amplia (WAN) global. Las redes VPC están lógicamente aisladas entre sí en Google Cloud.

En este laboratorio, se crea una red VPC en modo automático con reglas de firewall y dos instancias de VM. Luego, se convierte la red en modo automático a una red en modo personalizado y se crean otras redes en modo personalizado según el diagrama de arquitectura de red proporcionado. También se prueba la conectividad entre las redes.

## Objetivos

En este laboratorio, se deben realizar las siguientes tareas:

1. Explorar la red VPC predeterminada.
2. Crear una red en modo automático con reglas de firewall.
3. Convertir una red en modo automático a una red en modo personalizado.
4. Crear redes VPC en modo personalizado con reglas de firewall.
5. Crear instancias de VM utilizando Compute Engine.
6. Explorar la conectividad para instancias de VM entre redes VPC.

### Exploración de la Red Predeterminada

Cada proyecto de Google Cloud tiene una red predeterminada con subredes, rutas y reglas de firewall.

### Visualización de Subredes

La red predeterminada tiene una subred en cada región de Google Cloud. En la consola de Google Cloud, en el menú de navegación, haga clic en `VPC network > VPC networks`.


### Visualización de Rutas

- En el panel izquierdo, haga clic en `Routes`.
- Observe que hay una ruta para cada subred y una para el gateway de internet predeterminado (0.0.0.0/0).

### Visualización de Reglas de Firewall

Cada red VPC implementa un firewall virtual distribuido que puede configurar.

- En el panel izquierdo, haga clic en `Firewall`.
- Observe que hay 4 reglas de firewall de entrada para la red predeterminada:
  - `default-allow-icmp`
  - `default-allow-rdp`
  - `default-allow-ssh`
  - `default-allow-internal`

### Eliminación de la Red Predeterminada

- En el panel izquierdo, haga clic en `Firewall policies`.
- Selecciona todas las reglas de firewall de la red predeterminada y haga clic en `Delete`.
- En el menú de navegación, haga clic en `VPC network > VPC networks`, seleccione la red predeterminada y haga clic en `Delete VPC network`.

### Verificación de Eliminación

- En el panel izquierdo, haga clic en `Routes` y `Firewall`. No debería haber rutas ni reglas de firewall.

### Intento de Crear una VM sin Red VPC

- En el menú de navegación, haga clic en `Compute Engine > VM instances`.
- Haga clic en `Create Instance`. Debería aparecer un error indicando que no hay más redes disponibles.

### Creación de una Red en Modo Automático

Se crea una red en modo automático con dos instancias de VM.

### Creación de la Red

- En el menú de navegación, haga clic en `VPC network > VPC networks`.
- Haga clic en `Create VPC network`, nombre la red `mynetwork`, seleccione `Automatic` para el modo de creación de subred y todas las reglas de firewall disponibles. Luego, haga clic en `Create`.

### Creación de Instancias de VM

#### En la Región 1

- En el menú de navegación, haga clic en `Compute Engine > VM instances`.
- Haga clic en `Create Instance`, especifique los detalles y haga clic en `Create`.

#### En la Región 2

- Haga clic en `Create instance`, especifique los detalles y haga clic en `Create`.

### Verificación de Conectividad

- En el menú de navegación, haga clic en `Compute Engine > VM instances`.
- Conecte mediante SSH a `mynet-us-vm`.
- Pruebe la conectividad interna y externa utilizando `ping`.

### Conversión de la Red a Modo Personalizado

Se convierte la red en modo automático a una red en modo personalizado para evitar la creación automática de nuevas subredes.

- En el menú de navegación, haga clic en `VPC network > VPC networks`.
- Haga clic en `mynetwork`, luego en `Edit` y seleccione `Custom` para el modo de creación de subred. Finalmente, haga clic en `Save`.

### Creación de Redes en Modo Personalizado

Se crean dos redes adicionales en modo personalizado, `managementnet` y `privatenet`, junto con reglas de firewall y VM.

### Creación de la Red `managementnet`

- En la consola de Google Cloud, en el menú de navegación, haga clic en `VPC network > VPC networks`.
- Haga clic en `Create VPC Network`, nombre la red `managementnet`, seleccione `Custom` para el modo de creación de subred y configure los detalles de la subred. Luego, haga clic en `Create`.

### Creación de la Red `privatenet` mediante `gcloud`

- Active Cloud Shell y ejecute los comandos necesarios para crear la red y las subredes.

### Configuración de Reglas de Firewall

#### Para `managementnet`

- Cree reglas de firewall en la consola de Google Cloud para permitir tráfico ICMP, SSH y RDP.

#### Para `privatenet` mediante `gcloud`

- Utilice Cloud Shell para crear las reglas de firewall necesarias.

### Creación de Instancias de VM

#### `managementnet-us-vm`

- En la consola de Google Cloud, en el menú de navegación, haga clic en `Compute Engine > VM instances`.
- Cree la instancia con los detalles especificados.

#### `privatenet-us-vm` mediante `gcloud`

- Utilice Cloud Shell para crear la instancia de VM con los comandos necesarios.

## Exploración de la Conectividad entre Redes

Se explora la conectividad entre las instancias de VM para determinar el efecto de tener instancias en la misma zona versus en la misma red VPC.

### Ping a Direcciones IP Externas

- Se prueba la conectividad mediante `ping` a las direcciones IP externas de las instancias de VM.

### Ping a Direcciones IP Internas

- Se prueba la conectividad mediante `ping` a las direcciones IP internas de las instancias de VM.

## Conclusión

En este laboratorio, se exploró la red predeterminada y se determinó que no se pueden crear instancias de VM sin una red VPC. Luego, se creó una nueva red VPC en modo automático con subredes, rutas, reglas de firewall y dos instancias de VM, y se probó la conectividad de las instancias de VM. Dado que las redes en modo automático no se recomiendan para producción, se convirtió la red en modo automático a una red en modo personalizado.

A continuación, se crearon dos redes VPC adicionales en modo personalizado con reglas de firewall e instancias de VM utilizando la consola de Google Cloud y la línea de comandos `gcloud`. Luego se probó la conectividad entre redes VPC, lo que funcionó al hacer ping a direcciones IP externas pero no al hacer ping a direcciones IP internas.

Las redes VPC están aisladas lógicamente por defecto. Por lo tanto, no se permite la comunicación de direcciones IP internas entre redes a menos que se configuren mecanismos como el emparejamiento de VPC o VPN.

--

Este informe resume las actividades clave y los conocimientos adquiridos en laboratorios destacando el proceso y los resultados de cada tarea en Google Cloud Platform en la ruta: [Cloud Engineer Learning Path](https://www.cloudskillsboost.google/paths/11)