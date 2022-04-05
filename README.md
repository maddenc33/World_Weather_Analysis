# World_Weather_Analysis

#### Christopher Madden

---

## Project Overview

**Task**: Collect and analyze weather data across cities worldwide.
**Purpose**: PlanMyTrip will use the data to recommend ideal hotels based on clients' weather preferences.
**Method**: Create a Pandas DataFrame with 500 or more of the world's unique cities and their weather data in real time. This process will entail collecting, analyzing, and visualizing the data.

The analysis of the data will be split into three main stages:

1. Data Collectionaaaaaa
 - Use the NumPy module to generate more than 1,500 random latitudes and longitudes.
 - Use the citipy module to list the nearest city to the latitudes and longitudes.
 - Use the OpenWeatherMap API to request the current weather data from each unique city in your list.
 - Parse the JSON data from the API request.
 - Collect the following data from the JSON file and add it to a DataFrame:
    - City, country, and date
    - Latitude and longitude
    - Maximum temperature
    - Humidity
    - Cloudiness
    - Wind speed

2. Exploratory Analysis with Visualization
 - Create scatter plots of the weather data for the following comparisons:
    - Latitude versus temperature
    - Latitude versus humidity
    - Latitude versus cloudiness
    - Latitude versus wind speed
 - Determine the correlations for the following weather data:
    - Latitude and temperature
    - Latitude and humidity
    - Latitude and cloudiness
    - Latitude and wind speed
 - Create a series of heatmaps using the Google Maps and Places API that showcases the following:
    - Latitude and temperature
    - Latitude and humidity
    - Latitude and cloudiness
    - Latitude and wind speed

3. Visualization of Travel Data
 - Filter the Pandas DataFrame based on user inputs for a minimum and maximum temperature.
 - Create a heatmap for the new DataFrame.
 - Find a hotel from the cities' coordinates using Google's Maps and Places API, and Search Nearby feature.
 - Store the name of the first hotel in the DataFrame.
 - Add pop-up markers to the heatmap that display information about the city, current maximum temperature, and a hotel in the city.

---

## Resources

GitHub Repository URL:

https://github.com/maddenc33/World_Weather_Analysis

### Data Sources

 - OpenWeatherMap API

### Software

- Python 3.9.7
  - pandas library
  - matplotlib library
  - numpy library
  - scipy library

- Jupyter Notebook 6.4.5

- Anaconda 4.12.0

  >  conda version : 4.12.0
  > 
  >  conda-build version : 3.21.6
  > 
  >  python version : 3.9.7.final.0

---

## Challenge Overview

Challenge overview.

```python

# Example code.

```

---

## Challenge Summary

Challenge summary.
