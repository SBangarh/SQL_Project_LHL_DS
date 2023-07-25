Final-Project-Transforming-and-Analyzing-Data-with-SQL

Project/Goals:

1) To apply and extend my sql knowledge that I gained from the coursework and own research,
2) To improve my confidence in my coding abilities by application and feedback,
3) Try to go apply knowledge beyond the course work like statistical analysis in SQL
4) Most importantly, have fun.

Process:


1) I downloaded the csv files and explored them in Excel to determine potential data types and further my understanding of how the data is organized, 
2) I imported the data into pgadmin for further exploration and applied my sql knowledge
3) I attempted to understand the data and noted any data quality issues to clean later
4) I noticed there was a lot of data and some tables were better than others in terms of data quality
5) I read the “Starting with questions” to guide me and identify potential tables and columns to clean and use for analysis
6) I decided to create a view of a CTE. I selected and cleaned relevant columns (as identified in step 5) in the CTE. For instance, I only used nine columns from the All Sessions table when it originally had 32. 
7) I used the row number function to remove duplicates within the view.
8) I queried the views I created to answer various questions, sometimes creating additional CTE’s. 



## Results

This data had information regarding countries and cities, dates, prices, channel grouping, products, and units sold. It also had data about the time on site as well as some supply chain information like lead time. 

I used the data, after cleaning, to analyze revenue, orders, products, and channel grouping. My conclusions are below.

1) The United States is responsible for most of the revenue, occupying seven places in the top 10. 
2) The top 10 highest average order size was again dominated by the United States, although Canada and Japan were also present. It was interesting to see that the city with the highest revenue – Mountain View, United States – was ranked 10 for average order size.
3) It was difficult to determine a pattern for the product categories. A lot of it was my inability to properly clean the product categories to its main category.
4) Most of the orders were apparel items while electronics and Nest were second most.
5) Most of the site revenue was generated from unknown cities in the United States at over 10 percent. Every other geographic region accounted for less than or equal to 0.31 percent.


## Challenges
 
1) The size of the data files, especially the All Sessions and Analytics tables. I have never dealt with anything of that size. 
2) I got sidetracked quite a lot by going through rabbit holes
3) Being a perfectionist, I completed the project, but then I decided to start from scratch on Saturday.
4) I did not continuously commit every time I worked on the project which is a bad habit. I should push/ commit whenever I make changes. 
5) Confidence struggled at times, mostly because I wasn’t as prepared as I’d like to be. 
6) Struggled to write optimal code as most of my queries were upwards of 30 seconds

## Future Goals

1) I would have liked to perform some statistical analysis on revenue and the sentiment score and magnitudes. 
2) I would have really liked to properly clean the category data and properly have gone through the country/city pairs to ensure they were accurate and not assume. 
3) I learned a lot and I am excited to find my own data to play around with and start building my portfolio. This will also allow me to practice git and github.
4) I would like to learn more about code optimization as a lot of my queries were upwards of 30 seconds. Could be costly in a professional setting. 
