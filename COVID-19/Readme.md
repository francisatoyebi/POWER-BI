# COVID-19 Dashboard

### There are two version of this project at the moment and the major difference between them is the way the data was ingested.

The first which is the one currently live on the [Dashboard](https://bit.ly/32GNLTj). In this version of the dashboard, the data was ingested using a link, this was done because the repository on GitHub got so big and the data cannot be scraped as it doesn't display in the repo's web page, so instead, I leveraged on the raw file link which just returns a csv file and makes the ingestion more easier.

The raw links are as below
- [Global Confirmed Cases](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv)
- [Global Recovered Cases](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv)
- [Global Death Cases](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv)

**You can go through the content below this point to understand how I imported the data earlier using webscraping**
# 

Please Note: you can find the old dashboard file [here](https://github.com/francisatoyebi/POWER-BI/blob/main/COVID-19/Old_COVID%20Dashboard.pbix)

_...the most beautiful part of this dashboard/report for me is that the data was ingested into Power BI using **PYTHON**..._
<p align="center">
<img src="https://raw.githubusercontent.com/francisatoyebi/POWER-BI/main/COVID-19/Coronavirus_Covid-19.png" alt="covid image" width="50%"/>
</p>

Coronavirus has been with us for some time now and it seems more like it will still be here for a very long time although the advancement in research and medicine has reduced the amount of fatality that occurs as a result of this virus, it is still worthy to say that the virus is still out there, everyone should please get vaccinated and follow all necessary safety measures put in place. I first created a [Personal Global cases tracking dashboard](https://github.com/francisatoyebi/corona-bokeh) in May 2020 when the virus had us all in our homes, we were all afraid to go out and we missed our loved ones a lot.

## Tools Used
* Python (Libraries: Request, BeautifulSoup and Pandas) - Used for scraping data from JHU Github repository and ingesting the data into Power BI
* Power BI for data cleaning, modelling and report building

## Data Source
The data is being pulled from the [John Hopkins University repository on Covid 19](https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series) cases.

## What you will find here
I pulled data from the JHU repository using python so I can get the data flowing into the dashboard once the report refreshes.

## My Procedures
I will be discussing my thought process and workflow regarding this dashboard although I will be paying more attention to the challenges I encountered and highlighting how I solved them. 

### 1. Data Ingestion

Initially, I decided to download the repository into my local machine then pull the CSV file which contain the data into Power BI and it was very easy to detect the flaw in that procedure as I will need to be downloading the data daily for the dashboard to be updated. Well, I eventually decided to try this out for my first time, I decided to scrape the data off the JHU repository using Python and I did that using the code below:
``` Python
#Importing necessary library
import requests
from bs4 import BeautifulSoup
import pandas as pd

#Url to the John Hopkins DataSet for confirmed cases
url1 = 'https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv'

#Url to the John Hopkins DataSet for death cases
url2 = 'https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_deaths_global.csv'

#Url to the John Hopkins DataSet for recovered cases
url3 = 'https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv'

#Using the get  request to get the HTML code for the site
data1 =requests.get(url1)

data2 =requests.get(url2)

data3 =requests.get(url3)

#Formatting the HTML using Beautiful Soup
soupcon = BeautifulSoup(data1.content,'html.parser')
soupdeath = BeautifulSoup(data2.content,'html.parser')
souprec = BeautifulSoup(data3.content,'html.parser')

def scraper(scrape):
    '''A function meant to scrape data from the JHU repository'''
    covid = scrape.find_all('table', class_ = ["highlight tab-size js-file-line-container js-code-nav-container js-tagsearch-file"])[0]
    data = []
    for row in covid.find_all('tr'):
        data.append([val.text for val in row.find_all('td')][1])
    data = pd.DataFrame(data)
    return data

#Applying the function to scrape data from JHU repository
confirmed_cases = scraper(soupcon)
death_cases = scraper(soupdeath)
recovered_cases = scraper(souprec)
```

### 2. Data Cleaning in Power Query

After Pulling the data directly into Power Query with the code above, I needed to split the data into its respective columns as the data came in a singular column which was pretty easy as I just split the data using the split by delimiter function in Power BI and it worked perfectly but when I refreshed the report the next day, I realized that the report didn't update as expected, **OOPS!** _is that not the reason I wrote the code above?_
At this point I was confused and I couldn't understand why and I didn’t know what to do... Let me give you a little prior detail
I could have split the data in Python using the same function in Power BI but for obvious reasons, when I split with Python some country names had ',' between their names and I was splitting with the ',' this added an extra column for those countries and was giving me issues but when I split with Power BI for some reason I don’t fully understand yet (still investigating), Power BI understood and didn't split at the commas between the country names.

back to the problem of the data not updating, I realised that it is because when I used the split by delimiter function, Power BI automatically generates the number of columns the split should go into and this number doesn't update when I refresh the data model. So the solution: I manually increased the number of columns to 1500 to accommodate for more incoming data and by the way each day ever since Covid was reported in a column, so 1500 columns is a lot.

**Downside of this solution is that it increases the refresh time.**

### 3. Building the Report

Well, I didn't encounter many challenges while building the report as it just flowed like water from a pipe through my fingers. You can view and interact with the dashboard [here](https://bit.ly/32GNLTj)

## Conclusion
Well, I have been following this trend for a while now using my [initial dashboard built with Python](https://github.com/francisatoyebi/corona-bokeh), though many people believe that Covid is over while some believe they are immune against it and others with several myths and beliefs, but looking at the data and from my analysis, **I can tell you that Covid is still much out there**. Only on the 28th of December 2021 over 1.3million cases were reported and on the 29th of December 2021, over 1.7million cases were reported globally, **please let us follow all the guidelines outlined by the [WHO](https://www.who.int/health-topics/coronavirus#tab=tab_1) and our various local agencies depending on your country/state/province.**

### Please Get Vaccinated

<p align="center">
<img src="https://media.istockphoto.com/photos/text-on-wooden-cubes-green-plant-in-black-pot-on-a-wooden-background-picture-id1221480881?k=20&m=1221480881&s=612x612&w=0&h=zq8znyOmqRI8w-SeN2jLa96sJH2VTofsmnLUqyZ3F84=" alt="Stay Safe" width="50%"/>
</p>

## [Click here to view/interact with the dashboard](https://bit.ly/32GNLTj)
