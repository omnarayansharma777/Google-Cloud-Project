# Challenge scenario

You are asked to help a newly formed development team with some of their initial work on a new project. 
Specifically, they need a new BigLake table from a Cloud Storage file with the appropriate permissions to limit access to sensitive data columns; 
you receive the following request to complete the following tasks:

1. Create a BigLake table from an existing file on Cloud Storage.
2. Apply and verify policy tags to restrict access to columns containing sensitive data.
3. Remove direct IAM permissions to Cloud Storage for other users (after policy tags have been applied to protect the data).

This is Challenge Lab [ link](https://www.cloudskillsboost.google/games/5590/labs/35954) and my [Badge](https://www.credly.com/badges/c4391cd5-ea23-48a6-a4e0-b18fff129449)
# Task 1. Create a BigLake table using a Cloud Resource connection

1. Create a BigQuery dataset named online_shop that is multi-region in the United States.
2. Create a Cloud Resource connection named user_data_connection (multi-region in the United States) and use it to create a BigLake table named user_online_sessions in the online_shop dataset.

# Task 2. Apply and verify policy tags on columns containing sensitive data
1. Use the precreated taxonomy named taxonomy name to apply column-level policy tags on the table.
Apply the policy tag named sensitive-data-policy to the following columns in the user_online_sessions table:

  zip

  latitude

  ip_address

  longitude

2. Verify the column-level security by running a query that omits the protected columns.

# Task 3. Remove IAM permissions to Cloud Storage for other users

Follow Google best practices after migrating data to BigLake by removing IAM permissions for user 2 (User 2) to Cloud Storage.
Leave the IAM role for project viewer.
Remove only the IAM role for Cloud Storage.

