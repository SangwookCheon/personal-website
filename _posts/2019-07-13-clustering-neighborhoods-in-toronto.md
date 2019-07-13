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

# Introduction

Previous articles leading up to this post:
1. [Web-scraping a Wikipedia article using Beautiful Soup - Table](https://sangwookcheon.github.io/2019/07/12/web-scraping-using-beautiful-soup/p)
2. [Adding Latitude and Longitude given a location using Geocoder library](https://sangwookcheon.github.io/2019/07/12/geopy-adding-latitude-longitude/)

In this post I will use a cleaned dataset created from the previous two articles and cluster neighborhoods in Toronto based on their locations. I will use K-Means clustering, a popular unsupervised machine learning technique.

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

There are 37 columns in total, and I used `reset_index` function reset the index to 1, 2, 3, ... as the table was modified.

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
