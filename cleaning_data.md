What issues will you address by cleaning the data?

I only clean the data that needs to be used. These are the issues I addressed, I provided what I did and then explained why I did it. I put the Queries that I used, both lists to match each other. 

1. Converting columns from character varying to float/integer in order to use it for aggregation like SUM, AVG, etc. Used float because revenue and sales could have decimals 
2. Excluded NULLS to not interfere with data tables, to get the actual descending order (it puts nulls on top)
3. Removing non-valid entries in the city, country, v2productcategory columns such as '(not set)', 'not available in demo dataset' and '${escCatTitle}'
4. Checked to see if every SKU is distinct, did that by performing a query with distinct and query without to see if the number of rows returned was the same. (It wasn't - meaning SKU didn't contain unique values in the all_sessions table)
5. Checked for any invalid entries in the country and city columns by grouping each separately and checking manually. Grouping is done separately to shorten the output
6. Converted total_ordered to integer so that I can use it for aggregation
7. Converted visitid to an integer to be able to use in a join statement 
8. In the analytics table I divided unitprice by 1,000,000 and converted the data type to float because it has decimals, I did that in a temp table and used that table in my query



Queries:
Below, provide the SQL queries you used to clean your data.
1. CAST(totaltransactionrevenue AS FLOAT)
2. WHERE totaltransactionrevenue IS NOT NULL
3. where country <> '(not set)' AND city <> '(not set)' AND city <> 'not available in demo dataset' AND v2productcategory <> '(not set)' AND v2productcategory <> '${escCatTitle}'
4. SELECT DISTINCT SKU FROM all_sessions AND SELECT SKU FROM all_sessions 
5. GROUP BY country AND GROUP BY city (this is done with the rest of the query from question #2 that contains aggregation)
6. CAST(o.total_ordered AS integer)
7. FROM analyticsnew a JOIN all_sessions s ON CAST(s.visitid AS integer) = a.visitid
8. CREATE TABLE analyticsnew AS (SELECT (all the columns in the analytics table)..., CAST(unitprice AS float)/1000000 AS unitprice FROM analytics)




