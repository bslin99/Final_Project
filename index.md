## Welcome to CYPLAN255!

# Visualizing Demographic Changes Near Bay Area Rail Stations

### Scope and Motivation

This project is focusing on understanding the impacts of rail stations on key demographic areas. The Bay Area consist of multiple transportation agencies within the nine Bay Area Counties. Within those counties, there are seven rail lines that serves the residents of the Bay Area. The intended investigatation is whether rail transit stops are in denser areas, higher income areas, and/or areas of high white population concentration. The insipiration to answer this because of its equity applications, as rail is faster, reliable, and heavily subsidized compared to the bus.

The rail lines that will be focused on are BART (Bay Area Rapid Transit), SFMTA MUNI (San Francisco Municipal Railway), and SMART (Sonoma-Marin Area Rail Transit). 

Below is the graph of stations that are focused on in the project:

![image](https://user-images.githubusercontent.com/98051167/166744482-615261bb-0d81-4fbe-994f-de15cfdda468.png)
_Figure 1: Train Stations in the Bay Area_

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

The analysis of census data with all 10 years. Each year from 2010-2020 had to individually be analyzed to determine total income and average income for each respective year. The census data provided the number of individuals making the between a range of annual incomes.

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

The same process is ultilized when analyzing the racial demographics from the census data was

## Results

### Visualizing Income
In terms of output, the graphs below shows the percent change of Income within all Bay Area rail stations that were opened between 2010 and 2020. 

Income differences within the San Francisco Bay Area:
![image](https://user-images.githubusercontent.com/98051167/166804617-3d03feaf-86db-4790-bdef-84b0db5b6980.png)
_Figure 2: Income distribution before 2015 in the Bay Area_
![image](https://user-images.githubusercontent.com/98051167/166804714-3225a4d1-950c-4d60-ac8e-2577b4772ce2.png)
_Figure 3: Income distribution after 2015 in the Bay Area_

Income differences within the SMART corridor:
![image](https://user-images.githubusercontent.com/98051167/166805672-d1667b6f-3269-45f9-95f2-5c9a1750b05a.png)
_Figure 4: Income distribution before 2015 in the SMART Corridor_
![image](https://user-images.githubusercontent.com/98051167/166805014-3fd84542-644c-4134-b745-28dbd3d6d81f.png)
_Figure 5: Income distribution after 2015 in the SMART Corridor_

Income diffences within the MUNI corridor:
![image](https://user-images.githubusercontent.com/98051167/166805102-f7e122f5-cb07-4ae4-955f-178372f76e70.png)
_Figure 6: Income distribution before 2015 in the MUNI Corridor_
![image](https://user-images.githubusercontent.com/98051167/166805130-9245105d-c334-4fa1-9fe2-5b38ebfdb29e.png)
_Figure 7: Income distribution after 2015 in the MUNI Corridor_

As shown in _Figure 6_, it can be seen that the tracts that contain newly built train stations in the MUNI corridor were increasing across the board. There appears to be no tracts near the train stations that are expericing a decrease in income. For _Figure 7_, it can be seen that are both decreases and increases in income within certain tracts that contain a newly built train station. The two tracts of interest are the tracts that contain either Fisherman's Wharf or the ferry building. With respective to the tract containing Fisherman's Wharf, it can be seen that there is a decrease in annaul income after the MUNI train stations were implemented. This may be due to more individuals leaving that area as it's becoming more of a tourism spot rather than residental. In terms of the tract containing the ferry building, there appears to be a sustainable increase in income in that area around the ferry building. The incorporation of MUNI rail station leads to improvement of transportation accessiblity in that area. The improved accessibility may have lead to that area become more attractive as a residental option.

### Visualizing Change in Race (White Population)

General Bay Area
![image](https://user-images.githubusercontent.com/98051167/166809307-01e7a305-330c-49de-832f-5fd65dbb44bc.png)
_Figure 8: White population before 2015 in the Bay Area_
![image](https://user-images.githubusercontent.com/98051167/166809352-e216510c-8fd5-40fb-a258-732d1ec2e6fc.png)
_Figure 9: White population after 2015 in the Bay Area_

In _Figure 8_, from the years of 2010 to 2015, there does not appears to be any strong regional trends with percent white population. From years 2015 to 2020 (shown in _Figure 9_, there appears to be a lot of variation in terms of percent white population spanning across the Bay. Within Marin and Sonoma county, there appears to be a subtle increase in the white population. While in other counties, in particular San Francisco county, Santa Clara county, San Mateo county, and Alameda county, continue to experience fluctations with some areas experiencing increase in percent white and decrease in percent white.

Focusing on SMART
![image](https://user-images.githubusercontent.com/98051167/166809493-20c2f802-8734-4690-8d4a-5b6d50c3ffa5.png)
_Figure 10: White population before 2015 in the SMART Corridor_
![image](https://user-images.githubusercontent.com/98051167/166809531-fe86e2d3-ab81-471c-aa10-2ad2b5c79974.png)
_Figure 11: White population before 2015 in the SMART Corridor_

Focusing on MUNI
![image](https://user-images.githubusercontent.com/98051167/166809392-c7571d11-3c3d-4e93-a582-6afd3e4ffaef.png)
_Figure 12: White population before 2015 in the MUNI Corridor_
![image](https://user-images.githubusercontent.com/98051167/166809446-92c96275-c741-4277-8ac6-b452085a5107.png)
_Figure 13: White population before 2015 in the MUNI Corridor_

With _Figure 12_, it can be seen that the census tracts within the MUNI corridor within 2010 to 2015 are experiencing a subtle decrease in percent white population. There are few tracts that are experiencing a slight increase in

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/bslin99/Final_Project/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
