# Online Retail Data Cleaning

A comprehensive Python-based data cleaning and preprocessing project using the Online Retail transactional dataset.

## 📌 Project Overview

This project focuses on assessing, cleaning, validating, and preprocessing an online retail transaction dataset before downstream data analysis.

The workflow covers data quality assessment, missing-value analysis, duplicate detection, transaction validation, outlier investigation, temporal feature extraction, and final dataset validation.

## 📊 Dataset

The dataset contains online retail transactions with the following fields:

- InvoiceNo
- StockCode
- Description
- Quantity
- InvoiceDate
- UnitPrice
- CustomerID
- Country

### Initial Dataset

- **Records:** 541,909
- **Columns:** 8
- **Countries:** 38
- **Unique invoices:** 25,900
- **Unique stock codes:** 4,070
- **Date range:** December 2010 – December 2011

## 🎯 Objectives

- Assess the overall quality of the dataset
- Identify missing values
- Detect duplicate records
- Investigate negative quantities and cancelled transactions
- Identify invalid UnitPrice values
- Investigate extreme Quantity and UnitPrice values
- Identify non-product and administrative transactions
- Validate InvoiceDate values
- Create useful temporal features
- Produce a reliable processed dataset for further analysis

## 🔍 Data Quality Analysis

The initial analysis identified:

| Issue | Finding |
|---|---:|
| Missing CustomerID | 135,080 |
| Missing Description | 1,454 |
| Duplicate records | 5,268 |
| Negative Quantity records | 10,587 |
| Negative UnitPrice records | 2 |
| Zero UnitPrice records | 1,174 |
| Future InvoiceDate records | 0 |
| Invalid/missing InvoiceDate records | 0 |

## 🧹 Cleaning & Preprocessing

### 1. Missing Values

CustomerID values were not artificially imputed because customer identity cannot reliably be inferred from the available transaction fields.

Missing Description values were retained where the information was unavailable.

### 2. Duplicate Records

Duplicate records were identified and investigated before removal.

A total of **5,268 duplicate records** were identified.

### 3. Negative Quantities

Negative quantities were investigated in relation to cancelled invoices.

The analysis found:

- Negative quantity records: **10,587**
- Negative quantities associated with cancelled invoices: **9,251**
- Negative quantities without a cancelled invoice: **1,336**

Cancelled transactions were retained because they represent meaningful transaction history rather than automatically treating them as erroneous records.

### 4. Invalid Unit Prices

Two records contained negative UnitPrice values.

These records represented invalid pricing values and were removed.

Zero-price transactions were investigated separately because some may represent legitimate free or administrative transactions.

### 5. Quantity Outliers

The IQR method identified **57,598 potential Quantity outliers**.

However, extreme quantities were not automatically removed because high-volume transactions can represent legitimate bulk purchases.

The extreme values were therefore retained when there was insufficient evidence that they were erroneous.

### 6. UnitPrice Outliers

The IQR analysis produced:

- Q1: **1.25**
- Q3: **4.13**
- IQR: **2.88**
- Upper bound: **8.45**
- Potential UnitPrice outliers: **39,448**
- Potential outlier percentage: **7.37%**

Further investigation showed that many high UnitPrice values were associated with administrative or non-product transactions such as:

- POSTAGE
- DOTCOM POSTAGE
- Manual
- AMAZON FEE
- Adjust bad debt

These records were investigated rather than blindly removed.

### 7. Non-Product Transactions

Administrative and non-product transactions were identified using StockCode and Description patterns.

Examples include:

- POSTAGE
- DOTCOM POSTAGE
- AMAZON FEE
- Manual
- Adjust bad debt

These transactions were flagged using an `IsNonProductTransaction` field rather than automatically deleting them.

### 8. Date Validation

InvoiceDate was validated for:

- Missing values
- Invalid dates
- Future dates

Results:

- Missing InvoiceDate: **0**
- Invalid/missing InvoiceDate: **0**
- Future InvoiceDate: **0**

### 9. Temporal Feature Engineering

The following features were extracted from InvoiceDate:

- Year
- Month
- Day
- Hour
- DayOfWeek

These features enable subsequent time-based analysis.

## 🧮 Transaction Value

A `TransactionValue` feature was created using:

```text
TransactionValue = Quantity × UnitPrice