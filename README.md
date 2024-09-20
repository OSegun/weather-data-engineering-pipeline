# WEATHER DATA ENGINEERING PIPELINE WITH OPEN WEATHER MAP API, AWS AND POWER BI
This project aims to build a real-time data pipeline that processes weather data from the Open Weather Map API, analyzes streaming data, and enables data-driven decision-making using Power BI. The pipeline will use Apache Airflow, Python, and AWS Cloud services. You can assess a detailed overview of the project via [Google doc](https://docs.google.com/document/d/1be3mYmbaktMy2KTqfBOtpXfSvXfb9pKw_nesAzFVFek/edit?usp=sharing)

## Data Schema Documentation

### 1. Raw Data Schema

#### Table Name: raw_weather_data

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| id | UUID | Unique identifier for each record |
| timestamp | TIMESTAMP | Time of data collection |
| location_id | VARCHAR(50) | Unique identifier for the location |
| raw_data | JSON | Raw JSON response from the API |

### 2. Processed Data Schema

#### Table Name: processed_weather_data

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| id | UUID | Unique identifier for each record |
| timestamp | TIMESTAMP | Time of data collection |
| location_id | VARCHAR(50) | Unique identifier for the location |
| temperature | FLOAT | Temperature in Celsius |
| feels_like | FLOAT | 'Feels like' temperature in Celsius |
| temp_min | FLOAT | Minimum temperature in Celsius |
| temp_max | FLOAT | Maximum temperature in Celsius |
| pressure | INTEGER | Atmospheric pressure in hPa |
| humidity | INTEGER | Humidity percentage |
| visibility | INTEGER | Visibility in meters |
| wind_speed | FLOAT | Wind speed in m/s |
| wind_direction | INTEGER | Wind direction in degrees |
| cloudiness | INTEGER | Cloudiness percentage |
| rain_1h | FLOAT | Rain volume for the last 1 hour, in mm |
| snow_1h | FLOAT | Snow volume for the last 1 hour, in mm |
| weather_main | VARCHAR(50) | Main weather condition |
| weather_description | VARCHAR(200) | Detailed weather description |

### 3. Aggregated Data Schema

#### Table Name: daily_weather_summary

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| id | UUID | Unique identifier for each record |
| date | DATE | Date of the summary |
| location_id | VARCHAR(50) | Unique identifier for the location |
| avg_temperature | FLOAT | Average temperature for the day |
| min_temperature | FLOAT | Minimum temperature for the day |
| max_temperature | FLOAT | Maximum temperature for the day |
| avg_pressure | FLOAT | Average pressure for the day |
| avg_humidity | FLOAT | Average humidity for the day |
| total_rainfall | FLOAT | Total rainfall for the day |
| total_snowfall | FLOAT | Total snowfall for the day |
| dominant_weather | VARCHAR(50) | Most frequent weather condition |

### 4. Location Data Schema

#### Table Name: locations

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| id | VARCHAR(50) | Unique identifier for the location |
| name | VARCHAR(100) | Name of the location |
| country | VARCHAR(50) | Country of the location |
| latitude | FLOAT | Latitude coordinate |
| longitude | FLOAT | Longitude coordinate |
| timezone | VARCHAR(50) | Timezone of the location |

### Notes on Schema Design

1. Use of UUID for id fields ensures uniqueness across distributed systems.
2. Timestamp fields use high precision for accurate time-series analysis.
3. The raw_data JSON field allows storing all API response data, even if not immediately used.
4. Separate location data allows for efficient storage and easy addition of new locations.
5. Aggregated data schema supports various time-based analyses and reporting needs.

### Data Relationships

- Each record in `processed_weather_data` and `daily_weather_summary` relates to a specific `location` through the `location_id`.
- `raw_weather_data` serves as a backup and can be linked to `processed_weather_data` through the `timestamp` and `location_id`.

This schema design allows for efficient storage, processing, and retrieval of weather data at various levels of detail, supporting both real-time analysis and historical trending.
