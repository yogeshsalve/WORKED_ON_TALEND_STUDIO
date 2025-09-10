# Case Study: E-commerce Order Data Warehouse (Incremental Load)

## Project Overview
This Talend job reads data from one or more CSV files and dynamically determines the target database table name via context variables initialized within the job. It performs data transformations including enrichment through a lookup table and loads the results into the target database. The job includes comprehensive auditing by logging source and target record counts, job start and end times, and captures error and performance statistics to an audit table.

## Business Context

An e-commerce company has:

A transactional database (MySQL/Oracle/SQL Server) that stores orders (millions of rows, updated constantly).

A data warehouse (Snowflake/Redshift/BigQuery) used for analytics, dashboards, and reporting.

Analysts & business teams need near real-time insights: sales today, top customers, delayed shipments, etc.

## Challenge

The orders table is huge (100M+ rows).

Running a full load every hour/day would be extremely slow & costly.

Only the new or updated orders need to be processed.

## tables - queries

CREATE TABLE ecommerceorders (
    order_id BIGINT PRIMARY KEY,
    customer_id BIGINT,
    order_date DATETIME2 NOT NULL,
    order_status VARCHAR(50),
    last_updated DATETIME2 DEFAULT SYSDATETIME()
);

CREATE TABLE ecommerceorders_etl_control (
    control_id INT IDENTITY(1,1) PRIMARY KEY,
    source_table VARCHAR(100) NOT NULL,
    last_extraction_time DATETIME2 NOT NULL,
    remarks VARCHAR(255),
    updated_at DATETIME2 DEFAULT SYSDATETIME()
);