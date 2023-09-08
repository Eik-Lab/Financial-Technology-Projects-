## Data Acquisition Using Web Scraping, Web Crawlers, and APIs - An Overview (Part 1)

_Author: Aryan Chugh_  
_Source: Analytics Vidhya_  
_Publication Date: Jun 11, 2020_
_Link: https://medium.com/analytics-vidhya/data-acquisition-using-web-scraping-web-crawlers-and-apis-part-1-93f63ffa5e24_

---

### Introduction

In this guide, we'll dive deep into various methodologies for extracting information from the internet, namely through web scrapers, crawlers, and Python libraries like BeautifulSoup, urllib, and requests. All code snippets are accessible in a [GitHub repository](https://github.com/aryanchugh816/Data-Science/blob/master/01%20-%20Data%20Acquisition/01%20-%20Data%20Acquisition%20-%20Web%20Scrapping%20using%20BeautifulSoup.ipynb).

### Web Scraping with BeautifulSoup

Beautiful Soup, a Python tool, offers a streamlined way to extract data from HTML and XML files. It pairs up with your preferred parser, facilitating search, navigation, and modifications.

For our tutorial, we'll pull data from the Android version history table on Wikipedia:  
[Android Version History on Wikipedia](https://en.wikipedia.org/wiki/Android_version_history)

#### Fetching HTML Data with `urllib`

```python
from urllib.request import urlopen

android_url = "https://en.wikipedia.org/wiki/Android_version_history"
android_data = urlopen(android_url)
android_html = android_data.read()
android_data.close()
```

Here, `urlopen(url)` sends an HTTP request, returning the page's HTML, which we then read and close.

#### Parsing HTML with BeautifulSoup

```python
from bs4 import BeautifulSoup as soup

android_soup = soup(android_html, 'html.parser')
tables = android_soup.findAll('table', {'class':'wikitable'})
print("Number of Tables: {}".format(len(tables)))
```

Creating a BeautifulSoup object requires specifying the parser type. The `findAll()` method then searches for specific elements, in our instance, tables with the "wikitable" class.

##### Output:

`Number of Tables: 31`

Using the browser's inspect tool aids in locating element tags, class names, or specific IDs to use in our searches.

For example:

```python
a = android_soup.findAll('h1', {})
print(a)
print(type(a))
print(len(a))
```

#### Output:

```
[<h1 class="firstHeading" id="firstHeading" lang="en">Android version history</h1>]
<class 'bs4.element.ResultSet'>
1
```

As we continue, note that the initial two rows and the last row of the Android version history table aren't of interest. Likewise, the final "References" column doesn't offer relevant information.

Let's get the table's headers:

```python
android_table = tables[0]
headers = android_table.findAll('th')
# We will use these headers as columns names hence we have to store them in a variable
column_titles = [ct.text for ct in headers]
# slice out the carriage return (\n)
column_titles = [ct.text[:-1] for ct in headers]
# We wont be using the last column('References') hence we will remove it from our column names:
column_titles = column_titles[:-1]
print(len(column_titles))
print(column_titles)
```

#### Output:

```
4
['Name', 'Version number(s)', 'Initial release date', 'API level']
```

<br/>
<br/>
Now, let's gather the rows:

```python
rows_data = android_table.findAll('tr')[1:]
print("Total number of rows: {}".format(len(rows_data)))
# We will start with the third row as the first two rows have no name for the software versions
rows_data = rows_data[2:]
print("Number of rows we are going to display: {}".format(len(rows_data)))
```

```
Total number of rows: 18
Number of rows we are going to display: 16
```

Below is a summary of our final extraction process:

```python
table_rows = []
for row_id, row in enumerate(rows_data):
    if row_id == 15:  # Skip row with missing data
        continue
    current_row = []
    row_data = row.findAll('td', {})
    for col_id, data in enumerate(row_data):
        if col_id == 4:  # Skip the "References" column
            continue
        text = data.text.replace(",", "")[:-1]
        current_row.append(text)
    table_rows.append(current_row)
```

We can then save this nested list of table rows into a CSV file. Swap out YOUR_PATH with the place you want to save the data:

```python
import pandas as pd

YOUR_PATH= = "Data/android_version_history_pandas.csv"

pd.DataFrame(table_rows, columns=column_titles).to_csv(YOUR_PATH, index=False)
data_1 = pd.read_csv(YOUR_PATH)
data_1.head(10)
```

The end result is a clean CSV of our extracted web data.

### Conclusion

This article shed light on how Beautiful Soup, as an HTML parser, efficiently cleans and extracts essential data. Stay tuned for Part 2 of this series where we'll discuss data acquisition through APIs in "Data Acquisition Using Web Scraping, Web Crawlers, and APIs (Part 2)".
