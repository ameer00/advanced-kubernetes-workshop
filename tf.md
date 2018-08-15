Create a GCP Service Account with IAM roles/owner for terraform to be able to configure resources inside of your project.

```
export SERVICE_ACCOUNT_NAME=terraform-service-account
export SERVICE_ACCOUNT_DEST=~/.gcp/terraform-service-account.json
export PROJECT=$(gcloud info --format='value(config.project)')

gcloud iam service-accounts create \
    $SERVICE_ACCOUNT_NAME \
    --display-name $SERVICE_ACCOUNT_NAME

export SA_EMAIL=$(gcloud iam service-accounts list \
    --filter="displayName:$SERVICE_ACCOUNT_NAME" \
    --format='value(email)')

gcloud projects add-iam-policy-binding $PROJECT \
    --role roles/owner --member serviceAccount:$SA_EMAIL

mkdir -p $(dirname $SERVICE_ACCOUNT_DEST)

gcloud iam service-accounts keys create $SERVICE_ACCOUNT_DEST \
    --iam-account $SA_EMAIL
```

Go the to the main folder and add Project ID to the varibales.tf file.

```
sed -i -e s/PROJECT/$PROJECT/g variables.tf
```

Initialize Terraform (from the main folder)

```
terraform init
```


