# gasstation-price-analysis

## Analysis

The data was collected using a separate ETL pipeline repository and stored in a 
PostgreSQL database. The analysis covers fuel prices (Diesel, E5, E10) from 96 
gas stations in the Stuttgart area between February and March 2026.

The analysis includes:
- Average price trend over the entire period
- Price development by time of day
- Price comparison between brands
- Geographic distribution of gas stations

> The analysis is still in progress – more insights coming soon.

When starting the system, you first have to connect to the db again using "docker start pg-gasstation" in shell

## Data exploration

To gain a deeper understanding of the collected data, we performed an initial exploration. This helps us identify patterns, validate data quality, and ensure the dataset is ready for further analysis.

"""
df = pd.read_sql("SELECT * FROM abfragen", engine)

# Overview
print(f"Total datasets: {len(df)}")
print(f"Period: {df['retrieval_date'].min()} to {df['retrieval_date'].max()}")
print(f"Number of unique gas stations: {df['tankstellen_id'].nunique()}")
print(f"Number of unique days: {df['retrieval_date'].nunique()}")

# Datasets per day
print(f"\nDatasets per day:")
print(df.groupby('retrieval_date').size())
"""

Looking at the output:
"""
Total datasets: 188640
Period: 2026-02-15 to 2026-03-28
Number of unique gas stations: 96
Number of unique days: 42

Datasets per day:
retrieval_date
2026-02-15    2496
2026-02-16    4608
...
2026-03-27    4608
2026-03-28    1824
dtype: int64
"""

Data Overview
We queried the database for all records and analyzed the following key metrics:

Total datasets: 188,640
Time period: February 15, 2026, to March 28, 2026 (42 days)
Unique gas stations: 96
Unique days with data: 42

Data Collection Frequency
The data was collected at a high frequency to capture real-time fluctuations in fuel prices. Each day, we gathered:

Full days with complete data: 42 days
Data points per full day: 4,608
Data points per 30-minute interval: 96 gas stations × 1 data point per station per 30 minutes
This granularity ensures that we can analyze:

Daily price trends
Hourly variations
Price changes between different gas stations and brands
Observations

The dataset covers a consistent period of 42 days, allowing for robust time-series analysis.
The high frequency of data collection (every 30 minutes) provides a detailed view of price dynamics, enabling us to detect even subtle changes in fuel prices.
The 96 gas stations span the Stuttgart area, offering a comprehensive geographic distribution for comparison.




## Preisverlauf

<img src="assets/preisverlauf.png" width="1200" height="400"/>


<img width="1200" height="450" alt="image" src="https://github.com/user-attachments/assets/769e4a4c-a685-402e-95d4-511fb56ae94a" />

<img width="1912" height="885" alt="newplot" src="https://github.com/user-attachments/assets/416aed3a-6b4d-4a92-875d-10447aa11cb5" />

<img src="assets/brand_counts.png" width="1200" height="400"/>
