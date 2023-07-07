# walmart_store_analysis
# Introduction:
One of the world's largest retailers will evaluate 45 of its stores in order to improve their performance. Sales performance at different stores is examined, along with growth overall and holiday sales impacts. In order to optimize financial performance for the upcoming fiscal year, this analysis has been conducted to inform future planning.

# Dataset
The Walmart dataset, obtained from Kaggle, comprises a collection of data with over 6,000 rows. It provides detailed information about 45 Walmart stores, including weekly sales figures, holiday indicators, temperature records, fuel prices, and more.

# Tools:
For this particular project, I utilized the Google Bigquery .

# Business Question and Analysis

  -- check null values
  select
    *
   from `sqlproject-379113.walmart.analysis`
   where Weekly_Sales is null;

## 1. Which weeks had the maximum sales?
select
   Date,
   max(Weekly_Sales) as max_sales
from `sqlproject-379113.walmart.analysis`
group by Date
order by max_sales desc
limit 1;

## 2. Which year recorded the highest total sales?
select
   extract(year from Date) as year,
   sum(Weekly_Sales) as sales
from `sqlproject-379113.walmart.analysis`
group by year
order by sales desc
limit 1;

## 3. Which month had the highest sales in 2011?
select
   extract(month from Date) as mnth,
   sum(Weekly_Sales) as sales
from `sqlproject-379113.walmart.analysis`
where extract(year from Date)=2011
group by mnth
order by sales desc
limit 1;

## 4. Which store had the most sales?

select
   store,
   sum(Weekly_Sales) as sales
from `sqlproject-379113.walmart.analysis`
group by store
order by sales desc
limit 1;

## 5. What were the total sales during the holiday and non-holiday weeks?
select
   case
      when Holiday_Flag=0 then 'Holiday Week'
      else 'Non Holiday Week'
      end as week_type,
   sum(Weekly_Sales) as sales
from `sqlproject-379113.walmart.analysis`
group by week_type
order by sales desc;

## 6. Which holidays have the most sales?
select
  Date,
  Holiday_Flag,
  max(weekly_sales) as sales
from `sqlproject-379113.walmart.analysis`
where Holiday_Flag=0
group by Date,Holiday_Flag
order by sales desc;


## Which stores had the highest growth rate from 2010 to 2012?
WITH sales_2012 AS (
  SELECT
    store,
    weekly_sales
  FROM 
    `sqlproject-379113.walmart.analysis`
  WHERE 
    EXTRACT(YEAR FROM Date) = 2012
),
sales_2010 AS (
  SELECT
    store,
    weekly_sales
  FROM 
    `sqlproject-379113.walmart.analysis`
  WHERE 
    EXTRACT(YEAR FROM Date) = 2010
)
SELECT
  s2012.store AS store_id,
  ((s2012.weekly_sales - s2010.weekly_sales) / s2010.weekly_sales) * 100 AS growth_rate
FROM
  sales_2012 AS s2012
JOIN
  sales_2010 AS s2010
ON
  s2012.store = s2010.store
ORDER BY
  growth_rate DESC;

## Which stores had the lowest growth rate from 2010 to 2012?

WITH sales_2012 AS (
  SELECT
    store,
    weekly_sales
  FROM 
    `sqlproject-379113.walmart.analysis`
  WHERE 
    EXTRACT(YEAR FROM Date) = 2012
),
sales_2010 AS (
  SELECT
    store,
    weekly_sales
  FROM 
    `sqlproject-379113.walmart.analysis`
  WHERE 
    EXTRACT(YEAR FROM Date) = 2010
)
SELECT
  s2012.store AS store_id,
  ((s2012.weekly_sales - s2010.weekly_sales) / s2010.weekly_sales) * 100 AS growth_rate
FROM
  sales_2012 AS s2012
JOIN
  sales_2010 AS s2010
ON
  s2012.store = s2010.store
ORDER BY
  growth_rate ASC
LIMIT
  5;



# Insight: 
### Sales Performance and Seasonal Trends:
The weeks surrounding Christmas and Thanksgiving emerged as the highest sales periods, emphasizing their significance as major shopping seasons. Notably, the years 2010 and 2011 recorded the top five instances of maximum sales, while 2012 experienced comparatively lower sales during the holiday season. However, it is important to consider the overall sales performance throughout the year to gain a comprehensive understanding.

### Yearly Sales Analysis:
The year 2011 stood out as the most prosperous year for the analyzed Walmart store, exhibiting significant growth and success. Conversely, 2012 showcased comparatively lower sales, marking it as a relatively challenging year. The analysis of monthly sales revealed December as the peak sales month, aligned with the holiday season and increased consumer spending. Surprisingly, July also showed strong sales performance despite the absence of major holidays. On the other hand, January 2011 reflected the lowest sales figures, indicating potential challenges during that period.

### Store Performance Analysis:
Among the 45 Walmart stores analyzed, Store 20 emerged as the top performer in terms of sales, closely followed by Stores 4 and 14. However, only three locations experienced positive sales growth, while the remaining 42 encountered declining sales growth. This highlights the challenges faced by the majority of the stores in maintaining or improving their sales performance.

### Impact of Holidays on Sales:
The analysis underscores the significant contribution of holiday weeks to overall revenue, showcasing the impact of seasonal festivities. Notably, Thanksgiving, the Super Bowl, and Labour Day stood out as the top-performing holidays in terms of generating sales. Surprisingly, Christmas showed relatively lower performance compared to other observed holidays, challenging the conventional notion of high consumer spending during the holiday season.

