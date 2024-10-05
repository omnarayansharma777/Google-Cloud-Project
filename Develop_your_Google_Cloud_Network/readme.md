**Project Description**: This project involved setting up a secure and scalable infrastructure for deploying a WordPress application on Google Kubernetes Engine (GKE), ensuring isolation between development and production environments through the use of separate VPCs and a bastion host for secure access.

1. Created two isolated VPCs: dev-vpc for development/testing and prod-vpc for live production.
2. Configured a bastion host with dual network interfaces: one connected to dev-vpc and the other to prod-vpc.
3. Deployed WordPress using a Kubernetes cluster in dev-vpc, connected to a Google Cloud SQL instance for the backend database.
4. Utilized a Kubernetes LoadBalancer service to expose the WordPress application to external users.
5. Set uptime checks to monitor if anything goes wrong.
6. Given access to other engineer to do modification using IAM 

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
gcloud sql databases create wordpress --instance=griffin-dev-db
gcloud sql users create wp_user --password=stormwind_rules --instance=griffin-dev-db
gcloud sql users create wp_user --instance=griffin-dev-db --password=stormwind_rules --instance=griffin-dev-db
```
# Task 5. Create Kubernetes cluster
```
gcloud container clusters create griffin-dev --network griffin-dev-vpc --subnetwork griffin-dev-wp --machine-type e2-standard-4 --num-nodes=2 --zone=$ZONE
```
# Task 6. Prepare the Kubernetes cluster 
```
gsutil cp -r gs://cloud-training/gsp321/wp-k8s .
gcloud container clusters get-credentials griffin-dev --zone $ZONE
gcloud iam service-accounts keys create key.json \
    --iam-account=cloud-sql-proxy@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com
kubectl create secret generic cloudsql-instance-credentials \
    --from-file key.json
kubectl create -f wp-env.yaml
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





