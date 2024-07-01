# Informe: Gestión del Estado de Terraform

## Descripción General
Terraform almacena el estado de la infraestructura y configuración gestionada en un archivo terraform.tfstate, que puede estar local o remotamente. Este estado es esencial para mapear recursos reales a la configuración, realizar seguimiento de metadatos y mejorar el rendimiento.

## Objetivos del Laboratorio
Aprender a:
- Crear un backend local.
- Crear un backend en Google Cloud Storage.
- Refrescar el estado de Terraform.
- Importar una configuración.
- Gestionar configuraciones importadas.

## Requisitos
- Navegador de internet (se recomienda Chrome).
- Ventana de incógnito para evitar conflictos de cuenta.
- No utilizar cuenta personal de Google Cloud.

## Instrucciones
1. **Iniciar el Laboratorio**
   Haz clic en "Start Lab" y sigue las instrucciones para iniciar sesión con las credenciales temporales proporcionadas.

2. **Activar Cloud Shell**
   Haz clic en "Activate Cloud Shell".
   Verifica que el proyecto está configurado con:
   ```sh
   gcloud config list project
   mkdir terraform_project
   cd terraform_project
    ```

Inicializa Terraform:
```sh
terraform init
```
Define una configuración en main.tf:
```sh
resource "google_storage_bucket" "example_bucket" {
  name     = "my-unique-bucket-name"
  location = "US"
}
```
Aplica la configuración:
```sh
terraform apply
```

Crear un Backend en Google Cloud Storage

Configura el backend en main.tf:
```sh
terraform {
  backend "gcs" {
    bucket = "my-terraform-state-bucket"
    prefix = "terraform/state"
  }
}
```

Inicializa el backend remoto:
```sh
terraform init
```

Refrescar el Estado de Terraform

Refresca el estado:
```sh
terraform refresh
```
Importar una Configuración de Terraform

Importa un recurso existente:
```sh
terraform import google_storage_bucket.example_bucket my-existing-bucket
```

Gestionar la Configuración Importada

Aplica cambios necesarios:
```sh
terraform apply
```

## Paso a Paso: Importación de Configuración en Terraform

En esta guía aprenderás cómo importar un contenedor Docker existente y su imagen dentro de un espacio de trabajo de Terraform.

### 1. Crear un Contenedor Docker

Primero, crea un contenedor Docker llamado `hashicorp-learn` utilizando la imagen más reciente de NGINX desde Docker Hub y publicándolo en el puerto 8080:

```sh
docker run --name hashicorp-learn --detach --publish 8080:80 nginx:latest
```
Verifica que el contenedor esté corriendo correctamente:
```sh
docker ps
```
Importar el Contenedor a Terraform
Clona el repositorio de ejemplo que contiene los archivos de configuración de Terraform:
```sh
git clone https://github.com/hashicorp/learn-terraform-import.git
cd learn-terraform-import
```

Inicializa tu espacio de trabajo de Terraform:
```sh
terraform init
```
En el archivo main.tf, asegúrate de comentar o eliminar el argumento host del proveedor Docker para evitar errores de inicialización:
```sh
provider "docker" {
  # host    = "npipe:////.//pipe//docker_engine"
}
```
En el archivo docker.tf, define un recurso docker_container vacío que represente tu contenedor Docker con el nombre hashicorp-learn:

```sh
resource "docker_container" "web" {}
```
Encuentra el ID completo del contenedor Docker que deseas importar ejecutando:

```sh
docker ps
```

Ejecuta el comando terraform import para asociar el contenedor Docker existente con el recurso docker_container.web en Terraform. Sustituye <CONTAINER-ID> con el ID del contenedor Docker:
```sh
terraform import docker_container.web <CONTAINER-ID>
```
Verifica que el contenedor se ha importado correctamente en el estado de Terraform:

```sh
terraform show
```

3. Crear la Configuración en Terraform
Ejecuta terraform plan para validar la configuración actual en docker.tf. Esto mostrará errores relacionados con los argumentos requeridos que faltan, como image y name.

Para actualizar la configuración en docker.tf según el estado importado, puedes copiar el estado actual en el archivo usando:

```sh
terraform show -no-color > docker.tf
```
Edita docker.tf para incluir solo los atributos necesarios como image, name y ports del contenedor Docker.

Ejecuta nuevamente terraform plan para verificar que los errores se han resuelto y que la configuración coincide con el estado esperado.

4. Aplicar y Gestionar el Contenedor con Terraform

Aplica los cambios para sincronizar tu configuración actualizada con el contenedor Docker:
```sh
terraform apply
```
Confirma los cambios escribiendo yes cuando se te solicite.

5. Crear un Recurso de Imagen Docker
En algunos casos, puedes gestionar recursos como imágenes de Docker directamente en Terraform sin necesidad de importarlos explícitamente. Agrega un recurso docker_image en docker.tf para representar la imagen nginx:latest:


```sh
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```
6. Gestionar el Contenedor y Aplicar Cambios
Actualiza la configuración del contenedor Docker en docker.tf para usar la nueva imagen:
```sh
resource "docker_container" "web" {
  image = docker_image.nginx.image_id
  name  = "hashicorp-learn"
  ports {
    external = 8080
    internal = 80
    ip       = "0.0.0.0"
    protocol = "tcp"
  }
}
```
Ejecuta terraform apply nuevamente para aplicar los cambios y asegúrate de que no se detecten cambios adicionales.
7. Gestión y Destrucción de la Infraestructura
Cuando ya no necesites la infraestructura, puedes destruirla completamente con Terraform:

```sh
terraform destroy
```
Confirma escribiendo yes cuando se te solicite. 
Verifica que el contenedor Docker ha sido eliminado:

```sh
docker ps --filter "name=hashicorp-learn"
```
Este proceso te permite integrar infraestructura existente en tu flujo de trabajo de Terraform, asegurando una gestión eficiente y consistente de recursos tanto locales como en la nube.