# Explanation of the API Integration and Analysis Code

This document provides a detailed explanation of the code used for integrating with an API and performing data analysis. The code retrieves temperature and humidity data from a specified device over a 24-hour period, processes it, and generates visualizations to facilitate analysis.

## Overview

The code accomplishes the following primary functions:

- **Data Retrieval**: Fetches temperature and humidity data from a device via an API.
- **Data Processing**: Organizes the retrieved data and saves it to a CSV file.
- **Data Visualization**: Produces graphical representations to analyze trends and distributions.

This workflow supports effective monitoring and evaluation of environmental conditions recorded by the device.

## Code Breakdown

### 1. API Configuration and Setup

- **Libraries Utilized**:
    - requests: Enables HTTP requests to communicate with the API.
    - time: Provides functionality for timestamp calculations.
    - pandas: Supports data manipulation and storage.
    - matplotlib.pyplot and seaborn: Facilitate the creation of visualizations.
- **Configuration Details**:
    - **Endpoint**: The API is accessed via https://iot.enlightcloud.com/api/plugins/telemetry/DEVICE/{deviceId}/values/timeseries.
    - **Device Identification**: The target device is specified with the ID a349bfa0-c1dc-11ef-8a50-2597fc5b248e.
    - **Authentication**: A JSON Web Token (JWT) is included in the request headers for secure access.
    - **Headers**: Configured with the JWT and content type set to application/json.

### 2. Calculating Timestamps

- Timestamps define the 24-hour window for data retrieval:
    - **End Timestamp (end_ts)**: Represents the current time in milliseconds, calculated as time.time() * 1000.
    - **Start Timestamp (start_ts)**: Represents the time 24 hours prior to end_ts, computed by subtracting 86,400,000 milliseconds (24 hours).
- These values are used as parameters in the API request to specify the data range.

### 3. API Request and Data Processing

- **API Request**:
    - A GET request is executed using requests.get() with:
        - The URL formatted with the device ID.
        - Headers containing the authentication token.
        - Query parameters specifying sensor keys (temperature1, humidity1) and the time range (start_ts to end_ts).
- **Data Processing**:
    - On a successful response (HTTP status code 200), the JSON data is parsed into a structured format:
        - Each entry includes the sensor name, timestamp (in milliseconds), and value (converted to a float).
    - The data is organized into a pandas DataFrame.
    - Timestamps are converted to a readable datetime format using pd.to_datetime().
- **Data Storage**:
    - The processed DataFrame is exported to a CSV file named sensor_temp_hum_analysis.csv.
- **Error Handling**:
    - Failed requests return the status code and response text for diagnosis.
    - Connection issues are caught and reported separately.

### 4. Data Summary

- An initial analysis of the data is provided:
    - The first five rows of the DataFrame are displayed using df.head().
    - A statistical summary (e.g., count, mean, standard deviation, min, max) is generated with df.describe().

### 5. Visualization Functions

- A consistent visual style is applied using sns.set_theme(style="whitegrid") from seaborn.
- Three visualization functions are defined:
    - **plot_time_series**:
        - Creates a line plot to show sensor values over time.
        - Includes a title, labeled axes, and rotated x-axis ticks for readability.
    - **plot_histogram**:
        - Generates a histogram with a kernel density estimate (KDE) to display the distribution of values.
    - **plot_boxplot**:
        - Produces a box plot to illustrate the data's spread, median, and potential outliers.

### 6. Generating Plots

- The DataFrame is split into subsets:
    - temp1_df: Contains temperature1 data.
    - hum1_df: Contains humidity1 data.
- For each subset:
    - The first five rows are printed for inspection.
    - Three visualizations are created:
        - **Time Series Plot**: Shows trends over 24 hours (e.g., "Temperature (°C)" or "Humidity (%)").
        - **Histogram**: Depicts the frequency distribution of values.
        - **Box Plot**: Highlights statistical properties and outliers.
- These visualizations provide insights into the sensor data's behavior and patterns.

## Conclusion

This code delivers a robust solution for:

- **API Integration**: Retrieves sensor data using requests with secure authentication.
- **Data Management**: Processes and stores data efficiently with pandas.
- **Data Analysis**: Visualizes trends and distributions using seaborn and matplotlib.

It enables users to monitor temperature and humidity effectively, with results saved for future use and presented in clear, interpretable graphics.
