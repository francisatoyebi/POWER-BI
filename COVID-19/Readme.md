# COVID-19 Dashboard

_...the most beautiful part of this dashboard/report for me is that the data was ingested into Power BI using PYTHON..._

![animated illustration of COVID](https://raw.githubusercontent.com/francisatoyebi/POWER-BI/main/COVID-19/Coronavirus_Covid-19.png)

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

1. Data Ingestion
Initially, I decided to download the repository into my local machine then pull the csv file which contain the data into Power BI and it was very easy to detect the flaw in that procedure as I will need to be downloading the data on a daily basis for the dasboard to be updated. Well, I eventually decided to try this out for my first time, I decided to scrape the data off JHU repository using Python and I did that using the code below:
``` Python:
import padas as pd

```

## Discovery

## Conclusion
