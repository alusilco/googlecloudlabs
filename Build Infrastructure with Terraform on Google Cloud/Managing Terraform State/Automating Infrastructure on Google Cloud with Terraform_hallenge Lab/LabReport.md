Configuración inicial: Crear archivos de configuración
Crea los archivos y directorios vacíos en Cloud Shell o en el editor de Cloud Shell.

## Configuración inicial: Crear archivos de configuración

Crea los archivos y directorios vacíos en Cloud Shell o en el editor de Cloud Shell.

```bash
touch main.tf
touch variables.tf
mkdir modules
cd modules
mkdir instances
cd instances
touch instances.tf
touch outputs.tf
touch variables.tf
cd ..
mkdir storage
cd storage
touch storage.tf
touch outputs.tf
touch variables.tf
cd
```
Agrega lo siguiente a cada archivo variables.tf y completa con el ID del Proyecto de GCP:

```sh
variable "region" {
 default = "us-central1"
}

variable "zone" {
 default = "us-central1-a"
}

variable "project_id" {
 default = "<COMPLETA CON EL ID DEL PROYECTO>"
}
Agrega lo siguiente al archivo main.tf:
```
Agrega lo siguiente al archivo main.tf:
```sh
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "3.55.0"
    }
  }
}

provider "google" {
  project     = var.project_id
  region      = var.region
  zone        = var.zone
}

module "instances" {
  source     = "./modules/instances"
}
```

Ejecuta "terraform init" en Cloud Shell en el directorio raíz para inicializar Terraform.

```sh
terraform init
```


Para importar recursos existentes, como las instancias de Compute Engine, sigue los siguientes pasos:

Accede a Compute Engine > Instancias de VM en la consola de GCP.
Copia el ID de las instancias tf-instance-1 y tf-instance-2.
Navega al archivo modules/instances/instances.tf y copia la configuración proporcionada.
Usa el siguiente comando para importar la primera instancia
```sh
resource "google_compute_instance" "tf-instance-1" {
  name         = "tf-instance-1"
  machine_type = "n1-standard-1"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "default"
  }

  metadata_startup_script = <<-EOT
#!/bin/bash
EOT

  allow_stopping_for_update = true
}

resource "google_compute_instance" "tf-instance-2" {
  name         = "tf-instance-2"
  machine_type = "n1-standard-1"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "default"
  }

  metadata_startup_script = <<-EOT
#!/bin/bash
EOT

  allow_stopping_for_update = true
}
```

Para importar la primera instancia, usa el siguiente comando:
```sh
terraform import module.instances.google_compute_instance.tf-instance-1 <ID_DE_INSTANCIA_1>
```

TAREA 2: Configurar un backend remoto
Agrega el siguiente código al archivo modules/storage/storage.tf y completa con el nombre del bucket:
```sh
resource "google_storage_bucket" "storage-bucket" {
  name          = "<COMPLETA CON EL NOMBRE DEL BUCKET>"
  location      = "US"
  force_destroy = true
  uniform_bucket_level_access = true
}
```

Agrega lo siguiente al archivo main.tf:
```sh
module "storage" {
  source     = "./modules/storage"
}
```
Ejecuta los siguientes comandos para inicializar el módulo y crear el recurso del bucket de almacenamiento:
```sh
terraform init
terraform apply
```
Actualiza el bloque terraform en el archivo main.tf para que se vea así, completando con tu ID de Proyecto de GCP para el argumento de bucket:
```sh
terraform {
  backend "gcs" {
    bucket  = "<COMPLETA CON EL ID DEL PROYECTO>"
    prefix  = "terraform/state"
  }
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "3.55.0"
    }
  }
}
```
```sh
terraform init
```

TAREA 3: Modificar y actualizar la infraestructura
Accede a modules/instances/instances.tf. Reemplaza todo el contenido con lo siguiente, completando con el ID de tu Instancia 3:
```sh
resource "google_compute_instance" "tf-instance-1" {
  name         = "tf-instance-1"
  machine_type = "n1-standard-2"
  zone         = "us-central1-a"
  allow_stopping_for_update = true

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "default"
  }
}

resource "google_compute_instance" "tf-instance-2" {
  name         = "tf-instance-2"
  machine_type = "n1-standard-2"
  zone         = "us-central1-a"
  allow_stopping_for_update = true

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "default"
  }
}

resource "google_compute_instance" "<COMPLETA CON EL NOMBRE DE INSTANCIA 3>" {
  name         = "<COMPLETA CON EL NOMBRE DE INSTANCIA 3>"
  machine_type = "n1-standard-2"
  zone         = "us-central1-a"
  allow_stopping_for_update = true

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "default"
  }
}
```


```sh
terraform init
terraform apply
```

TAREA 4: Marcar y eliminar recursos
Marca el recurso tf-instance-3 con el siguiente comando, completando con el ID de tu Instancia 3:
```sh
terraform taint module.instances.google_compute_instance.<COMPLETA CON EL NOMBRE DE INSTANCIA 3>
```
```sh
terraform init
terraform apply
```
Elimina el recurso tf-instance-3 del archivo instances.tf. Elimina el siguiente fragmento de código:
```sh
resource "google_compute_instance" "<COMPLETA CON EL NOMBRE DE INSTANCIA 3>" {
  name         = "<COMPLETA CON EL NOMBRE DE INSTANCIA 3>"
  machine_type = "n1-standard-2"
  zone         = "us-central1-a"
  allow_stopping_for_update = true

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-10"
    }
  }

  network_interface {
    network = "default"
  }
}
```

```sh
terraform apply
```

TAREA 5: Usar un módulo del Registro
Copia y pega lo siguiente al final del archivo main.tf, completando con el Número de Versión y el Nombre de Red indicados en el desafío:


```sh
module "vpc" {
    source  = "terraform-google-modules/network/google"
    version = "~> <COMPLETA CON EL NÚMERO DE VERSIÓN>"

    project_id   = "qwiklabs-gcp-04-f2c1c01a09d3"
    network_name = "<COMPLETA CON EL NOMBRE DE LA RED>"
    routing_mode = "GLOBAL"

    subnets = [
        {
            subnet_name           = "subnet-01"
            subnet_ip             = "10.10.10.0/24"
            subnet_region         = "us-central1"
        },
        {
            subnet_name           = "subnet-02"
            subnet_ip             = "10.10.20.0/24"
            subnet_region         = "us-central1"
            subnet_private_access = "true"
            subnet_flow_logs      = "true"
            description           = "Esta subred tiene una descripción"
        }
    ]
}
```

Ejecuta los siguientes comandos para inicializar el módulo y crear la VPC:

```sh
terraform init
terraform apply
```

TAREA 6: Configurar un firewall
Agrega el siguiente recurso al archivo main.tf, completando con el ID de Proyecto de GCP y el Nombre de Red:
```sh
resource "google_compute_firewall" "tf-firewall" {
  name    = "tf-firewall"
  network = "projects/<COMPLETA CON EL ID DEL PROYECTO>/global/networks/<COMPLETA CON EL NOMBRE DE LA RED>"

  allow {
    protocol = "tcp"
    ports    = ["80"]
  }

  source_tags = ["web"]
  source_ranges = ["0.0.0.0/0"]
}
```
