# Accessing the Google Cloud Console and Cloud Shell

Run the below commands in the cloud shell terminal:

```
gcloud auth list
gcloud config list project 
export USERNAME=
export REGION=
export ZONE=
export BUCKET_NAME=
export BUCKET_NAME_2=
```

## Task 1: Explore the Google Cloud Console

```
gcloud storage buckets create gs://$BUCKET_NAME --location=$REGION
gsutil uniformbucketlevelaccess set off gs://$BUCKET_NAME
gcloud compute instances create first-vm --zone=$ZONE --machine-type=e2-micro --tags=http-server
gcloud iam service-accounts create test-service-account --display-name="test-service-account"
gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member serviceAccount:test-service-account@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com --role roles/editor
```


## Task 2: Explore Cloud Shell

```
gcloud storage buckets create gs://$BUCKET_NAME_2 --location=$REGION
gcloud compute zones list | grep $REGION
gcloud config set compute/zone $ZONE
MY_VMNAME=second-vm
gcloud compute instances create $MY_VMNAME \
--machine-type "e2-standard-2" \
--image-project "debian-cloud" \
--image-family "debian-11" \
--subnet "default"
gcloud iam service-accounts create test-service-account2 --display-name "test-service-account2"
gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member serviceAccount:test-service-account2@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com --role roles/viewer
```

## Task 3: Work with Cloud Storage in Cloud Shell

```
gsutil cp gs://cloud-training/ak8s/cat.jpg cat.jpg
gsutil cp cat.jpg gs://$BUCKET_NAME
gsutil cp gs://$BUCKET_NAME/cat.jpg gs://$BUCKET_NAME_2/cat.jpg
gsutil acl get gs://$BUCKET_NAME/cat.jpg  > acl.txt
cat acl.txt
gsutil acl set private gs://$BUCKET_NAME/cat.jpg
gsutil acl get gs://$BUCKET_NAME/cat.jpg  > acl-2.txt
cat acl-2.txt
gcloud auth activate-service-account --key-file credentials.json
gsutil cp gs://$BUCKET_NAME/cat.jpg ./cat-copy.jpg
gsutil cp gs://$BUCKET_NAME_2/cat.jpg ./cat-copy.jpg
gcloud config set account $USERNAME
gsutil cp gs://$BUCKET_NAME/cat.jpg ./copy2-of-cat.jpg
gsutil iam ch allUsers:objectViewer gs://$BUCKET_NAME
```

## Task 4: Explore the Cloud Shell Editor

```
git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git

mkdir test
```
```
echo Finished cleanup! >> orchestrate-with-kubernetes/cleanup.sh

cd orchestrate-with-kubernetes
cat cleanup.sh
```

Crear el archivo index.html y pegar el contenido HTML con la URL de la imagen del gato reemplazada:

```
<html><head><title>Cat</title></head>
<body>
<h1>Cat</h1>
<img src="https://storage.googleapis.com/qwiklabs-Google Cloud-1aeffbc5d0acb416/cat.jpg">
</body></html>
```
```
sudo apt-get remove -y --purge man-db
sudo touch /var/lib/man-db/auto-update
sudo apt-get update
sudo apt-get install nginx

gcloud compute scp index.html first-vm:index.nginx-debian.html --zone=europe-west1-c

sudo cp index.nginx-debian.html /var/www/html
```

--

Este informe resume las actividades clave y los conocimientos adquiridos en laboratorios destacando el proceso y los resultados de cada tarea en Google Cloud Platform en la ruta: [Cloud Engineer Learning Path](https://www.cloudskillsboost.google/paths/11)