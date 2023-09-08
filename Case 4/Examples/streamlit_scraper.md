Building a Search Engine Scraper with Streamlit
===============================================

##### *Web scraping is an effective technique to acquire data from the web with minimal manual effort*

---
_Author: KSV Muralidhar_

_Source: Nerd For Tech_ 

_Publication Date: Aug 10, 2021_

_Link: https://medium.com/nerd-for-tech/building-a-search-engine-scraper-with-streamlit-b616e5bd293c_

---

## Web scraping
Web scraping enables us to acquire data from the web with minimal manual effort. However, this technique must be used with caution such that it doesn’t degrade the performance of a website. Before proceeding, we’ll discuss a few tips to follow to protect ourselves from being blacklisted by a website as a consequence of degrading its performance.

1.  We shouldn’t overload a website’s server with requests, as in a DDoS attack, which is an unethical practice. If we overload a website’s server, we may get blacklisted. We can use the **sleep()** function of the **time** package in Python to give random pauses between our requests.
2.  We must keep on switching user-agents. We may maintain a csv file of user-agents and keep switching them often. We’ll discuss more on user agents later in this article.
3.  We must keep on saving the scraped data. This ensures that we don’t loose our data when a website blacklists us. We can restart scraping from the last saved checkpoint instead of starting over.
4.  If possible, we may use the **selenium** package of Python which enables us to mimic human activity like scrolling, clicks, etc. This may lower our chance of being blacklisted.
5.  We must go through and obey a website’s ‘robots.txt’ to identify the pages prohibited from being accessed and also the user agents that aren’t allowed to access the website. We can access the ‘robots.txt’ using ‘domain/robots.txt’. For example, we can access the robots.txt of bing.com using [bing.com/robots.txt](http://bing.com/robots.txt).

## Search engine scraper

In this article, we’ll build a search engine scraper\* using BeautifulSoup and Streamlit. The app takes a search string as an input, scrapes the search results from the first page of Bing and displays them. The app also returns a data frame of the search results. Below is the Python code of the Streamlit app and is clearly explained using comments.

**\* This app is built only to illustrate the process of web scraping. It is highly recommended to use the search engine’s API for extracting search results. Web scraping should only be the last resort.**

Link to gist: [bing_scrape_streamlit.py](https://gist.github.com/ksv-muralidhar/a78830a9a2b75ecb71eb79599a391c6a#file-bing_scrape_streamlit-py)


```python

import pandas as pd
from bs4 import BeautifulSoup
import requests as r
import streamlit as st

st.markdown('<h1 style="background-color: gainsboro; padding-left: 10px; padding-bottom: 20px;">Search Engine Scraper</h1>', unsafe_allow_html=True)
query = st.text_input('', help='Enter the search string and hit Enter/Return')
query = query.replace(" ", "+") #replacing the spaces in query result with +

if query: #Activates the code below on hitting Enter/Return in the search textbox
    try:#Exception handling 
        req = r.get(f"https://www.bing.com/search?q={query}",
                    headers = {"user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36"})
        result_str = '<html><table style="border: none;">' #Initializing the HTML code for displaying search results
        
        if req.status_code == 200: #Status code 200 indicates a successful request
            bs = BeautifulSoup(req.content, features="html.parser") #converting the content/text returned by request to a BeautifulSoup object
            search_result = bs.find_all("li", class_="b_algo") #'b_algo' is the class of the list object which represents a single result
            search_result = [str(i).replace("<strong>","") for i in search_result] #removing the <strong> tag
            search_result = [str(i).replace("</strong>","") for i in search_result] #removing the </strong> tag
            result_df = pd.DataFrame() #Initializing the data frame that stores the results
            
            for n,i in enumerate(search_result): #iterating through the search results
                individual_search_result = BeautifulSoup(i, features="html.parser") #converting individual search result into a BeautifulSoup object
                h2 = individual_search_result.find('h2') #Finding the title of the individual search result
                href = h2.find('a').get('href') #title's URL of the individual search result
                cite = f'{href[:50]}...' if len(href) >= 50 else href # cite with first 20 chars of the URL
                url_txt = h2.find('a').text #title's text of the individual search result
                #In a few cases few individual search results doesn't have a description. In such cases the description would be blank
                description = "" if individual_search_result.find('p') is None else individual_search_result.find('p').text
                #Appending the result data frame after processing each individual search result
                result_df = result_df.append(pd.DataFrame({"Title": url_txt, "URL": href, "Description": description}, index=[n]))
                count_str = f'<b style="font-size:20px;">Bing Search returned {len(result_df)} results</b>'
                ########################################################
                ######### HTML code to display search results ##########
                ########################################################
                result_str += f'<tr style="border: none;"><h3><a href="{href}" target="_blank">{url_txt}</a></h3></tr>'+\
                f'<tr style="border: none;"><strong style="color:green;">{cite}</strong></tr>'+\
                f'<tr style="border: none;">{description}</tr>'+\
                f'<tr style="border: none;"><td style="border: none;"></td></tr>'
            result_str += '</table></html>'
            
        #if the status code of the request isn't 200, then an error message is displayed along with an empty data frame        
        else:
            result_df = pd.DataFrame({"Title": "", "URL": "", "Description": ""}, index=[0])
            result_str = '<html></html>'
            count_str = '<b style="font-size:20px;">Looks like an error!!</b>'
            
    #if an exception is raised, then an error message is displayed along with an empty data frame
    except:
        result_df = pd.DataFrame({"Title": "", "URL": "", "Description": ""}, index=[0])
        result_str = '<html></html>'
        count_str = '<b style="font-size:20px;">Looks like an error!!</b>'
    
    st.markdown(f'{count_str}', unsafe_allow_html=True)
    st.markdown(f'{result_str}', unsafe_allow_html=True)
    st.markdown('<h3>Data Frame of the above search result</h3>', unsafe_allow_html=True)
    st.dataframe(result_df)

```

The above code can be run by executing the following command in the terminal of a local machine.

```
streamlit run filename.py
```

**filename.py** is the python script file containing the above code. Below is a screenshot of the output we get after executing the command. In this case, the name of the file containing the Python script is **bing\_scrape\_streamlit.py**. Hence, the command to run the code would be as shown below.

```
streamlit run bing\_scrape\_streamlit.py
```

Below is the output of the app in a local browser after executing the above command in the terminal of a local machine. We can also see a data frame of the results at the bottom of the output.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/1183f15e-2d94-4c1c-945c-e9dd3131f99a)



How are the extracted search results useful?
--------------------------------------------

*   To find the website of a list of companies, assuming the first search result (of a company) would mostly be a company’s website. This can be achieved by looping the code over a list of companies and storing the scraped results / responses from API requests. Most API also support bulk requests. However, this may need some further validation, as not every first search result contains a company’s website.
*   To find the URLs of the social media pages of people/companies. This can be done by including a person/company’s name and the name of the required social media website in the search string. Site search may be more helpful.
*   To find the latest news related to a company.

All the above mentioned tasks demand a lot of manual effort that increases with the size of the entities to search. Hence, web scraping/APIs reduce this manual effort to a greater extent. However, human-in-the-loop is required to validate the results.

We’ll discuss a few questions that may arise in the mind of a beginner in web scraping.

**1. What is requests.get()?**

‘requests’ package enables us to handle HTTP requests in Python. Its **get()** function sends a query to the web server and returns a response.

**2. What is a user agent?**

User agent describes the browser and operating environment being used. This enables the web server to send the content that best suits our browser/device. A browser on a mobile device has a different user agent form that of Google Chrome on a Windows system. Switching user agents protects us from being blacklisted as the web server gets requests from multiple devices.

**3. What is BeautifulSoup?**

BeautifulSoup parses HTML/XML documents enabling us to extract data from web pages. We must pass the content/text of a request’s response to the BeautifulSoup class for it to parse. We can then extract the elements/tags of the web page’s HTML script.

**4. How do we select the right HTML tag?**

We can identify the right tag by inspecting the source code of a web page. The **‘inspect’** functionality is available in all the popular web browsers. We’ll discuss how to **‘inspect’** a web page’s source code.

**Step 1:** We’ll right click on the element in a web page that we want to scrape and select **‘Inspect’**. For search engine scraper, we want to scrape the title, URL and description of a search result. Hence, we’ll right click on the URL in the first search result.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/da027152-799a-4b98-bb83-ad280a8f57cc)


**Step 2:** We must select a tag (parent tag) that embeds all the tags (child tags) we want to scrape. For example, we need to scrape the search results, so we’ll select a parent tag that embeds all the tags attached to a single search result. In the figure below, `<li class=”b\_algo”>` embeds all the tags attached to a single search result. Now we can extract each child tag from `<li class=”b\_algo”>` by processing each `<li class=”b\_algo”>` (search result) individually.

We shouldn’t select only `<h2>` (title of search result) or `<p>` (description of a search result). This may pick up other `<h2>`'s and `<p>`'s in the page other than the ones associated with the search results. This may also make it difficult for us to map a list of `<h2>` to a list of `<p>`'s.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/45490612/3f430001-d423-416c-ae8b-e3859edc0d83)


Hence, web scraping helps us in reducing the manual effort required for acquiring data from the web. We may use the same code multiple times to extract data from a website, provided the structure of the website stays the same. Code to extract the HTML script from a web page is similar for all the websites. What’s different is the process of cleaning and extracting the required data from the HTML script.

However, I highly recommend using search engine APIs for extracting search results. Web scraping should only be the last resort. Even if you opt for web scraping, it must be done without degrading the performance of a website and should only be used to extract the publicly available information.

Know more about my work at [https://ksvmuralidhar.in/](https://ksvmuralidhar.in/)
