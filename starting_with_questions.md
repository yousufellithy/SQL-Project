Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:
SELECT country, city, SUM(CAST(totaltransactionrevenue AS FLOAT)) AS transactionrev
FROM all_sessions 
WHERE totaltransactionrevenue IS NOT NULL AND city <> 'not available in demo dataset'
GROUP BY country, city
ORDER BY transactionrev DESC

Answer: Cleary United States has the highest transaction revenues by far from any other country, 2.Israel, 3. Australia, 4, Canada, 5. Switzerland. For cities, The top  city is San Francisco. Sunnyvale, 3.Atlanta, 4.Palo Alto, 5.Tel Aviv-Yafo. The final answer uses one query but I used 2 separate  queries to to get the top 5 for cities and the top 5 for countries separately.


**Question 2: What is the average number of products ordered from visitors in each city and country?**

SQL Queries:
Select s.country, s.city, (SUM(CAST(o.total_ordered AS integer))/(COUNT(DISTINCT(s.fullvisitorid)))) AS avgpervistor 
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


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:
select country, city, v2productcategory, COUNT(DISTINCT(fullvisitorid)) AS visitors
from all_sessions 
where country <> '(not set)' AND city <> '(not set)' AND city <> 'not available in demo dataset' 
		AND v2productcategory <> '(not set)' AND v2productcategory <> '${escCatTitle}'
group by  country, city, v2productcategory
order by visitors DESC, country, city, v2productcategory 

Answer:
Visitors in Mountain View, United States are very interested in Men's apparel and the United States as a whole, Men's t-shirts were the most popular product category. London (UK), Chennai (India), Bengaluru (India) and Melbourne (Australia) most popular category was YouTube. Across all countries and cities, apparel has the highest number of visitors followed by 'shop by brand' with Youtube usually the path that visitors choose. 

Some answers are found through grouping by cities and countries separately.

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

Some patterns that can be seen are that 'Kick Ball' is the top product sold for the top 10 cities in the world. Additionally, United States cities take up 9 out of the top 10 cities in the world in terms of units sold. After the top 10, the cities and countries become very diverse and it becomes difficult to establish a pattern. 


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

- The revenue appears to be highest in cities located in the United States, after the first top 9 cities the countries become more diverse. Cities from India, Ireland, Mexico and Australia. The top city Mountain View, United States has a revenue of 51,980,215, cities that come after are almost 50% less in revenue. 
- The geographical location of the cities does not have an impact on revenue and neither does the population size of the city. 
- Some countries have consistent demand like Spain and Brazil but many countries have a huge range where there are large gaps between cities' revenues. For example, countries like Germany Berlin and Hamburg have a difference of over 2 million in revenue. 
- The fluctuation between cities/countries could be due to significant demand for specific categories.






