# Deploy a Simple Python App to Cloud Run

This repo is using GitHub Actions to deploy a Simple Python App to Cloud Run in GCP.

## Overview

Running unit tests<br>
Authenticate to Google Cloud<br>
Authenticate Docker to Artifact Registry<br>
Build a docker container<br>
Publish it to Google Artifact Registry<br>
Deploy it to Cloud Run

## Prerequisites

- Two GitHub environments (stage and production)
- Ensure the required Google Cloud APIs are enabled:
  - Cloud Run: run.googleapis.com
  - Artifact Registry: artifactregistry.googleapis.com
- Create and configure Workload Identity Federation for GitHub (https://github.com/google-github-actions/auth#setting-up-workload-identity-federation)
- Ensure the required IAM permissions are granted
  - Cloud Run: roles/run.admin, roles/iam.serviceAccountUser (to act as the Cloud Run runtime service account)
  - Artifact Registry: roles/artifactregistry.admin (project or repository level)
- Create two Artifact Registry repositories for `stage` and `production`

## Github Environment setup

1. On your repository, go to **Settings** > **Environments** (under Code and automation) > **New environment**

2. Add two environments, `stage` and `production`

3. For each environment, add the following secrets and variables

#### Secrets

| Name          | Example value                                                                           |
| ------------- | --------------------------------------------------------------------------------------- |
| WI_PROVIDER   | projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider |
| WIF_SA        | my-service-account@my-project.iam.gserviceaccount.com                                   |

#### Variables

| Name          | Example value   | 
| ------------- | --------------- |
| PROJECT_ID    | project-12345   | 
| REGION        | us-west1        |
| ARTIFACT_REPO | prod-repo       |
| SERVICE_NAME  | app-run         |

> NOTE: To use Google Container Registry instead, replace `${{ vars.REGION }}-docker.pkg.dev` with `gcr.io`

## References
- https://docs.github.com/en/actions
- https://github.com/google-github-actions/deploy-cloudrun
- https://github.com/google-github-actions/auth