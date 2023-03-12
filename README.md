# STEM-Salaries-Prediction

As STEM students, we are constantly monitoring the job market. The goal of this project is to understand how different metrics and demographic variables affect STEM employees' salaries. By utilizing random forest, GBT, linear, and logistic regression models, we examine the importance of multiple characteristics in determining an employee’s base salary, such as company, location, years of work experience, and job title. 
The data used comes from Levels.fyi, a website where employees can post information about their jobs and themselves. The end goal was to find the best model at accurately predicting base salaries. Additional information was gathered, providing insight into what features each model found to be most important. The predictions and inferences that were made on this data provide a wealth of information to current/future STEM employees.  

## Data Collection & Cleaning 
Started in 2017, Levels.fyi is a website that promotes pay transparency in the tech industry by providing employees a platform to anonymously post information about their base salary, job title, years of work experience, and more. This project uses a dataset hosted on Kaggle.com of posts web scraped from Levels.fyi. The original shape of this dataset was 62,646 rows by 29 columns. There were 15 different job titles represented in this dataset. Some of the key columns used in this project are listed below in Table 1, and a data dictionary describing all of the original columns is provided in the appendix.

The initial cleaning process involved correcting the dataset’s schema as well as removing incorrect and/or unneeded values across many rows and columns. For example, the “basesalary”, “yearsofexperience”, and “yearsatcompany” columns were set to integer-type. Entries like “99”, “SE1”, and null values were removed from the “Title” column. Hardcoded “NA” and “null” values were removed from the “Education”, “Race”, and “Gender” columns, and values such as “0” and “1” were removed from the “Education” and “Gender” columns. 

One major issue with this dataset was the inconsistency of company names across rows. Companies like JP Morgan & Chase, for example, were originally listed as “JP Morgan”, “JPMorgan Chase”, “JPMORGAN”, and more. With 1,636 unique values originally in the “Company” column, it was important to find a way to programmatically standardize company names. Our solution to this problem was a multi-step function that relied on Python dictionaries to reduce processing time. First, all values in the “Company” column were converted to be fully lowercase.  Then, regular expressions were used to find “sub-patterns” across rows. If the string “JP'' was found in the strings “JP Morgan”, “JPMorgan Chase”, or “JPMORGAN”, then our function would either make or update the corresponding key-value pair. After processing the entire dataset, the key-value pairs were sorted alphabetically, and we manually went through each pair to find any company names that needed rectification. Once an incorrect name was found, we used the regexp_replace pyspark function to correct the name. 61 companies had their names standardized, and ~1800 values were updated. After cleaning, the shape of the dataset was 21,581 rows by 19 columns.  

## Methology
The eight features used in our machine learning models are listed in the table below. Six of these features were string-type data and needed to be indexed for use in random forest (RF), gradient boosting trees (GBT), linear regression, and logistic regression models. In addition to indexing these columns, we decided to subset the cleaned dataset by excluding companies that did not have at least 20 rows worth of data. After building an initial set of models, it was clear that the “Company” column was an important feature. We did not want our models to be influenced by high or low salaries from a company that appeared in the cleaned dataset only a handful of times. This threshold of at least 20 observations per company allowed us to keep ~16,833 rows of data and examine 145 unique companies in our models. 
Each of the following subsections details the specific steps for each model as well as the scoring methodology used to evaluate them.

<b><p align="center"> Table - Descriptions of Features Used in Models </p></b>
![image](https://user-images.githubusercontent.com/30213777/224519730-42467d2a-7db1-4a01-b8aa-d90b670ab94d.png)


## Conclusion 
The goal of this project was to understand the factors that determine salary for STEM employees. Below are our final observations:
1)	Software engineering manager is the best job in terms of average base salary
2)	A data scientist needs ~5 years of work experience on average for an average base salary of $150,000 
3)	Working in developed countries with very large tech industries such as the U.S., Denmark, and Switzerland can contribute to a higher base salary
4)	Company, country, and total number of years of work experience have the most significant impact on determining an employee’s base salary
5)	Switching companies after gaining some experience is a better way for employees to improve their salary than staying with the same company


## References
1)  https://stackoverflow.com/questions/51063624/whats-the-equivalent-of-pandas-value-counts-in-pyspark 
2) https://sparkbyexamples.com/pyspark/pyspark-retrieve-top-n-from-each-group-of-dataframe/ 
3) https://www.learnpythonwithrune.org/plot-world-data-to-map-using-python-in-3-easy-steps/ 
4) https://towardsdatascience.com/simplest-way-of-creating-a-choropleth-map-by-u-s-states-in-python-f359ada7735e
