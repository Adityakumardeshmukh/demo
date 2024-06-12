# Extract Data from BigQuery to SQL Database

This guide explains how to extract data from Google BigQuery and save it to a local SQL database using Python. The script uses libraries like BigQuery, SQLAlchemy, and Pandas.

## Prerequisites

Before running the script, ensure you have:
1. **Google Cloud Service Account Key**: A JSON key file for a Google Cloud service account with access to BigQuery.
2. **Python Environment**: Python installed with the required libraries.

## Requirements

Install the necessary Python packages by running:
```sh
pip install -r requirements.txt
```

**requirements.txt**:
```
cachetools==5.3.3
certifi==2024.6.2
charset-normalizer==3.3.2
google-api-core==2.19.0
google-auth==2.30.0
google-cloud-bigquery==3.24.0
google-cloud-core==2.4.1
google-crc32c==1.5.0
google-resumable-media==2.7.0
googleapis-common-protos==1.63.1
greenlet==3.0.3
grpcio==1.64.1
grpcio-status==1.62.2
idna==3.7
numpy==1.26.4
packaging==24.0
pandas==2.2.2
proto-plus==1.23.0
protobuf==4.25.3
pyasn1==0.6.0
pyasn1_modules==0.4.0
python-dateutil==2.9.0.post0
pytz==2024.1
requests==2.32.3
rsa==4.9
six==1.16.0
SQLAlchemy==2.0.30
typing_extensions==4.12.1
tzdata==2024.1
urllib3==2.2.1
```

## Script Overview

The script extracts data from Google BigQuery and writes it to a local SQL database. It performs the following steps:

1. **Authenticate**: Use the JSON key file to authenticate with Google Cloud.
2. **Run Query**: Execute a query on BigQuery.
3. **Convert Data**: Convert the query result to a Pandas DataFrame.
4. **Save Data**: Save the DataFrame to a local SQL database.

## Script

Here is the complete script:

```python
from google.cloud import bigquery
import pandas as pd
from sqlalchemy import create_engine

def extract_bigquery_to_sql(json_key_path, project_id, query, destination_table, sql_connection_string):
    # Authenticate using the JSON key file
    client = bigquery.Client.from_service_account_json(json_key_path, project=project_id)

    # Run the query
    query_job = client.query(query)

    # Convert the result to a Pandas DataFrame
    df = query_job.to_dataframe()

    # Write the DataFrame to the local SQL database
    engine = create_engine(sql_connection_string)
    df.to_sql(destination_table, engine, if_exists='replace', index=False)

    print(f'Data successfully extracted from BigQuery and written to SQL table "{destination_table}".')

# Example usage:
json_key_path = "path/to/your/json/key.json"
project_id = "your-project-id"
query = "SELECT * FROM your_dataset.your_table"
destination_table = "local_table_name"
sql_connection_string = "sqlite:///path/to/your/sqlite/database.db"

extract_bigquery_to_sql(json_key_path, project_id, query, destination_table, sql_connection_string)
```

### Parameters

- **json_key_path**: Path to the JSON key file for the Google Cloud service account.
- **project_id**: Google Cloud project ID.
- **query**: SQL query to run on BigQuery.
- **destination_table**: Name of the table in the local SQL database where data will be saved.
- **sql_connection_string**: Connection string for the local SQL database (example uses SQLite).

### Example Usage

Replace the placeholders with your actual values:

```python
json_key_path = "path/to/your/json/key.json"
project_id = "your-project-id"
query = "SELECT * FROM your_dataset.your_table"
destination_table = "local_table_name"
sql_connection_string = "sqlite:///path/to/your/sqlite/database.db"

extract_bigquery_to_sql(json_key_path, project_id, query, destination_table, sql_connection_string)
```

## Conclusion

This script makes it easy to transfer data from Google BigQuery to a local SQL database. Make sure you have the prerequisites, install the necessary Python packages, and update the example usage with your specific details.
