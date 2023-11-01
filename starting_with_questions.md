Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:
SELECT country, city, SUM(CAST(totaltransactionrevenue AS FLOAT)) AS transactionrev
FROM all_sessions 
WHERE totaltransactionrevenue IS NOT NULL AND city <> 'not available in demo dataset'
GROUP BY country, city
ORDER BY transactionrev DESC

Answer: Cleary United States has the highest transaction revenues by far from any other countries, 2.Israel, 3. Australia, 4, Canada, 5. Switzerland. For cities, The top  city is San Francisco .Sunnyvale, 3.Atlanta, 4.Palo Alto, 5.Tel Aviv-Yafo. The final answer uses one query but you could use 2 separate  queries to to give the top 5 for cities and and top 5 for countries.

QA: 1. Converted cloumn totaltransactionrevenue to float in order to use sum and because sales could have decimals hence why better than converting to integer.
	2. Didnt include null in order to get the actual descending order (it puts nulls on top) in the where
	3. There's a city 'not available in demo dataset' which I'll take out so it doesnt mess with the data using the where function


**Question 2: What is the average number of products ordered from visitors in each city and country?**

SQL Queries:
Select s.country, s.city, (SUM(CAST(o.total_ordered AS integer))/(COUNT(s.fullvisitorid))) AS avgpervistor 
from all_sessions s
JOIN orders o
ON s.productsku = o.sku
WHERE city <> 'not available in demo dataset' and city <> '(not set)'
GROUP BY s.country, s.city
order by avgpervistor DESC


Answer:
These are the top 10 when cities and countries are grouped.
"Saudi Arabia"	"Riyadh"	319
"Czechia"	"Brno"	319
"United States"	"Rexburg"	250
"United States"	"Sacramento"	189
"Portugal"	"Lisbon"	189
"United States"	"Kalamazoo"	105
"Russia"	"Saint Petersburg"	101
"United States"	"Avon"	100
"Italy"	"Rome"	97
"Canada"	"Sherbrooke"	97

QA: 1. check to see if every sku is distinct - it it 
	2. To check for any non valid values in cities or countries - I group by country and group by city seperatly to shorten the list and check for any non valid values
	3. Took out 'not available in demo dataset' and  '(not set)' from city using where
    4. converted total_ordered to integer, so I can use aggregate
    5. Rouneded, you cant have half a product 


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:
select country, city, v2productcategory, COUNT(fullvisitorid) AS visitors
from all_sessions 
where country <> '(not set)' AND city <> '(not set)' AND city <> 'not available in demo dataset' 
		AND v2productcategory <> '(not set)' AND v2productcategory <> '${escCatTitle}'
group by  country, city, v2productcategory
order by visitors DESC, country, city, v2productcategory 

Answer:
Visitors in Mountain View, United States are very interested in Men's apparel and United States as a whole, Men's t-shirts were the most popular product category. London (UK), Chennai (India), Bengaluru (India) and Melbourne (Australia) most popular category was YouTube. Across all countries and cities, apparel has highest number of visitors followed by 'shop by brand' with Youtube usually the path that visitors choose. 

Some answers are found through grouping by cities and countries seperatly.

QA:  Took out all invalid entries like '(not set)' from country, 'not available in demo dataset' and '(not set)' from city, '(not set)' and '${escCatTitle}' from v2productcategory using where function





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries: 
SELECT country, city, name, totalsold
FROM	(SELECT s.country, s.city, p.name, 
			 RANK() OVER(PARTITION BY s.city ORDER BY SUM(CAST(p.orderedquantity AS integer)) DESC) AS RANK, SUM(CAST(p.orderedquantity AS integer)) AS totalsold
		FROM all_sessions s
		JOIN products p
			ON s.productsku = p.sku
		where country <> '(not set)' AND city <> '(not set)' AND city <> 'not available in demo dataset'
		GROUP BY country, city, p.name
		ORDER BY country, city, totalsold DESC)
WHERE RANK = 1  
ORDER BY totalsold DESC


Answer: 
"United States"	"Mountain View"	" Kick Ball"	75850
"United States"	"Chicago"	" Kick Ball"	30340
"United States"	"New York"	" Kick Ball"	30340
"United States"	"San Francisco"	" Kick Ball"	30340
"United States"	"Palo Alto"	" Cam Outdoor Security Camera - USA"	19033
"United States"	"Sunnyvale"	" Cam Outdoor Security Camera - USA"	16314
"United States"	"Los Angeles"	" Kick Ball"	15170
"United States"	"San Bruno"	" Kick Ball"	15170
"United States"	"Council Bluffs"	" Kick Ball"	15170
"Japan"	"Minato"	" Kick Ball"	15170

Some patterns that can be seen are that 'Kick Ball' is the top product sold for the top 10 cities in the world. Additionaly, United States cities take up 9 out of the top 10 cities in the world in terms of units sold. After the top 10, the citis and countries become very diverse and it becomes difficult to establish a pattern. 

QA: 1.Converted total_ordered to integer, so I can use aggregate
	2.Took out all invalid entries like '(not set)' from country, 'not available in demo dataset' and '(not set)' from city using where function






**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries: 
CREATE TABLE analyticsnew AS(
SELECT visitnumber, visitid, visitstarttime, date, fullvisitorid, channelgrouping, socialengagementtype, unitssold, pageview, timeonsite, bounces, revenue, CAST(unitprice AS float)/1000000 AS unitprice 
	FROM analytics 
);

SELECT s.country, s.city, SUM(a.unitprice * p.orderedQuantity) AS revenue 
FROM analyticsnew a
JOIN all_sessions s 
	ON CAST(s.visitid AS integer) = a.visitid
JOIN products p
	ON s.productsku = p.sku
where country <> '(not set)' AND city <> '(not set)' AND city <> 'not available in demo dataset'
GROUP BY s.country, s.city
ORDER BY s.country, s.city 



Answer:
QA: 1.IN the analytics table i divided unitprice by 1,000,000 and converted the data type to float by creating a temporary table
-- 	2. Took out all invalid entries like '(not set)' from country, 'not available in demo dataset' and '(not set)' from city using where function

- The revenue appears to be highest in cities located in the United States, after the first top 9 cities the countries become more diverse. Cities from from India, Ireland, Mexico and Australia. The top city Mountain View, United States has revenue of 51,980,215, cities that come after are almost 50% less in revenue. 
- The geographical location of the cities does not have an impact on revenue and neither does that population size of the city. 
- Some countries have consistent demand like Spain and Brazil but many countries have a huge range where there are large gaps between cities' revenues. For example, countries like Germany Berlin and Hamburg have a difference of over 2 million in revenue. 
- The fluxuation between cities/countries could be due to siginificant demand for spcific categories.






