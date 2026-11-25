# PySpark Flight Data Pipeline

## Project Overview

This project uses Apache Spark (PySpark) to process and analyze large-scale US flight data from 2010 to 2025. The dataset contains approximately 100.9 million flight records and more than 1 GB of Parquet data.

The goal is to demonstrate distributed data processing, data cleaning, transformations, aggregations, SQL queries, joins, and performance optimization using PySpark.

## Dataset

- **Source:** [Bureau of Transportation Statistics (BTS) flight data](https://data.mathvsvibes.com/bts/)
- **Format:** Parquet
- **Time period:** 2010–2025
- **Records:** Approximately 100.9 million
- **Size:** Approximately 1.1 GB

## Technologies

- Python
- Apache Spark (PySpark)
- Spark SQL
- Parquet
- Jupyter Notebook

## Data Processing Pipeline

The pipeline includes:

1. Loading 16 yearly Parquet files into a Spark DataFrame.
2. Filtering out cancelled and diverted flights.
3. Removing records with missing departure or arrival delay values.
4. Creating a `delayed` indicator, where a flight is considered delayed if its arrival delay is at least 15 minutes.
5. Aggregating flight delays by airline, year, month, and airport.
6. Joining flight records with an airport lookup dataset.
7. Running SQL queries using Spark SQL.
8. Writing processed results to Parquet.

## Key Findings

- **2025:** Had the highest arrival-delay rate among the analyzed years, at 22.31%.
- **2020:** Had the lowest arrival-delay rate, at 9.82%.
- **Chicago O'Hare (ORD):** Had the highest delay percentage among the 10 airports included in the airport lookup, at 23.46%.
- **Seattle-Tacoma (SEA):** Had the lowest delay percentage among those 10 airports, at 16.91%.

The airport comparison uses a demonstration lookup table containing 10 airports, rather than a complete airport reference dataset.

## Performance Analysis

The pipeline processes approximately 100.9 million flight records across 16 years. Early filtering removes cancelled and diverted flights and records with missing delay values. Selecting only the columns needed for analysis also reduces unnecessary data processing. The physical execution plan and Spark UI were used to examine how Spark executes the pipeline.

The groupBy aggregations require data to be grouped across partitions, which can involve shuffling. The airport lookup join connects flight records with airport information. Parquet supports column pruning and filter pushdown when the query and file structure allow these optimizations.

Caching was tested using the 2025 flight DataFrame. The uncached count took 0.43 seconds, while the first cached count took 2.94 seconds because Spark had to process and store the data. A repeated cached count took only 0.04 seconds, demonstrating how caching can improve performance when the same DataFrame is reused.

## Screenshots

The `screenshots/` folder contains evidence of:

- Dataset loading and schema
- Data cleaning and transformations
- Aggregation and join results
- Spark SQL queries
- Lazy transformations versus eager actions
- Physical execution plan
- Spark UI query details
- Caching performance comparison

## Project Structure

```text
pyspark-flight-data-pipeline/
│
├── data/                # Source Parquet files (not included in GitHub)
├── output/              # Processed Parquet results
├── screenshots/         # Performance and execution screenshots
├── flight_data_pipeline.ipynb
└── README.md
```

## Running the Project

1. Install Python and Java.
2. Install PySpark:

   ```bash
   pip install pyspark
   ```

3. Download the yearly Parquet files from the dataset source.
4. Place the files in the `data/` folder.
5. Open the notebook in JupyterLab.
6. Run the notebook cells in order.

## Output

The pipeline produces aggregated flight-delay results and saves processed data in Parquet format.