# Case Study: Audited Dynamic CSV to Database Loader with Context-driven Target and Logging

## Project Overview
This Talend job reads data from one or more CSV files and dynamically determines the target database table name via context variables initialized within the job. It performs data transformations including enrichment through a lookup table and loads the results into the target database. The job includes comprehensive auditing by logging source and target record counts, job start and end times, and captures error and performance statistics to an audit table.

---

## Functional Requirements & Components Used

- **Job Initialization and Completion**  
  - `tPreJob` to initialize job processing  
  - `tPostJob` for cleanup and post-processing  

- **Database Connection Management**  
  - `tDBConnection` to establish DB connection using context variables loaded dynamically  
  - `tDBClose` to properly close the database connection at job end  

- **Context Variable Handling**  
  - `tJava` and `tJavaRow` for creating and initializing context variables dynamically, including the target table name  

- **Input and Lookup Data**  
  - File components (e.g., `tFileInputDelimited`) to read CSV input data  
  - `tDBInput` to read from a lookup table for enriching source data  

- **Data Transformation**  
  - `tMap` to map, transform, filter, and enrich data sets using lookup results  
  - `tJavaRow` for custom row-level Java transformation if needed  

- **Database Writing**  
  - `tDBOutput` to insert or update data into the target table specified by context variable  
  - `tDBRow` for running any custom SQL statements if necessary  

- **Error and Metrics Capture**  
  - `tLogCatcher` to capture and log runtime errors for troubleshooting  
  - `tStatCatcher` to gather job statistics such as source and target row counts, job start and end timestamps  

- **Audit Logging**  
  - Auditing job execution details by writing captured statistics into an audit table with columns:  
    `job_name`, `source_name`, `target_name`, `source_count`, `target_count`, `job_start`, `job_end`  
  - Use of `tDBOutput` to insert audit records  

---

## Audit Table Schema Example

| Column Name  | Data Type    | Description                      |
|--------------|--------------|---------------------------------|
| job_name     | VARCHAR(100) | Name of the Talend job          |
| source_name  | VARCHAR(100) | Name or description of data source |
| target_name  | VARCHAR(100) | Target database table name      |
| source_count | INTEGER      | Number of records read          |
| target_count | INTEGER      | Number of records written       |
| job_start    | TIMESTAMP    | Job start timestamp             |
| job_end      | TIMESTAMP    | Job end timestamp               |

---

## Job Flow Summary

### Initialization and Connection Setup
- **tPreJob:** Entry point for job initialization.
- **tJava:** Initializes context variables such as target table name, CSV file path, and database connection credentials dynamically within the job.
- **tDBConnection:** Opens the database connection using the context variables to allow dynamic configuration.

### Data Reading and Transformation
- **tDBInput:** Reads lookup data from a reference table to enrich incoming records.
- **tFileInputDelimited:** Reads data from CSV file(s).
- **tJavaRow:** Applies any custom Java logic for row-level processing.
- **tMap:** Performs data mapping, filtering, and enrichment using the lookup data.

### Data Loading and Custom SQL Execution
- **tDBOutput:** Inserts or updates processed data into the target table, the name of which is provided via context variables.
- **tDBRow:** Executes any custom SQL statements needed for advanced processing or housekeeping.

### Logging, Auditing, and Cleanup
- **tLogCatcher:** Captures runtime errors and exceptions.
- **tStatCatcher:** Collects job statistics including source and target record counts and job start/end timestamps.
- **tDBOutput:** Writes auditing metrics captured by tStatCatcher into an audit table with columns [job_name, source_name, target_name, source_count, target_count, job_start, job_end].
- **tDBClose:** Closes the database connection gracefully.
- **tPostJob:** Marks job completion and cleanup.

flowchart TD
A[tPreJob] --> B[tJava: Initialize Context Variables]
B --> C[tDBConnection: Open DB Connection]
C --> D[tDBInput: Load Lookup Table Data]
D --> E[tFileInputDelimited: Read CSV Files]
E --> F[tJavaRow: Custom Row Logic]
F --> G[tMap: Transform and Enrich Data]
G --> H[tDBOutput: Write to Context-driven Target Table]
H --> I[tDBRow: Optional Custom SQL]
I --> J[tLogCatcher: Error Logging]
J --> K[tStatCatcher: Metrics Collection]
K --> L[tDBOutput: Write Audit Records]
L --> M[tDBClose: Close DB Connection]
M --> N[tPostJob]