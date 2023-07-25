What issues will you address by cleaning the data?

I looked for the following errors that could impact data integrity:

- Duplicates and nulls,
- Proper data types where appropriate,
- White space,
- Any additional formatting like capitalization or condensing of strings,
- Do values and data types make sense (e.g., unit prices were too high)

I looked at the All Sessions and Analytics tables and noticed there were too many columns and that not all were relevant to the assigned questions and my own. I reduced All Sessions and Analytics to 9 columns each prior to cleaning. So, I created views and used CTE’s to clean the relevant data for this assignment. The queries below are those I used to create the views while simultaneously cleaning the data. I will pull the data cleaning parts of the code and post them below that of the view.

Below is a summary of what I did to the data:

- I used CASE statements to fix any cities that were not cities.
- I used trim to trim the white space in product names.
- I converted dates to date data types with CAST (::).
- I attempted to condense the product categories, but only managed to 	remove a portion, thus leaving the product category improperly 	cleaned. Also, I struggled with trying to map the incorrect values 	to the correct one, so I just made a case statement to make them 	‘Unknown’.
- I decreased the unit price column in analytics by a factor of 	1,000,000.
- Any nulls in units sold were converted to zero using COALESCE.
- Any average values calculated were CAST to NUMERIC and rounded to zero 	decimal places.
- The ratio column was determined to be (total ordered / stock level); 	however, some had 0 for stock level which would result in a 	mathematical error. Thus, I used CASE to check if the rounded value 	was null or a double precision and converted them to varchar.

Areas where I could have better cleaned the code:

- Product categories could have been done better. I only managed to 	remove ‘Home/’.
- There were some inaccurate country/city pairings. I assumed they would be correct as most were. Some of my outputs had “United States, Paris” and “Canada, New York”. 

Queries:
Below, provide the SQL queries you used to clean your data.

All Sessions:

```create or replace view all_sessions_view as (
	with cte_alv as (
		select 
			full_visitor_id,
			visit_id,
			country,
			case
				when city = '(not set)' then country
				when city = 'not available in demo dataset' then 				country
				else city
			end as city,
			date::date as date,
			product_sku,
			trim(v2_product_name) as product_name,
			case
				when v2_product_category = '(not set)' then 					'Unknown'
				when v2_product_category = '${escCatTitle}' then 				'Unknown'
				else v2_product_category
			end as product_category,
			row_number() over (partition by full_visitor_id, 				visit_id) as row_cte_alv
		from all_sessions
		where country != '(not set)'
	)
	select *
	from cte_alv
	where row_cte_alv = 1
)  ```

Analytics:

``` create or replace view analytics_view as (
	with cte_analytics as (
		select
			full_visitor_id,
			visit_id,
			date::varchar::date as date,
			visit_number,
			channel_grouping,
			"social_engagement_Type",
			coalesce(units_sold, 0) as units_sold,
			unit_price / 1000000 as price,
		row_number() over (partition by full_visitor_id, visit_id, 		visit_number) as row_cte_analytics
		from analytics
	)
	select *
	from cte_analytics
	where row_cte_analytics = 1
) ```





Products:

``` create or replace view products_view as (
	with cte_products as(
		select
			product_sku,
			trim(name) as product_name,
			ordered_quantity,
			stock_level,
			restocking_lead_time,
			sentiment_score,
			sentiment_magnitude,
			row_number() over (partition by product_sku, name) as 			row_products_view
		from products
	)
	select *
	from cte_products
	where row_products_view = 1
) ```


Sales by Sku (sbs):

``` create or replace view sbs_view as (
	with cte_sbs as (
		select 
			product_sku,
			total_ordered,
			row_number() over (partition by product_sku, 					total_ordered) as row_sbs_view
		from sales_by_sku
	)
	select *
	from cte_sbs
	where row_sbs_view = 1
)```

Sales Report (sr):

``` create or replace view sr_view as (
	with cte_sr as (
		select
			product_sku,
			total_ordered,
			trim(name) as product_name,
			stock_level,
			sentiment_score,
			sentiment_magnitude,
			case 
				when round(ratio::numeric,2) is null then 'N/A'
				else round(ratio::numeric,2)::varchar(100)
			end as ratio,
			row_number() over (partition by product_sku, name) as 			row_cte_sr
		from sales_report
	)
	select *
	from cte_sr
	where row_cte_sr = 1
)```
