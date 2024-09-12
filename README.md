# Google Cloud Vertex AI Fruit and Vegetable Image Model

## Overview

This repository contains example code for Fruit and Vegetable image model training workflow using several different approaches, mostly involving Google Cloud Vertex AI.

## Workflows

You can work on any or all of the workflows as long as you follow the "before you begin" steps below.

| **Workflow** | **Description** | **Code** |
|---|---|---|
| 1. **Kaggle Example** | Example notebooks from kaggle.com using the Kaggle dataset and working with open source models in a local notebook environment | [01-kaggle-example](01-kaggle-example/) |
| 2. **Kaggle + Vertex AI** | Example using Kaggle dataset as training data and using Vertex AI for development environment and compute resources for training job | [02-kaggle-vertex-ai](02-kaggle-vertex-ai/) |
| 3. **Kaggle + Imagen + Vertex AI** | Example workflow generating a synthetic dataset with Imagen API and using Vertex AI for development environment and compute resources for training job | [03-imagen-vertex-ai](03-imagen-vertex-ai/) |

## Before you begin

### Set up your Google Cloud project

**The following steps are required, regardless of your notebook environment.**

1. [Select or create a Google Cloud project](https://console.cloud.google.com/cloud-resource-manager). When you first create an account, you get a $300 free credit towards your compute/storage costs.
2. [Make sure that billing is enabled for your project](https://cloud.google.com/billing/docs/how-to/modify-project).
3. [Enable the Storage, Vertex AI, Arifact Registry and Cloud Build APIs](https://console.cloud.google.com/flows/enableapi?apiid=storage.googleapis.com,aiplatform.googleapis.com,cloudbuild.googleapis.com,artifactregistry.googleapis.com).
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
gcloud services enable storage.googleapis.com \
  aiplatform.googleapis.com \
  build.googleapis.com \
  artifactregistry.googleapis.com 
```

## Create Development Environment - Workbench Notebook instance

Create a workbench notebook with the following gcloud command below or via the [Google Cloud console](https://console.cloud.google.com/vertex-ai/workbench/instances) and then upload this notebook

```sh
gcloud workbench instances create fruit-veg-image-model-instance \
  --location=us-central1-a \
  --machine-type=n1-standard-8
```

references:

* doc: https://cloud.google.com/vertex-ai/docs/workbench/instances/create
* gcloud: https://cloud.google.com/sdk/gcloud/reference/workbench/instances

## Download code to your development enviromnet

Download this repository to your Workbench instance if needed. You can use the notebook GUI or run the command below:

```sh
git clone https://github.com/justinjm/google-cloud-fruit-and-vegetable-image-model.git
```

Once the code is downloaded, you're ready to go! Pick your notebook(s) and happy modeling :)

## Acknowledgements

* [Lars Ahlfors)](https://github.com/lahlfors) - for his contributions of authoring modeling code and overall technical guidance.
