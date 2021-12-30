# COVID-19 Dashboard

_...the most beautiful part of this dashboard/report for me is that the data was ingested into Power BI using PYTHON..._

<img src="https://raw.githubusercontent.com/francisatoyebi/POWER-BI/main/COVID-19/Coronavirus_Covid-19.png" alt="covid image" width="50%"/>

Corona virus has been with us for some time now and it seem more like it will still be here for a very long time even tho the advancement in research and medicine will definitely reduce the amount of fatality that occurs as a result of this virus. I first carried out created a tracking dashboard in May 2020 when the virus had us all in our homes, we were all afraid to go out and we missed our loved ones alot. 

## Tools Used
* Python (Request, BeautifulSoup and Pandas) - Used for scraping data from JHU Github repository and ingesting the data into Power BI
* Power BI for data cleaning, modelling and report building

## Data Source
The data is being pulled from the [John Hopkins University repository on Covid 19]('https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series) cases.

## What you will find here
Basically, I pulled data from JHU repo using python so I can get the data flowing into the dashboard once the report refreshes.

## My Procedures
Basically, I will be discussing my thought process and work flow regarding this dashboard. 

**1. Data Ingestion**

Initially, I decided to download the repository into my local machine then pull the csv file which contain the data into Power BI and it was very easy to detect the flaw in that procedure as I will need to be downloading the data on a daily basis for the dasboard to be updated. Well, I eventually decided to try this out for my first time, I decided to scrape the data off JHU repository using Python and I did that using the code below:
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

**2. Data Cleaning in Power Query**
After Pulling the data directly into power query with the code above, I needed to split the data into its respective colums as the data came in a singular column which was pretty easy as I just split the data using the split by delimiter function in Power BI and it worked perfectly but when I refreshed the report the next day, I realized that the report didn't update as expected, **OOPS!** _is that not the reason I wrote the code above?_
At this point I was confused and I couldn't understand why and I dont know what to do... Let me give you a little prior detail

I could have splitted the data in Python using the same function in Power BI but for obvious reasons, when I split with Python some country names had ',' between their names and I was splitting with the ',' this added extra column for those countries and was giving me issues but when I splitted with Power BI for some reason I cant explain, Power BI understood and didnt split at the commas between the country names.

back to the problem of the data not updating, I realised that it is because when I used the split by delimeter function, Power BI automatically generates the number of columns the split should go into and this number doesnt update when I refresh the data model. So the solution: I manually increased the number of columns to 1500 so as to accomodate for more incoming data and by the way each day ever since Covid was reported in a column, so 1500 colum is a lot.

**Downside of this solution is that it increases the refresh time.**

**3. Building the Report**
Well, I didn't encounter much challenges while building the report as it just flowed like water from a pipe through my fingers.

## Conclusion
Well, I have been following this trend for a while now using my initial dashboard _built with Pyton and although many people believe that COVID is over and all, I can tell you that Covid is still much out there and with us, please let us follow all the guidelines 
