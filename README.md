# What and Where are the World's Oldest Businesses

![](400px-Eingang_zum_St._Peter_Stiftskeller.jpg)

Image: St. Peter Stiftskeller, founded 803. Credit: [Pakeha](https://commons.wikimedia.org/wiki/File:Eingang_zum_St._Peter_Stiftskeller.jpg)

## Description

- An important part of business is planning for the future and ensuring that the business survives changing market conditions.
- Some businesses do this remarkably well and last for hundreds of years. BusinessFinancing.co.uk [researched](https://businessfinancing.co.uk/the-oldest-company-in-almost-every-country/) the oldest company that is still in business in (almost) every country and compiled the results into a dataset.
- In this project, we will explore data from BusinessFinancing.co.uk on the world's oldest businesses: when were they founded, and which industries do they belong to?

## Data

### `categories` table

| Column | Definition | Data Type |
|-|-|-|  
|category_code| Code for the category of the business |`varchar`|
|category| Description of the business category |`varchar`|

### `countries` table

| Column | Definition | Data Type |
|-|-|-|
|country_code| ISO 3166-1 3-letter country code |`varchar`|
|country| Name of the country |`varchar`|
|continent| Name of the continent that the country exists in |`varchar`|


### `businesses` table

| Column | Definition | Data Type |
|-|-|-|
|business| Name of the business |`varchar`|  
|year_founded| Year the business was founded |`int`|
|category_code| Code for the category of the business |`varchar`|
|country_code| ISO 3166-1 3-letter country code |`char`|

## Problem statement and Key Findings

**1. The oldest business in the world:**
```sql
SELECT MIN(year_founded) AS min, MAX(year_founded) AS max
FROM businesses
```
- Answer:
  
| min | max |
|-|-|
|578|1999|

- The SQL query uses aggregate functions to find range of the founding years of the business in the `businesses` dataset.
- The oldest business in the world was founded in the year 578. 

**2. How many businesses were founded before 1000?**
```sql
SELECT COUNT(*)
FROM businesses
WHERE year_founded < 1000
```
- Answer:
  
| count | 
|-|
|6|

- The SQL query filters out the number of businesses that were founded before th year 1000.
- Six businesses in the dataset were founded before 1000, with the oldest dating back to 578.

**3. Which businesses were founded before 1000?:**
```sql
SELECT *
FROM businesses
WHERE year_founded < 1000
ORDER BY year_founded ASC
```
- Answer:
  
| business | year_founded | category_code | country_code |
|-|-|-|-|
|Kongō Gumi| 578 | CAT6 | JPN |
|St. Peter Stifts Kulinarium| 803 | CAT4 | AUT |
|Staffelter Hof Winery| 862 | CAT9 | DEU |
|Monnaie de Paris| 864 | CAT12 | FRA |
|The Royal Mint| 886 | CAT12 | GBR |
|Sean's Bar| 900 | CAT4 | IRL |

- The SQL query finds more details about the 6 businesses that were founded before the year 1000.
- The result shows the business name, the year it was founded, the code of the industry it operates in and the country it was founded in.
- The world's oldest business still operating today is in Japan.

**4. Exploring the categories**
```sql
SELECT business, year_founded, country_code, category
FROM businesses AS b
INNER JOIN categories AS c
ON b.category_code = c.category_code
WHERE year_founded < 1000
ORDER BY year_founded
```
- Answer:

| business | year_founded | country_code | category |
|-|-|-|-|
|Kongō Gumi| 578 | JPN | Construction |
|St. Peter Stifts Kulinarium| 803 | Cafés, Restaurants & Bars |
|Staffelter Hof Winery| 862 | DEU | Distillers, Vintners, & Breweries |
|Monnaie de Paris| 864 | FRA | Manufacturing & Production |
|The Royal Mint| 886 | GBR | Manufacturing & Production |
|Sean's Bar| 900 | IRL | Cafés, Restaurants & Bars |
  
- The SQL query joins the two tables in order to get the categories of the business that the above businesses operates in.
- The query's result shows that the world oldest company is a construction company.
- The results also shows the other companies business area, which were founded before 1000. 

**5. Counting the categories**
```sql
SELECT category, COUNT(category) as n
FROM categories as c
JOIN businesses as b
ON c.category_code = b.category_code
GROUP BY category
ORDER BY n DESC
LIMIT 10
```
Answer: 

| category | n |
|-|-|
|Banking & Finance|	37 |
|Distillers, Vintners, & Breweries|	22 |
|Aviation & Transport| 19 |
|Postal Service|	16 |
|Manufacturing & Production| 15 |
|Media|	7 |
|Agriculture|	6 |
|Cafés, Restaurants & Bars|	6 |
|Food & Beverages| 6 |
|Tourism & Hotels| 4 |

- The SQL query retrieves the Top 10 categories of businesses in th dataset.
- "Banking & Finance" is the most common category among the oldest businesses globally.

**6. Oldest Business by Continent:** 
```sql
SELECT MIN(year_founded) as oldest, continent
FROM businesses as b
JOIN countries as c
ON b.country_code = c.country_code
GROUP BY continent
ORDER BY oldest
```

Answer: 

| oldest | continent |
|-|-|
|578|	Asia |
|803|	Europe |
|1534|	North America |
|1565|	South America |
|1772|	Africa |
|1809| Oceania |

- The SQL result shows the oldest business in each continent still operating.

**7. Joining everything for further analysis**
```sql
SELECT b.business, b.year_founded, ca.category, co.country, co.continent
FROM businesses as b
JOIN categories as ca ON b.category_code = ca.category_code
JOIN countries as co ON b.country_code = co.country_code
```

Answer: 

| business |	year_founded	| category	| country |	continent |
|-|-|-|-|-|
|Spinzar Cotton Company| 1930 |	Agriculture	| Afghanistan |	Asia |
|ALBtelecom|	1912	| Telecommunications | Albania |	Europe |
|Andbank|	1930 |	Banking & Finance |	Andorra |	Europe |
|Liwa Chemicals|	1939 |	Manufacturing & Production |	United Arab Emirates | Asia |

- The results are just a preview, you can visit the complete result [here]() in the notebook.
- Tables were joined to create a comprehensive dataset for more in-depth analysis, combining information on the business, founding year, category, country, and continent.

8. **Categories by Continent:**
   - Counts of businesses in each continent and category were examined, shedding light on the distribution of business types across different regions.

9. **Filtered Results:**
   - The analysis was refined to focus on continent/category pairs with a count greater than 5, providing a more manageable view of significant trends.
