# Informe: Introducción a VPC Networking y Google Compute Engine

## Resumen

Google Cloud Virtual Private Cloud (VPC) proporciona funcionalidades de red a instancias de máquinas virtuales (VM) de Compute Engine, contenedores de Kubernetes Engine y el entorno flexible de App Engine. Sin una red VPC, no se pueden crear instancias de VM, contenedores ni aplicaciones de App Engine. Por lo tanto, cada proyecto de Google Cloud tiene una red predeterminada para comenzar.

Una red VPC es similar a una red física, pero virtualizada dentro de Google Cloud. Una red VPC es un recurso global que consta de una lista de subredes virtuales regionales en centros de datos, todas conectadas por una red de área amplia (WAN) global. Las redes VPC están lógicamente aisladas entre sí en Google Cloud.

En este laboratorio, creas una red VPC en modo automático con reglas de firewall y dos instancias de VM. Luego, exploras la conectividad para las instancias de VM.

## Objetivos

En este laboratorio, aprenderás a realizar las siguientes tareas:

- Explorar la red VPC predeterminada
- Crear una red en modo automático con reglas de firewall
- Crear instancias de VM usando Compute Engine
- Explorar la conectividad para las instancias de VM

## Configuración y requisitos

Para cada laboratorio, obtienes un nuevo proyecto de Google Cloud y un conjunto de recursos por un tiempo fijo sin costo.

1. Inicia sesión en Qwiklabs usando una ventana de incógnito.
2. Nota el tiempo de acceso del laboratorio (por ejemplo, 1:15:00) y asegúrate de poder terminar dentro de ese tiempo.
3. Cuando estés listo, haz clic en "Start lab".
4. Anota tus credenciales del laboratorio (nombre de usuario y contraseña). Las usarás para iniciar sesión en Google Cloud Console.
5. Haz clic en "Open Google Console".
6. Haz clic en "Use another account" y copia/pega las credenciales del laboratorio.
7. Acepta los términos y omite la página de recursos de recuperación.

**Nota**: No hagas clic en "End Lab" a menos que hayas terminado el laboratorio o quieras reiniciarlo. Esto borrará tu trabajo y eliminará el proyecto.

## Tarea 1: Explorar la red predeterminada

Cada proyecto de Google Cloud tiene una red predeterminada con subredes, rutas y reglas de firewall.

### Ver las subredes

1. En Google Cloud Console, en el menú de navegación, haz clic en "VPC network" > "VPC networks".
2. Haz clic en "default".
3. Haz clic en "Subnets".

Nota la red predeterminada con sus subredes. Cada subred está asociada con una región de Google Cloud y un bloque CIDR RFC 1918 para su rango de direcciones IP internas y una puerta de enlace.

### Ver las rutas

Las rutas indican a las instancias de VM y a la red VPC cómo enviar tráfico desde una instancia a un destino, ya sea dentro de la red o fuera de Google Cloud. Cada red VPC viene con algunas rutas predeterminadas para enrutar tráfico entre sus subredes y enviar tráfico de instancias elegibles a Internet.

1. En el panel izquierdo, haz clic en "Routes".
2. En "Effective Routes", haz clic en "Network" y selecciona "default".
3. Haz clic en "Region" y selecciona la región asignada por Qwiklabs.
4. Haz clic en "View".

Nota que hay una ruta para cada subred. Estas rutas son gestionadas automáticamente, pero puedes crear rutas estáticas personalizadas para dirigir algunos paquetes a destinos específicos.

### Ver las reglas de Firewall

Cada red VPC implementa un firewall virtual distribuido que puedes configurar. Las reglas de firewall te permiten controlar qué paquetes están permitidos para viajar a qué destinos. Cada red VPC tiene dos reglas de firewall implícitas que bloquean todas las conexiones entrantes y permiten todas las conexiones salientes.

1. En el panel izquierdo, haz clic en "Firewall".

Nota que hay 4 reglas de firewall de entrada para la red predeterminada:
- default-allow-icmp
- default-allow-rdp
- default-allow-ssh
- default-allow-internal

Estas reglas de firewall permiten el tráfico de entrada ICMP, RDP y SSH desde cualquier lugar (0.0.0.0/0) y todo el tráfico TCP, UDP e ICMP dentro de la red (10.128.0.0/9).

### Eliminar las reglas de Firewall

1. Selecciona todas las reglas de firewall de la red predeterminada.
2. Haz clic en "Delete" y confirma la eliminación.

### Eliminar la red predeterminada

1. En el menú de navegación, haz clic en "VPC network" > "VPC networks".
2. Selecciona la red predeterminada.
3. Haz clic en "Delete VPC network" y confirma la eliminación.

Espera a que la red se elimine antes de continuar. Sin una red VPC, no hay rutas ni reglas de firewall.

### Intentar crear una instancia de VM

Verifica que no se puede crear una instancia de VM sin una red VPC.

1. En el menú de navegación, haz clic en "Compute Engine" > "VM instances".
2. Haz clic en "Create instance".
3. Acepta los valores predeterminados y haz clic en "Create".

Nota el error: no hay más redes y no hay red disponible.

## Tarea 2: Crear una red VPC y instancias de VM

### Crear una red VPC en modo automático con reglas de Firewall

1. En el menú de navegación, haz clic en "VPC network" > "VPC networks".
2. Haz clic en "Create VPC network".
3. Para "Name", escribe "mynetwork".
4. Para "Subnet creation mode", selecciona "Automatic".
5. Para "Firewall", selecciona todas las reglas disponibles.

Haz clic en "Create". Cuando la nueva red esté lista, nota que se creó una subred para cada región.

### Crear una instancia de VM en la Región 1

1. En el menú de navegación, haz clic en "Compute Engine" > "VM instances".
2. Haz clic en "Create instance".
3. Especifica los siguientes valores y deja los demás ajustes por defecto:
   - Name: mynet-us-vm
   - Region: Región 1
   - Zone: Zona 1
   - Series: E2
   - Machine type: e2-micro (2 vCPU, 1 GB memory)

Haz clic en "Create".

### Crear una instancia de VM en la Región 2

1. Haz clic en "Create instance".
2. Especifica los siguientes valores y deja los demás ajustes por defecto:
   - Name: mynet-eu-vm
   - Region: Región 2
   - Zone: Zona 2
   - Series: E2
   - Machine type: e2-micro (2 vCPU, 1 GB memory)

Haz clic en "Create".

Nota que las direcciones IP externas de ambas instancias de VM son efímeras.

## Tarea 3: Explorar la conectividad para las instancias de VM

### Verificar la conectividad para las instancias de VM

1. En el menú de navegación, haz clic en "Compute Engine" > "VM instances".
2. Nota las direcciones IP externas e internas de "mynet-eu-vm".

Para "mynet-us-vm", haz clic en "SSH" para lanzar una terminal y conectarte.

#### Probar la conectividad a la IP interna de "mynet-eu-vm"

1. En la terminal de "mynet-us-vm", ejecuta el siguiente comando, reemplazando la IP interna de "mynet-eu-vm":
