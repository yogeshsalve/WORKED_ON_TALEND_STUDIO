# Talend Job: Load Multiple CSV Files into SQL Server

## Overview

This Talend job is designed to dynamically read all CSV files from a specified directory and load their contents into a target SQL Server database table. It demonstrates how Talend can handle batch processing of multiple files seamlessly.

## Components Used

- **tFileList**: Lists all the CSV files in the input directory.
- **tFileInputDelimited**: Reads the content of each CSV file dynamically.
- **tDBOutput**: Writes the data into the SQL Server target table.
- **Iterate connections**: Controls the flow of processing multiple files one by one.

## Job Workflow

1. **tFileList** scans the directory for files matching the pattern (e.g., `*.csv`).
2. For each file, the job iterates via the **Iterate** connection.
3. **tFileInputDelimited** reads the current CSV file using the dynamic file path.
4. Data is streamed and inserted into the SQL Server table via **tDBOutput**.
5. The process repeats until all files in the directory are processed.

## Usage Instructions

1. Place all CSV files to be loaded in the configured directory.
2. Configure the directory path in the **tFileList** component.
3. Configure the database connection properties in **tDBOutput** for your SQL Server instance.
4. Verify the schema matches the CSV file structure and SQL Server table structure.
5. Run the job in Talend Studio.
6. Monitor logs for successful data loading of all files.

## Benefits

- Automates loading of multiple files without manual intervention.
- Supports scalable batch processing.
- Reduces manual data import errors.
- Reusable in different environments via context variables.

## Notes

- Ensure all CSV files have consistent schema/format.
- Validate SQL Server table schema compatibility.
- Adjust batch size or commit interval in tDBOutput for performance tuning.
- Add error handling components (optional) for robust processing.

---

For any questions or support, please contact YOGESH SALVE at yogeshbalkrishnasalve@gmail.com.

