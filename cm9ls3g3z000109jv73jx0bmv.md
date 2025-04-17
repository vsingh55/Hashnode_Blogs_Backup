---
title: "Designing a Serverless NBA Sports Analytics System on AWS"
seoTitle: "Serverless NBA Analytics System on AWS"
seoDescription: "Build a serverless NBA data analytics system using AWS services for efficient, scalable, and cost-effective sports data analysis and storage"
datePublished: Thu Apr 17 2025 19:53:50 GMT+0000 (Coordinated Universal Time)
cuid: cm9ls3g3z000109jv73jx0bmv
slug: data-lake
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1744901615613/624451ab-d170-4226-92a1-4c8b2760b415.png
tags: aws, python, automation, s3, serverless-architecture, aws-glue, aws-athena, powertocloud, devopsallstarschallenge

---

## Introduction

### Context and Background

The project was initiated to address the challenge of efficiently collecting, storing, and analyzing NBA sports data for advanced sports analytics. The organizational pain points included the lack of a centralized, scalable data repository and the absence of an automated pipeline to ingest and query NBA player data. The strategic objective was to build a robust data lake infrastructure on AWS that supports scalable data storage, seamless integration with analytics tools, and cost-effective querying capabilities.

### Personal Role and Approach

My specific contribution was designing and implementing the entire data lake setup pipeline using AWS services and integrating it with external NBA data sources. I began with an initial assessment of the requirements, which included reliable data ingestion from a third-party API, scalable storage, metadata management, and query capability. My strategic thinking process focused on leveraging AWS managed <mark>services like S3, Glue, and Athena</mark> **to build a serverless, scalable, and cost-efficient solution.**

## Technical Journey

### Problem Definition

The technical challenge was to ingest NBA player data from an external API into a scalable data lake architecture that supports efficient querying and analytics. Existing infrastructure lacked automated data ingestion, centralized storage, and metadata cataloging, limiting performance and scalability. Constraints included handling large datasets, ensuring data consistency, and enabling performant SQL queries over JSON data.

## Solution Design

### Technology Selection Rationale

AWS was chosen due to its mature ecosystem for data lakes:

* **Amazon S3** for durable, scalable object storage.
    
* **AWS Glue** for metadata cataloging and schema management.
    
* **Amazon Athena** for serverless interactive querying using standard SQL.  
    Alternatives like setting up an on-premise Hadoop cluster or using other cloud providers were considered but ruled out due to higher operational overhead and cost. The decision-making criteria prioritized scalability, cost-efficiency, ease of integration, and minimal maintenance.
    

## Architectural Design

The conceptual approach was to create a pipeline that:

* Fetches NBA data from the sportsdata.io API.
    
* Stores raw JSON data in an S3 bucket as line-delimited JSON files.
    
* Uses AWS Glue to create a database and table metadata pointing to the S3 data.
    
* Configures Athena to query the data directly from S3 using the Glue catalog.  
    Design principles included modularity, automation, and leveraging serverless managed services to minimize infrastructure management.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744919325873/81d50c09-a2e2-479f-9b21-44a390411d44.png align="center")
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744919300748/564e4c06-5cb2-4808-a3da-83892eb23250.png align="center")
    

## Solution Strategies

* Use of line-delimited JSON format for efficient storage and querying.
    
* Automating Glue database and table creation programmatically.
    
* Configuring Athena output location dynamically for query results.
    
* Environment variable management with dotenv for secure API key handling.
    

## Implementation Challenges

Challenges encountered included:

* Defining Glue table schema correctly for JSON data with proper SerDe configuration.
    
* <div data-node-type="callout">
    <div data-node-type="callout-emoji">💡</div>
    <div data-node-type="callout-text"><em>Serializer/Deserializer - </em>a plug-in that extracts (deserializes) raw data into columns for querying, and can also serialize structured data back into the raw format for storage</div>
    </div>
    
* Ensuring eventual consistency of S3 bucket creation before proceeding with subsequent steps.
    
* Debugging integration issues between Glue and Athena.
    

### Detailed Implementation Walkthrough

The implementation process followed these key steps:

1. **IAM policy:** Set up the necessary policy to create resources.
    
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "s3:CreateBucket",
                    "s3:PutObject",
                    "s3:GetObject",
                    "s3:DeleteObject",
                    "s3:ListBucket"
                ],
                "Resource": [
                    "arn:aws:s3:::sports-analytics-data-lake",
                    "arn:aws:s3:::sports-analytics-data-lake/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "glue:CreateDatabase",
                    "glue:DeleteDatabase",
                    "glue:GetDatabase",
                    "glue:GetDatabases",
                    "glue:CreateTable",
                    "glue:DeleteTable",
                    "glue:GetTable",
                    "glue:GetTables",
                    "glue:UpdateTable"
                ],
                "Resource": [
                    "arn:aws:glue:*:*:catalog",
                    "arn:aws:glue:*:*:database/glue_nba_data_lake",
                    "arn:aws:glue:*:*:table/glue_nba_data_lake/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "athena:StartQueryExecution",
                    "athena:GetQueryExecution",
                    "athena:GetQueryResults"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "s3:PutObject"
                ],
                "Resource": [
                    "arn:aws:s3:::sports-analytics-data-lake/athena-results/*"
                ]
            }
        ]
    }
    ```
    
2. **Infrastructure Setup:** First, I created the core S3 bucket that would serve as the foundation of our data lake:
    
    ```python
    def create_s3_bucket():
        """Create an S3 bucket for storing sports data."""
        try:
            if region == "ap-south-1":
                s3_client.create_bucket(Bucket=bucket_name)
            else:
                s3_client.create_bucket(
                    Bucket=bucket_name,
                    CreateBucketConfiguration={"LocationConstraint": region},
                )
            print(f"S3 bucket '{bucket_name}' created successfully.")
        except Exception as e:
            print(f"Error creating S3 bucket: {e}")
    ```
    
    This function handles the region-specific bucket creation syntax required by AWS.
    
3. **Glue Database Creation:** Next, I established a Glue database to serve as the organizational container for our data catalog:
    
    ```python
    def create_glue_database():
        """Create a Glue database for the data lake."""
        try:
            glue_client.create_database(
                DatabaseInput={
                    "Name": glue_database_name,
                    "Description": "Glue database for NBA sports analytics.",
                }
            )
            print(f"Glue database '{glue_database_name}' created successfully.")
        except Exception as e:
            print(f"Error creating Glue database: {e}")
    ```
    
4. **Data Ingestion Pipeline:** The core of the solution is the data extraction and loading process. I implemented an API client that securely retrieves data from SportsData.io:
    
    ```python
    def fetch_nba_data():
        """Fetch NBA player data from sportsdata.io."""
        try:
            headers = {"Ocp-Apim-Subscription-Key": api_key}
            response = requests.get(nba_endpoint, headers=headers)
            response.raise_for_status()  # Raise an error for bad status codes
            print("Fetched NBA data successfully.")
            return response.json()  # Return JSON response
        except Exception as e:
            print(f"Error fetching NBA data: {e}")
            return []
    ```
    
    To ensure Athena compatibility, I implemented a function to convert standard JSON arrays to line-delimited JSON format:
    
    ```python
    def convert_to_line_delimited_json(data):
        """Convert data to line-delimited JSON format."""
        print("Converting data to line-delimited JSON format...")
        return "\n".join([json.dumps(record) for record in data])
    ```
    
    The upload function then handles writing this properly formatted data to S3:
    
    ```python
    def upload_data_to_s3(data):
        """Upload NBA data to the S3 bucket."""
        try:
            # Convert data to line-delimited JSON
            line_delimited_data = convert_to_line_delimited_json(data)
    
            # Define S3 object key
            file_key = "raw-data/nba_player_data.jsonl"
    
            # Upload JSON data to S3
            s3_client.put_object(
                Bucket=bucket_name,
                Key=file_key,
                Body=line_delimited_data
            )
            print(f"Uploaded data to S3: {file_key}")
        except Exception as e:
            print(f"Error uploading data to S3: {e}")
    ```
    
5. **Metadata Management:** With data in S3, the next step was creating the Glue table definition that would allow Athena to query it:
    
    ```python
    def create_glue_table():
        """Create a Glue table for the data."""
        try:
            glue_client.create_table(
                DatabaseName=glue_database_name,
                TableInput={
                    "Name": "nba_players",
                    "StorageDescriptor": {
                        "Columns": [
                            {"Name": "PlayerID", "Type": "int"},
                            {"Name": "FirstName", "Type": "string"},
                            {"Name": "LastName", "Type": "string"},
                            {"Name": "Team", "Type": "string"},
                            {"Name": "Position", "Type": "string"},
                            {"Name": "Points", "Type": "int"}
                        ],
                        "Location": f"s3://{bucket_name}/raw-data/",
                        "InputFormat": "org.apache.hadoop.mapred.TextInputFormat",
                        "OutputFormat": "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat",
                        "SerdeInfo": {
                            "SerializationLibrary": "org.openx.data.jsonserde.JsonSerDe"
                        },
                    },
                    "TableType": "EXTERNAL_TABLE",
                },
            )
            print(f"Glue table 'nba_players' created successfully.")
        except Exception as e:
            print(f"Error creating Glue table: {e}")
    ```
    
    Note the use of the JsonSerDe serialization library, which is critical for properly parsing the JSON data in Athena.
    
6. **Query Configuration:** Finally, I configured Athena to ensure query results would be stored in a designated S3 location:
    
    ```python
    def configure_athena():
        """Set up Athena output location."""
        try:
            athena_client.start_query_execution(
                QueryString="CREATE DATABASE IF NOT EXISTS nba_analytics",
                QueryExecutionContext={"Database": glue_database_name},
                ResultConfiguration={"OutputLocation": athena_output_location},
            )
            print("Athena output location configured successfully.")
        except Exception as e:
            print(f"Error configuring Athena: {e}")
    ```
    
7. **Orchestration:** The main function ties everything together in the proper sequence:
    
    ```python
    def main():
        print("Setting up data lake for NBA sports analytics...")
        create_s3_bucket()
        time.sleep(5)  # Ensure bucket creation propagates
        create_glue_database()
        nba_data = fetch_nba_data()
        if nba_data:  # Only proceed if data was fetched successfully
            upload_data_to_s3(nba_data)
        create_glue_table()
        configure_athena()
        print("Data lake setup complete.")
    ```
    

**Configuration management** was handled using environment variables loaded via the `dotenv` package to securely manage API keys and endpoints.

## Outcomes and Impact

## Quantifiable Results

* Automated ingestion of NBA player data into a centralized data lake.
    
* Reduction in manual data processing time from minutes to it’s fraction part.
    
* Cost savings by using serverless AWS services with pay-per-query Athena.
    
* Scalability to handle growing datasets without infrastructure changes.
    

## Technical Achievements

* Implemented a fully automated data lake setup pipeline.
    
* Demonstrated advanced use of AWS Glue for schema and metadata management.
    
* Leveraged Athena for efficient querying of JSON data stored in S3.
    
* Pushed the boundaries of serverless data analytics infrastructure for sports data.
    

## Learning and Reflection

Key insights included the importance of:

* Proper schema design in Glue for JSON data.
    
* Handling AWS service eventual consistency.
    
* The power of serverless architectures for scalable data analytics.  
    Unexpected challenges like bucket creation delays were mitigated with strategic wait times. Future improvements could include incremental data updates and integration with visualization tools.
    

## Conclusion

This project significantly advanced the organization's capability to perform NBA sports analytics by building a scalable, automated data lake on AWS. Lessons learned around AWS Glue and Athena integration will inform future data engineering projects. Potential future developments include real-time data ingestion and machine learning model integration for predictive analytics.

## Technical Appendix

| Component | Technology/Service |
| --- | --- |
| Data Storage | Amazon S3 |
| Metadata Catalog | AWS Glue |
| Query Engine | Amazon Athena |
| Data Source | sportsdata.io NBA API |
| Environment Mgmt | Python dotenv package |
| Programming Language | Python |

The full project code and setup script are available on GitHub: [AWS-NBA-DevOpsAllStars-Challenge](https://github.com/vsingh55/AWS-NBA-DevOpsAllStars-Challenge/tree/main/D3-Sports%20Analytics%20Data%20Lake)1.