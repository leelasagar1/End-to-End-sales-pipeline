# Azure Sales Data ETL Pipeline Project

## Overview
This project implements an end-to-end ETL (Extract, Transform, Load) pipeline for sales data using Azure Data Factory, Azure SQL Database, Azure Data Lake Storage, and Databricks. The solution demonstrates modern data engineering practices including incremental loading, medallion architecture (bronze, silver, gold layers), and dimensional modeling.

## Architecture
![Azure Data Pipeline Architecture]()

## Technologies Used
- **Azure Data Factory**: Orchestration of data pipelines
- **Azure SQL Database**: Source system for transactional data
- **Azure Data Lake Storage**: Data lake implementation with bronze/silver/gold layers
- **Azure Databricks**: Data transformation and dimensional modeling
- **GitHub**: Source control and project management

## Pipeline Implementation

### 1. Source Prep Pipeline
This pipeline extracts data from GitHub source repositories and loads it into Azure SQL Database.

**Key Features:**
- Source data extraction from GitHub
- Initial data load into Azure SQL Database
- Data validation and consistency checks

### 2. Incremental Load Pipeline
This pipeline incrementally moves data from SQL Database to Data Lake, implementing an efficient change data capture approach.

**Pipeline Activities:**
1. **last_load (Lookup)**: Retrieves the timestamp of the last successful data load from the watermark table
2. **current_load (Lookup)**: Identifies the maximum date from the source data to determine new records
3. **copy_incremental_data (Copy Data)**: Transfers only new/changed records to the bronze layer of the data lake
4. **watermarkUpdate (Stored Procedure)**: Updates the watermark table with the latest processed timestamp

**Incremental Load Implementation:**
- Watermark table tracks the last processed data timestamp
- Each pipeline run only processes new or changed records
- Optimizes performance and reduces unnecessary data transfer

## Databricks Data Processing

### Bronze to Silver Layer Transformation
The silver layer notebook implements data quality rules, standardization, and business logic:
- Data cleansing and validation
- Schema standardization
- Business rule application
- Error handling logic

### Silver to Gold Layer (Dimensional Model)
The gold layer implements a dimensional model with 4 dimension tables and 1 fact table:

**Dimension Tables:**
- Customer Dimension
- Product Dimension
- Date Dimension
- Location Dimension

**Fact Table:**
- Sales Fact Table

**Key Implementation Features:**
- Separate notebook for each dimension and fact table
- Handles both initial load and incremental updates
- Implements SCD Type-1 (Slowly Changing Dimension) pattern
- Optimized for query performance

## Data Modeling
The dimensional model follows a star schema design to optimize analytical queries and reporting:
