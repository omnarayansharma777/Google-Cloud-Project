# Challenge scenario

In this project, I worked as both a Data Sharing Partner and a Customer, setting up a data-sharing mechanism between two organizations. My primary objective was to create authorized views, assign specific user permissions and update customer data.


![Loading](https://github.com/omnarayansharma777/Google-Cloud-Project/blob/main/Share_Data_using_Google_Data_Cloud/J7qOIElm12DamovB4BG42nuSgyeRAG02XA_EnFKPc6o%3D.png)

# Task 1. Create the partner authorized view

As the Data Sharing Partner, I created an authorized view of my table  in my dataset. I then assigned the BigQuery Data Viewer role to a specific customer user, granting them access to this table view.

# Task 2: Update the Customer Data Table

Next, I acted as the Customer and updated my customer data by matching zip codes from the shared view. I ran a query that modified rows in my customer table to ensure the correct county data was applied.

# Task 3: Create the Customer Authorized View

I created an authorized view within the customer project and shared this view with the Data Sharing Partner. I assigned the BigQuery Data Viewer role to the partner, allowing them to view the grouped data.

# Task 4: Create a Data Visualization Using Looker Studio

Finally, I used Looker Studio to visualize the shared data. I created a column chart that displayed the number of customers per county. This chart provided a clear overview of customer distribution.


![alt](https://github.com/omnarayansharma777/Google-Cloud-Project/blob/main/Share_Data_using_Google_Data_Cloud/looker_report.png)
