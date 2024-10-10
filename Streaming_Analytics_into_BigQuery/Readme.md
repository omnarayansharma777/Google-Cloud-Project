# Challenge scenario
You are just starting your junior data engineer role. So far you have been helping teams create and manage data using BigQuery, Pub/Sub, and Dataflow.You have been asked to assist the team with streaming temperature data into BigQuery using Pub/Sub and Dataflow.

This is Challenge Lab [ link](https://www.cloudskillsboost.google/course_templates/752/labs/461559) and my [Badge](https://www.credly.com/badges/5d833ae7-86ff-4a13-8606-0c028e204445)

1. ## Create a Cloud Storage bucket
```
 gsutil mb gs://qwiklabs-gcp-03-50865082cddf
```
2. ##  Create a BigQuery dataset and table

```
bq mk sensors_237
bq mk --table qwiklabs-gcp-03-50865082cddf:sensors_237.temperature_272 data:string
```

3. ## Set up a Pub/Sub topic

```
gcloud pubsub topics create sensors-temp-60295
gcloud pubsub subscriptions create sensors-temp-60295-sub --topic=sensors-temp-60295
```
4. ## Run a Dataflow pipeline to stream data from Pub/Sub to BigQuery
Note : If you facing issue . stop and enable dataflow api again
```
 gcloud dataflow jobs run dfjob-20512     --gcs-location gs://dataflow-templates-us-west1/latest/PubSub_to_BigQuery     --region us-west1     --worker-machine-type e2-medium     --staging-location gs://qwiklabs-gcp-03-50865082cddf/temp     --parameters inputTopic=projects/qwiklabs-gcp-03-50865082cddf/topics/sensors-temp-60295,outputTableSpec=qwiklabs-gcp-03-50865082cddf:sensors_237.temperature_272
```

5.  ## Publish a test message to the topic and validate data in BigQuery

```
gcloud pubsub topics publish sensors-temp-60295 --message='{"data": "73.4 F"}'
bq query --nouse_legacy_sql "SELECT * FROM \`qwiklabs-gcp-03-50865082cddf.sensors_237.temperature_272\`"
```
