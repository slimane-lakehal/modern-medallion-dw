# Naming Conventions

This document outlines the naming conventions used for schemas, tables, views, columns, and other objects in the data warehouse.

## Table of Contents

1. [General Principles](#general-principles)
2. [Table Naming Conventions](#table-naming-conventions)
3. [Column Naming Conventions](#column-naming-conventions)
4. [Object Naming Conventions](#object-naming-conventions)

## General Principles

* Use snake_case with lowercase letters and underscores
* Use English for all names
* Avoid SQL reserved words
* Be consistent and descriptive
* Use common abbreviations only when widely understood

## Table Naming Conventions

### Bronze Layer Tables

* Maintain source system naming exactly as received
* Format: `<sourcesystem>_<entity>`
* Examples:
  * `crm_cust_info` - Customer information from CRM
  * `erp_loc_a101` - Location data from ERP

### Silver Layer Tables

* Standardize names while preserving source system context
* Format: `<sourcesystem>_<standardized_name>`
* Examples:
  * `crm_customer_info` - Standardized customer data
  * `erp_location` - Standardized location information

### Gold Layer Tables

* Use business-oriented names with appropriate prefixes
* Format: `<category>_<entity>`
* Categories:
  * `dim_` - Dimension tables
  * `fact_` - Fact tables
  * `report_` - Reporting views
* Examples:
  * `dim_customer`
  * `fact_sales`
  * `report_monthly_revenue`

## Column Naming Conventions

### Key Columns

* Surrogate Keys: Use `_key` suffix
  * `customer_key`
  * `product_key`
* Natural Keys: Use `_id` suffix
  * `order_id`
  * `customer_id`
* Foreign Keys: Use referenced table name with `_key` suffix
  * `customer_key` (in fact tables)
  * `product_key` (in fact tables)

### Business Columns

* Use clear, descriptive names
* Avoid abbreviations unless standard in business context
* Include units where applicable
* Examples:
  * `first_name`
  * `birth_date`
  * `sales_amount_usd`
  * `quantity_units`

### Technical Columns

* Prefix with `dwh_` for warehouse-specific metadata
* Examples:
  * `dwh_load_date` - Record load timestamp
  * `dwh_source` - Source system identifier
  * `dwh_version` - Version number for SCD
  * `dwh_active` - Active record flag
  * `dwh_batch_id` - ETL batch identifier

## Object Naming Conventions

### Views

* Business Views: Use purpose-based prefixes
  * `rpt_daily_sales`
  * `bi_customer_profile`
* Technical Views: Use layer prefixes
  * `silver_customer_combined`
  * `gold_product_current`

### Functions

* Scalar Functions: Use verb_noun format
  * `clean_customer_name`
  * `calculate_age`
* Table Functions: Add `_tf` suffix
  * `get_active_customers_tf`
  * `find_duplicates_tf`

### Stored Procedures

* ETL Procedures: Use action_target format
  * `load_bronze`
  * `transform_silver`
  * `build_gold`
* Utility Procedures: Use clear action verbs
  * `refresh_materialized_views`
  * `validate_data_quality`

### Indexes

* Primary Key: `pk_<table>_<column>`
  * `pk_dim_customer_customer_key`
* Foreign Key: `fk_<table>_<referenced_table>`
  * `fk_fact_sales_dim_customer`
* Non-Clustered: `ix_<table>_<columns>`
  * `ix_fact_sales_order_date`
* Unique: `uq_<table>_<columns>`
  * `uq_dim_customer_customer_id`
