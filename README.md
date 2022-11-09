# Github Actions and dynamic developer environments
###
gcloud set project PROJECT_ID


### Set up workload identity federation
1- Enable the IAM, Resource Manager, Service Account Credentials, and Security Token Service (STS) APIs
2- create a workload identity federation pool
```
gcloud iam workload-identity-pools create "github" \
  --location="global" \
  --display-name="github action pool"
```
3- create a workload identity federation provider
```
gcloud iam workload-identity-pools providers create-oidc "github" \
  --project="${PROJECT_ID}" \
  --location="global" \
  --workload-identity-pool="my-pool" \
  --display-name="Demo provider" \
  --attribute-mapping="google.subject=assertion.sub,attribute.actor=assertion.actor,attribute.aud=assertion.aud" \
  --issuer-uri="https://token.actions.githubusercontent.com"
```
4- create a service account with editor role
5- authorise service account impersonation
```
gcloud iam service-accounts add-iam-policy-binding "my-service-account@${PROJECT_ID}.iam.gserviceaccount.com" \
  --project="${PROJECT_ID}" \
  --role="roles/iam.workloadIdentityUser" \
  --member="principalSet://iam.googleapis.com/projects/1234567890/locations/global/workloadIdentityPools/my-pool/attribute.repository/my-org/my-repo"
```
⚠️ change the principalSet... with the one found in the provider pool (IAM principal)

### setup artifact registry
1- enable artifact registry api
2- create a repository named "reliable" in us-central1

### setup cloud run
1- enable cloud run api

### setup secrets
1- enable Secret Manager API
2- add secret in secret manger named "GITHUB_ACTION_SECRET"
3- add permission roles/secretmanager.secretAccessor to ...-compute@developer.gserviceaccount.com
