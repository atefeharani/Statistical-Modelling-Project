# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
The "Statistical Modelling with Python" project aims to leverage statistical modelling techniques using the Python programming language. The project involves the integration of various data sources, including Foursquare and Yelp. The primary goals are to explore, analyze, and model the availability of bikes at different stations.      
The goals are:   

- **Data Collection and Exploration:**   
  - Utilize APIs from Foursquare and Yelp to gather information about bike stations in a given location. 
  -Combine the obtained data into a structured format for analysis.
  - Explore the distribution of bike stations.
  - Understand the characteristics of the data.
    
- **Statistical Modelling:**

  - Build a linear regression model to analyze the relationship between location features and the number of available bikes at stations.

- **Classification Model:**
  - Transform the regression model into a classification model to predict bike availability as a binary outcome.
  - Evaluate the classification model's performance using metrics like accuracy and classification report.

- **Database Integration:**
  - Store the processed data and results in an SQLite database for future reference and analysis.

- **Documentation and Reporting:**
  - Maintain a well-structured and documented Jupyter Notebook.
  - Provide a comprehensive README file that outlines the project's objectives, methodologies, and key findings.

- Conclusion and Recommendations:
  - Summarize the key findings.



## Process


- **Data Collection:**
   - Gathered data from various sources, including Foursquare, Yelp, and CityBikes API.
   - Utilized the `requests` library to fetch information about bike stations, restaurants.

- **Data Preprocessing:**
   - Merged datasets to create a comprehensive DataFrame (`new_df`).
   - Stored relevant information in a SQLite database.

- **Statistical Modeling:**
   - Built a linear regression model using the `statsmodels` library.
   - Explored relationships between bike availability and location features (Latitudes, Longitudes).

- **Classification Model:**
   - Converted the regression model into a classification model.
   - Used a threshold to classify predictions into two classes (0 or 1).

- **README Completion:**
   - Provided explanations for key steps in the project.

- **Future Work:**
   - Outlined potential enhancements for the project.



## Results
### Bike Share Toronto Network Overview

```python
import requests

url = "http://api.citybik.es/v2/networks/bixi-toronto"
response = requests.get(url)

data = response.json()
data
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/6a3a706e-937e-439d-9ee9-6d1cb83577f0)


The data retrieved from the Bike Share Toronto network provides valuable information about the available bike stations, their locations, and current bike status.   
The network information includes network name, city, country, latitude, longitude, companies, GBFS Href.   

Station information includes station name, location, available bikes, empty slots

### Observations

- The bike stations have varying numbers of available bikes and empty slots.
- Some stations have e-bikes available.
- Payment options include key, transit card, credit card, and phone.
- Stations support both bike renting and returning.



The following code retrieves information about bike stations from a web API response. It initializes empty lists to store details such as station names, latitudes, longitudes, and the number of available bikes. Through iteration over the stations in the API response, the code collects and appends these details to their respective lists. The output displays a summary of each bike station, including its name, geographical coordinates, and the current number of available bikes. 

```python
stations = data.get("network", {}).get("stations", [])

station_names = []
latitudes = []
longitudes = []
num_bikes = []


for station in stations:
    station_names.append(station.get("name", "N/A"))
    latitudes.append(station.get("latitude", "N/A"))
    longitudes.append(station.get("longitude", "N/A"))
    num_bikes.append(station.get("free_bikes", "N/A"))

print("Station Names:", station_names)
print("Latitudes:", latitudes)
print("Longitudes:", longitudes)
print("Number of Available Bikes:", num_bikes)
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/9450e660-6030-43e7-bed8-2a57c938e359)


The following code is used to create a data frame that consists of four columns: 'Station Names,' 'Latitudes,' 'Longitudes,' and 'Number of Available Bikes'.

```python
import pandas as pd
df = pd.DataFrame({
'Station Names': station_names,
'Latitudes': latitudes,
'Longitudes': longitudes,
'Number of Available Bikes': num_bikes
})
df.head()
```
![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/06b27cbb-b57f-4638-9c05-84072ba1726f)

### Foursquare API Integration

The following code demonstrates how to make an API request to Foursquare.

```python
import requests

url = "https://api.foursquare.com/v3/places/search?radius=1000"

headers = {
    "accept": "application/json",
    "Authorization": "fsq3X8Azy660TCvS/+1UqQfMJViXsw44ZJjxaeV6nC1RYOk="
}

response = requests.get(url, headers=headers)

print(response.text)
```
![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/c5794213-b22e-4704-916f-841e82005302)


## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
