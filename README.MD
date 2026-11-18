# Spotify data engineering pipeline

Follow Spotify track data from ingestion to reporting. Start with the [architecture](./architecture), walk through the [implementation guide](./IMPLEMENTATION_GUIDE.md), or jump to the [Airflow DAGs](./dags) and [Athena queries](./athena).

![Tests](https://img.shields.io/badge/tests-passing-brightgreen)
![Python](https://img.shields.io/badge/python-3.9+-blue)
![AWS](https://img.shields.io/badge/AWS-S3%20%7C%20Lambda%20%7C%20Glue%20%7C%20Athena-orange)
![Databricks](https://img.shields.io/badge/Databricks-PySpark-red)
![Snowflake](https://img.shields.io/badge/Snowflake-ELT-blue)
![PowerBI](https://img.shields.io/badge/Power%20BI-Dashboard-yellow)

AWS S3 | Lambda | EventBridge | Glue | Athena | Docker Airflow | Snowflake | Databricks (PySpark) | Power BI

This project demonstrates a full, enterprise-level ETL + ELT pipeline, ingesting Spotify artist track data, transforming it across multi-cloud systems, and loading it into analytics and BI layers.

Built using 11 industry technologies, this system simulates what a real Data Engineering team builds for scalable analytics.

Note: This is a portfolio project designed to demonstrate real-world data engineering patterns, tooling integration, and production-style workflows. All infrastructure was created, validated, and deprovisioned responsibly to avoid unnecessary cloud costs.


🎯 Project Intent

This project was built to mirror how modern data engineering teams design, test, and operate analytics pipelines in production environments.

The focus areas include:
	•	End-to-end pipeline architecture
	•	Cross-platform data movement (AWS → Databricks → Snowflake → BI)
	•	Bronze / Silver / Gold data modeling
	•	Data quality validation across systems
	•	Cost-aware cloud usage and deprovisioning
	•	Clear documentation and testability

This is not a tutorial project, but a realistic, production-style system suitable for portfolio review and technical interviews.


🧠 High-Level Architecture

![Architecture Diagram](architecture/spotify_etl_architecture.png)

Full Workflow
	1.	Spotify API → Lambda (Ingestion)
	2.	Lambda → S3 (Raw/Processed Data)
	3.	Glue Crawler → Athena (Schema inference + querying)
	4.	Airflow (Docker) → Orchestrates Athena, Glue, Databricks, Snowflake jobs
	5.	Databricks (PySpark) → Bronze → Silver → Gold tables
	6.	Snowflake → ELT transformations + stored procedures + tasks
	7.	Power BI → Final visualization layer



    🛠 Tech Stack

Cloud: AWS S3, Lambda, EventBridge, Glue, Athena
Orchestration: Apache Airflow (Docker)
Processing: Databricks, PySpark
Data Warehouse: Snowflake
Analytics: Power BI
Programming: Python 3
CI/CD: Git + GitHub


📂 Repository Structure


spotify-etl-data-engineering/
│
├── src/                       # Local ingestion & transformation logic
│   ├── ingestion/
│   ├── transform/
│   └── config.py
│
├── lambda/                    # AWS Lambda ingestion & transform functions
│
├── Eventbridge/               # EventBridge schedules & setup docs
│
├── glue/
│   ├── crawler_config.json
│   ├── processed_schema.json
│   ├── transformed_schema.json
│   └── s3_samples/
│
├── athena/
│   ├── tables/
│   ├── workgroup_config.json
│   ├── results_samples/
│   └── top10_artist_stats_query.sql
│
├── databricks/
│   ├── notebooks/
│   ├── queries/
│   ├── tables/
│   └── result_samples/
│
├── snowflake/
│   ├── sql/
│   ├── queries/
│   ├── procedures/
│   ├── tables/
│   └── results_samples/
│
├── dags/                      # Airflow DAGs
│
├── s3/                        # Local S3 object samples
│
├── powerbi/                   # Power BI dashboards & screenshots
│
├── IMPLEMENTATION_GUIDE.md    # Testing & validation notes
├── PERFORMANCE_METRICS.md     # Performance & cost observations
└── README.md               



🎧 Spotify Ingestion

Python | Spotify API

Features
	•	Fetches top tracks from multiple artists
	•	Automatically retrieves OAuth access tokens
	•	Saves raw and processed CSV outputs
	•	Fully environment-variable driven (no secrets in repo)

AWS Lambda Ingest
	•	Calls Spotify API
	•	Cleans and structures data
	•	Writes processed output to S3 (spotify/processed/)

EventBridge Integration
	•	Scheduled hourly execution
	•	JSON configuration included
	•	End-to-end setup documentation provided


☁ AWS Data Layer

spotify/
    raw/
    processed/
    transformed/
    athena-results/

    🟣  AWS Glue & Athena 

Glue Crawler
	•	Automatically infers schemas
	•	Creates processed and transformed tables
	•	Configuration files included

Athena
	•	Dedicated analytics workgroup
	•	Predefined SQL analytics queries
	•	Sample query results included



⚙ Orchestration
    🟢 Airflow (Docker)

The Airflow DAG orchestrates the full pipeline:
	1.	Run local Spotify extraction
	2.	Upload data to S3
	3.	Trigger Glue crawler
	4.	Execute Athena analytics
	5.	Trigger Databricks workflows
	6.	Execute Snowflake procedures
	7.	Run data quality checks
	8.	Produce final pipeline summary

Folder: dags/spotify_etl_dag.py
Includes:
	•	XCom usage
	•	Snowflake operators
	•	HTTP hooks (Databricks)
	•	Retry logic & error handling
	•	Inline documentation


🔥 Databricks
PySpark | Delta-Lake

Notebook Assets
	•	Spotify_Spark_Processing.ipynb
	•	Python script version
	•	HTML export

Bronze Layer
	•	Raw ingest from S3
	•	Table: spotify_tracks_bronze

Silver Layer
	•	Deduplication
	•	Type casting
	•	Calculated columns
	•	Table: spotify_tracks_silver

Gold Layer
	•	Aggregations & analytics features
	•	Table: spotify_tracks_gold
	•	Popularity summary outputs

Databricks job config also included:
	•	cluster config markdown
	•	job markdown
	•	SQL queries
	•	Result samples


❄ Snowflake
Warehouse | ELT | Automation

Includes:
	•	Database, schema, warehouse setup
	•	External stages & file formats
	•	Bronze / Silver / Gold tables
	•	Stored procedures
	•	Scheduled tasks
	•	Data quality checks
	•	Result samples


📊 Power BI

Analytics & Visualization

Dashboards include:
	•	Artist popularity
	•	Track duration distribution
	•	Explicit vs non-explicit analysis
	•	Bronze → Silver → Gold row comparison
	•	Release trends


 🧪 Data Quality Checks (Across Platforms)

Implemented validations include:
	•	Athena vs Snowflake row counts
	•	Gold-layer aggregation checks
	•	Popularity summary verification
	•	Sample DQ outputs included



	🧪 Testing & Quality Assurance

This project includes a focused, practical test suite to validate core pipeline logic
without requiring live cloud infrastructure.

Validated components include:
	•	Spotify ingestion (local & Lambda)
	•	Transformation logic (duration, categorization, timestamps)
	•	S3-triggered Lambda behavior
	•	Error-handling scenarios

Run Tests:
pip install -r tests/requirements_test.txt
pytest -q

Run specific tests:
pytest tests/test_extract_local.py -q
pytest tests/test_spotify_lambda_ingest.py -q
pytest tests/test_spotify_lambda_transform.py -q

📖 Detailed testing notes: IMPLEMENTATION_GUIDE.md



📊 Performance & Cost Notes 

Pipeline execution behavior and cost considerations were evaluated during development.
	•	End-to-end pipeline validated successfully
	•	Data quality confirmed across platforms
	•	Infrastructure deprovisioned after validation

📊 Full details available in PERFORMANCE_METRICS.md


🔐 Environment Configuration

All credentials are handled via environment variables only.
No secrets are committed to this repository.

Example:
SPOTIFY_CLIENT_ID=your_client_id
SPOTIFY_CLIENT_SECRET=your_client_secret

AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
AWS_DEFAULT_REGION=us-east-2

S3_BUCKET_NAME=your-bucket


🎬 Demo Video

/demo/
   pipeline_demo.mp4



🧩 How to Run This Project (Locally / Cloud)

Local (Airflow + Docker)
docker-compose up -d

Upload DAG: dags/spotify_etl_dag.py

Access UI: http://localhost:8080

Quick Validation: pytest -q
