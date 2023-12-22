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

This section processes data from the Foursquare API response, extracts relevant information, and organizes it into a data frame for further analysis.

```python
# api settings
client_id = "XRE0S0B3T2I2IOZNA3FQLPO52LJBCONQHSW5TYPIX2BX5RLM"
client_secret = "UDMTB32WT5RCTF5EOF01MN0XY4W5GWRWGKRSIGZJP24TMFKY"
v = '20231010'

location = '43.653226,-79.3831843' # Toronto
url = 'https://api.foursquare.com/v2/venues/explore'

params = {
    'client_id': client_id,
    'client_secret': client_secret,
    'v': v,
    'll': location,
    'query': 'bike',
    'limit': 50,
    'radius': 1000
}

resp = requests.get(url=url, params=params)
data = resp.json()
data
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/3fe9f30b-387f-4421-a383-7a0bca9adc4d)

The code below retrieves detailed information about restaurants from the Foursquare API, including ratings, names, addresses, price categories, likes, and more.

```python
# restaurant details (rating,name,place, ...)

url = 'https://api.foursquare.com/v2/venues/explore'
all_restaurants = []
for restaurant in data['response']['groups'][0]['items']:
    id_restaurant = restaurant['venue']['id']

    url = f'https://api.foursquare.com/v2/venues/{id_restaurant}'
    params = {
        'client_id': client_id,
        'client_secret': client_secret,
        'v': v
    }

    resp = requests.get(url=url, params=params)

    if resp.status_code == 200:
        restaurant_data = resp.json()
        venue = restaurant_data['response']['venue']

        all_restaurants.append(
            {
            'name': venue['name'],
            'address': ', '.join(venue['location']['formattedAddress']),
            #'url': venue['url'],
            'price_category': venue['price']['tier'] if venue.get('price') else None,
            'likes': venue['likes']['count'],
            'rating': venue['rating'],
            'api': 'foursquare',
        })
df_fs = pd.DataFrame(all_restaurants)
df_fs
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/ccda729b-6072-40c5-bea4-12f2c43a7cf2)


```python
import requests

api_key = "cFFfkZl10-YC0eRoV3TuuH5C3jnbXdUm9ZkF7xm_S2abcOI4dz678UhWbmcYtmjnOp2I9TmISOw3j5S-Pjhgd46kFGqBHL4Z40BeLo6xBskKc5URK5AZMTw8fct-ZXYx"
url = 'https://api.yelp.com/v3/businesses/search'
headers = {
    'Authorization': f'Bearer {api_key}'
}
params = {
    'term': 'restaurants',
    'location':  '40.7243,-74.0018',  # NYC
    'radius': 1000
}

response = requests.get(url, headers=headers, params=params)
data = response.json()
all_restaurants = []

for business in data['businesses']:
    name = business['name']
    rating = business['rating']
    location = business['location']['address1']

    all_restaurants.append(
        {
            'name': name,
            'address': location,
            'rating': rating,
            'api': 'yelp',
        }
    )

df_fs2 = pd.DataFrame(all_restaurants)
df_fs2
```
![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/d51b6472-eb55-4408-ba32-647124e34f2f)

The provided code utilizes the pandas library to sort a DataFrame (`df_fs`) of restaurant data based on their ratings in descending order. The top 10 highest-rated restaurants are then extracted and stored in a new DataFrame (`top_10_restaurants`).

```python
import pandas as pd

df_sorted = df_fs.sort_values(by='rating', ascending=False)

top_10_restaurants = df_sorted.head(10)

top_10_restaurants
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/0fe15af1-8bf9-4572-a43a-76ecc2a98d0d)

The provided code merges two DataFrames (`df_fs` and `df_fs2`) based on common columns such as 'name', 'address', 'rating', and 'api'. The merge is performed using an outer join, meaning that it includes all rows from both DataFrames.
```python
new_df = df_fs.merge(df_fs2, on=['name', 'address', 'rating', 'api'], how='outer')
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/5b670d9c-bf49-4c63-851d-1dcb53470157)


The provided code creates a scatter plot to visualize the relationship between the 'Rating' and 'Likes' columns in the merged DataFrame (`new_df`). The scatter plot helps to identify any patterns or trends between these two variables.


```python
import matplotlib.pyplot as plt

plt.figure(figsize=(8, 6))
plt.scatter(new_df['rating'], new_df['likes'])
plt.title('Relationship between Rating and Likes')
plt.xlabel('Rating')
plt.ylabel('Likes')
plt.show()
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/4e823dd1-e524-41e9-b786-ededb3dba0a9)


### Storing Data in SQLite Database

The provided code demonstrates the process of storing DataFrames (`df_fs`, `df_fs2`, and `new_df`) into an SQLite database.

#### Instructions:
1. Connect to an SQLite database and specify the path.
2. Save DataFrame `df_fs` to an SQLite table named 'foursquare_data'.
3. Save DataFrame `df_fs2` to an SQLite table named 'yelp_data'.
4. Save DataFrame `new_df` to an SQLite table named 'merged_data'.
5. Commit changes and close the database connection.

```python

import sqlite3

db_path =  "/content/my_database.db"
conn = sqlite3.connect(db_path)
df_fs.to_sql('foursquare_data', conn, if_exists='replace', index=False)
df_fs2.to_sql('yelp_data', conn, if_exists='replace', index=False)
new_df.to_sql('merged_data', conn, if_exists='replace', index=False)
conn.commit()
conn.close()

print("Results have been stored in the SQLite database.")

```
To ensure the successful joining of DataFrames and inspect the data, the code below displays the first few rows of each DataFrame (`df_fs`, `df_fs2`, and `new_df`).

```python
print("Original DataFrame (df_fs):")
print(df_fs.head())

print("\nOriginal DataFrame (df_fs2):")
print(df_fs2.head())

print("\nMerged DataFrame (new_df):")
print(new_df.head())
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/562c77b9-12b1-4e26-bb02-1913540943ce)

Check the number of rows in each DataFrame:

```python
print("Number of rows in df_fs:", len(df_fs))
print("Number of rows in df_fs2:", len(df_fs2))
print("Number of rows in new_df:", len(new_df))
```

![image](https://github.com/atefeharani/Statistical-Modelling-Project/assets/67924193/e99acaef-88b9-4219-8815-19cbcfd52be5)






## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
