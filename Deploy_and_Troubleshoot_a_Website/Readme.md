# Challenge scenario
Your company is ready to launch a brand new product! Because you are entering a totally new space, 
you have decided to deploy a new website as part of the product launch. The new site is complete, 
but the person who built the new site left the company before they could deploy it.

My challenge is to deploy the site in the public cloud .

U can find [ Lab Link](https://www.cloudskillsboost.google/games/5590/labs/35948) 

## Task 0 . Created environment variable

 export ZONE=us-east4-a 
 
 INSTANCE_NAME=tst-pln-gy1

## Task 1. Create a Linux VM instance
Create a Linux virtual machine, name it tst-pln-gy1 and specify the zone as us-east4-a.

```
gcloud compute instances create $INSTANCE_NAME --zone $ZONE
```
## Task 2. Enable public access to VM instance
While creating the Linux instance, make sure to apply the appropriate firewall rules so that potential customers can find your new product.

```
gcloud compute firewall-rules create allow-http --network=default --allow tcp:80 --target-tags=http-server --direction=INGRESS
gcloud compute instances add-tags $INSTANCE_NAME --tags=http-server --zone=$ZONE
```
## Task 3. Running a basic Apache Web Server
Deploy a simple Apache web server (a placeholder for the new product site) to learn the basics of running a server on a virtual machine instance.

```
gcloud compute ssh  "tst-pln-gy1" --zone $ZONE
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
```
## Task 4. Test your server
```
gcloud compute instances describe $INSTANCE_NAME --zone=$ZONE --format="get(networkInterfaces[0].accessConfigs[0].natIP)"
```
http://EXTERNAL_IP
