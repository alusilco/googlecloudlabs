# Informe: Implementación de un LAMP Stack utilizando Google Cloud Marketplace

Este laboratorio describe el proceso para desplegar rápidamente un stack LAMP (Linux, Apache, MySQL, PHP) en una instancia de Compute Engine usando Google Cloud Marketplace. El stack LAMP proporcionado por Bitnami ofrece un entorno completo de desarrollo web para Linux que se puede lanzar con un solo clic. A continuación, se detalla cada uno de los pasos necesarios para completar el laboratorio.

## Objetivos del Laboratorio

El objetivo principal es aprender a lanzar una solución utilizando Google Cloud Marketplace.

## Tarea 1: Iniciar sesión en Google Cloud Console

1. **Acceder a Qwiklabs**: Utiliza una ventana de incógnito para iniciar sesión en Qwiklabs.
2. **Tiempo del laboratorio**: Asegúrate de completar el laboratorio dentro del tiempo asignado (por ejemplo, 1:15:00). No hay opción de pausa; puedes reiniciar si es necesario, pero deberás comenzar desde el inicio.
3. **Credenciales del laboratorio**: Anota las credenciales del laboratorio (nombre de usuario y contraseña) y utilízalas para iniciar sesión en Google Cloud Console.
4. **Abrir Google Console**: Haz clic en "Open Google Console", utiliza otra cuenta y copia/pega las credenciales del laboratorio.
5. **Aceptar términos**: Acepta los términos y omite la página de recursos de recuperación.

## Tarea 2: Desplegar un LAMP Stack usando Cloud Marketplace

1. **Navegar al Marketplace**: En Google Cloud Console, en el menú de navegación, haz clic en "Marketplace".
2. **Buscar LAMP**: En la barra de búsqueda, escribe "LAMP" y presiona ENTER.
3. **Seleccionar Bitnami LAMP Stack**: En los resultados de búsqueda, selecciona el paquete de Bitnami para LAMP.
4. **Iniciar Despliegue**: Haz clic en "GET STARTED" en la página del LAMP.
5. **Aceptar términos y acuerdos**: Marca la casilla de términos y acuerdos, y haz clic en "AGREE".
6. **Desplegar la solución**: En la ventana emergente "Successfully agreed to terms", haz clic en "DEPLOY". Si es la primera vez que usas Compute Engine, se inicializará la API de Compute Engine.
7. **Configurar despliegue**: Selecciona la zona de despliegue (por ejemplo, ZONE) y configura la máquina con la serie E2 y el tipo de máquina e2-medium. Deja los demás ajustes por defecto y haz clic en "Deploy".
8. **Estado del despliegue**: El estado del despliegue se mostrará en la ventana de la consola. Cuando se complete, aparecerá el mensaje "lampstack-1 has been deployed".
9. **Detalles del despliegue**: Una vez instalado el software, se mostrará un resumen de los detalles de la instancia, incluida la dirección del sitio.

## Tarea 3: Verificar el Despliegue

1. **Verificar el sitio**: Una vez completado el despliegue, haz clic en el enlace de la dirección del sitio en el panel derecho. Si el sitio no responde, espera 30 segundos y vuelve a intentarlo.
2. **Mensaje de confirmación**: Alternativamente, haz clic en "Visit the site" en la sección "Get started with Bitnami package for LAMP". Se abrirá una nueva pestaña del navegador mostrando un mensaje de felicitaciones, confirmando que el servidor Apache HTTP está funcionando.

## Finalizar el Laboratorio

1. **Terminar el laboratorio**: Cuando hayas completado el laboratorio, haz clic en "End Lab". Google Cloud Skills Boost eliminará los recursos que has utilizado y limpiará la cuenta.
2. **Calificar la experiencia**: Se te dará la oportunidad de calificar la experiencia del laboratorio y proporcionar comentarios.

## Conclusión

En este laboratorio, se aprendió a desplegar un stack LAMP en una instancia de Compute Engine utilizando Google Cloud Marketplace. Este proceso simplifica la configuración de un entorno de desarrollo web completo, demostrando la eficiencia y conveniencia de utilizar soluciones preconfiguradas en la nube.

Este informe resume las actividades clave y los conocimientos adquiridos en laboratorios destacando el proceso y los resultados de cada tarea en Google Cloud Platform en la ruta: [Cloud Engineer Learning Path](https://www.cloudskillsboost.google/paths/11)