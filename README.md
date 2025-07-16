# 🎵 **Global Beats 100: A Cloud-Native Spotify Data Engineering Pipeline**

## 🚀 Overview

**Global Beats** is a fully automated, cloud-native data engineering pipeline that ingests, processes, and analyzes the world's top 100 Spotify tracks daily. Leveraging the power of AWS, Apache Spark, Airflow, and AWS Glue, Redshift, this project demonstrates how to build a scalable, production-ready ETL system for real-time music analytics.

---

## 🏗️ **Architecture**
Here's how the data moves through the system:
![Global Beats Architecture](https://drive.google.com/uc?export=view&id=1v1m7lEvFSWYDfrMfULrWvW3XrC0A7iwU "Global Beats Data Pipeline Architecture")

Spotify API: This is where it all starts. We pull the raw global top 100 track information directly from the Spotify API.

***Apache Airflow Scheduler***: Airflow is the brain of the operation. It's set up to run daily, orchestrating the entire data extraction, transformation, and loading (ETL) process. It reliably triggers the first step of our pipeline: the data extraction.

***AWS Lambda (Data Extractor)***: When triggered by Airflow, this serverless function jumps into action. It's written in Python and uses libraries like spotipy to connect to the Spotify API, fetch the latest top 100 tracks data, and then uses boto3 to directly save the raw JSON files into an S3 bucket. Being serverless means it automatically scales and only costs money when it's running.

***AWS S3 (Raw Data Lake)***: This is our central storage for all the raw, untouched data coming from Spotify. S3 is incredibly reliable, can store virtually unlimited amounts of data, and is very cost-effective. We organize the data with a clear folder structure (like by year, month, and day) to make it easy to find and manage.

***AWS Glue Crawler***: Once new raw data lands in S3, a Glue Crawler automatically scans it. It figures out the data's structure (its schema), identifies data types, and discovers how the data is partitioned. All this information is then recorded in the Glue Data Catalog. This automation saves a lot of manual work in defining schemas.

***AWS Glue Data Catalog***: Think of this as the central library for all our data. It stores the metadata (information about the data, like its structure and where it's located) for all our datasets in S3. This catalog makes it easy for other services, like AWS Glue ETL and Amazon Athena, to understand and query the data without needing separate schema definitions.

***AWS Glue ETL (Apache Spark)***: This is where the magic of data transformation happens. AWS Glue runs powerful PySpark scripts, using the Apache Spark engine to process the data on a distributed cluster. It reads the raw data from S3 (using the schema from the Glue Data Catalog) and performs essential tasks like cleaning, normalizing, enriching (e.g., combining with historical data or adding new calculated fields), and deduplicating the data. The transformed, high-quality data is then saved back into another S3 bucket, often in an optimized format like Parquet.

***AWS S3 (Processed Data Lake)***: This S3 bucket holds all the transformed and cleaned data. It's structured and optimized, typically in columnar formats like Parquet, which makes it very efficient for analytical queries and for loading into our data warehouse.

***Amazon Redshift (Data Warehouse)***: The final stop for our analytical-ready data. The processed data from S3 is loaded into Redshift using highly optimized COPY commands. Redshift is a powerful, petabyte-scale data warehouse designed specifically for fast, complex analytical queries and business intelligence reports. It's perfect for diving deep into historical music trends.

***Amazon Athena (Ad-hoc Queries)***: For quick, on-the-fly analysis, we use Athena. This serverless query service lets analysts run standard SQL queries directly against the data stored in our S3 data lakes (both raw and processed). Athena uses the schemas from the Glue Data Catalog, offering a flexible and cost-effective way to explore data without needing to load it into Redshift first.

***AWS CloudWatch (Monitoring & Logging)***: CloudWatch keeps an eye on everything. It collects logs and metrics from all the AWS services in the pipeline, allowing us to monitor their health, track performance, and set up alerts for any issues or anomalies.

***AWS IAM & VPC (Security & Networking)***: These are the fundamental AWS services that ensure the entire architecture is secure and isolated. IAM manages all permissions and access controls, ensuring only authorized users and services can interact with resources. VPC provides a private, isolated network environment where all our AWS resources run, giving us granular control over network security.

---
## 🛠️ **Tech Stack & Skills Demonstrated**

- **Cloud & DevOps:**  
  AWS Lambda, S3, Glue, Redshift, Athena, CloudWatch, IAM, VPC

- **Big Data Processing:**  
  Apache Spark (via AWS Glue) for distributed ETL

- **Workflow Orchestration:**  
  Apache Airflow for DAG-based scheduling and dependency management

- **Data Engineering:**  
  ETL, schema evolution, data cataloging, partitioning, and optimization

- **Analytics & Visualization:**  
  SQL (Athena/Redshift), PowerBI

- **Python Ecosystem:**  
  pandas, numpy, spotipy, boto3, pyspark

---

## 📦 **How It Works**

### 1. **Extraction (Airflow + Lambda)**
Airflow triggers a Lambda function daily, which authenticates with the Spotify API and downloads the latest Top 100 Global tracks, artists, and albums. The raw data is stored in S3 with timestamped folders for versioning.

### 2. **Schema Discovery (Glue Crawler)**
Glue Crawlers scan the S3 bucket, infer the schema, and update the Glue Data Catalog, making the data queryable and discoverable.

### 3. **Transformation (Glue ETL + Spark)**
Glue ETL jobs (written in PySpark) clean, normalize, and enrich the data. This includes deduplication, type casting, flattening nested structures, and joining with historical data for trend analysis.

### 4. **Loading (Redshift)**
The processed data is loaded into Amazon Redshift using the COPY command, optimized for analytics and BI workloads.

### 5. **Analytics (Athena & Redshift)**
Analysts can run ad-hoc SQL queries using Athena directly on S3 or leverage Redshift for complex, large-scale analytics.

### 6. **Visualization**
Data is ready for visualization in Tableau, PowerBI, or AWS QuickSight, enabling real-time dashboards and insights into global music trends.

![Spotify Data Pipeline Diagram](https://drive.google.com/uc?export=view&id=1bN5KqMk0gNLLK_hmfpCjpaqFv73q92Jw "Spotify Data Pipeline Diagram")

---

## 🌟 **Key Features & Innovations**

- **End-to-End Automation:**  
  Zero manual intervention—Airflow and AWS services handle everything from extraction to analytics.

- **Scalable & Cost-Efficient:**  
  Serverless and distributed components scale with data volume, minimizing costs and maximizing performance.

- **Production-Grade Monitoring:**  
  CloudWatch integration ensures robust monitoring, alerting, and logging for operational excellence.

- **Extensible & Modular:**  
  Easily extend to new data sources, additional transformations, or downstream consumers.



## 📝 **Getting Started**

### Prerequisites
pip install pandas <br>
pip install numpy <br>
pip install spotipy<br>
pip install boto3<br>
pip install pyspark<br>
pip install apache-airflow<br>

---

### Installation & Setup

To get this project up and running, follow these steps:

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/dhirengshetty14/awscloud-data-etl-spotify100.git](https://github.com/dhirengshetty14/awscloud-data-etl-spotify100.git)
    cd awscloud-data-etl-spotify100
    ```

2.  **Set up Python Environment:**
    It's recommended to use a virtual environment:
    ```bash
    python -m venv venv
    source venv/bin/activate # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *(You'll need to create a `requirements.txt` file from your `pip install` list)*

4.  **AWS Configuration:**
    * **Create an AWS Account:** If you don't have one, sign up at [aws.amazon.com](https://aws.amazon.com/).
    * **Configure AWS CLI:** Ensure you have the AWS CLI installed and configured with appropriate credentials (IAM user with programmatic access, or an assumed role) that have permissions to create/manage S3 buckets, Lambda functions, Glue resources, Redshift, etc.
        ```bash
        aws configure
        ```
    * **IAM Roles:** Detail the specific IAM roles required for Lambda, Glue, and Redshift to access S3 and other services. (e.g., "Lambda execution role with S3 read/write," "Glue service role with S3 and Data Catalog access," "Redshift role for S3 COPY"). You might even provide CloudFormation/Terraform snippets for these if you have them.

5.  **Spotify API Credentials:**
    * Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/).
    * Create a new application to get your `Client ID` and `Client Secret`.
    * **Store Credentials Securely:** **DO NOT** hardcode these in your code. Recommend using environment variables or AWS Secrets Manager.
        ```bash
        export SPOTIPY_CLIENT_ID="your_client_id"
        export SPOTIPY_CLIENT_SECRET="your_client_secret"
        ```
        (Or explain how to use AWS Secrets Manager if that's your chosen method).

6.  **S3 Bucket Creation:**
    * You'll need at least two S3 buckets: one for raw data and one for processed data.
    * ```bash
        aws s3 mb s3://your-raw-data-bucket-name
        aws s3 mb s3://your-processed-data-bucket-name
        ```
        (Replace with actual unique bucket names)

7.  **Redshift Cluster Setup:**
    * Briefly mention creating a Redshift cluster. You could link to AWS documentation or provide high-level steps (e.g., "Provision a Redshift cluster," "Ensure security groups allow access from Glue/your network").
    * Mention creating the necessary tables in Redshift (e.g., `CREATE TABLE` statements for your processed Spotify data). You might have these DDLs in a `sql/` directory.

8.  **Airflow Setup:**
    * For local development, you might run Airflow via Docker. Provide Docker Compose instructions.
    * For production, briefly mention managed Airflow (MWAA) or a dedicated EC2 setup.
    * Explain how to deploy your Airflow DAGs to the Airflow environment.
    * Detail any Airflow connections (e.g., S3 connection, Spotify API connection if Airflow directly invokes it, or simply a connection for the Lambda trigger).

### Running the Pipeline

Once everything is set up:

1.  **Deploy Airflow DAG:** Place your Airflow DAG file (`spotify_pipeline_dag.py` or similar) into your Airflow DAGs folder.
2.  **Enable and Trigger DAG:** Access the Airflow UI, enable the `Global_Beats_Pipeline` DAG, and trigger it manually for the first run, or wait for its scheduled execution.
3.  **Monitor Progress:** Monitor the execution in the Airflow UI, CloudWatch logs, and check your S3 buckets and Redshift tables for data.

