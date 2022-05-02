## Welcome to CYPLAN255!

# Visualizing Demographic Changes Near Bay Area Rail Stations

### Scope and Motivation

This project is focusing on understanding the impacts of rail stations on key demographic areas. The Bay Area consist of multiple transportation agencies within the nine Bay Area Counties. Within those counties, there are seven rail lines that serves the residents of the Bay Area. The rail lines that will be focused on are BART, MUNI, and SMART. 

Below is the graph of stations that are focused on in the project:

**INSERT GRAPH HERE IDK HOW TO DO IT YET**

The time frame that was focused on is between 2010 and 2020. The census tracts around each respective station of focus was analyed for temporal changes, before and after the station was opened. The demographic area that was the main focus was Household Income. 

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



## Results




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
