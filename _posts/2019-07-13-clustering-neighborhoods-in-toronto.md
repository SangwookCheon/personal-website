---
layout: post
title: "Clustering Neighborhoods in Toronto based on their locations using K-Means"
date: 2019-07-13
categories:
  - Learn-DS
  - Clustering
description: Using K-Means Clustering, segment neighborhoods in Toronto based on their locations
image: /images/vidar-nordli-mathisen-kcM_-FmnsVo-unsplash (1).jpg
---

See this [Jupyter Notebook](https://nbviewer.jupyter.org/github/SangwookCheon/Counrsera_Capstone/blob/master/Segmenting-and-Clustering-Neighborhoods-in-Toronto.ipynb) to view all the code and outputs.

# Introduction

Previous articles leading up to this post:
1. [Web-scraping a Wikipedia article using Beautiful Soup - Table](https://sangwookcheon.github.io/2019/07/12/web-scraping-using-beautiful-soup/p)
2. [Adding Latitude and Longitude given a location using Geocoder library](https://sangwookcheon.github.io/2019/07/12/geopy-adding-latitude-longitude/)

In this post I will use a cleaned dataset created from the previous two articles and cluster neighborhoods in Toronto based on their locations. I will use K-Means clustering, a popular unsupervised machine learning technique.

# Setting up the data

First step is to install all the libraries needed for this project:
```python
import numpy as np # library to handle data in a vectorized manner

import pandas as pd # library for data analsysis
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)

import json # library to handle JSON files

#!conda install -c conda-forge geopy --yes # uncomment this line if you haven't completed the Foursquare API lab
from geopy.geocoders import Nominatim # convert an address into latitude and longitude values

import requests # library to handle requests
from pandas.io.json import json_normalize # tranform JSON file into a pandas dataframe

# Matplotlib and associated plotting modules
import matplotlib.cm as cm
import matplotlib.colors as colors

# import k-means from clustering stage
from sklearn.cluster import KMeans

#!conda install -c conda-forge folium=0.5.0 --yes # uncomment this line if you haven't completed the Foursquare API lab
import folium # map rendering library
```

In the previous articles, I scraped a Wikipedia page containing a table about Neighborhoods in Canada, processed the table, and added latitude/longitude coordinates for each Postal Code. Below is the table to work with:

![](/in-post-images/Screen Shot 2019-07-13 at 11.45.23.png)

I named this table `neighborhoods`. I decided to use data pertaining only to Toronto, so I now need to get rows with borough names that contain 'Toronto'. To do this:

```python
for i in range(0, len(neighborhoods)):
    if 'Toronto' not in neighborhoods.loc[i, 'Borough']:
        neighborhoods.drop(i, inplace=True)

neighborhood.reset_index(drop=True)
```
Now we have this new table:
![](/in-post-images/Screen Shot 2019-07-13 at 16.11.20.png)

There are 38 columns in total, and I used `reset_index` function reset the index to 1, 2, 3, ... as the table was modified.

## Quick Visualization

Let's start by visualizing where the different neighborhoods are in Toronto. After quick Google Search, it is found that the lat-long of Toronto is [43.6532, -79.3832].

```python
latitude = 43.6532
longitude = -79.3832

# create map of Toronto using latitude and longitude values
map_toronto = folium.Map(location=[latitude, longitude], zoom_start=10)

# add markers to map
for lat, lng, borough, neighborhood in zip(neighborhoods['Latitude'], neighborhoods['Longitude'], neighborhoods['Borough'], neighborhoods['Neighborhood']):
    label = '{}, {}'.format(neighborhood, borough)
    label = folium.Popup(label, parse_html=True)
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        popup=label,
        color='blue',
        fill=True,
        fill_color='#3186cc',
        fill_opacity=0.7,
        parse_html=False).add_to(map_toronto)

map_toronto
```
![](/in-post-images/Screen Shot 2019-07-13 at 16.38.16.png)
[View Map]()

# Exploring one neighborhood

Before going into clustering neighborhoods, let's explore one neighborhood as a starter. [Foursquare API](https://developer.foursquare.com/) will be used to explore top 10 venues around the first neighborhood in the table.

```python
url = 'https://api.foursquare.com/v2/venues/explore?client_id={}&client_secret={}&ll={},{}&v={}&radius={}&limit={}'.format(CLIENT_ID, CLIENT_SECRET, latitude, longitude, VERSION, radius, LIMIT)
results = requests.get(url).json()
results
```
This yields the following json file containing 10 search results (only two are shown to save space):

```json
{'meta': {'code': 200, 'requestId': '5d29e93b6bdee6002cda5324'},
 'response': {'groups': [{'items': [{'reasons': {'count': 0,
       'items': [{'reasonName': 'globalInteractionReason',
         'summary': 'This spot is popular',
         'type': 'general'}]},
      'referralId': 'e-0-5227bb01498e17bf485e6202-0',
      'venue': {'categories': [{'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/parks_outdoors/neighborhood_',
          'suffix': '.png'},
         'id': '4f2a25ac4b909258e854f55f',
         'name': 'Neighborhood',
         'pluralName': 'Neighborhoods',
         'primary': True,
         'shortName': 'Neighborhood'}],
       'id': '5227bb01498e17bf485e6202',
       'location': {'cc': 'CA',
        'city': 'Toronto',
        'country': 'Canada',
        'distance': 168,
        'formattedAddress': ['Toronto ON', 'Canada'],
        'labeledLatLngs': [{'label': 'display',
          'lat': 43.65323167517444,
          'lng': -79.38529600606677}],
        'lat': 43.65323167517444,
        'lng': -79.38529600606677,
        'state': 'ON'},
       'name': 'Downtown Toronto',
       'photos': {'count': 0, 'groups': []}}},
     {'reasons': {'count': 0,
       'items': [{'reasonName': 'globalInteractionReason',
         'summary': 'This spot is popular',
         'type': 'general'}]},
      'referralId': 'e-0-4ad4c05ef964a520a6f620e3-1',
      'venue': {'categories': [{'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/parks_outdoors/plaza_',
          'suffix': '.png'},
         'id': '4bf58dd8d48988d164941735',
         'name': 'Plaza',
         'pluralName': 'Plazas',
         'primary': True,
         'shortName': 'Plaza'}],
       'id': '4ad4c05ef964a520a6f620e3',
       'location': {'address': '100 Queen St W',
        'cc': 'CA',
        'city': 'Toronto',
        'country': 'Canada',
        'crossStreet': 'at Bay St',
        'distance': 106,
        'formattedAddress': ['100 Queen St W (at Bay St)',
         'Toronto ON M5H 2N1',
         'Canada'],
        'labeledLatLngs': [{'label': 'display',
          'lat': 43.65227047322295,
          'lng': -79.38351631164551}],
        'lat': 43.65227047322295,
        'lng': -79.38351631164551,
        'postalCode': 'M5H 2N1',
        'state': 'ON'},
       'name': 'Nathan Phillips Square',
       'photos': {'count': 0, 'groups': []}}},
```

How can we convert this json file into a Pandas DataFrame? We need to first extract only the information we need, flatten the json file, and construct a table.

```python
# function that extracts the category of the venue
def get_category_type(row):
    try:
        categories_list = row['categories']
    except:
        categories_list = row['venue.categories']

    if len(categories_list) == 0:
        return None
    else:
        return categories_list[0]['name']

venues = results['response']['groups'][0]['items']

nearby_venues = json_normalize(venues) # flatten JSON

# filter columns
filtered_columns = ['venue.name', 'venue.categories', 'venue.location.lat', 'venue.location.lng']
nearby_venues =nearby_venues.loc[:, filtered_columns]

# filter the category for each row
nearby_venues['venue.categories'] = nearby_venues.apply(get_category_type, axis=1)

# clean columns
nearby_venues.columns = [col.split(".")[-1] for col in nearby_venues.columns]

nearby_venues.head()
```

All the information about each venue is stored in `items` section of the json.

![](/in-post-images/Screen Shot 2019-07-13 at 22.33.33.png)

# Exploring All Neighborhoods in Toronto
Now that one neighborhood is explored, let's explore 10 venues for every neighborhood in Toronto.

Function below is used to repeat the process above for all 38 neighborhoods.
```python
def getNearbyVenues(names, latitudes, longitudes, radius=500):

    venues_list=[]
    for name, lat, lng in zip(names, latitudes, longitudes):
        print(name)

        # create the API request URL
        url = 'https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(
            CLIENT_ID,
            CLIENT_SECRET,
            VERSION,
            lat,
            lng,
            radius,
            LIMIT)

        # make the GET request
        results = requests.get(url).json()["response"]['groups'][0]['items']

        # return only relevant information for each nearby venue
        venues_list.append([(
            name,
            lat,
            lng,
            v['venue']['name'],
            v['venue']['location']['lat'],
            v['venue']['location']['lng'],
            v['venue']['categories'][0]['name']) for v in results])

    nearby_venues = pd.DataFrame([item for venue_list in venues_list for item in venue_list])
    nearby_venues.columns = ['Neighborhood',
                  'Neighborhood Latitude',
                  'Neighborhood Longitude',
                  'Venue',
                  'Venue Latitude',
                  'Venue Longitude',
                  'Venue Category']

    return(nearby_venues)

#Run the function to get venues for all 38 Neighborhoods
toronto_venues = getNearbyVenues(names=neighborhoods['Neighborhood'],
                                   latitudes=neighborhoods['Latitude'],
                                   longitudes=neighborhoods['Longitude']
                                  )
```

To show the first five rows:
![](/in-post-images/Screen Shot 2019-07-13 at 22.40.47.png)

### One-hot Encoding
One-hot encoding is used to use Clustering on this data table. Now, let's do this to the `toronto_vanues` table.
```python
# one hot encoding
toronto_onehot = pd.get_dummies(toronto_venues[['Venue Category']], prefix="", prefix_sep="")

# add neighborhood column back to dataframe
toronto_onehot['Neighborhood'] = toronto_venues['Neighborhood']

# move neighborhood column to the first column
fixed_columns = [toronto_onehot.columns[-1]] + list(toronto_onehot.columns[:-1])
toronto_onehot = toronto_onehot[fixed_columns]

toronto_onehot.head()
```
![](/in-post-images/Screen Shot 2019-07-22 at 12.21.11.png)

```python
toronto_grouped = toronto_onehot.groupby('Neighborhood').mean().reset_index()
toronto_grouped
```
This groups the table by 'Neighborhood' leaving a single row (instead of 10) for each neighborhood. We take the mean of frequency of each category.

Briefly analyzing the grouped table leads to this: It can be noticed that Toronto has a variety of restaurants with different styles, such as Greek, Eastern European, Sushi, Indian, American, etc. Cafe and Coffee Shop shows up often as one of the top 5 venues.

```python
def return_most_common_venues(row, num_top_venues):
    row_categories = row.iloc[1:]
    row_categories_sorted = row_categories.sort_values(ascending=False)

    return row_categories_sorted.index.values[0:num_top_venues]

num_top_venues = 10

indicators = ['st', 'nd', 'rd']

# create columns according to number of top venues
columns = ['Neighborhood']
for ind in np.arange(num_top_venues):
    try:
        columns.append('{}{} Most Common Venue'.format(ind+1, indicators[ind]))
    except:
        columns.append('{}th Most Common Venue'.format(ind+1))

# create a new dataframe
neighborhoods_venues_sorted = pd.DataFrame(columns=columns)
neighborhoods_venues_sorted['Neighborhood'] = toronto_grouped['Neighborhood']

for ind in np.arange(toronto_grouped.shape[0]):
    neighborhoods_venues_sorted.iloc[ind, 1:] = return_most_common_venues(toronto_grouped.iloc[ind, :], num_top_venues)

neighborhoods_venues_sorted.head()
```

![](https://github.com/SangwookCheon/pictures/blob/master/Screen%20Shot%202019-07-22%20at%2012.26.46.png?raw=true)

# Clustering
Based on the `toronto_grouped` table, we can cluster neighborhoods according to their trending places. From this point, please check out the [Jupyter Notebook](https://nbviewer.jupyter.org/github/SangwookCheon/Counrsera_Capstone/blob/master/Segmenting-and-Clustering-Neighborhoods-in-Toronto.ipynb) from `in [56]:`. Here, I analyze each of the 5 clusters to see if there is any distinguishable factor.

Thank you for reading this article! I will come back with an article that focuses on an issue that is entirely based on my personal interest :smile: I am currently working on Seoul's public dataset on its air quality. Recently air pollution is becoming a more serious problem in South Korea, and unfortunately, this is not simply the problem of polluted air coming in from China. I will analyze relevant datasets provided by the government to see if there are any overlooked patterns.
