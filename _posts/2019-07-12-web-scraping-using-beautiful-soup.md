---
layout: post
comments: true
title: "Web-scraping a Wikipedia article using Beautiful Soup - Table"
date: 2019-07-12
categories:
  - Learn-DS
  - Obtaining-Data
description: How to scrape Wikipedia page using Beautiful Soup library in Python.
image:
---
# About Beautiful Soup

Oftentimes, data we are looking for isn't stored in the best format possible. While in some cases data is already prepared for us, such as dataset on Kaggle, sometimes we need to scrape the data out of a certain source. In my case, I had to get a table out of this [Wikipedia article](https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M) containing all the neighborhoods in Canada.

I've recently found that a Python library called Beautiful Soup allows me to efficiently carry out this task.

According to [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/),

> Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work.

As of now, it is important to know that this library enables us to automatically scrape or get web content that is cleanable out of any website.

![](/in-post-images/Screen Shot 2019-07-12 at 21.43.13.png)
*This is the wikipedia article. I need to get the table out of this webpage so that it can be analyzed using Python.*

```html
<table class="wikitable sortable jquery-tablesorter">
<thead><tr>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending">Postcode</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending">Borough</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending">Neighbourhood
</th></tr></thead><tbody>
<tr>
<td>M1A</td>
<td>Not assigned</td>
<td>Not assigned
</td></tr>
<tr>
<td>M2A</td>
<td>Not assigned</td>
<td>Not assigned
</td></tr>
<tr>
<td>M3A</td>
<td><a href="/wiki/North_York" title="North York">North York</a></td>
<td><a href="/wiki/Parkwoods" title="Parkwoods">Parkwoods</a>
</td></tr>
<tr>
...
```
In HTML, the table looks like the above. `<tr>` is a unique row in the table, and each `<td>` is a table value.

Using Beautiful Soup, we will convert this HTML code into a Pandas DataFrame, which then can be clean and analyzed using Python.

# Import Libraries

First we import important libraries including Beautiful Soup, and other tools that go hand-in-hand.

```python
import numpy as np
import pandas as pd
import requests
from bs4 import BeautifulSoup
```
Note: bs4 might need to be installed. 'Requests' library gets HTML file from the webpage.

# Loading HTML and finding the table section

Now load the HTML file from a Wikipedia page containing information about postal codes of Canada. This will be processed using Beautiful Soup.

```python
#.text to get pure HTML file.
source = requests.get('https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M').text
soup = BeautifulSoup(source, 'lxml')

# To quickly see the html
print(soup.prettify())
```
`.prettify` puts indents on HTML file so that is is more readable.

```python
# From HTML, find a portion pertaining to the table.
#_class is used to find a unique class name of the table, to make sure we find the correct table if there are multiple tables.
raw_table = soup.find('table', class_='wikitable sortable')
print(raw_table.prettify())
```
# Processing

Now let's process the HTML of the table to convert it into a DataFrame. There are many rows with unassigned borough, and we need to ignore them as they have missing information that cannot be filled. A row is missing 'Neighborhood', then we should set it to be the same as borough.
```python
#Get all rows from the table. <tr> tag refers to a row.
rows_all = raw_table.find_all('tr')

#Initialize empty list.
row_data = []

#For each row, find table values and append it to the list.
for row in rows_all:
    td = row.find_all('td')

    #A list of table values in one row
    row = [i.text for i in td]

    #Only add cells with borough, and with three values
    if len(row) != 0 and row[1] != 'Not assigned':
        row[2] = row[2].rstrip() #to remove /n
        #If Neighborhood is not assigned, the name is same as borough
        if row[2] == 'Not assigned':
            row[2] = row[1]
          #Append the row to the big list.
        row_data.append(row)
```

Below is how to convert the list to a Pandas DataFrame:
```python
data = pd.DataFrame(row_data, columns=['PostalCode', 'Borough', 'Neighborhood'])

#preview ten rows of DataFrame
data.head(10)
```
![](/in-post-images/Screen Shot 2019-07-12 at 22.09.12.png)

The table is correctly loaded! We need to do one more step, which is to combine neighborhoods with same Postal Code into one row. So some of the postal codes will have multiple neighborhoods listed and separated by a comma.
```python
postal_codes = data.PostalCode.unique()
print(postal_codes)
```
![](/in-post-images/Screen Shot 2019-07-12 at 22.12.19.png)
These are the unique Postal Codes.

```python
#Set a new empty table with just column names.
clean_data = pd.DataFrame(columns=['PostalCode', 'Borough', 'Neighborhood'], index=None)

#Populate the new dataframe with unique postal codes.
for code in postal_codes:
    # Get DataFrame containing rows with same postal code.
    df = data.loc[data['PostalCode'] == code]
    borough = df.iloc[0, 1]
    # Join each column into a string containing all neighborhoods in the same code.
    df = ', '.join(df['Neighborhood'].tolist())

    #Add to the new dataframe
    clean_data = clean_data.append({'PostalCode': code, 'Borough': borough , 'Neighborhood': df}, ignore_index=True)
```
![](/in-post-images/Screen Shot 2019-07-12 at 22.16.09.png)
Now we can see that some postal codes now have a neighborhoods column with multiple names. The table is fully cleaned now, and ready to go. Sometimes it is useful to save this cleaned table as a csv to the local machine.

# Recap

In this post I explored some of the features of Beautiful Soup. It is often used together with Requests library to load HTML and find what we need from the HTML file. We can use `.find` and `find_all` to get textvalues out of HTML.

I will explore the library deeper in a future post. Thank you for reading!
