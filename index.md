## Welcome to CYPLAN255!

# Visualizing Demographic Changes Near Bay Area Rail Stations

### Scope and Motivation

This project is focusing on understanding the impacts of rail stations on key demographic areas. The Bay Area consist of multiple transportation agencies within the nine Bay Area Counties. Within those counties, there are seven rail lines that serves the residents of the Bay Area. The intended investigatation is whether rail transit stops are in denser areas, higher income areas, and/or areas of high white population concentration. The insipiration to answer this because of its equity applications, as rail is faster, reliable, and heavily subsidized compared to the bus.

The rail lines that will be focused on are BART (Bay Area Rapid Transit), SFMTA MUNI (San Francisco Municipal Railway), and SMART (Sonoma-Marin Area Rail Transit). 

Below is the graph of stations that are focused on in the project:

![image](https://user-images.githubusercontent.com/98051167/167046581-77f0ae90-2525-474d-bcef-dcce2fd70ad7.png)

The time frame that was focused on is between 2010 and 2020. The census tracts around each respective station of focus was analyed for temporal changes, before and after the station was opened. The two demographic areas of focus was Household Income and Race. 

## Methodology

In order to investigate the impact of Bay Area Rail Stations, we collected data from Metropolitan Transportation Commission (MTC) Data and American Community Survey (ACS) Data from 2010 - 2020. The MTC dataset provided train station data while the ACS data provides average household income for each respective tract.

Packages that are ultilized in juypter notebook for the analysis are below:

```markdown
import json      # library for working with JSON-formatted text strings
import requests  # library for accessing content from web URLs
import pprint    # library for cleanly printing Python data structures
pp = pprint.PrettyPrinter()
import pandas as pd
import earthpy as et
import geopandas as gpd
from matplotlib import pyplot as plt
```
The data that was used was extracted from MTC and ACS Data. 

The tracts were exported from the MTC API through the code below while train stations were exported from a CSV file. 
```markdown
tracts= gpd.read_file("https://services3.arcgis.com/i2dkYWmb4wHvYPda/arcgis/rest/services/region_censustract_clp/FeatureServer/5/query?outFields=*&where=1%3D1&f=geojson")
```
The data for train stations in the Bay Area was accessed with the following link: https://opendata.mtc.ca.gov/maps/efd75b7bb3c04dbda06c6e7cd73e9336. 
With regards to retrieving demographic data, the American Community Survey 2010-2020 5-year estimates, Tables B01001A (Race) and B19001 (Income) was used in the analysis. The analysis of census data with all 10 years. Each year from 2010-2020 had to individually be analyzed to determine total income and average income for each respective year. The census data provided the number of individuals making the between a range of annual incomes. The clean up and analysis was performed with the code below.

```markdown
census_2011_1 = census_2011.iloc[1:,[0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32]]
census_2011_2 = census_2011_1.astype(int)

#Calculating total income
census_2011_2['B19001_002E'] = census_2011_2['B19001_002E']*5000
census_2011_2['B19001_003E'] = census_2011_2['B19001_003E']*12500
census_2011_2['B19001_004E'] = census_2011_2['B19001_004E']*17500
census_2011_2['B19001_005E'] = census_2011_2['B19001_005E']*22500
census_2011_2['B19001_006E'] = census_2011_2['B19001_006E']*27500
census_2011_2['B19001_007E'] = census_2011_2['B19001_007E']*32500
census_2011_2['B19001_008E'] = census_2011_2['B19001_008E']*37500
census_2011_2['B19001_009E'] = census_2011_2['B19001_009E']*42500
census_2011_2['B19001_010E'] = census_2011_2['B19001_010E']*47500
census_2011_2['B19001_011E'] = census_2011_2['B19001_011E']*55000
census_2011_2['B19001_012E'] = census_2011_2['B19001_012E']*67500
census_2011_2['B19001_013E'] = census_2011_2['B19001_013E']*87500
census_2011_2['B19001_014E'] = census_2011_2['B19001_014E']*112500
census_2011_2['B19001_015E'] = census_2011_2['B19001_015E']*137500
census_2011_2['B19001_016E'] = census_2011_2['B19001_016E']*175000
census_2011_2['B19001_017E'] = census_2011_2['B19001_017E']*250000

#Handling the NaN error that we couldnt solve
census_2011_2['TotalIncome'] = census_2011_2[1:].sum(axis=1)
census_2011_3 = census_2011_2
census_2011_3.iloc[0,-1] = census_2011_2.iloc[0,:].sum()

#Calculating average
census_2011_3['AverageIncome'] = census_2011_3['TotalIncome']/census_2011_3['B19001_001E']
census_2011_4 = census_2011_3

#Identification
census_2011_4['trctid'] = census_2011['GEO_ID'].str[9:] #Adding ID

census_2011_5 = census_2011_4[["trctid","AverageIncome"]] #print
census_2011_5
```

The same process from income data was ultilized when analyzing the racial demographics within the San Francisco Bay Area.

## Results

### Visualizing Income
In terms of output, the graphs below shows the percent change of Income within all Bay Area rail stations that were opened between 2010 and 2020. 

**Income differences within the San Francisco Bay Area:**
![image](https://user-images.githubusercontent.com/98051167/167046628-af4f0cfc-0ed7-4d6c-abd1-04fc59453b30.png)
![image](https://user-images.githubusercontent.com/98051167/167046644-1c3f3693-300f-4758-8c55-d5ed818fa659.png)

In _Figure 2_, we do not see any strong regional trends besides growth across the board between 2010 and 2015. There are a few exceptions but those show only small decreases in income. Between 2015 and 2020, we see more variation in income across the Bay Area as seen in _Figure 3_. Income is increasing along the inner Bay Area near the water, and decreasing as you move further east. All of our new train stations are located near cities, so it appears that there is income growth around the train stations. A particularly interesting case study is the Antioch extension located in the East Bay. This station is immediately surrounded by blue, but as you move further into the periphery from the station, income actually decreases. This shows a contrast from the 2010-2015 period, where these tracts were experiencing growth. Perhaps the station was the treatment that concentrated income growth. 

**Income differences within the SMART corridor:**
![image](https://user-images.githubusercontent.com/98051167/167046681-bfb5a501-05c7-4b07-92ab-025a4779da5f.png)
![image](https://user-images.githubusercontent.com/98051167/167046688-fb81a2d9-9df2-41ab-96c4-75abc4c91302.png)

From 2010-2017 (shown in _Figure 4_), we see income in Marin and Sonoma counties along the SMART train corridor increasing across the board, with no tracts showing a decrease in income. After the station opened, we began to see some decreases in income in the 2017-2020 period. Some of the strongest census tract growth occurs outside of the station census tracts as shown in _Figure 5_. The 3 southernmost stops in San Rafael and Larkspur show the largest variation in income change. San Rafael is has a lower median income than the rest of Marin County, and is more densely populated. Because these tracts are smaller and more populated, we can see the neighborhoods that experienced income decreases. The tracts with decreasing income are closer to Highway 101 and Interstate 580 (east of the stations) and have to cross the freeway to get to the stations, experiencing less of the potential upside of living near the train, perhaps decreasing the station's positive impacts on income. Additionally, users in San Rafael live significantly closer to the train’s end station in Larkspur to access the San Francisco Ferry, so residents may opt for other more convenient modes to get to the ferry.

**Income diffences within the MUNI corridor:**
![image](https://user-images.githubusercontent.com/98051167/167046707-820e5739-1412-4b69-8cce-f36e54fe047e.png)
![image](https://user-images.githubusercontent.com/98051167/167046725-58d446bd-d78c-4325-932d-84896e8f0baa.png)

As shown in _Figure 6_, it can be seen that the tracts that contain newly built train stations in the MUNI corridor were increasing across the board. There appears to be no tracts near the train stations that are expericing a decrease in income. For _Figure 7_, it can be seen that are both decreases and increases in income within certain tracts that contain a newly built train station. The two tracts of interest are the tracts that contain either Fisherman's Wharf or the ferry building. With respective to the tract containing Fisherman's Wharf, it can be seen that there is a decrease in annaul income after the MUNI train stations were implemented. This may be due to more individuals leaving that area as it's becoming more of a tourism spot rather than residental. In terms of the tract containing the ferry building, there appears to be a sustainable increase in income in that area around the ferry building. The incorporation of MUNI rail station leads to improvement of transportation accessiblity in that area. The improved accessibility may have lead to that area become more attractive as a residental option.

### Visualizing Change in Race (White Population)

**Percent White Trends in the Bay Area**
![image](https://user-images.githubusercontent.com/98051167/167046791-25d8a632-b41b-4f68-b8ee-3d0b3be48c19.png)
![image](https://user-images.githubusercontent.com/98051167/167046797-39c31dbc-848e-4169-9d58-d562160c89c8.png)

In _Figure 8_, from the years of 2010 to 2015, there does not appears to be any strong regional trends with percent white population. From years 2015 to 2020 (shown in _Figure 9_), there appears to be a lot of variation in terms of percent white population spanning across the Bay. Within Marin and Sonoma county, there appears to be a subtle increase in the white population. While in other counties, in particular San Francisco county, Santa Clara county, San Mateo county, and Alameda county, continue to experience fluctations with some areas experiencing increase in percent white and decrease in percent white.

**Percent White Trends on SMART Corridor**
![image](https://user-images.githubusercontent.com/98051167/167046965-54cc296b-5195-4999-8768-9f9329c56a64.png)
![image](https://user-images.githubusercontent.com/98051167/167046980-062fad3b-6f73-441c-9316-ae1b1fd02778.png)

Before the SMART train opened (shown in _Figure 10_), the share of white people living in Sonoma and Marin counties was decreasing. After 2017, both counties showed increases in the share of white residents across the board, which is shown in _Figure 11_. This could be because white people are moving into the counties, or non-white people are moving out, or a combination of the two. However, rail users tend to be higher income and white. With income and the share of white people increasing, there is an opportunity to build transit oriented development near these stations to diversify ridership and the cities with stations. 

**Percent White Trends on MUNI Corridor**
![image](https://user-images.githubusercontent.com/98051167/167046925-a147325f-0a6f-456a-a32f-3c1f673e29d6.png)
![image](https://user-images.githubusercontent.com/98051167/167046934-29fb247a-4d24-49d7-88a3-2a85b7ae6a35.png)

With _Figure 12_, it can be seen that the census tracts within the MUNI corridor within 2010 to 2015 are experiencing a subtle decrease in percent white population. However with _Figure 13_, the trend the figure has is quite similiar to that of the income distribution between 2015 and 2020. The tract containing the Fisherman's Wharf experiences a decrease in percent white over those 5 years while the tract containing the area near the ferry building experiences an increase in white population. The fact that two tracts within the same MUNI corridor experiencing opposing behaviors with both income and white populations raises the question about the true impact of MUNI rail stations, even if there is an impact at all.

## Conclusions and Next Steps

This project showed us the difficulty in conducting planning research. Because this is an observational study, we cannot determine whether the demographic changes in the surrounding census tracts of a new train station were created by the train station itself. Since this research is spatial in design, we would need to attempt a suitability analysis to control for other factors to isolate the effects of the train stations. However, the data from this project can show where rail lines are placed and who has access to them, particularly in the short term before people may adjust where they work or live based on a new train station. 

As far as data limitations and next steps, the main issues we ran into was the scale of the census tract being irregular. In addition to not being the same in population or geographic scale, the tracts may change shape over time as the population in a tract grows or shrinks beyond what the Census deems acceptable. Knowing this, if we had more time we would buffer around each station or use equal area bins around each station to try to limit the changes in geographic scale. We would also like to use data beyond our 10 year timeframe, and perhaps look at additional variables such as home value, property tax value assessments, and housing unit density. 

