What are your risk areas? Identify and describe them.

Firstly, The limitations imposed by time and expertise can exert a notable impact on the depth and quality of the analysis, potentially affecting the overall usefulness of our findings. Secondly, inadequate communication of findings or misinterpretation of results is a potential pitfall. Misinterpreting consumer preferences in the data may lead to a misalignment of inventory. If certain product categories are not clearly communicated or if they are not properly stored (like spelling and spacing), it could result in stocking the wrong products, potentially leading to excess inventory or stockouts. Thirdly, the presence of inaccurate, incomplete, or inconsistent data poses a significant risk, potentially leading to misguided conclusions. Some columns are either entirely empty or lack sufficient information, making it challenging to decipher the true context of the presented information. Lastly, the fullvisistid column needed to be used a lot as a unique value to count it, but the column did not contain unique values, so I used the distinct function when counting the column values. 



QA Process:
Describe your QA process and include the SQL queries used to execute it.

To go back and check over my queries and see if the table that is outputted makes sense in terms of what is being asked and the context of how the output should look.

SELECT city FROM all_sessions AND SELECT country FROM all_sessions and go through them to check they're all valid values 

SELECT * FROM all_sessions WHERE country = 'USA' and Select * from all_sessions where country = 'United States' --- to check if there aren't multiple versions of the same country, did the same with the UK

(DELETE FROM all_sessions WHERE city = 'not available in demo dataset' and city = 'not set' -- did that just to double check all entries are valid, did for country and for v2categoryname

SELECT * FROM all_sessions WHERE totaltransactionrevenue IS NULL -- To check how if the data I took totaltransactionrevenue was not NULL (this is added to a query that already gave the output I needed, and is to double check)

DISTINCT(fullvisitid)

