**Note: I used the existing views I created in the cleaning data file

Question 1: What are the number of unique visitors for each channel group?

SQL Queries:

/* select 
		distinct channel_grouping as channel_group,
		count(full_visitor_id) as num_unique_visitors
	from analytics_view
	group by channel_grouping
	order by count(full_visitor_id) desc
 */


Answer: 

The top three visitor channels were organic search, referral, and direct. Social was the forth most popular. Given the data is only from 2016 and 2017, it is likely that this company is relatively new and has invested in marketing to drive the numbers in the top four.


Question 2: Is there any appearance of a relationship between the number of products sold for a product sku and sentiment score and sentiment magnitude?

SQL Queries:

/* select 
	srv.product_sku,
	srv.product_name,
	product_category,
	units_sold,
	sentiment_score,
	sentiment_magnitude,
	ratio
from all_sessions_view asv
join sr_view srv
on asv.product_sku = srv.product_sku
join analytics_view av
on asv.full_visitor_id = av.full_visitor_id
where units_sold > 0
group by srv.product_sku, srv.product_name, product_category, units_sold, sentiment_score, sentiment_magnitude, ratio
order by units_sold desc

 */





Answer:

I am assuming sentiment_score and sentiment_magnitude are an average of purchaser ratings a short time after checkout; similar to a rating. Given this assumption, there does not appear to be a relation between the number of products sold and the score and magnitude associated with the product. Further statistical analysis is required, but my SQL skills are still being developed. 


Question 3: What was the total revenue by year and location?

SQL Queries:

/* select
	country,
	city,
	extract(year from asv.date) as year,
	sum((units_sold * price)) as revenue
from analytics_view av
join all_sessions_view asv
on av.full_visitor_id = asv.full_visitor_id
where units_sold > 0
group by country, city,  extract(year from asv.date)
order by year, revenue desc
*/


Answer:

In 2016, sales were restricted to the United States and were cumulatively under 10,000. Sales exploded in 2017, both in the United States and the world reaching both Europe and Asia. There seemed to be high growth in this company’s data, especially when combined with the visitor channel data.

Question 4: how many visitors per country per year?

SQL Queries:

/* select 
	country, 
	extract(year from asv.date) as year, 
	count(*) as num_visitors
from analytics_view av
join all_sessions_view asv
on av.full_visitor_id = asv.full_visitor_id
where units_sold > 0
group by country, year
order by year */

Answer:
In 2016, there were only 22 visitors and all were from the United States. Visitors expanded globally in 2017. Canada had second most at six while the United States had 90.


Question 5: 

SQL Queries:

Answer:

