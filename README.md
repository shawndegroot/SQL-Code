# Stratascratch-Code
Solutions to Stratascratch SQL questions.

### Easy
### [1. Most Lucrative Products](https://platform.stratascratch.com/coding/2119-most-lucrative-products?code_type=1)
   
You have been asked to find the 5 most lucrative products in terms of total revenue for the first half of 2022 (from January to June inclusive).

Output their IDs and the total revenue.

Table: online_orders

```
SELECT product_id, sum(cost_in_dollars*units_sold) as revenue
from online_orders t1
where date > '2021-12-31'
and date < '2022-07-01'
group by product_id
order by revenue desc
limit 5
```
### [2. Number of Shipments Per Month](https://platform.stratascratch.com/coding/2056-number-of-shipments-per-month?code_type=1)

Write a query that will calculate the number of shipments per month. The unique key for one shipment is a combination of shipment_id and sub_id. Output the year_month in format YYYY-MM and the number of shipments in that month.

Table: amazon_shipment

```
select to_char(shipment_date, 'YYYY-MM') AS date, count(sub_id) as shipments
from amazon_shipment t1
group by date
order by date
```
#### 3. Find the most profitable company in the financial sector of the entire world along with its continent

https://platform.stratascratch.com/coding/9663-find-the-most-profitable-company-in-the-financial-sector-of-the-entire-world-along-with-its-continent?code_type=1

Find the most profitable company from the financial sector. Output the result along with the continent.

Table: forbes_global_2010_2014

```
select company, continent
from forbes_global_2010_2014 t1
where sector = 'Financials'
order by profits desc
limit 1
```

