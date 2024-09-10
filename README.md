# Fruit and Vegtable Image Classification POC

## Before you Begin

## Setup 

### Grant permissions to Default Compute Engine Service Account 

The Workbench instance we will create should use the Default Compute Engine Service account.

Grant the necessary roles via GCP console or via gcloud commands 

roles

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

### Create Workbench Notebook instance

Create a workbench notebook with the following gcloud command below or via the [Google Cloud console](https://console.cloud.google.com/vertex-ai/workbench/instances) and then upload this notebook

```sh
gcloud workbench instances create fruit-veg-image-model \
  --location=us-central1-a \
  --machine-type=n1-standard-32 \
  --metadata=idle-timeout-seconds=
```

references:

* doc: https://cloud.google.com/vertex-ai/docs/workbench/instances/create
* gcloud: https://cloud.google.com/sdk/gcloud/reference/workbench/instances
* Note: `--metadata=idle-timeout-seconds=` is purposely set with no value to ensure instance does not shutdown during the long-running operation. See more here: https://cloud.google.com/vertex-ai/docs/workbench/instances/idle-shutdown#cli


