# ETL Pipeline

A robust Extract, Transform, Load (ETL) pipeline built with Apache Airflow that automatically fetches NASA's Astronomy Picture of the Day (APOD) data and stores it in a PostgreSQL database.

## 🚀 Features

- **Automated Daily Data Collection**: Fetches NASA APOD data daily using the official NASA API
- **Data Transformation**: Extracts and cleans relevant fields (title, explanation, URL, date, media type)
- **PostgreSQL Integration**: Stores processed data in a structured PostgreSQL database
- **Dockerized Environment**: Complete containerized setup for easy deployment
- **Airflow Orchestration**: Reliable task scheduling and monitoring with Apache Airflow
- **Error Handling**: Built-in error handling and retry mechanisms

## 📋 Prerequisites

- Docker and Docker Compose
- Astronomer CLI (for local development)
- NASA API Key (free from [api.nasa.gov](https://api.nasa.gov/))

## ��️ Installation

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd ETL_Pipeline
```

### 2. Set Up NASA API Connection

1. Get your free NASA API key from [api.nasa.gov](https://api.nasa.gov/)
2. Configure the connection in Airflow:
   - Go to Airflow UI → Admin → Connections
   - Create a new connection with:
     - **Connection Id**: `nasa_api`
     - **Connection Type**: `HTTP`
     - **Host**: `https://api.nasa.gov`
     - **Extra**: `{"api_key": "YOUR_API_KEY"}`

### 3. Set Up PostgreSQL Connection

Configure the PostgreSQL connection in Airflow:
- **Connection Id**: `my_postgres_connection`
- **Connection Type**: `Postgres`
- **Host**: `postgres_db` (Docker service name)
- **Schema**: `postgres`
- **Login**: `postgres`
- **Password**: `postgres`
- **Port**: `5432`

## 🚀 Quick Start

### Using Astronomer CLI (Recommended)

```bash
# Install Astronomer CLI
curl -sSL install.astronomer.io | sudo bash

# Start the development environment
astro dev start

# Access Airflow UI at http://localhost:8080
# Default credentials: admin/admin
```

### Using Docker Compose

```bash
# Start PostgreSQL database
docker-compose up -d postgres

# Start Airflow (requires additional Airflow services)
# This is handled automatically by Astronomer CLI
```

## 📊 Data Schema

The pipeline creates and populates the following PostgreSQL table:

```sql
CREATE TABLE apod_data (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255),
    explanation TEXT,
    url TEXT,
    date DATE,
    media_type VARCHAR(50)
);
```

## 🔄 Pipeline Workflow

The ETL pipeline consists of the following steps:

1. **Create Table**: Ensures the `apod_data` table exists in PostgreSQL
2. **Extract**: Fetches daily APOD data from NASA API
3. **Transform**: Extracts and cleans relevant fields
4. **Load**: Stores the processed data in PostgreSQL


## �� Configuration

### DAG Configuration

The ETL pipeline runs daily with the following settings:
- **Schedule**: `@daily`
- **Start Date**: Yesterday (to avoid missing data)
- **Catchup**: Disabled (prevents backfilling)

### Database Configuration

- **Database**: PostgreSQL 13
- **Container Name**: `postgres_db`
- **Port**: 5432 (internal), 5433 (external if exposed)
- **Credentials**: postgres/postgres

## �� Monitoring

Access the Airflow UI to:
- Monitor DAG runs and task status
- View task logs and debug issues
- Manually trigger DAG runs
- Monitor data pipeline performance

## 🧪 Testing

Run the included tests:

```bash
# Run DAG tests
python -m pytest tests/
```

## 🚀 Deployment

### Local Development

```bash
astro dev start
```

### Production Deployment

1. Push to Astronomer Cloud:
```bash
astro deploy
```

2. Or deploy to your own Airflow instance using the provided Docker configuration.

## �� API Reference

### NASA APOD API

- **Endpoint**: `https://api.nasa.gov/planetary/apod`
- **Method**: GET
- **Parameters**: `api_key` (required)
- **Rate Limit**: 1000 requests per hour

### Data Fields

| Field | Type | Description |
|-------|------|-------------|
| title | String | The title of the image |
| explanation | Text | Detailed explanation of the image |
| url | String | URL to the image/video |
| date | Date | Date of the APOD |
| media_type | String | Type of media (image/video) |


## �� Acknowledgments

- [NASA](https://www.nasa.gov/) for providing the APOD API
- [Apache Airflow](https://airflow.apache.org/) for the orchestration framework
- [Astronomer](https://www.astronomer.io/) for the development tools

