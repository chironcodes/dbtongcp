# dbtongcp



Creating a service account that will work with dbt core


```sh

export GOOGLE_CLOUD_PROJECT=your_project

gcloud iam service-accounts create dbt-core-runner \
    --display-name="Dbt Job Runner" \
    --project $GOOGLE_CLOUD_PROJECT


DBT_CORE_SERVICE_ACCOUNT=dbt-core-runner@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com

```



## Give it all permissions required.

```sh

gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT \
    --member serviceAccount:$DBT_CORE_SERVICE_ACCOUNT \
    --project $GOOGLE_CLOUD_PROJECT \
    --role roles/bigquery.dataEditor \
    --role roles/bigquery.jobUser \
    --role roles/iam.serviceAccountUser \
    --role roles/iam.serviceAccountTokenCreator
    
```


## Export it locally.
```sh

gcloud iam service-accounts keys create secrets/dbt-core-job-runner.json \
    --iam-account $DBT_CORE_SERVICE_ACCOUNT

tr -d '\n' < secrets/dbt-core-job-runner.json > secrets/dbt-core-job-runner-oneline.json


```