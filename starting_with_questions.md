Answer the following questions and provide the SQL queries used to find the answer.

**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

``` select
	country,
	city,
	sum(units_sold * price) as total_revenue
from analytics_view av
join all_sessions_view asv
on av.full_visitor_id = asv.full_visitor_id
group by country, city
order by total_revenue desc
limit 10```

Answer:

Cities, unknown and known, occupied eight of the top 10 spots. The exceptions were Mexico(8th) and the United Kingdom(10th).

 "United States"	"United States"	50028
"United States"	"San Francisco"	1547
"United States"	"Mountain View"	1386
"United States"	"San Bruno"		1245
"United States"	"Sunnyvale"		684
"United States"	"Palo Alto"		497
"United States"	"New York"		491
"Mexico"		"Mexico"		476
"United States"	"Kirkland"		242
"United Kingdom"	"London"		206

**Question 2: What is the average number of products ordered from visitors in each city and country?** 


SQL Queries:

```select
	country,
	city,
	round(avg(units_sold), 0) as avg_products_ordered
from analytics_view av
join all_sessions_view asv
on av.full_visitor_id = asv.full_visitor_id
where units_sold > 0
group by country, city
order by avg(units_sold) desc
limit 10```

Answer:

The United States were popular again with the highest average number of products ordered. The exception were, in 9th and 10th, Toronto and Canada with unknown cities. Interesting to note there was a country/city combination of United States and Paris. Likely an error in my cleaning. 

"United States"	"San Jose"		87
"United States"	"United States"	86
"United States"	"Chicago"		26
"United States"	"Mountain View"	15
"United States"	"New York"		5
"United States"	"Salem"		4
"United States"	"Pittsburgh"	4
"United States"	"Paris"		4
"Canada"		"Toronto"		3
"Canada"		"Canada"		2


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```with product_category_country as (
	select
		country,
		city,
		product_category,
		asv.product_name,
		units_sold,
		dense_rank() over (partition by country, city, 			product_category order by units_sold) as product_rank
	from analytics_view av
	join all_sessions_view asv
	on av.full_visitor_id = asv.full_visitor_id
	join products_view pv
	on asv.product_sku = pv.product_sku
	where units_sold > 0
	group by country, city, product_category, asv.product_name, units_sold
	order by country desc
)
select country, city, product_category, product_name, units_sold, product_rank
from product_category_country
where product_rank = 1
group by country, city, product_category, product_name, units_sold, product_rank
order by units_sold desc```



Answer:
I did not notice a particular pattern within country and city perspective. There seemed to be a variety of product categories amongst the countries and cities. The presence of unknown cities also impacted analysis in addition to my inability to properly clean product categories. Output not shown as I don’t think it adds value to my answer.


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?** 


SQL Queries:

 
```with product_category_country as (
	select
		country,
		city,
		product_category,
		asv.product_name,
		units_sold,
		dense_rank() over (partition by country, city order by units_sold desc) as product_rank
	from analytics_view av
	join all_sessions_view asv
	on av.full_visitor_id = asv.full_visitor_id
	join products_view pv
	on asv.product_sku = pv.product_sku
	where units_sold > 0
	group by country, city, product_category, asv.product_name, units_sold
	order by units_sold desc
)
select country, city, product_category, product_name, units_sold, product_rank
from product_category_country
where product_rank = 1
group by country, city, product_category, product_name, units_sold, product_rank
order by country desc
limit 10```


Answer:

Most countries/cities had an item from the apparel category as their top order item. Electronics and Nest were the second most popular. 





**Question 5: Can we summarize the impact of revenue generated from each city/country?** 

SQL Queries:

```with revenue_summary as (
	select
		country,
		city,
		sum(units_sold * price) as total_revenue,
		(select sum(units_sold * price)
			from analytics_view
			where units_sold > 0) as site_revenue
	from analytics_view av
	join all_sessions_view asv
	on av.full_visitor_id = asv.full_visitor_id
	where units_sold > 0
	group by country, city
	order by total_revenue desc
)
select country, city, round((total_revenue / site_revenue), 5) * 100 as percent_of_total_revenue
from revenue_summary
group by country, city, (total_revenue / site_revenue)
order by (total_revenue / site_revenue) desc```


Answer:

10 percent of total site revenue was derived from unknown cities in the United States, while every other geographic region was less than or equal to 0.31%. 
 
"United States"	"United States"	10.10200
"United States"	"San Francisco"	0.31200
"United States"	"Mountain View"	0.28000
"United States"	"San Bruno"		0.25100
"United States"	"Sunnyvale"		0.13800
"United States"	"Palo Alto"		0.10000
"United States"	"New York"		0.09900
"Mexico"		"Mexico"		0.09600
"United States"	"Kirkland"		0.04900
"United Kingdom"	"London"		0.04200
