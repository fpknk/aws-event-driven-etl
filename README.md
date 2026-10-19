# AWS Event-Driven ETL Pipeline

An end-to-end Data Engineering project using AWS, Python, and Pandas.

The pipeline automatically reads customer orders from S3, converts them into a clean table, and saves them as Parquet files. The data can then be analyzed using SQL in Amazon Athena.

## 1. Project Architecture

```text
Customer Orders (JSON)
        ↓
Amazon S3 (Raw Data)
        ↓
AWS Lambda (Python + Pandas)
        ↓
Amazon S3 (Parquet Files)
        ↓
AWS Glue Crawler
        ↓
AWS Glue Data Catalog
        ↓
Amazon Athena (SQL)
```

## 2. Technologies Used

| Technology | Purpose |
|---|---|
| Amazon S3 | Stores raw and processed files |
| AWS Lambda | Automatically runs Python code |
| Python + Pandas | Reads and transforms the data |
| Boto3 | Connects Python to AWS |
| Parquet | Stores data in an efficient format |
| AWS Glue | Finds the columns and data types |
| Amazon Athena | Analyzes data using SQL |
| CloudWatch | Shows logs and errors |
| IAM | Controls AWS permissions |

## 3. How the Pipeline Works

### Step 1 — Extract

- A JSON file is uploaded to Amazon S3.
- S3 automatically triggers AWS Lambda.
- Lambda reads the file using Python and Boto3.

### Step 2 — Transform

- Pandas converts nested JSON into a table.
- Each purchased product becomes a separate row.
- The result contains **23 rows and 12 columns**.

**Example:**

Before: One order contains a mouse and a keyboard.

After:

| order_id | customer | product |
|---|---|---|
| 1 | John | Mouse |
| 1 | John | Keyboard |

### Step 3 — Load

- Python converts the table into a Parquet file.
- Lambda saves the file in S3.
- The filename includes a timestamp.

### Step 4 — AWS Glue

- Glue Crawler scans the Parquet file.
- It finds the columns and data types.
- It saves this information in the Glue Data Catalog.

### Step 5 — Amazon Athena

Athena uses SQL to analyze the Parquet data stored in S3.

Example:

```sql
SELECT COUNT(*) AS total_rows
FROM orders_parquet_datalake;
```

Result: **23 rows**.

## 4. Project Files

```text
aws-event-driven-etl/
├── data/
│   └── orders_etl.json
├── flatten_data.ipynb
├── lambda_function.py
├── .gitignore
└── README.md
```

## 5. Results

- Processed 12 customer orders into 23 rows.
- Automated JSON processing using S3 and Lambda.
- Saved the processed data as Parquet.
- Created a table using AWS Glue.
- Successfully queried the data using Athena.

The ETL process runs automatically when a JSON file is uploaded. The Glue Crawler is currently run manually.

## 6. What I Learned

- **ETL:** Extract, Transform, Load.
- **Event-driven pipeline:** Runs automatically when new data arrives.
- **AWS Lambda:** Runs Python code without managing servers.
- **JSON flattening:** Converts nested JSON into a table.
- **Parquet:** Efficient format for storing analytical data.
- **AWS Glue:** Discovers and records the data structure.
- **Amazon Athena:** Uses SQL to query files stored in S3.

## 7. Future Improvements

- Automatically run Glue after Lambda.
- Add checks for missing or incorrect data.
- Prevent duplicate records.
- Add alerts when the pipeline fails.