# Deploy Bank of Anthos sample application on an Anthos cluster

This page walks you through the steps required to deploy the [Bank of Anthos](https://github.com/GoogleCloudPlatform/bank-of-anthos) sample application on an Anthos cluster using Terraform and Anthos Config Management (ACM).

## Pre-requisites

Setting up the sample requires that you have a [Google Cloud Platform (GCP) project](https://cloud.google.com/resource-manager/docs/creating-managing-projects#console), connected to your billing account.

## Deploy the sample

Once you have ensured the all the pre-requisites are met, follow the steps below to create an Anthos cluster and deploy the sample application.

1. Clone the repo:
`git clone https://github.com/GoogleCloudPlatform/bank-of-anthos.git`
1. Set $TF_VAR_project enviornment variable to a project that has billing enabled:
`export TF_VAR_project=<your project id>`
1. Enable Terraform to use the default service account. Follow the prompts to login via the URL and enter the verification code:
`gcloud auth application-default login --no-launch-browser`
1. Move into the terraform directory that has the installation scripts:
`cd terraform`
1. Initialize Terraform:
`terraform init`
1. See what resources will be created:
    `terraform plan`
1. Create the resources and deploy the sample:
    `terraform apply`

## Delete the sample and the cluster

Once you have finished working with the sample, you can tear down the sample application and the cluster 

1. Run `terraform destroy` from the `terraform` directory.

Please note that this does not delete the project where the Anthos cluster was created.

## Troubeshooting

* Error about a GCP API not enabled e.g.:

    ```
    Error: Error creating Feature: failed to create a diff: failed to retrieve Feature resource: googleapi: Error 403: GKE Hub API has not been used in project {project-number} before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/gkehub.googleapis.com/overview?project={project-number} then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry.
    ```

  As the error suggests, sometime if an API has just been enabled, the action may not have propogated to all the systems. So wait a few minutes and apply the terraform again.

* Failure to create GKE Hub membership or feature e.g.:

    ```
    Error creating Feature: Resource already exists.
    ```

  This is likely because you already have the GKE Hub membership or feature enabled. To resolve the error, edit the `acm.tf` and remove the resource that already exists.
