# Reporte de Laboratorio

## Implementación de Private Google Access y Cloud NAT en Google Cloud

## Introducción

En este laboratorio, se implementa el acceso privado a Google y Cloud NAT para una instancia de VM que no tiene una dirección IP externa. Posteriormente, se verifica el acceso a direcciones IP públicas de APIs y servicios de Google y otras conexiones a internet. Las instancias de VM sin direcciones IP externas están aisladas de las redes externas. Usando Cloud NAT, estas instancias pueden acceder a internet para actualizaciones y parches, y en algunos casos, para el inicio del sistema. Como un servicio gestionado, Cloud NAT proporciona alta disponibilidad sin necesidad de gestión e intervención por parte del usuario.

## Objetivos

En este laboratorio, aprenderemos a realizar las siguientes tareas:

1. Configurar una instancia de VM sin dirección IP externa.
2. Conectar a una instancia de VM usando un túnel de Proxy con Consciencia de Identidad (IAP).
3. Habilitar el Acceso Privado a Google en una subred.
4. Configurar una puerta de enlace de Cloud NAT.
5. Verificar el acceso a direcciones IP públicas de APIs y servicios de Google y otras conexiones a internet.

### Pasos Preliminares

1. Iniciar sesión en Qwiklabs usando una ventana de incógnito.
2. Notar el tiempo de acceso del laboratorio (por ejemplo, 1:15:00) y asegurarse de que se pueda finalizar dentro de ese tiempo.
3. No hay función de pausa. Se puede reiniciar si es necesario, pero se debe comenzar desde el principio.
4. Cuando esté listo, hacer clic en "Start lab".
5. Anotar las credenciales del laboratorio (nombre de usuario y contraseña). Se utilizarán para iniciar sesión en la consola de Google Cloud.
6. Hacer clic en "Open Google Console".
7. Hacer clic en "Use another account" y copiar/pegar las credenciales para este laboratorio en los campos solicitados.
8. Aceptar los términos y omitir la página de recursos de recuperación.
9. No hacer clic en "End Lab" a menos que se haya terminado el laboratorio o se quiera reiniciarlo. Esto borrará el trabajo realizado y eliminará el proyecto.

## Tarea 1: Crear la Instancia de VM

Crear una red VPC con algunas reglas de firewall y una instancia de VM que no tenga una dirección IP externa, y conectar a la instancia utilizando un túnel IAP.

#### Crear una Red VPC y Reglas de Firewall

1. En la consola de Google Cloud, ir a `VPC network > VPC networks`.
2. Hacer clic en "Create VPC network".
3. Configurar los siguientes detalles:
   - Nombre de la red: `my-vpc`
   - Subnet creation mode: `Custom`
4. Hacer clic en "Create".

#### Crear Reglas de Firewall

1. En el menú de navegación, ir a `VPC network > Firewall`.
2. Hacer clic en "Create firewall rule".
3. Configurar los detalles de la regla:
   - Name: `allow-icmp`
   - Network: `my-vpc`
   - Targets: `All instances in the network`
   - Source IP ranges: `0.0.0.0/0`
   - Allowed protocols and ports: `icmp`
4. Hacer clic en "Create".

Repetir el proceso para crear las siguientes reglas de firewall:
   - `allow-ssh`: Permitirá tráfico SSH.
   - `allow-internal`: Permitirá tráfico interno.

### Crear la Instancia de VM sin Dirección IP Externa

1. En el menú de navegación, ir a `Compute Engine > VM instances`.
2. Hacer clic en "Create instance".
3. Configurar los siguientes detalles:
   - Name: `my-vm`
   - Region: Seleccionar una región disponible
   - Zone: Seleccionar una zona disponible
   - Machine type: `n1-standard-1`
   - Networking, disks, security, management, sole tenancy:
     - Networking
       - Network: `my-vpc`
       - Subnetwork: `default`
       - External IP: `None`
4. Hacer clic en "Create".

### Conectar a la Instancia Usando un Túnel IAP

1. En el menú de navegación, ir a `Compute Engine > VM instances`.
2. Hacer clic en `SSH > Connect using IAP`.
3. Aceptar el aviso de permiso y continuar.
4. Una vez conectado, ejecutar un comando de prueba como `ping google.com` para verificar la conectividad externa (debe fallar, ya que la instancia no tiene IP externa).

## Tarea 2: Habilitar Private Google Access

### Habilitar Private Google Access en una Subred

1. En el menú de navegación, ir a `VPC network > VPC networks`.
2. Seleccionar la red `my-vpc`.
3. Hacer clic en `Subnets`.
4. Seleccionar la subred `default`.
5. Hacer clic en `Edit`.
6. Marcar la opción `Enable Private Google Access`.
7. Hacer clic en `Save`.

### Verificar Private Google Access

1. Conectar a la instancia de VM usando IAP.
2. Ejecutar `ping metadata.google.internal` para verificar el acceso a los servicios de Google.

## Tarea 3: Configurar una Puerta de Enlace Cloud NAT

### Crear una Puerta de Enlace de Cloud NAT

1. En el menú de navegación, ir a `VPC network > NAT`.
2. Hacer clic en `Get started` o `Create NAT gateway`.
3. Configurar los siguientes detalles:
   - Name: `my-nat-gateway`
   - VPC network: `my-vpc`
   - Region: Seleccionar la región de la subred
   - NAT mapping mode: `Automatic`
   - Subnetworks: Seleccionar `default`
4. Hacer clic en `Create`.

### Verificar la Configuración de Cloud NAT

1. Conectar a la instancia de VM usando IAP.
2. Ejecutar `ping google.com` para verificar la conectividad externa (debe funcionar ahora).

### Verificar el Acceso a Direcciones IP Públicas de APIs y Servicios de Google

1. Conectar a la instancia de VM usando IAP.
2. Ejecutar `curl https://www.googleapis.com/discovery/v1/apis` para verificar el acceso a los servicios de Google.

### Conclusión

En este laboratorio, se ha configurado una instancia de VM sin dirección IP externa y se ha habilitado el acceso privado a Google y Cloud NAT. Esto ha permitido que la instancia acceda a internet para actualizaciones y parches, y en algunos casos, para el inicio del sistema. Se ha verificado el acceso a direcciones IP públicas de APIs y servicios de Google y otras conexiones a internet.

### Reflexiones

1. **Importancia de Cloud NAT**: Cloud NAT es crucial para garantizar que las instancias de VM sin direcciones IP externas puedan acceder a los recursos necesarios para mantenerse actualizadas y operativas.
2. **Acceso Privado a Google**: Habilitar Private Google Access asegura que las instancias de VM pueden acceder a los servicios de Google de manera segura y eficiente.
3. **Seguridad**: La configuración sin IP externa y el uso de IAP proporcionan una capa adicional de seguridad al reducir la superficie de ataque.

En resumen, este laboratorio ha demostrado cómo utilizar herramientas y servicios de Google Cloud para gestionar y asegurar el acceso a internet para instancias de VM sin comprometer la seguridad ni la disponibilidad.

--

Este informe resume las actividades clave y los conocimientos adquiridos en laboratorios destacando el proceso y los resultados de cada tarea en Google Cloud Platform en la ruta: [Cloud Engineer Learning Path](https://www.cloudskillsboost.google/paths/11)