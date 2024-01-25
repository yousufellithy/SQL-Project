# Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The objective of this project is to review a set of files for analysis and interpretation. The aim is to extract valuable insights from the data. The project goals include enhancing proficiency in data reading, as well as developing the capability to identify potential flaws and issues within the data even before the cleaning or scraping process begins. Most importantly to find the answers to the objects/questions given. 

## Process
STEP 1 - Import the data: Begin by importing the CSV files onto the SQL analysis tool and ensuring all columns were transferred

STEP 2 - Data exploration: Get a first glance at all the tables and going through them to see what they could potentially represent and how they all connect. This is the stage to set primary keys and foreign keys. 

STEP 3 - Quality assessment: At this stage, it's important to see the quality of the data that's been imported, check the data types and see if they're correct. This step was merged with the second step as it involved data cleaning, which was done on a need-to-use basis. After the question/objective is given, pick the columns that need to be used and then do the data cleaning for the columns but the cleaning was shown in the output and not editing the actual data set. This was done because it becomes very difficult to reverse the editing of the dataset (in case it was done incorrectly).

STEP 4 - Solve questions: Look at the objective and figure out a query to get the solutions while maintaining the integrity of the data, additionally create new questions to be answered to give a wider understanding of the data and how it could be used.

STEP 5 - Quality assessment: Going over the outputs and double-checking if they make sense in terms of context and if all the data types are correct. This is done through where function, etc. 


## Results
Managed to find the solutions to all the objectives given but difficult to ensure the degree of accuracy of the output due to not being 100% sure of what each column represented. Other than that patterns could be recognized about products, categories and cities/countries. These recognized patterns could be used for business decision-making. For the majority of the outputs, the top results contained the United States and its cities, especially Mountain View. These cities have high website site traffic and revenue generation.

## Challenges 
 - The biggest challenge I faced was to understand what each column represented. Many of the columns' names were similar and gave the impression that they contained the same values, but upon reviewing the values I discovered the columns were not related to each other. This made it difficult to choose which column to use to find the solutions I needed.
 - The cleaning of the data, didn't want to change the values too much so I don't skew the results.
 - Checking non-valid values in columns like city and country.
 - The unique identifier not being unique, ie. fullvisitorid I needed this column to be unique in order to use it but it wasn't.

## Future Goals
- Clean all the data first (normalizing all columns and ensuring no inconsistencies)
- Create a better PK and FK
- Add a footnote for each column to describe what the column’s values represent
