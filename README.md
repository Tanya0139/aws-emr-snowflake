
# 🏠 redfin-real-estate-pipeline

Real estate data pipeline using Apache Airflow, Amazon EMR and Snowflake. Auto-ingestion using Snowpipe and insightful dashboard for 5M+ listings from Redfin S3 data.

---

## 🖼️ Gallery – Quick Preview
**Dashboard**  
![image](https://github.com/user-attachments/assets/6af5590e-0234-4692-ba3a-5e8f3bb18afc)



**Airflow DAG**  
![Airflow DAG](https://github.com/Tanya0139/aws-emr-snowflake/blob/main/screenshots-from-project/airflow.png)

**S3 Bucket Event Alert**  
![Bucket Event Alert](https://github.com/Tanya0139/aws-emr-snowflake/blob/main/screenshots-from-project/bucket-event-alert.png)

**EC2 Instance**  
![EC2 Instance](https://github.com/Tanya0139/aws-emr-snowflake/blob/main/screenshots-from-project/ec2-instance.png)

**EMR Cluster**  
![EMR Cluster](https://github.com/Tanya0139/aws-emr-snowflake/blob/main/screenshots-from-project/emr-cluster.png)

**EMR Cluster Steps**  
![EMR Cluster Steps](https://github.com/Tanya0139/aws-emr-snowflake/blob/main/screenshots-from-project/emr-cluster-steps.png)

**Steps in EMR Cluster**  
![Steps EMR Cluster](https://github.com/Tanya0139/aws-emr-snowflake/blob/main/screenshots-from-project/steps-emr-cluster.png)

**Snowflake Snowpipe Auto-Ingest**  
![Snowflake Snowpipe](https://github.com/Tanya0139/aws-emr-snowflake/blob/main/screenshots-from-project/snowflake-snowpipe-anr.png)

---

## 🛠️ Project Overview

This project fetches real estate data from **Redfin’s public S3 bucket**, transforms it using **Apache Spark on EMR**, and loads it into **Snowflake** with **Snowpipe** for auto-ingestion. The final output powers a rich dashboard for city, state, and property-type-based analytics.

---

## 🧰 Tech Stack

- **Redfin S3 Data** – Source listings (5M+ entries)
- **Apache Airflow** – Pipeline orchestration
- **Amazon EMR + PySpark** – Data transformation
- **Snowflake** – Data warehouse
- **Snowpipe** – Automated data loading
- **Power BI / Tableau** – Dashboard visualization

---

## 📡 Architecture

```mermaid
graph TD;
  S3[Redfin S3 Bucket] -->|Daily Sync| Airflow
  Airflow -->|Launch Job| EMR
  EMR -->|PySpark Transform| CleanedData
  CleanedData -->|Stage| Snowflake_Stage
  Snowflake_Stage -->|Auto Ingest| Snowpipe
  Snowpipe -->|Warehouse Load| SnowflakeTable
  SnowflakeTable -->|Power Insights| Dashboard
```

---

## 🚀 Setup Guide

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/redfin-real-estate-pipeline.git
cd redfin-real-estate-pipeline
```

### 2. Set Environment Variables

Create a `.env` file:

```env
AWS_ACCESS_KEY_ID=your_aws_key
AWS_SECRET_ACCESS_KEY=your_aws_secret
SNOWFLAKE_USER=your_username
SNOWFLAKE_PASSWORD=your_password
SNOWFLAKE_ACCOUNT=your_account
```

---

## ⛅ Airflow DAG Sample

```python
from airflow import DAG
from airflow.providers.amazon.aws.operators.emr import EmrAddStepsOperator
# DAG sample continues...
```
