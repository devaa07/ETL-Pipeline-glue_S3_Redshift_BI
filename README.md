# ETL-Pipeline-glue_S3_Redshift_BI
Building Data Pipeline using Apache Airflow, Glue, S3, Redshift, PowerBI . Performing Customer Churn Data Analytics

![Screen Shot 2024-01-02 at 7 25 17 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/192e3b30-dcc1-4378-8491-218eff18bd0e)

# Step 1: Go to this link https://www.kaggle.com/datasets/yeanzc/telco-customer-churn-ibm-dataset?resource=download and download the dataset. convert this excel file into csv and patition this file into 3 files. Automate this process in the future

# Step 2: Create the EC2 instance with key-value pair and security group. configure the inbound rule to the port 8080 with HTTP

# Step 3: Create the virtual environment and install the following dependencies.

        sudo apt update
        sudo apt install python3-pip
        sudo apt install python3.10-venv
        python3 -m venv customer_churn_youtube_venv
        source customer_churn_youtube_venv/bin/activate 
        sudo pip install apache-airflow
        pip install apache-airflow-providers-amazon


# Step 4:  Establish the remote connection with Ec2 instance in VSCode.

<img width="1188" alt="Screen Shot 2024-01-02 at 7 34 47 PM" src="https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/af462cd0-87bc-4eff-864f-91a0150fc87f">

# Step 5: Create the S3 bucket and upload the csv file.

<img width="1310" alt="Screen Shot 2024-01-02 at 7 36 58 PM" src="https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/51e09e28-4358-4ee7-aca6-b0f9fcd8cd00">

# Step 6: Create the AWS Glue crawler to be able to crawl the file in s3 and dump this file into AWS Data catalog. Also assign the IAM rolwe with S3 permissions to glue

![Screen Shot 2024-01-02 at 7 40 48 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/ec561b69-a234-433c-89ab-8b3dd36296dc)

# Step 7: Run the crawler and verify the data is dumped into the table within glue database.

![Screen Shot 2024-01-02 at 7 41 47 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/1196b823-f37b-4290-b6d0-1dc3360d675a)

![Screen Shot 2024-01-02 at 7 42 20 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/fa0b4abe-28d7-441f-bb12-48e9d1ac4fb3)

# Step 8: Create the SWS Anthena to be able to query the data and perform some analytics.

![Screen Shot 2024-01-02 at 7 42 20 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/ef627c6a-d226-47bc-95c0-b06736012580)

# Step 9: Create the Redshift cluster and create the table 

        CREATE TABLE IF NOT EXISTS customer_churn(
         CustomerID VARCHAR(255),
         City VARCHAR(255),
         Zip_Code INTEGER,
         Gender VARCHAR(255), 
         Senior_Citizen VARCHAR(255),
         Partner VARCHAR(255),
         Dependents VARCHAR(255), 
         Tenure_Months INTEGER,
         Phone_Service VARCHAR(255),
         Multiple_Lines VARCHAR(255),
         Internet_Service VARCHAR(255),
         Online_Security VARCHAR(255),
         Online_Backup VARCHAR(255), 
         Device_Protection VARCHAR(255),
         Tech_Support VARCHAR(255),
         Streaming_TV VARCHAR(255),
         Streaming_Movies VARCHAR(255),
         Contract VARCHAR(255),
         Paperless_Billing VARCHAR(255),
         Payment_Method VARCHAR(255),
         monthly_charges FLOAT,
         Total_Charges FLOAT,
         Churn_Label VARCHAR(255),
         Churn_Value INTEGER,
         Churn_Score INTEGER,
         Churn_Reason TEXT
)

# Step 10:  Create the Redshift data connection within Glue.

![Screen Shot 2024-01-02 at 7 49 34 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/f1980e67-64ec-4ee1-b4f4-a0878c539061)

# Step 11: Create Glue crawler (glue-redshit-crawler) and the data source as shown the in the picture below. Also, add the database as the target glue data catalog

![Screen Shot 2024-01-02 at 7 56 39 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/446189d5-8a30-48eb-8b66-9f81fa4afa23)


![Screen Shot 2024-01-02 at 7 54 10 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/853e73b6-bec2-4551-a7eb-6a1f1ac04c00)

# Step 12: Run the glue-redshit-crawler. I got the error below and let's fix this

![Screen Shot 2024-01-02 at 7 56 39 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/470ee997-1eac-471b-acf7-f707d76cbdfb)

# Step 13: go to the redshift cluster VPC and enable the S3 endpoint for the VPC

![Screen Shot 2024-01-02 at 7 58 56 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/6ca96fa4-2d16-450d-8bd9-98e49647a206)

# Step 14: Create the AWS Glue ETL to automate Step 11 and Step 12. Basically crawling file from S3, perform some transformations and load this data into Redshift.

![Screen Shot 2024-01-02 at 8 04 59 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/a6888291-69b2-4da8-9a0b-2e963f29f3f0)

# Step 15: Now Automate the Step 11 using Airflow DAG. refer the DAG code in the below file in the repo.
            customers_churn_dag.py

# Step 16: Add the AWS connection in the aiflow connection with access  key and secret key details and configure the ec2 instance with this credentials as well

![Screen Shot 2024-01-02 at 8 10 18 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/c7b0bcab-6b48-4c66-8b63-970db6126934)

![Screen Shot 2024-01-02 at 8 04 59 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/222cad67-81fc-4feb-b188-e9915043ac9a)

# Step 17: Trigger the DAG and see the job got succeded and query the results in the redshift query tool

![Screen Shot 2024-01-02 at 8 11 59 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/02f0e232-72df-488b-8d4c-b0ee5d8491f8)

![Screen Shot 2024-01-02 at 8 13 25 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/a382249c-8bfe-4e50-a836-62e456f08ba3)

# Step 18: Establish the connection from PowerBI to Redshift and perform some analytics using PowerBI

![Screen Shot 2024-01-02 at 8 14 50 PM](https://github.com/devaa07/ETL-Pipeline-glue_S3_Redshift_BI-/assets/126756574/c50c3fb0-2e5a-4ede-9830-f74f3e9dcd73)








