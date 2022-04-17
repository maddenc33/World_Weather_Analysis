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
 - Google Maps API
 - cities.csv

### Software

- Python 3.9.7
  - pandas library
  - matplotlib library
  - numpy library
  - scipy library
    - stats module
    - linregress module
  - time library
  - datetime library
  - citipy library
  - gmaps library

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

Latitude vs. Max Temperature

![Latitude vs. Max Temperature](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig1.png?raw=true)

Latitude vs. Humidity

![Latitude vs. Humidity](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig2.png?raw=true)

Latitude vs. Cloudiness

![Latitude vs. Cloudiness](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig3.png?raw=true)

Latitude vs. Wind Speed

![Latitude vs. Wind Speed](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig4.png?raw=true)

Notice which of the four demonstrates a strong correlation with latitude: temperature.  There is no correlation between latitude and either humidity, cloudiness, or wind speed.

Next I divided all of the cities into two groups: Northern Hemisphere and Southern Hemisphere.  I then plotted the resulting eight scatterplots with linear regression data.

Here is an example of the code I used to separate the cities into groups and plot a linear regression:

```python

# Create Northern and Southern Hemisphere DataFrames.
northern_hemi_df = city_data_df.loc[(city_data_df["Lat"] >= 0)]
southern_hemi_df = city_data_df.loc[(city_data_df["Lat"] < 0)]

# Linear regression on the Northern Hemisphere
x_values = northern_hemi_df["Lat"]
y_values = northern_hemi_df["Max Temp"]

# Call the function.
plot_linear_regression(x_values,
                       y_values,
                       "Linear Regression on the Northern Hemisphere \n for Maximum Temperature",
                       'Max Temp',
                       (10,40))

```

Northern Hemisphere Latitude vs. Max Temperature

![Northern Hemisphere Latitude vs. Max Temperature](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig5.png?raw=true)

Southern Hemisphere Latitude vs. Max Temperature

![Southern Hemisphere Latitude vs. Max Temperature](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig6.png?raw=true)

Northern Hemisphere Latitude vs. Humidity

![Northern Hemisphere Latitude vs. Humidity](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig7.png?raw=true)

Southern Hemisphere Latitude vs. Humidity

![Southern Hemisphere Latitude vs. Humidity](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig8.png?raw=true)

Northern Hemisphere Latitude vs. Cloudiness

![Northern Hemisphere Latitude vs. Cloudiness](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig9.png?raw=true)

Southern Hemisphere Latitude vs. Cloudiness

![Southern Hemisphere Latitude vs. Cloudiness](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig10.png?raw=true)

Northern Hemisphere Latitude vs. Wind Speed

![Northern Hemisphere Latitude vs. Wind Speed](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig11.png?raw=true)

Southern Hemisphere Latitude vs. Wind Speed

![Southern Hemisphere Latitude vs. Wind Speed](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig12.png?raw=true)

Next I created four heat maps to visualize the temperature, humidity, cloudiness, and wind speed data.

Here is an example of the code used to generate the worldwide temperature heat map:

```python

# Heatmap of temperature.

# Get the latitude and longitude.
locations = city_data_df[["Lat", "Lng"]]

# Get the maximum temperature.
max_temp = city_data_df["Max Temp"]
    
# Assign the figure variable.
fig = gmaps.figure(center=(25, 0),
                   zoom_level=1.5)

# Assign the heatmap variable.
heat_layer = gmaps.heatmap_layer(locations,
                                 weights=[max(temp, 0) for temp in max_temp],
                                 dissipating=False,
                                 max_intensity=300,
                                 point_radius=4)

# Add the heatmap layer.
fig.add_layer(heat_layer)

```

Worldwide Temperature Heat Map

![Worldwide Temperature Heat Map](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig13.png?raw=true)

Worldwide Humidity Heat Map

![Worldwide Humidity Heat Map](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig14.png?raw=true)

Worldwide Cloudiness Heat Map

![Worldwide Cloudiness Heat Map](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig15.png?raw=true)

Worldwide Wind Speed Heat Map

![Worldwide Wind Speed Heat Map](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig16.png?raw=true)

Notice the similarity between the humidity and cloudiness heat maps.  This intuitively makes sense: cloudiness and humidity have an obvious correlation.

#### 3. Visualization of Travel Data

For the app I am creating, I need to prompt the user to enter the minimum and maximum temperature ranges.  For this example I am using 75-90 degrees Fahrenheit as the range.

Here is an example of the code used to create a list of preferred cities that match the input criteria:

```python

# Ask the customer to add a minimum and maximum temperature value.
min_temp = float(input("What is the minimum temperature you would like for your trip? "))
max_temp = float(input("What is the maximum temperature you would like for your trip? "))

# Filter the dataset to find the cities that fit the criteria.
preferred_cities_df = city_data_df.loc[(city_data_df["Max Temp"] <= max_temp) & \
                                       (city_data_df["Max Temp"] >= min_temp)]

```

Then I had to find the nearest hotel based on coordinates and plot a heat map of vacation spots with a pop-up marker for each city.

Here is the code used:

```python

# Create DataFrame called hotel_df to store hotel names along with city, country, max temp, and coordinates.
hotel_df = preferred_cities_df[["City", "Country", "Max Temp", "Lat", "Lng"]].copy()
hotel_df["Hotel Name"] = ""

# Set parameters to search for a hotel.
params = {
    "radius": 5000,
    "type": "lodging",
    "key": g_key
}

# Iterate through the DataFrame.
for index, row in hotel_df.iterrows():
    # Get the latitude and longitude.
    lat = row["Lat"]
    lng = row["Lng"]

    # Add the latitude and longitude to location key for the params dictionary.
    params["location"] = f"{lat},{lng}"

    # Use the search term: "lodging" and our latitude and longitude.
    base_url = "https://maps.googleapis.com/maps/api/place/nearbysearch/json"
    
    # Make request and get the JSON data from the search.
    hotels = requests.get(base_url, params=params).json()
    
    # Grab the first hotel from the results and store the name.
    try:
        hotel_df.loc[index, "Hotel Name"] = hotels["results"][0]["name"]
        print(hotels["results"][0]["name"])
    except (IndexError):
        print("Hotel not found... skipping.")

info_box_template = """
<dl>
<dt>Hotel Name</dt><dd>{Hotel Name}</dd>
<dt>City</dt><dd>{City}</dd>
<dt>Country</dt><dd>{Country}</dd>
<dt>Max Temp</dt><dd>{Max Temp} °F</dd>
</dl>
"""

# Store the DataFrame Row.
hotel_info = [info_box_template.format(**row) for index, row in hotel_df.iterrows()]

# Add a heatmap of temperature for the vacation spots and a pop-up marker for each city.
locations = hotel_df[["Lat", "Lng"]]
max_temp = hotel_df["Max Temp"]

fig = gmaps.figure(center=(30.0, 31.0),
                   zoom_level=1.5)

heat_layer = gmaps.heatmap_layer(locations,
                                 weights=max_temp,
                                 dissipating=False,
                                 max_intensity=300,
                                 point_radius=4)

marker_layer = gmaps.marker_layer(locations,
                                  info_box_content=hotel_info)

fig.add_layer(heat_layer)
fig.add_layer(marker_layer)

# Store the DataFrame Row.
hotel_info = [info_box_template.format(**row) for index, row in hotel_df.iterrows()]

```

Here is the resulting heat map with pop-up markers:

Vacation Spot Heat Map with Pop-Ups

![Vacation Spot Heat Map with Pop-Ups](https://github.com/maddenc33/World_Weather_Analysis/blob/main/weather_data/Fig17.png?raw=true)

The final map I created were a travel itinerary between four candidate cities from the list.  Below is an image of an example travel itinerary, and the pop-up markers for cities.

![Directions Layer Map](https://github.com/maddenc33/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map.png.png?raw=true)

![Directions Layer Map Markers](https://github.com/maddenc33/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map_markers.png.png?raw=true)

---
