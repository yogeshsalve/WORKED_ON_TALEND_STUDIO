# Talend ETL Project: Modular Job Design using tRunJob

This project demonstrates how to design modular ETL jobs in **Talend Open Studio** using `tRunJob`.  
Instead of creating a single large job, we break the workflow into **three child jobs** and control execution with a **Master Job**.

---

## 📂 Project Structure

### 1. Job A – CSV to Staging
- **Purpose:** Reads CSV files and loads raw data into the **staging table**.  
- **Components:**
  - `tFileInputDelimited` → Reads the CSV file.
  - `tMap` → Maps CSV columns to staging table fields.
  - `tDBOutput` → Inserts data into the staging table.
- **Notes:** Validates file format and handles missing columns.

---

### 2. Job B – Data Cleaning and Transformation
- **Purpose:** Cleans and transforms data in the **staging table** for consistency.  
- **Components:**
  - `tDBInput` → Reads data from staging table.
  - `tMap` → Applies transformations (e.g., uppercase names, remove duplicates, fix nulls).
  - `tDBOutput` → Updates staging or intermediate table with clean data.
- **Notes:** Use context variables to select input/output tables dynamically.

---

### 3. Job C – Load to Final Database
- **Purpose:** Loads clean, transformed data from staging into the **final target database**.  
- **Components:**
  - `tDBInput` → Reads clean data from staging.
  - `tMap` → Applies final business transformations if needed.
  - `tDBOutput` → Inserts data into the final fact/dimension tables.
- **Notes:** Ensure primary key constraints and indexes are applied.

---

### 4. Master Job – Orchestrator
- **Purpose:** Executes Job A → Job B → Job C **sequentially** using `tRunJob`.  
- **Components:**
  - `tRunJob (Job A)` → OnSubjobOk → `tRunJob (Job B)` → OnSubjobOk → `tRunJob (Job C)`
- **Features:**
  - Sequential execution with **error handling**.
  - Pass **context variables** to child jobs for dynamic configuration.
  - Modular and reusable design.
- **Diagram:**

