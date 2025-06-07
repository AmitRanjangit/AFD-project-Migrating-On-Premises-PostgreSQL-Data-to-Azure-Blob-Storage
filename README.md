# AFD-project-Migrating-On-Premises-PostgreSQL-Data-to-Azure-Blob-Storage

This project demonstrates how to use Azure Data Factory (ADF) to migrate structured data from a PostgreSQL database (on-premises or cloud-hosted) to Azure Blob Storage in a delimited text format. The goal is to set up a scalable and automated pipeline for periodic data extraction and archiving.

## Project Structure

- pipeline1.json – ADF pipeline that performs the copy operation
- datasets/ – Contains linked dataset definitions for PostgreSQL source and Blob Storage sink
- integrationRuntime/ – Configuration for Self-Hosted Integration Runtime (if used)
- linkedServices/ – Connections to PostgreSQL and Blob Storage

## Technologies Used

- Azure Data Factory (ADF)
- Azure Blob Storage
- PostgreSQL
- Self-hosted Integration Runtime (for on-prem or private network PostgreSQL)

## Pipeline Workflow

### Step-by-Step Process

1. Integration Runtime Configuration  
   Self-Hosted IR (ironprempsql) is configured to securely access the PostgreSQL instance from ADF.

2. Source Dataset: PostgreSqlTable1  
   Reads data from a table or view in PostgreSQL.  
   Uses PostgreSqlV2Source with a query timeout of 2 hours.

3. Sink Dataset: DelimitedText1  
   Data is written to Azure Blob Storage in .txt format.  
   All fields are quoted using quoteAllText: true.

4. Copy Activity: Copy data1  
   Executes the pipeline to extract from PostgreSQL and load into Blob Storage.  
   Uses TabularTranslator for type conversion (with truncation allowed).

## Key Configuration Details

| Setting                | Value                                 |
|------------------------|----------------------------------------|
| Source Type            | PostgreSqlV2Source                     |
| Sink Type              | DelimitedTextSink                     |
| Blob Format            | .txt with all fields quoted           |
| Timeout                | 2 hours (query), 12 hours (activity)  |
| Retry                  | 0 (no retries configured)             |
| Translator             | Tabular, with type conversion         |

## Prerequisites

- A running PostgreSQL server (local or cloud)
- Azure Blob Storage account and container
- Azure Data Factory instance
- Self-hosted Integration Runtime installed and running (if PostgreSQL is on-prem)
- Firewall rules allowing ADF access to Blob and database

## Use Cases

- Periodic backups of transactional data
- Data lake ingestion from relational databases
- Archiving legacy system data to Azure

## Output

- The resulting files are saved in the specified Blob Storage container
- Format: .txt, delimited, with all text fields quoted for consistency

## Related Files

- pipeline1.json – Defines the full copy pipeline logic
- datasets/PostgreSqlTable1.json – Source dataset
- datasets/DelimitedText1.json – Sink dataset
- linkedServices/PostgreSqlls.json – PostgreSQL linked service
- linkedServices/AzureBlobStorage.json – Blob Storage linked service

## Future Improvements

- Parameterize dataset and pipeline for dynamic table names
- Add data validation and logging steps
- Schedule trigger for periodic ETL

## Author

Amit Ranjan  
GitHub: [https://github.com/AmitRanjangit](https://github.com/AmitRanjangit)

