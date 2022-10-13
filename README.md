# data-512-homework_2

This activity helps in understanding how bias can come up in different areas of analysis, and may consequently end up misrepresenting the data and lead to incorrect interpretations.

### The input files are - <br>
* politicians_by_country.SEPT.2022.csv - <br>
The Wikipedia [Category:Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries.
 * population_by_country_2022.csv  <BR>
 This dataset is taken from the [world population data sheet](https://www.prb.org/international/indicator/population/table) published by the Population Reference Bureau.

To create the final data for the analysis, we use the following two APIs: <br>

* **API:Info request to get a range of metadata on an article, including the most current revision ID of the article page.**

This API gets the metadata for a specified article. We use this API to get the last revision ID since we will need this to get the quality score of the article.
The documentation is available at - [API:Info Documentation](https://www.mediawiki.org/wiki/API:Info)
  
* **ORES Request to get the quality of the article from its last revision ID**

From the last revision ID that we receive from the Page Info API, we can get the quality of the article. [ORES](https://www.mediawiki.org/wiki/ORES) is a machine learning tool that can provide estimates of Wikipedia article quality. The article quality estimates are, from best to worst: <br>
FA - Featured article <br>
GA - Good article <br>
B - B-class article <br>
C - C-class article <br>
Start - Start-class article <br>
Stub - Stub-class article  
  
  We generate the dataframe by invoking these APIs and performing merge operations, calculating population at country and region level, and calculating per-capita article count for countries and regions. The code for each of these steps is detailed in the Jupyter Notebook.
  
  Along the way, we generate a few lists, text files, and csv files with the following details - <br>
  
  ### The intermediary lists are - <br>
  
  * unscored_article_list - This is the list of articles for which the score could not be calculated. It may be because the lastrevid field is not available, or the page was taken down since the article_list was created.
  * zero_population - This is the list of countries for which the population is 0. This might cause the article-per-capita to be INF when the field is calculated at a later stage. Such rows then have to be manually removed.
  
 ### The output files that are generated are - <br>
  
  * wp_countries-no_match.txt - This is the list of countries for which either the population dataset does not have an entry for the equivalent Wikipedia country, or vice-versa. 
  * wp_politicians_by_country.csv - We save the final merged dataset with the required columns in a csv file - wp_politicians_by_country.csv
This file contains the list of all articles with their quality, country, population, region and revision ID.
  
  ### Data Considerations - <br>
  We have to deal with data inconsistencies such as redundant articles across countries, or with the exact same information as well. These were removed before the analysis. 
  We also have to keep a note of the countries with 0 population as these will generate articles_per_capita rows with "inf" values, which is incorrect.
  
  ### Analysis Results <br>
  The results from the analysis are as follows - 
  
  * **Top 10 countries by coverage: The 10 countries with the highest total articles per capita** <br>
  
  <img width="575" alt="image" src="https://user-images.githubusercontent.com/40142355/195708349-19d5ff3a-04b3-4232-8240-55b055f33f0e.png">

  
  * **Bottom 10 countries by coverage: The 10 countries with the lowest total articles per capita** <br>
  
  <img width="421" alt="image" src="https://user-images.githubusercontent.com/40142355/195708406-210c4003-2eef-4441-9313-c6f7c964dcc1.png">

  
  * **Top 10 countries by high quality: The 10 countries with the highest high quality articles per capita** <br>

  <img width="493" alt="image" src="https://user-images.githubusercontent.com/40142355/195708827-fa31e971-b7f0-4014-a97d-e08ecbc34fb9.png">

  
  * **Bottom 10 countries by high quality: The 10 countries with the lowest high quality articles per capita** <br>
  
  <img width="389" alt="image" src="https://user-images.githubusercontent.com/40142355/195709119-84d58861-17ca-49f1-9bfd-6cf08db3c6dd.png">

  
  * **Geographic regions by total coverage: A rank ordered list of geographic regions (in descending order) by total articles per capita** <br>
  
  <img width="707" alt="image" src="https://user-images.githubusercontent.com/40142355/195709955-7e60ed84-b669-46c1-934e-3d11abfac1c8.png">

  
  * **Geographic regions by high quality coverage: Rank ordered list of geographic regions (in descending order) by high quality articles per capita** <br>
  
  <img width="701" alt="image" src="https://user-images.githubusercontent.com/40142355/195710220-078f34d7-d9e9-4f14-95c5-1df206ef268b.png">

  
  ### Research Implications
  
