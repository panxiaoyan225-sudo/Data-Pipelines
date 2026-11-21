## Data Pipelines: Reliable Ingestion, Validation, and Automation

This repository demonstrates practical data-engineering patterns for moving information from source systems into structured, usable datasets.

It brings together **API ingestion, SQL processing, data-quality validation, database connectivity, monitoring, and workflow automation**. The emphasis is on building dependable data workflows with appropriate tools—not adding infrastructure for its own sake.

[**View the Data Pipeline Infographic**](https://panxiaoyan225-sudo.github.io/Data-Pipelines/data_infographic.html)

## Why this work matters

Analytical and AI systems are only as dependable as the data they receive. Data pipelines therefore need to do more than move records. They must handle changing inputs, protect credentials, validate structure and logic, manage failures, and make execution observable.

This repository explores those responsibilities through three implementation patterns:

1. Local automation using native Windows tools.
2. Database-to-database processing using SQL.
3. Automated execution through Azure DevOps and GitHub Actions.

## Architecture

The workflow is organized into four practical layers:

| Layer | Responsibility |
|---|---|
| Ingestion | Retrieve data from REST APIs and flat files |
| Validation | Check schema integrity and logical consistency |
| Persistence and linkage | Store structured data and connect relational databases |
| Monitoring and audit | Record execution, detect selected anomalies, and issue alerts |

A custom validation module (`data_validator.py`) is used to enforce data-quality checks. The repository also includes Slack-integrated monitoring for notifications.

## Orchestration patterns

### 1. Local automation: Windows Task Scheduler

A master `run_all_pipelines.bat` script coordinates local execution. This approach is useful for lightweight workflows where native scheduling is sufficient.

### 2. Azure DevOps CI/CD

An earlier implementation uses `azure-pipelines.yml` and a self-hosted agent. Sensitive values are supplied through Azure DevOps variable groups rather than hard-coded in scripts.

### 3. GitHub Actions with a self-hosted Windows runner

The repository also demonstrates GitHub Actions orchestration using a self-hosted Windows runner configured as a service.

The workflow separates orchestration from the main pipeline scripts and uses repository secrets for credentials. The implementation includes environment and directory configuration intended to make scheduled execution more stable.

## Featured implementation scenarios

### Scenario 1: Secure API ingestion

`api_pipeline.py` retrieves JSON data from a REST API using token-based authentication. Configuration is supplied outside the source code, and request handling includes timeouts and exception handling for common connection and HTTP failures.

### Scenario 2: Python-to-SQL loading

The ingestion logic is separated from the database-loading step. The SQL-loading implementation uses parameterized statements, maps nested JSON into a tabular structure, and handles transaction rollback when an operation fails.

### Scenario 3: Database-to-database processing with SQL

`database_link.sql` demonstrates cross-database operations through SQLite database attachment. It illustrates how relational data can be joined across separate database files without routing every transformation through application code.

`run_sql_link.py` provides the execution wrapper for the SQL workflow.

## Resource-conscious execution

One design consideration in this repository is choosing an execution environment that matches the workload.

A local Windows environment and persistent virtual environment can avoid the setup and resource overhead of heavier container-based workflows for suitable tasks. In the author's local comparison, moving to native execution reduced observed RAM overhead from more than 4 GB to under 200 MB. This is a result from that specific setup, not a general benchmark for all workloads.

The broader principle is simple: **reliability comes from appropriate design, validation, and orchestration—not from infrastructure size alone.**

## Security and configuration

- Keep API tokens, database passwords, and notification credentials outside source files.
- Use `.env` for local configuration where applicable.
- Use Azure DevOps variable groups or GitHub repository secrets for CI/CD credentials.
- Keep local environment files and sensitive artifacts out of version control.

## Repository contents

The repository includes pipeline scripts, SQL examples, orchestration configuration, and a visual overview. Key components include:

```text
pipeline/
├── .env                         # Local secrets/configuration; not committed
├── .gitignore
├── practice_warehouse.db        # Demonstration database
└── APIKey_DataConection/
    ├── api_pipeline.py          # REST API ingestion
    ├── Python to SQL.py         # SQL loading workflow
    ├── check_db_data.py         # Data inspection/validation
    ├── database_link.sql        # Cross-database SQL example
    └── run_sql_link.py          # SQL execution wrapper
```

Folder names and exact paths may differ between repository versions.

## Position within the broader work

This repository focuses on the data foundation: how information is acquired, checked, connected, and made available to downstream analysis.

It complements:

- [Data-Driven Foresight](https://github.com/panxiaoyan225-sudo/Data-Driven-Foresight), which uses structured historical data for predictive analysis.
- [Notebooks: Databricks and Colab](https://github.com/panxiaoyan225-sudo/Notebooks_Databricks-Colab), which compares data-processing approaches across execution environments.
- [Agentic RAG Pipeline](https://github.com/panxiaoyan225-sudo/Agentic_RAG_Pipeline), which applies retrieval and evidence evaluation to organizational knowledge.

**Reliable intelligence begins with reliable data movement and validation.**
