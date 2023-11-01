Question 1:  What are the top 10 cities with the most visitors?


SQL Queries: 
SELECT city, COUNT(DISTINCT(fullvisitorid)) AS numofvisitors
FROM all_sessions
WHERE city <> 'not available in demo dataset' and city <> '(not set)'
GROUP BY city
ORDER BY numofvisitors DESC
LIMIT 10 

Answer: 
"Mountain View"	1177
"New York"	647
"San Francisco"	464
"Sunnyvale"	366
"San Jose"	247
"Los Angeles"	222
"London"	201
"Chicago"	199
"Toronto"	134
"Seattle"	120


Question 2: TOP 5 categories with the highest ordered quantity?

SQL Queries:
SELECT s.v2productcategory AS category, SUM(p.orderedquantity) AS totalordered
FROM all_sessions s
JOIN products p
	ON s.productsku = p.sku
GROUP by category
ORDER BY totalordered DESC

Answer:
"Home/Shop by Brand/YouTube/"	1687502
"Home/Nest/Nest-USA/"	596438
"Home/Office/"	577977
"Home/Accessories/Fun/"	488378
"Home/Drinkware/"	350592


Question 3:  Order the channels according to their revenue generation from highest to lowest

SQL Queries:
CREATE TABLE analyticsnew AS( SELECT visitnumber, visitid, visitstarttime, date, fullvisitorid, channelgrouping, socialengagementtype, unitssold, pageview, timeonsite, bounces, revenue, CAST(unitprice AS float)/1000000 AS unitprice FROM analytics );

SELECT a.channelgrouping, SUM(a.unitprice * p.orderedQuantity) AS revenue 
FROM analyticsnew a 
JOIN all_sessions s 
	ON CAST(s.visitid AS integer) = a.visitid 
JOIN products p 
	ON s.productsku = p.sku
GROUP BY a.channelgrouping
ORDER BY revenue DESC

Answer:
"Organic Search"	267982450.69999993
"Referral"	198788085.47000182
"Direct"	108165725.74000065
"Paid Search"	21084626.07000018
"Affiliates"	4192484.6699999953
"Display"	3494603.149999996

