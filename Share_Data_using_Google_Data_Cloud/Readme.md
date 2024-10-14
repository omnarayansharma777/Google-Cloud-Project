# **Challenge Scenario**

In this project, I worked both as a **Data Sharing Partner** and a **Customer**, setting up a data-sharing mechanism between two organizations. My primary objective was to create authorized views, assign specific user permissions, and update customer data.

You can find the full lab details [here](https://www.cloudskillsboost.google/course_templates/657/labs/462733) and view my **Google Cloud Skill Badge** [here](https://www.credly.com/badges/d9b2897f-9078-4b4d-9655-85334f54dc42).

![Lab Overview](https://github.com/omnarayansharma777/Google-Cloud-Project/blob/main/Share_Data_using_Google_Data_Cloud/J7qOIElm12DamovB4BG42nuSgyeRAG02XA_EnFKPc6o%3D.png)

---

## **Task 1: Create the Partner Authorized View**

As the **Data Sharing Partner**, I created an **authorized view** from my table within the dataset. After creating the view, I assigned the **BigQuery Data Viewer** role to a specific customer user, granting them access to this table view for secure data sharing.

---

## **Task 2: Update the Customer Data Table**

Next, in the role of the **Customer**, I updated my customer data by matching zip codes from the shared view. I ran a query that modified several rows in my customer table to ensure that the correct county data was applied, enhancing the data accuracy.

---

## **Task 3: Create the Customer Authorized View**

In this step, I created an **authorized view** within the customer project. This view grouped customer data by county, and I shared it with the **Data Sharing Partner**. I also assigned the **BigQuery Data Viewer** role to the partner, allowing them to view the grouped customer data seamlessly.

---

## **Task 4: Create a Data Visualization Using Looker Studio**

Finally, I used **Looker Studio** to visualize the shared customer data. I created a **column chart** to display the number of customers per county, providing a clear and visual overview of customer distribution across counties.

![Data Visualization](https://github.com/omnarayansharma777/Google-Cloud-Project/blob/main/Share_Data_using_Google_Data_Cloud/looker_report.png)

---

This project demonstrates the efficient use of **BigQuery** and **Looker Studio** for secure data sharing and visualization between organizations.
