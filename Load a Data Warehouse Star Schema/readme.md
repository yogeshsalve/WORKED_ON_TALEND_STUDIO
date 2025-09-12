# 🚀 Case Study: Load a Data Warehouse Star Schema with Talend

## 📌 Business Problem
A retail company wants to build a **Sales Data Warehouse** for reporting and analytics.  
Data originates from **e-commerce transactions API (orders)** and must be transformed into a **Star Schema**.

---

## 🏗️ Target Data Warehouse Schema

- **Fact Table**
  - `SalesFact`
- **Dimensions**
  - `CustomerDim`
  - `ProductDim`
  - `DateDim`

---

## 🎯 Goals

✔ **Incremental Load** – only new/changed data should be loaded  
✔ **Surrogate Keys** – use generated keys for dimension tables  
✔ **Optimized for BI Dashboards** – ensure schema is reporting-ready  

---

## ⚙️ ETL Workflow (Talend)

1. **Extract**
   - Connect to **e-commerce Orders API**
   - Fetch data incrementally (using `last_updated` or timestamp field)

2. **Transform**
   - Clean & standardize customer, product, and date fields  
   - Generate **surrogate keys** for dimensions  
   - Ensure referential integrity between Fact & Dimension tables  

3. **Load**
   - Insert into **dimension tables** (`CustomerDim`, `ProductDim`, `DateDim`)  
   - Insert into **fact table** (`SalesFact`) with surrogate key references  

---


## 🏗️ Target Schema Design

### Dimension Tables
```sql
CREATE TABLE CustomerDim (
    CustomerKey INT IDENTITY(1,1) PRIMARY KEY,
    CustomerID VARCHAR(50),
    CustomerName VARCHAR(100),
    City VARCHAR(100),
    Country VARCHAR(100),
    EffectiveDate DATETIME,
    ExpiryDate DATETIME,
    IsActive BIT
);

CREATE TABLE ProductDim (
    ProductKey INT IDENTITY(1,1) PRIMARY KEY,
    ProductID VARCHAR(50),
    ProductName VARCHAR(100),
    Category VARCHAR(100),
    EffectiveDate DATETIME,
    ExpiryDate DATETIME,
    IsActive BIT
);

CREATE TABLE DateDim (
    DateKey INT PRIMARY KEY,
    FullDate DATE,
    Year INT,
    Month INT,
    Day INT,
    Quarter INT
);

```

### Fact Table
```sql
CREATE TABLE SalesFact (
    SalesFactKey BIGINT IDENTITY(1,1) PRIMARY KEY,
    OrderID VARCHAR(50),
    CustomerKey INT,
    ProductKey INT,
    DateKey INT,
    Quantity INT,
    Amount DECIMAL(10,2),
    FOREIGN KEY (CustomerKey) REFERENCES CustomerDim(CustomerKey),
    FOREIGN KEY (ProductKey) REFERENCES ProductDim(ProductKey),
    FOREIGN KEY (DateKey) REFERENCES DateDim(DateKey)
);
```
