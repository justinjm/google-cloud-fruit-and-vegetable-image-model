# Google Cloud Fruit and Vegtable Image Model

## Overview

This repository contains example code to train an image classification model using Google Cloud and Keras. 

Specifically, you will: 

1. Generate synthentic images of fruit and vegtables that are ripe or rotten
2. Save images in Google Cloud Storage 
3. Train a custom image classification model with Keras and perform inference on an example image 

Finally, see the `src` folder for an example notebook from the Kaggle dataset: [Fruit and Vegetable Disease (Healthy vs Rotten)](https://www.kaggle.com/datasets/muhammad0subhan/fruit-and-vegetable-disease-healthy-vs-rotten)

## Before you begin

### Set up your Google Cloud project

**The following steps are required, regardless of your notebook environment.**

1. [Select or create a Google Cloud project](https://console.cloud.google.com/cloud-resource-manager). When you first create an account, you get a $300 free credit towards your compute/storage costs. 
2. [Make sure that billing is enabled for your project](https://cloud.google.com/billing/docs/how-to/modify-project).
3. [Enable the Vertex AI, Arifact Registry and Cloud Build APIs](https://console.cloud.google.com/flows/enableapi?apiid=aiplatform.googleapis.com,cloudbuild.googleapis.com,artifactregistry.googleapis.com).
4. If you are running this notebook locally, you need to install the [Cloud SDK](https://cloud.google.com/sdk).

### Grant permissions to Default Compute Engine Service Account

The Workbench instance we will create should use the Default Compute Engine Service account.

Grant the necessary roles via GCP console or via gcloud commands below by opening a Cloud Shell session (click terminal logo in top right corner of the Google Cloud Console)

roles to grant: 

* roles/storage.objectAdmin
* roles/aiplatform.admin

#### gcloud

Run the following, copy/pasting one line at a time. Replace the `<your-project-id>` with your GCP Project ID found in the console.

```sh
PROJECT_ID="<your-project-id>" 

gcloud config set project $PROJECT_ID

gcloud projects describe $GOOGLE_CLOUD_PROJECT > project-info.txt

PROJECT_NUM=$(cat project-info.txt | sed -nre 's:.*projectNumber\: (.*):\1:p')

SVC_ACCOUNT="${PROJECT_NUM//\'/}-compute@developer.gserviceaccount.com"

gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member serviceAccount:$SVC_ACCOUNT --role roles/storage.objectAdmin

gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member serviceAccount:$SVC_ACCOUNT --role roles/aiplatform.admin
```

### Enable APIs

Run the gcloud command below to enable the necessary APIs:

```sh
gcloud services enable aiplatform.googleapis.com \
  storage.googleapis.com \
  artifactregistry.googleapis.com 
```

### Create Workbench Notebook instance

Create a workbench notebook with the following gcloud command below or via the [Google Cloud console](https://console.cloud.google.com/vertex-ai/workbench/instances) and then upload this notebook

```sh
gcloud workbench instances create fruit-veg-image-model-instance \
  --location=us-central1-a \
  --machine-type=n1-standard-8
```

references:

* doc: https://cloud.google.com/vertex-ai/docs/workbench/instances/create
* gcloud: https://cloud.google.com/sdk/gcloud/reference/workbench/instances
