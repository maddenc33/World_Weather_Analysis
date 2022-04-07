# World_Weather_Analysis

#### Christopher Madden

---

## Project Overview

**Task**: Collect and analyze weather data across cities worldwide.
**Purpose**: PlanMyTrip will use the data to recommend ideal hotels based on clients' weather preferences.
**Method**: Create a Pandas DataFrame with 500 or more of the world's unique cities and their weather data in real time. This process will entail collecting, analyzing, and visualizing the data.

The analysis of the data will be split into three main stages:

1. Data Collection
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

#### 1. Data Collection

I am using the OpenWeatherMap API to find weather data for random cities.  To generate a list of random cities, I generated random latitudes and longitudes and used citipy.nearest_city method to retrieve the city name for each pair of coordinates.  Once we have city names, we pass the city name to the API to find the JSON weather data for the city.

Below is the code used to make the API call and create a dataframe of weather data for the randomly selected cities:

```python

# Create an empty list to hold the weather data.
city_data = []

# Print the beginning of the logging.
print("Beginning Data Retrieval     ")
print("-----------------------------")

# Create counters.
record_count = 1
set_count = 1

# Loop through all the cities in the list.
for i, city in enumerate(cities):

    # Group cities in sets of 50 for logging purposes.
    if (i % 50 == 0 and i >= 50):
        set_count += 1
        record_count = 1
    # Create endpoint URL with each city.
    city_url = url + "&q=" + city

    # Log the URL, record, and set numbers and the city.
    print(f"Processing Record {record_count} of Set {set_count} | {city}")
    
    # Add 1 to the record count.
    record_count += 1
    
    # Run an API request for each of the cities.
    try:
        # Parse the JSON and retrieve data.
        city_weather = requests.get(city_url).json()
        # Parse out the needed data.
        city_lat = city_weather["coord"]["lat"]
        city_lng = city_weather["coord"]["lon"]
        city_max_temp = city_weather["main"]["temp_max"]
        city_humidity = city_weather["main"]["humidity"]
        city_clouds = city_weather["clouds"]["all"]
        city_wind = city_weather["wind"]["speed"]
        city_country = city_weather["sys"]["country"]
        # Convert the date to ISO standard.
        city_date = datetime.utcfromtimestamp(city_weather["dt"]).strftime('%Y-%m-%d %H:%M:%S')
        # Append the city information into city_data list.
        city_data.append({"City": city.title(),
                          "Lat": city_lat,
                          "Lng": city_lng,
                          "Max Temp": city_max_temp,
                          "Humidity": city_humidity,
                          "Cloudiness": city_clouds,
                          "Wind Speed": city_wind,
                          "Country": city_country,
                          "Date": city_date})

# If an error is experienced, skip the city.
    except:
        print("City not found. Skipping...")
        pass

# Indicate that Data Loading is complete.
print("-----------------------------")
print("Data Retrieval Complete      ")
print("-----------------------------")

# Convert the array of dictionaries to a Pandas DataFrame and reorder the columns.
city_data_df = pd.DataFrame(city_data)
new_column_order = ["City", "Country", "Date", "Lat", "Lng", "Max Temp", "Humidity", "Cloudiness", "Wind Speed"]
city_data_df = city_data_df[new_column_order]

```

Once the dataframe with the weather data for the cities has been created, graphical visualizations and statistical analysis of that data can now be performed.

#### 2. Exploratory Analysis with Visualization

I created four scatterplots demonstrating the relationship between latitude vs. temperature, humidity, cloudiness, and wind speed.

Here is an example of the code used to generate the latitude vs. max temperature scatterplot:

```python

# Build the scatter plot for latitude vs. max temperature.
plt.scatter(lats,
            max_temps,
            edgecolor="black", linewidths=1, marker="o",
            alpha=0.8, label="Cities")

# Incorporate the other graph properties.
plt.title(f"City Latitude vs. Max Temperature "+ time.strftime("%x"))
plt.ylabel("Max Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)

```

The resulting four scatterplots:

![Latitude vs. Max Temperature](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig1.png?raw=true)

![Latitude vs. Humidity](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig2.png?raw=true)

![Latitude vs. Cloudiness](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig3.png?raw=true)

![Latitude vs. Wind Speed](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig4.png?raw=true)

Notice which of the four demonstrates a strong correlation with latitude: temperature.

---

## Challenge Summary

Challenge summary.
