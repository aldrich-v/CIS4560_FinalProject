Used Car Market Analytics — Hadoop + Hive Big Data Project

This project analyzes a 9.3GB U.S. used car listings dataset using Hadoop, HDFS, and Hive/Beeline to uncover trends in car prices, listing volume, geographic distribution, and market behavior.
The goal is to demonstrate big-data processing, cleaning, and analytical workflows using cloud-based Hadoop clusters.

🚗 Dataset

Source: Kaggle
https://www.kaggle.com/datasets/ananaymital/us-used-cars-dataset

Size: ~9.3GB (≈3.4M rows)

Fields include: VIN, make, model, year, price, mileage, fuel type, body type, city, latitude, longitude, listed date

🧹 Data Cleaning & Transformation

The raw 9.3GB CSV was cleaned and reduced to ~2GB using:

HDFS ingestion and storage

Hive table creation (used_cars_raw → used_cars_clean)

Removing invalid rows:

Price ≤ 0

Mileage ≤ 0

Missing latitude/longitude

Missing date

Extracting:

listed_year

listed_month

Creating analysis-ready tables:

used_cars_master

city_stats_geo

price_time_series

top5_makes, top5_fuel_types

🛠️ Technologies

Hadoop HDFS

Hive / Beeline

Linux / SSH

Excel 3D Maps

CSV-based visual analytics

Hadoop cloud environment

📊 Analytical Queries
1. Monthly time-series of avg price & listing volume
SELECT
  listed_year,
  listed_month,
  COUNT(*) AS listing_volume,
  AVG(price) AS avg_price
FROM used_cars_master
GROUP BY listed_year, listed_month
ORDER BY listed_year, listed_month;

2. Top 5 Most Sold Car Makes
SELECT make, COUNT(*) AS total_listings
FROM used_cars_master
GROUP BY make
ORDER BY total_listings DESC
LIMIT 5;

3. Top 5 Fuel Types
SELECT fuel_type, COUNT(*) AS fuel_count
FROM used_cars_master
GROUP BY fuel_type
ORDER BY fuel_count DESC
LIMIT 5;

4. City-Level Geo Stats
SELECT city, AVG(price) AS avg_price,
       COUNT(*) AS listing_count,
       latitude, longitude
FROM used_cars_master
GROUP BY city, latitude, longitude;

🌎 Visualizations Produced

(Using Excel 3D Maps + Geo Coordinates)

3D Geospatial Heatmap of average prices across U.S. cities

Geographic listing density map using latitude/longitude

Time-series graph of listing volume (2017–2020)

Time-series graph of average prices (2017–2020)

Top 5 makes + top 5 fuel types bar charts

📁 Project Structure
├── hive_queries/
│   ├── create_tables.sql
│   ├── cleaning.sql
│   ├── time_series.sql
│   ├── geostats.sql
│   └── top5.sql
├── data/
│   ├── used_cars_clean.csv
│   ├── city_stats_geo.csv
│   ├── price_time_series.csv
│   └── top5_visuals.csv
├── visualizations/
│   ├── 3D_map_screenshots/
│   ├── time_series_plots/
│   └── bar_charts/
└── README.md

🔍 Key Findings

Listing volume increased steadily from 2017 to early 2020.

Average prices show regular seasonal fluctuations.

Large urban areas (Los Angeles, Phoenix, Dallas, Chicago) have very high listing densities.

Most common makes: Ford, Chevrolet, Toyota, Nissan, Honda.

Most common fuel types: Gasoline dominates heavily.

🧑‍💻 How to Reproduce

Upload Kaggle dataset to Hadoop cluster:

hdfs dfs -put used_cars_data.csv /user/<username>/used_cars/


Create Hive tables via Beeline:

CREATE TABLE used_cars_raw (...);


Clean and transform the dataset.

Export cleaned tables:

hdfs dfs -get /user/<username>/out/*.csv .


Build visualizations in Excel using CSV output.
