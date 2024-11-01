# Challenge scenario

You are asked to help a newly formed development team with some of their initial work on a new project using Pub/Sub to manage messages. 
Specifically, they need to create a new Pub/Sub topic with an event trigger to a Cloud Run sink using Eventarc

This is Challenge Lab [ link](https://www.cloudskillsboost.google/games/5632/labs/36137) and my [Badge]()

# Task 1. Create a Pub/Sub topic

```
gcloud pubsub topics create project_id-topic
```

# Task 2. Create a Cloud Run sink

```
gcloud run deploy pubsub-events --image gcr.io/cloudrun/hello --allow-unauthenticated --max-instances=3
```

# Task 3. Create and test a Pub/Sub event trigger using Eventarc

```
gcloud eventarc triggers create trigger-pubsub --destination-run-service=pubsub-events --event-filters="type=google.cloud.pubsub.topic.v1.messagePublished --transport-topic=project_id-topic --location=Reigon"
gcloud pubsub topics publish project_id-topic --message "Hello"
```
