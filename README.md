# gasstation-price-analysis



The data was collected using a separate ETL pipeline repository and stored in a 
PostgreSQL database. The analysis covers fuel prices (Diesel, E5, E10) from 96 
gas stations in the Stuttgart area between February and March 2026.

The goal of this analysis is to find out, how the avg. fuel price changes over time and when it is the best time to fuel up your car. 
How does the weekday, time of the day and the choice of gas station impact the price you pay for gas?

> The analysis is still in progress – more insights coming soon.


## Data exploration

To gain a deeper understanding of the collected data, we performed an initial exploration. This helps us identify patterns, validate data quality, and ensure the dataset is ready for further analysis.


Data Overview
We queried the database for all records and analyzed the following key metrics:

- **Total datasets:** 188,640
- **Time period:** February 15, 2026, to March 28, 2026 (42 days)
- **Unique gas stations:** 96
- **Unique days with data:** 42

Data Collection Frequency
The data was collected at a high frequency to capture real-time fluctuations in fuel prices. Each day, we gathered:

- **Full days with complete data:** 42 days
- **Data points per full day:** 4,608
- **Data points per 30-minute interval:** 96 gas stations × 1 data point per station per 30 minutes


Checking for missing values:

```
Missing values:
id                  0
tankstellen_id      0
diesel             62
e5                302
e10                22
isopen              0
retrieval_time      0
retrieval_date      0
dtype: int64
```

Checking which gas stations were affected and when these gaps occurred.




|Missing E5-values per gas station       |     |
|:---------------------------------------|----:|
| ('Access Station Stuttgart', 'Access') | 281 |
| ('MTS Waschpark', 'SB Tankstelle')     |  21 |

|Missing E10-values per gas station  |     |
|:-----------------------------------|----:|
| ('MTS Waschpark', 'SB Tankstelle') |  22 |

Missing Diesel-values per gas station:
|Missing Diesel-values per gas station |   0 |
|:-------------------------------------|----:|
| ('MTS Waschpark', 'SB Tankstelle')   |  62 |


| retrieval_date   |   E5 |   E10 |   Diesel |
|:-----------------|-----:|------:|---------:|
| 2026-02-18       |   21 |     0 |        0 |
| 2026-02-19       |   48 |     0 |        0 |
| 2026-02-20       |   48 |     0 |        0 |
| 2026-02-21       |   48 |     0 |        0 |
| 2026-02-22       |   48 |     0 |        0 |
| 2026-02-23       |   48 |     0 |        0 |
| 2026-02-24       |   20 |     0 |        0 |
| 2026-03-01       |   12 |    13 |       16 |
| 2026-03-02       |    9 |     9 |        9 |
| 2026-03-20       |    0 |     0 |        7 |
| 2026-03-21       |    0 |     0 |       11 |
| 2026-03-22       |    0 |     0 |       10 |
| 2026-03-23       |    0 |     0 |        9 |


The missing values are not random but concentrated on just two stations.
The gaps were not continuous, but appeared in short, isolated time windows across a few days.

Given that this was time-series data, I decided to use a Forward Fill method to fill these values, since fuel prices typically change gradually, making it reasonable to assume that the last observed price is the most accurate estimate for the missing values. This approach preserves the temporal structure of the data while minimizing the impact on the overall trend and averages.





Minimal Impact: I compared the average fuel prices with and without Forward Fill. The deviations were negligible:

|        |   With NaN (ignored) |   With Forward Fill |   Deviation (%) |
|:-------|---------------------:|--------------------:|----------------:|
| e5     |               1.8951 |              1.8937 |           -0.07 |
| e10    |               1.8353 |              1.8337 |           -0.09 |
| diesel |               1.9037 |              1.9067 |            0.16 |

The deviations are well below 1%, confirming that Forward Fill does not significantly distort the results.

Geographical Data

Also I want to check the geographical data and the different gas station brands

<img src="assets/map2.png" width="1200" height="1200"/>

Checking for different brands we see that there are are several big brands with a high number of gasstations in Suttgart, like Shell and Aral.

However there also small and unknown brands with just one gasstation. We will check later how prices differ from the big brands to small ones.

|    | brand                            |   anzahl |
|---:|:---------------------------------|---------:|
|  0 | Shell                            |       18 |
|  1 | ARAL                             |       16 |
|  2 | ESSO                             |       12 |
|  3 | AGIP ENI                         |        9 |
|  4 | JET                              |        9 |
|  5 | AVIA                             |        8 |
|  6 | AVIA XPress                      |        3 |
|  7 | TotalEnergies                    |        3 |
|  8 | RAN                              |        3 |
|  9 | Supermarkt-Tankstelle            |        3 |
| 10 | SB                               |        1 |
| 11 | Freie Tankstelle                 |        1 |
| 12 | Mr. Wash Autoservice AG          |        1 |
| 13 | Sprint                           |        1 |
| 14 | Access                           |        1 |
| 15 | SCHARR-Tankstelle                |        1 |
| 16 | ORLEN Express                    |        1 |
| 17 | BFT                              |        1 |
| 18 | HEM                              |        1 |
| 19 | SB Tankstelle                    |        1 |
| 20 | freie Tankstelle                 |        1 |
| 21 | MTB Automatentankstelle Fellbach |        1 |

<img src="assets/brand_counts.png" width="1200" height="400"/>

## Analysis


<img src="assets/preisverlauf.png" width="1200" height="400"/>


<img width="1200" height="450" alt="image" src="https://github.com/user-attachments/assets/769e4a4c-a685-402e-95d4-511fb56ae94a" />

<img width="1912" height="885" alt="newplot" src="https://github.com/user-attachments/assets/416aed3a-6b4d-4a92-875d-10447aa11cb5" />


