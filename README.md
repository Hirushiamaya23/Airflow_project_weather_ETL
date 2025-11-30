# Weather ETL Pipeline using Apache Airflow

This project implements a fully automated ETL (Extract, Transform, Load) pipeline for processing historical weather data using **Apache Airflow**.  
The pipeline retrieves the dataset, transforms it into daily and monthly aggregates, validates the results, and loads the processed data into a **SQLite database**.

This project was completed as part of the *Orchestrating an ETL Pipeline in Airflow for Historical Weather Data* assignment.

---

## Technologies Used
- **Apache Airflow**
- **Python**
- **Pandas**
- **SQLite**
- **SQLAlchemy**
- **Kaggle API (optional for automated extraction)**

---

# Project Structure

---

# ETL Pipeline Overview

## 1. **Extract**
- Uses Airflow to retrieve the dataset path.
- (Optional) Supports Kaggle API ZIP extraction.
- Passes the CSV path to downstream tasks using **XCom**.

---

## 2. **Transform**
The transformation step includes:

###  Data Cleaning
- Convert `Formatted Date` to `datetime`.
- Handle missing values in temperature, humidity, wind speed, etc.
- Remove duplicates.
- Normalize humidity if values > 1.

###  Feature Engineering
- **Daily Aggregates** using resampling.
- **Monthly Aggregates** for temperature, humidity, visibility, pressure, etc.
- **Mode Precip Type** per month.
- **Wind Strength Category** based on Beaufort scale.

###  Output
Creates two CSV files:
- `daily_weather.csv`
- `monthly_weather.csv`

---

## 3. **Validate**
Validation includes:
- Temperature range check (-50°C to 50°C)
- Humidity within [0, 1]
- Wind speed non-negative
- Missing value inspection

Only if validation succeeds does the pipeline proceed to the load step.

---

## 4. **Load**
Loads the transformed datasets into an SQLite database:

### Tables:
- `daily_weather`
- `monthly_weather`

This step uses SQLAlchemy and ensures the tables are replaced on each run.

---

#  Database Example Output

SQLite terminal preview:


Tables store daily & monthly weather metrics successfully.

---

#  Airflow DAG
The final DAG contains the following tasks:

1. `extract_data`
2. `transform_data`
3. `validate_data`
4. `load_daily`
5. `load_monthly`

Trigger rules ensure validation must succeed before loading.


#  How to Run the Pipeline

### 1. Start Airflow
```bash
airflow standalone
