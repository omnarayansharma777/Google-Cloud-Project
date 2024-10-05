







# Task 1. Create development VPC

```
gcloud compute networks create griffin-dev-vpc --subnet-mode custom
gcloud compute networks subnets create griffin-dev-wp  --range=192.168.16.0/20 --network=griffin-dev-vpc --region=us-central1
gcloud compute networks subnets create griffin-dev-mgmt --range=192.168.32.0/20 --network=griffin-dev-vpc --region=us-central1
```

# Task 2. Create production VPC

```
gcloud compute networks create griffin-prod-vpc --subnet-mode=custom
gcloud compute networks subnets create griffin-prod-wp --range=192.168.48.0/20 --network=griffin-prod-vpc --region=us-central1
gcloud compute networks subnets create griffin-prod-mgmt --range=192.168.64.0/20 --network=griffin-prod-vpc --region=us-central1
```

# Task 3. Create bastion host

```
gcloud compute instances create bastion --network-interface=network=griffin-dev-vpc,subnet=griffin-dev-mgmt  --network-interface=network=griffin-prod-vpc,subnet=griffin-prod-mgmt --tags=ssh --zone=$ZONE
gcloud compute firewall-rules create ssh-dev --network=griffin-dev-vpc --rules=tcp:22 --source-ranges=0.0.0.0/0 --target-tags=ssh --action=ALLOW
gcloud compute firewall-rules create ssh-prod --network=griffin-prod-vpc --rules=tcp:22 --source-ranges=0.0.0.0/0 --target-tags=ssh --action=ALLOW
```

# Task 4. Create and configure Cloud SQL Instance
```
gcloud sql instances create griffin-dev-db --database-version=MYSQL_5_7 --region=$REGION --root-password='omdev'
gcloud sql users create wp_user --password=stormwind_rules --instance=griffin-dev-db
gcloud sql users create wp_user --instance=griffin-dev-db --password=stormwind_rules --instance=griffin-dev-db
```
# Task 5. Create Kubernetes cluster
```
gcloud container clusters create griffin-dev --network griffin-dev-vpc --subnetwork griffin-dev-wp --machine-type e2-standard-4 --num-nodes=2 --zone us-central1-c
```
# Task 6. Prepare the Kubernetes cluster
```
gsutil cp -r gs://cloud-training/gsp321/wp-k8s .
gcloud iam service-accounts keys create key.json \
    --iam-account=cloud-sql-proxy@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com
kubectl create secret generic cloudsql-instance-credentials \
    --from-file key.json
```
update  usename and password in wp-env.yaml

# Task 7. Create a WordPress deployment
Replace YOUR_SQL_INSTANCE with griffin-dev-db's Instance connection name.
```
kubectl create -f wp-deployment.yaml
kubectl create -f wp-service.yaml
```
# Task 8. Enable monitoring
Copy wordpress endpoint ip -> go to uptime checks paste ip in host and path = /

# Task 9. Provide access for an additional engineer
GO to IAM roles and provide access for second user





