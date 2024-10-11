# Delhivery Business case study : Feature Engineering

## About Delhivery

Delhivery is the largest and fastest-growing fully integrated player in India by revenue in Fiscal 2021. They aim to build the operating system for commerce, through a combination of world-class infrastructure, logistics operations of the highest quality, and cutting-edge engineering and technology capabilities.

The Data team builds intelligence and capabilities using this data that helps them to widen the gap between the quality, efficiency, and profitability of their business versus their competitors.

🎯 Objective

The company wants to understand and process the data coming out of data engineering pipelines:

• Clean, sanitize and manipulate data to get useful features out of raw fields

• Make sense out of the raw data and help the data science team to build forecasting models on it


## Dataset
      
| Feature                      | Description                                                                                                |
|:----------------------------|:------------------------------------------------------------------------------------------------------------|
| data                         | Indicates whether the data is for testing or training                                                      |
| trip_creation_time           | Timestamp of trip creation                                                                                 |
| route_schedule_uuid          | Unique ID for a particular route schedule                                                                  |
| route_type                   | Transportation type                                                                                        |
| FTL                          | Full Truck Load: Shipments with no other pickups or drop-offs along the way                                |
| Carting                      | Handling system using small vehicles (carts)                                                               |
| trip_uuid                    | Unique ID given to a particular trip (may include different source and destination centers)                |
| source_center                | Source ID of trip origin                                                                                   |
| source_name                  | Source name of trip origin                                                                                 |
| destination_center           | Destination ID                                                                                             |
| destination_name             | Destination name                                                                                           |
| od_start_time                | Trip start time                                                                                            |
| od_end_time                  | Trip end time                                                                                              |
| start_scan_to_end_scan       | Time taken to deliver from source to destination                                                           |
| is_cutoff                    | Unknown field                                                                                              |
| cutoff_factor                | Unknown field                                                                                              |
| cutoff_timestamp             | Unknown field                                                                                              |
| actual_distance_to_destination| Distance in kilometers between source and destination warehouse                                           |
| actual_time                  | Actual time taken to complete the delivery (cumulative)                                                    |
| osrm_time                    | Time computed by the open-source routing engine (includes traffic, major and minor roads) (cumulative)     |
| osrm_distance                | Distance computed by the open-source routing engine (includes traffic, major and minor roads) (cumulative) |
| factor                       | Unknown field                                                                                              |
| segment_actual_time          | Segment time, i.e., time taken by a subset of the package delivery                                         |
| segment_osrm_time            | OSRM segment time, i.e., time computed by the routing engine for a subset of the package delivery          |
| segment_osrm_distance        | OSRM segment distance, i.e., distance covered by a subset of the package delivery                          |
| segment_factor               | Unknown field                                                                                              |

# steps performed

The following steps were performed on the dataset:

1. **Data Cleaning and Missing Value Handling:**
   - Handled any missing values present in the dataset.
   - Analyzed the structure of the data to better understand the fields and their types.
   - Merged rows based on the provided hint, particularly focusing on `trip_uuid`.

2. **Feature Engineering:**
   - Extracted features from `Destination Name` by splitting into city, place code, and state.
   - Extracted features from `Source Name` by splitting into city, place code, and state.
   - Extracted time-based features like month, year, and day from the `Trip_creation_time` field.

3. **In-depth Analysis and Additional Feature Creation:**
   - Created a new feature by calculating the time taken between `od_start_time` and `od_end_time`.
   - Dropped the original columns if necessary, based on the feature creation.
   - Compared the difference between the newly created time feature and `start_scan_to_end_scan`. Conducted hypothesis testing and visual analysis to check for significant differences.

4. **Hypothesis Testing and Visual Analysis:**
   - Conducted hypothesis testing and visual analysis between:
     - Aggregated `actual_time` values and aggregated `OSRM time` values (after merging rows based on `trip_uuid`).
     - Aggregated `actual_time` values and aggregated `segment actual time` values.
     - Aggregated `OSRM distance` values and aggregated `segment OSRM distance` values.
     - Aggregated `OSRM time` values and aggregated `segment OSRM time` values.

5. **Outlier Detection and Handling:**
   - Detected outliers in the numerical variables using visual analysis techniques.
   - Handled the outliers using the IQR (Interquartile Range) method to minimize their impact on analysis.

6. **Encoding and Scaling:**
   - Performed one-hot encoding for categorical variables like `route_type` to convert them into numerical form.
   - Normalized/Standardized numerical features using MinMaxScaler or StandardScaler for better model performance.

# Business Recommendations/Insights

1. **Comparison of `actual_time` and `osrm_time`:**
   - **Violin Plot:** Distributions show similar central tendency and spread, indicating predicted times generally align with actual times, though variability and outliers exist.
   - **Scatter Plot:** Positive correlation observed, with notable variability at lower time values.
   - **Bland-Altman Plot:** Differences between the two are centered around zero, indicating no systematic bias, but some outliers exist.

2. **Comparison of `actual_time` and `segment_actual_time_sum`:**
   - **Violin Plot:** Both distributions are centered around zero, showing similar spread but slightly different shapes.
   - **Scatter Plot:** Positive correlation noted, with significant variability at lower time values.
   - **Bland-Altman Plot:** Differences mostly centered around zero, indicating no radical systematic bias.

3. **Comparison of `osrm_distance` and `segment_osrm_distance_sum`:**
   - **Violin Plot:** Similar central tendency, but osrm_distance shows varied peakedness, suggesting a reliable difference.
   - **Scatter Plot:** Positive correlation present, with variability at lower distance values.
   - **Bland-Altman Plot:** Differences are centered around zero, indicating no radical systematic bias.

4. **Comparison of `osrm_time` and `segment_osrm_time_sum`:**
   - **Violin Plot:** Both distributions are centered around zero, with slightly different shapes.
   - **Scatter Plot:** Positive correlation with significant variability at lower time values.
   - **Bland-Altman Plot:** Differences centered around zero, indicating no radical systematic bias.

5. **Wilcoxon Tests:** 
   - P-values indicate no major differences in means for most combinations, except for `osrm_distance` and `segment_osrm_distance_sum`, which shows a reliable difference, though not below the alpha level of 0.05
