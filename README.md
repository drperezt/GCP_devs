# Enterprise FP&A Data Pipeline on GCP & BigQuery

[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Google Cloud Platform](https://img.shields.io/badge/GCP-BigQuery%20%7C%20Colab-4285F4?style=flat&logo=googlecloud&logoColor=white)](https://cloud.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An end-to-end financial data engineering and FP&A modeling pipeline designed to automate headcount salary run-rates, benefit ratio allocations, dynamic inflation compounding, and incremental financial reporting.

This repository demonstrates enterprise data architecture patterns using **Google Colab**, **Google Cloud Platform (GCP)**, and **BigQuery**, bridging dynamic business parameters with automated cloud data processing.

> **Note:** All code, table schemas, proprietary business logic, and financial configurations in this repository have been fully anonymized and sanitized to adhere to enterprise security and confidentiality protocols.

---

## 📋 Executive Summary & Problem Statement

Traditional FP&A workflows often rely on static spreadsheets prone to manual errors, version drift, and slow execution when processing large General Ledger datasets. 

This project unifies Python data engineering with scalable BigQuery SQL operations to create an automated, idempotent financial modeling engine.

### Key Capabilities
* **Automated Run-Rate Projections:** Dynamically projects base salaries, step-function wage increases, statutory benefits, and flexible vouchers.
* **Low-Code Parameter Control:** Enables business users to manage dynamic assumptions (inflation rates, cost center mappings) via Google Sheets without touching underlying pipeline code.
* **Targeted Delta Reprocessing:** Implements idempotent sub-segment reprocessing (partition-aware `DELETE`/`INSERT` and `MERGE` routines) to update dynamic forecasts without full-table scans.
* **Unified GCP Security:** Operates with native Application Default Credentials (ADC) to ensure safe, keyless authentication between Colab, Google Drive, and BigQuery.

---

## 🏗 System Architecture

```mermaid
flowchart TD
    classDef source fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef param fill:#fff3e0,stroke:#f57c00,stroke-width:2px;
    classDef engine fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;
    classDef storage fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef output fill:#efebe9,stroke:#5d4037,stroke-width:2px;

    subgraph Inputs["1. Data Sources & Inputs"]
        A1[Accounting Journal Actuals]:::source
        A2[Google Sheets Config Master]:::source
        A3[Headcount & Benefits Rosters]:::source
    end

    subgraph Config["2. Dynamic Configuration"]
        B1[Parameters: Inflation, Wage % Increases]:::param
        B2[Master Catalogs: CECO, CEBE, Accounts]:::param
    end

    subgraph Processing["3. Core Calculation Engine"]
        C1[Historics & Actuals Engine]:::engine
        C2[Base Salary & Payroll Projections]:::engine
        C3[Vouchers & Flexible Benefits]:::engine
        C4[Historical Benefit Ratio Allocations]:::engine
    end

    subgraph Storage["4. Staging & Delta Reprocessing"]
        D1[Targeted Sub-Segment Updates]:::storage
        D2[BigQuery Financial Budget Datasets]:::storage
    end

    subgraph BI["5. Business Intelligence"]
        E1[Consolidated Forecast Data Marts]:::output
    end

    A1 --> C1
    A2 --> B1
    A3 --> C3
    B1 --> C2
    B1 --> C3
    B2 --> C1
    B2 --> C2
    C1 --> C4
    C2 --> C4
    C1 --> D2
    C2 --> D2
    C3 --> D2
    C4 --> D2
    D2 --> D1
    D2 --> E1
