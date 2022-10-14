# data-512-homework_2

This activity helps in understanding how bias can come up in different areas of analysis, and may consequently end up misrepresenting the data and lead to incorrect interpretations.

As a part of this analysis, we will be using a dataset of political figures from different countries and the population of those countries. We will be analysing these numbers to see how the number of total articles per capita in a country (or region) vary, and how the number of "highly-rated" articles change across the population in a country or region.

### The input files are - <br>
* politicians_by_country.SEPT.2022.csv - <br>
The Wikipedia [Category:Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries.
 * population_by_country_2022.csv  <BR>
 This dataset is taken from the [world population data sheet](https://www.prb.org/international/indicator/population/table) published by the Population Reference Bureau.

To create the final data for the analysis, we use the following two APIs: <br>

* **API:Info request to get a range of metadata on an article, including the most current revision ID of the article page.**

This API gets the metadata for a specified article. We use this API to get the last revision ID since we will need this to get the quality score of the article.
The documentation is available at - [API:Info Documentation](https://www.mediawiki.org/wiki/API:Info) <br>
The license to use the contents and API is [API Creative Commons License](https://creativecommons.org/licenses/by-sa/3.0/)
  
* **ORES Request to get the quality of the article from its last revision ID**

ORES helps human readers perform "wiki-work" and enhance productivity by automating some tasks.

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
  
Going over the results of the analysis, we see that Europe region in general (Southern, Western, Eastern), and the Caribbean region have the highest number of articles per capita, and also the highest number of high quality articles per capita. The South and East Asia regions on the other hand have the lowest number of articles per capita. <br>
I earlier felt that this bias emerges from the disparity between the first world and third world countries. However, other reasons also seem to play an important role – non-English speaking countries might have articles in their regional languages that are better rated than the English articles from those regions, but the major difference seems to arise from the population differences in the countries in top vs bottom lists. The top 10 countries are mostly small nations with smaller populations, and bottom 10 countries largely seem to be densely populated. This would mean that a lot of the bias in this data exists in the form of *linguistic and population bias.*

**1.	What biases did you expect to find in the data (before you started working with it), and why?**
Before working with the data, I expected that the English speaking regions will have the highest number of articles per capita. 
I also expected to see a lot of first world countries on the list – the richer nations such as United States, United Kingdom, Australia, Canada, New Zealand were expected to top the lists. <br>
Hence, I expected some inherent bias to exist in the ratings of the articles based on the economic and linguistic characteristics of the country. <br>

Sources - <br>
Five of the largest of these are sometimes described as the "core Anglosphere"; they are the United States of America (with at least 231 million native English speakers), the United Kingdom (60 million), Canada (19 million), Australia (at least 17 million), and New Zealand (4.8 million).
Source: [Wikipedia]( https://en.wikipedia.org/wiki/English-speaking_world)
<br>
Examples of first-world countries include the United States, Canada, Australia, New Zealand, and Japan. Several Western European nations qualify as well, especially Great Britain, France, Germany, Switzerland, and the Scandanavian countries.
Source: [Investopedia]( https://www.investopedia.com/terms/f/first-world.asp)

<br>

**2.	What (potential) sources of bias did you discover in the course of your data processing and analysis?** <br>
When I started working with the data, I realised that the countries I expected to see on the list were not there, reason being that the dataset did not have political figures from those countries on the list. The selection bias seems to impact the overall results of the analysis. The present results seem to stem from a combination of population figures and the divide between English and non-English speaking nations. 
For instance, countries with small population (Southern European nations such as Andorra, Monenegro, Croatia) high a higher number of articles per capita as compared to countries with larger population (such as Asian countries of India, China, etc). <br>
Furthermore, even within the Asian and African countries, the list dramatically changes between total articles and high-quality articles, as the poorer nations such as Pakistan, Uganda, Sudan, Nigeria make appearances on the list of countries with lowest number of per capita high-quality articles.

<br>

**3.	What might your results suggest about (English) Wikipedia as a data source?** <br>
Wikipedia is a data-source that is largely driven by English speaking and English reading population. Most of the articles that are housed in this data source are expected to be better rated if they are related to an English speaking nations as compared to a non-English speaking nation. However, since the dataset excludes the top English speaking nations, the final results of top nations are countries with their first language as French, Montenegrin, Catalan, and other local languages. The bottom nations are also non-English speaking nations of Iran, Sudan, Mexico, China, etc. This difference seems to stem from differences in population (by countries as well as by region) than any other factor – the more the population of a country, the lesser the number of per-capita articles from the country. 
Thus, using English Wikipedia (in its entirety) as a datasource would mean agreeing to deal with the linguistic bias that comes with it.





