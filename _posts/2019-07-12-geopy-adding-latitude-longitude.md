---
layout: post
title: "Adding Latitude and Longitude given a location using Geocoder library"
date: 2019-07-12
categories:
  - Learn-DS
  - Obtaining-Data
description: How to add Latitude and Longitude given a location using Geocoder library
image: /images/nasa-_SFJhRPzJHs-unsplash (1).jpg
---

# About Geocoder
[Geocoder](https://geocoder.readthedocs.io/) is a python library that enables us to get Latitude and Longitude value given an address anywhere in the world. I will work with the cleaned dataset I created with Beautiful soup from this [article](https://sangwookcheon.github.io/2019/07/12/web-scraping-using-beautiful-soup/). I will Latitude and Longitude coordinates to each Postal Code.

It is important to note that Geocoder library can sometimes be inconsistent, returning 'None' instead of coordinates. This library is free-of-charge, but you might want to use a more stable but paid service like [Google Geocoding API](https://developers.google.com/maps/documentation/geocoding/start). For this task, using Geocoder library is enough.

First let's import Geocoder library and other tools:
```python
import geocoder
import pandas as pd
```
![](/in-post-images/Screen Shot 2019-07-12 at 22.16.09.png)
Here are the first 10 rows of my cleaned table. In total, there are 103 rows, meaning that there are 103 unique postal codes.

We first need to initialize lat-long coordinates to be an empty list, which will be filled for each postal code and added to the table. We also need to get postal codes from the table.

I stored the clean data table as `data` by using `read_csv` function of Pandas.
```python
# initialize variable to None
lat_lng_coords = None
postal_codes = data['PostalCode'].tolist()
```

Now is the part where I loop until coordinates are derived from the `geocoder.google` function, and then add it to the original table.
```python
# loop until we get the coordinates
for index, code in enumerate(postal_codes):
    while(lat_lng_coords is None):
        g = geocoder.google('{}, Toronto, Ontario'.format(code))
        lat_lng_coords = g.latlng
        print(lat_lng_coords)

    latitude = lat_lng_coords[0]
    longitude = lat_lng_coords[1]
    data.loc[index, 'Latitude'] = latitude
    data.loc[index, 'Longitude'] = longitude

    #Re-initialize the variable.
    lat_lng_coords = None
```
Doing this might take some time, because there is loading time when getting coordinates. When finished, now there is an updated table with two new columns, named `latitude` and `longitude`, as shown below:
![](/in-post-images/Screen Shot 2019-07-13 at 11.45.23.png)

# Recap
Geocoder library can be used to get latitude and longitude values using a street address. This library is free-of-charge, but can sometimes be inconsistent in yielding results.

Getting coordinates is important in many applications, such as putting marks on trending places on a map and clustering places based on their latitude and longitude values.

Here is a [link to Jupyter Notebook](https://github.com/SangwookCheon/non-e-stop-python-data-science/blob/master/Machine-Learning/Example%20Projects/IBM%20Capstone%20Project/add_latlon.ipynb) for this article.

You can also learn about Foursquare API, a map-related API with many social features, by referring to [this notebook](https://github.com/SangwookCheon/non-e-stop-python-data-science/blob/master/Machine-Learning/Example%20Projects/IBM%20Capstone%20Project/foursquare_api_exploration.ipynb).
