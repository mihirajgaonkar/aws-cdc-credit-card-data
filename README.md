# aws-cdc-credit-card-disputed-transactions

## Overview:

One of the major factors for credit card businesses is managing disputed transactions. This project presents an ETL pipeline designed to perform analytics and store real-time updates on transactions while capturing any changes in DynamoDB.

![alt text](https://github.com/mihirajgaonkar/aws-cdc-credit-card-data/blob/main/aws%20architecture.png)

## Services Used:

- **DynamoDB:** 
  - A NoSQL table is created in DynamoDB with basic fields such as Transaction_ID (Primary/partition key), user_id, Business (indicating the type of transaction), Status (processed, disputed), Location (State in USA), and Amount.
  - DynamoDB streams are enabled to capture any manual changes or updates in the table.

- **EventBridge:** 
  - EventBridge is used to transfer updates from DynamoDB to Kinesis Data Stream. 
  - It allows for filtering or data enrichment before forwarding the data.

- **Kinesis:** 
  - Kinesis Data Streams and Kinesis Data Firehose capture the changed events from DynamoDB.
  - Based on certain limits, the data is packaged and batch processed before being stored in an S3 bucket
  - you can check the sample payload here [link](https://github.com/mihirajgaonkar/aws-cdc-credit-card-data/blob/main/sample_json.json).

- **Lambda Transformer:** 
  - An intermediate Lambda transformer captures only the JSON data from Kinesis Data Firehose payloads and stores it in S3.

- **Athena:** 
  - Glue crawler is used to parse the data in the S3 bucket and store it in relational tables created in Athena. 
  - Data can be queried using SQL queries in Athena.

- **Visualization Tools:** 
  - Quicksight or PowerBI are utilized to perform calculations, analytics, and create visually engaging dashboards/reports for non-technical stakeholders.

![alt text](https://github.com/mihirajgaonkar/aws-cdc-credit-card-data/blob/main/AWS%20Quicksight.png)

## Future Improvements:

   - Create S3 buckets for error logging.
   - Based on business use case, create separate buckets for disputed transactions and all transactions.This will reduce query costs through Athena, especially if we only need to perform analytics on disputed transactions.
   - Instead of using sessions to connect to DynamoDB, use access keys and secret access keys for enhanced security.
   - Utilize VPC and deploy resources within it.
   - Create IAM roles and provide the respective policies instead of using the Root user and attaching Full access policies for resources. This will enhance security and facilitate error troubleshooting.
