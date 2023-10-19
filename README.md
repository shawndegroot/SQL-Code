# Stratascratch-Code
Solutions to Stratascratch SQL questions.

## Easy
### [1. Most Lucrative Products](https://platform.stratascratch.com/coding/2119-most-lucrative-products?code_type=1)
   
You have been asked to find the 5 most lucrative products in terms of total revenue for the first half of 2022 (from January to June inclusive).

Output their IDs and the total revenue.

Table: online_orders

```
SELECT PRODUCT_ID, SUM(cost_in_dollars*units_sold) AS revenue
FROM online_orders t1
WHERE date > '2021-12-31'
AND date < '2022-07-01'
GROUP BY product_id
ORDER BY revenue desc
LIMIT 5
```
### [2. Number of Shipments Per Month](https://platform.stratascratch.com/coding/2056-number-of-shipments-per-month?code_type=1)

Write a query that will calculate the number of shipments per month. The unique key for one shipment is a combination of shipment_id and sub_id. Output the year_month in format YYYY-MM and the number of shipments in that month.

Table: amazon_shipment

```
SELECT TO_CHAR(shipment_date, 'YYYY-MM') AS date, COUNT(sub_id) AS shipments
FROM amazon_shipment t1
GROUP BY date
ORDER BY date
```
### [3. Find the most profitable company in the financial sector of the entire world along with its continent](https://platform.stratascratch.com/coding/9663-find-the-most-profitable-company-in-the-financial-sector-of-the-entire-world-along-with-its-continent?code_type=1)

Find the most profitable company from the financial sector. Output the result along with the continent.

Table: forbes_global_2010_2014

```
SELECT company, continent
FROM forbes_global_2010_2014 t1
WHERE sector = 'Financials'
ORDER BY profits DESC
LIMIT 1
```

## Medium

### [4. Ranking Most Active Guests](https://platform.stratascratch.com/coding/10159-ranking-most-active-guests?code_type=1)

Rank guests based on the number of messages they've exchanged with the hosts. Guests with the same number of messages as other guests should have the same rank. Do not skip rankings if the preceding rankings are identical.
Output the rank, guest id, and number of total messages they've sent. Order by the highest number of total messages first.

Table: airbnb_contacts

```
SELECT id_guest, DENSE_RANK() OVER (ORDER BY SUM(n_messages) DESC) AS ranks, SUM(n_messages)
FROM airbnb_contacts t1
GROUP BY id_guest
ORDER BY ranks
```

### [5. Highest Salary In Department](https://platform.stratascratch.com/coding/9897-highest-salary-in-department?code_type=1)

Find the employee with the highest salary per department.
Output the department name, employee's first name along with the corresponding salary.

Table: employee

```
SELECT department, first_name, salary
FROM employee t1
WHERE (department, salary) IN 
    (SELECT department, MAX(salary)
    FROM employee t1
    GROUP BY department)
```

### [6. Make a report showing the number of survivors and non-survivors by passenger class](https://platform.stratascratch.com/coding/9881-make-a-report-showing-the-number-of-survivors-and-non-survivors-by-passenger-class?code_type=1)

Make a report showing the number of survivors and non-survivors by passenger class.
Classes are categorized based on the pclass value as:
pclass = 1: first_class
pclass = 2: second_classs
pclass = 3: third_class
Output the number of survivors and non-survivors by each class.

Table: titanic

```
SELECT survived, SUM(CASE WHEN pclass = 1 THEN 1 ELSE 0 END) AS first_class, 
                SUM(CASE WHEN pclass = 2 THEN 1 ELSE 0 END) AS second_class, 
                SUM(CASE WHEN pclass = 3 THEN 1 ELSE 0 END) AS third_class
FROM titanic t1
GROUP BY survived
```

## Hard

### [7. Top 5 States With 5 Star Businesses](https://platform.stratascratch.com/coding/10046-top-5-states-with-5-star-businesses?code_type=1)

Find the top 5 states with the most 5 star businesses. Output the state name along with the number of 5-star businesses and order records by the number of 5-star businesses in descending order. In case there are ties in the number of businesses, return all the unique states. If two states have the same result, sort them in alphabetical order.

Table: yelp_business

```
SELECT state, COUNT(business_id) AS counts
FROM yelp_business t1
WHERE stars = 5
GROUP BY state
ORDER BY counts DESC, state
LIMIT 6
```

### [8. Counting Instances in Text](https://platform.stratascratch.com/coding/9814-counting-instances-in-text?code_type=1)

Find the number of times the words 'bull' and 'bear' occur in the contents. We're counting the number of times the words occur so words like 'bullish' should not be included in our count.
Output the word 'bull' and 'bear' along with the corresponding number of occurrences.

Table: google_file_store

```
SELECT 'bull' AS words, COUNT(*)
FROM google_file_store t1
WHERE contents ILIKE '%bull%'
UNION
SELECT 'bear' AS words, COUNT(*)
FROM google_file_store t1
WHERE contents ILIKE '%bear%'
```


### [9. Host Popularity Rental Prices](https://platform.stratascratch.com/coding/9632-host-popularity-rental-prices?code_type=1)

You’re given a table of rental property searches by users. The table consists of search results and outputs host information for searchers. Find the minimum, average, maximum rental prices for each host’s popularity rating. The host’s popularity rating is defined as below:
0 reviews: New
1 to 5 reviews: Rising
6 to 15 reviews: Trending Up
16 to 40 reviews: Popular
more than 40 reviews: Hot

Tip: The id column in the table refers to the search ID. You'll need to create your own host_id by concating price, room_type, host_since, zipcode, and number_of_reviews.

Output host popularity rating and their minimum, average and maximum rental prices.

Table: airbnb_host_searches

```
WITH cte AS
    (SELECT DISTINCT CASE 
        WHEN number_of_reviews = 0 THEN 'New'
        WHEN 1<=number_of_reviews AND number_of_reviews<=5 THEN 'Rising' 
        WHEN 6<=number_of_reviews AND number_of_reviews<=15 THEN 'Trending Up'
        WHEN 16<=number_of_reviews AND number_of_reviews<=40 THEN 'Popular'
        WHEN number_of_reviews > 40 THEN 'Hot'
        END AS popularity, 
        price, room_type, host_since, zipcode, number_of_reviews
    FROM airbnb_host_searches t1)
    
SELECT popularity, MIN(price), AVG(price), MAX(price)
FROM cte
GROUP BY popularity
```

