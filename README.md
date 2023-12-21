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

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
