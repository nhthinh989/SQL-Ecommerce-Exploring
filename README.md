# [SQL] E-commerce Exploring

## Table of Contents:
1. [Introduction and Motivation](#data)
2. [The goal of creating this project](#clean_data)
3. [Import raw data](#cau3)
4. [Read and explain dataset](#cau4)
5. [Ask questions and solve it](#cau5)
6. [Conclusion](#cau6)

<div id='data'/>
  
## 1. Introduction and Motivation

This project analyzed the business performance of an eCommerce website using SQL in Google BigQuery, leveraging advanced techniques like window and aggregate functions. The dataset, publicly available on Google BigQuery, contains user session data from Google Analytics in 2017.

The analysis covered key aspects such as website traffic, customer behavior, revenue trends, sales performance, and conversion optimization. Insights from this study supported the Sales and Marketing teams in making data-driven decisions to improve business strategies.

<div id='clean_data'/>
  
## 2. The goal of creating this project
- Overview of website activity
- Bounce rate analysis
- Revenue analysis
- Transactions analysis
- Products analysis

<div id='cau3'/>
  
## 3. Import raw data
  
The eCommerce dataset is stored in a public Google BigQuery dataset. To access the dataset, follow these steps:
- Log in to your Google Cloud Platform account and create a new project.
- Navigate to the BigQuery console and select your newly created project.
- Select "Add Data" in the navigation panel and then "Search a project".
- Enter the project ID **"bigquery-public-data.google_analytics_sample.ga_sessions"** and click "Enter".
- Click on the **"ga_sessions_"** table to open it.
  
<div id='cau4'/>
  
## 4. Read and explain dataset

https://support.google.com/analytics/answer/3437719?hl=en
  | Field Name                       | Data Type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|----------------------------------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fullVisitorId                    | STRING    | The unique visitor ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| date                             | STRING    | The date of the session in YYYYMMDD format.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| totals                           | RECORD    | This section contains aggregate values across the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| totals.bounces                   | INTEGER   | Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| totals.hits                      | INTEGER   | Total number of hits within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| totals.pageviews                 | INTEGER   | Total number of pageviews within the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| totals.visits                    | INTEGER   | The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| trafficSource.source             | STRING    | The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| hits                             | RECORD    | This row and nested fields are populated for any and all types of hits.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| hits.eCommerceAction             | RECORD    | This section contains all of the ecommerce hits that occurred during the session. This is a repeated field and has an entry for each hit that was collected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| hits.product                     | RECORD    | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| hits.product.productQuantity     | INTEGER   | The quantity of the product purchased.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| hits.product.productRevenue      | INTEGER   | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| hits.product.productSKU          | STRING    | Product SKU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| hits.product.v2ProductName       | STRING    | Product Name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| hits.eCommerceAction.action_type | STRING    | The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0.<br>Usually this action type applies to all the products in a hit, with the following exception: when `hits.product.isImpression = TRUE`, the corresponding product is a product impression that is seen while the product action is taking place (i.e., a "product in list view").<br><strong>Example query to calculate number of products in list views:</strong><br>SELECT<br>COUNT(hits.product.v2ProductName)<br>FROM [foo-160803:123456789.ga_sessions_20170101]<br>WHERE hits.product.isImpression == TRUE<br><br><strong>Example query to calculate number of products in detailed view:</strong><br>SELECT<br>COUNT(hits.product.v2ProductName),<br>FROM<br>[foo-160803:123456789.ga_sessions_20170101]<br>WHERE<br>hits.ecommerceaction.action_type = '2'<br>AND (BOOLEAN(hits.product.isImpression) IS NULL<br> OR BOOLEAN(hits.product.isImpression) == FALSE) |

<div id='cau5'/>
  
## 5. Ask questions and solve it
**5.1 alculate total visit, pageview, transaction for Jan, Feb and March 2017**

~~~~sql
   SELECT 
    FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) month
    ,SUM(totals.visits) visits
    ,SUM(totals.pageviews) pageviews
    ,SUM(totals.transactions) transactions
   FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
   WHERE _table_suffix BETWEEN '0101' AND '0331'
   GROUP BY month
   ORDER BY month;
~~~~

| month  | visits | pageviews | transactions |
|--------|--------|-----------|--------------|
| 201701 | 64694  | 257708    | 713          |
| 201702 | 62192  | 233373    | 733          |
| 201703 | 69931  | 259522    | 993          |

From January to March 2017, the eCommerce website saw steady growth in visits, pageviews, and transactions, with a significant surge in March. This suggests increased engagement, effective marketing, or seasonal influence. Further analysis of user behavior can optimize future strategies.

**5.2 Bounce rate per traffic source in July 2017**
~~~~sql
SELECT trafficSource.source
       ,COUNT(visitNumber) total_visits
       ,SUM(totals.bounces) total_no_of_bounces
       ,ROUND((SUM(totals.bounces)/COUNT(visitNumber))*100,2) bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
GROUP BY trafficSource.source
ORDER BY total_visits DESC;

~~~~
|source                     |total_visits|total_no_of_bounces|bounce_rate|
|---------------------------|------------|-------------------|-----------|
|google                     |38400       |19798              |51.56      |
|(direct)                   |19891       |8606               |43.27      |
|youtube.com                |6351        |4238               |66.73      |
|analytics.google.com       |1972        |1064               |53.96      |
|Partners                   |1788        |936                |52.35      |
|m.facebook.com             |669         |430                |64.28      |
|google.com                 |368         |183                |49.73      |
|dfa                        |302         |124                |41.06      |
|sites.google.com           |230         |97                 |42.17      |
|facebook.com               |191         |102                |53.4       |
|reddit.com                 |189         |54                 |28.57      |
|qiita.com                  |146         |72                 |49.32      |
|baidu                      |140         |84                 |60.0       |
|quora.com                  |140         |70                 |50.0       |
|bing                       |111         |54                 |48.65      |
|mail.google.com            |101         |25                 |24.75      |
|yahoo                      |100         |41                 |41.0       |
|blog.golang.org            |65          |19                 |29.23      |
|l.facebook.com             |51          |45                 |88.24      |
|groups.google.com          |50          |22                 |44.0       |
|t.co                       |38          |27                 |71.05      |
|google.co.jp               |36          |25                 |69.44      |
|m.youtube.com              |34          |22                 |64.71      |
|dealspotr.com              |26          |12                 |46.15      |
|productforums.google.com   |25          |21                 |84.0       |
|ask                        |24          |16                 |66.67      |
|support.google.com         |24          |16                 |66.67      |
|int.search.tb.ask.com      |23          |17                 |73.91      |
|optimize.google.com        |21          |10                 |47.62      |
|docs.google.com            |20          |8                  |40.0       |
|lm.facebook.com            |18          |9                  |50.0       |
|l.messenger.com            |17          |6                  |35.29      |
|adwords.google.com         |16          |7                  |43.75      |
|duckduckgo.com             |16          |14                 |87.5       |
|google.co.uk               |15          |7                  |46.67      |
|sashihara.jp               |14          |8                  |57.14      |
|lunametrics.com            |13          |8                  |61.54      |
|search.mysearch.com        |12          |11                 |91.67      |
|outlook.live.com           |10          |7                  |70.0       |
|tw.search.yahoo.com        |10          |8                  |80.0       |
|phandroid.com              |9           |7                  |77.78      |
|plus.google.com            |8           |2                  |25.0       |
|connect.googleforwork.com  |8           |5                  |62.5       |
|m.yz.sm.cn                 |7           |5                  |71.43      |
|search.xfinity.com         |6           |6                  |100.0      |
|google.co.in               |6           |3                  |50.0       |
|online-metrics.com         |5           |2                  |40.0       |
|hangouts.google.com        |5           |1                  |20.0       |
|google.ru                  |5           |1                  |20.0       |
|s0.2mdn.net                |5           |3                  |60.0       |
|googleads.g.doubleclick.net|4           |1                  |25.0       |
|in.search.yahoo.com        |4           |2                  |50.0       |
|m.sogou.com                |4           |3                  |75.0       |
|away.vk.com                |4           |3                  |75.0       |
|m.baidu.com                |3           |2                  |66.67      |
|getpocket.com              |3           |                   |           |
|siliconvalley.about.com    |3           |2                  |66.67      |
|github.com                 |2           |2                  |100.0      |
|centrum.cz                 |2           |2                  |100.0      |
|plus.url.google.com        |2           |                   |           |
|myactivity.google.com      |2           |1                  |50.0       |
|wap.sogou.com              |2           |2                  |100.0      |
|calendar.google.com        |2           |1                  |50.0       |
|google.it                  |2           |1                  |50.0       |
|msn.com                    |2           |1                  |50.0       |
|google.co.th               |2           |1                  |50.0       |
|google.cl                  |2           |1                  |50.0       |
|moodle.aurora.edu          |2           |2                  |100.0      |
|m.sp.sm.cn                 |2           |2                  |100.0      |
|uk.search.yahoo.com        |2           |1                  |50.0       |
|au.search.yahoo.com        |2           |2                  |100.0      |
|search.1and1.com           |2           |2                  |100.0      |
|amp.reddit.com             |2           |1                  |50.0       |
|images.google.com.au       |1           |1                  |100.0      |
|es.search.yahoo.com        |1           |1                  |100.0      |
|arstechnica.com            |1           |                   |           |
|google.bg                  |1           |1                  |100.0      |
|news.ycombinator.com       |1           |1                  |100.0      |
|it.pinterest.com           |1           |1                  |100.0      |
|mx.search.yahoo.com        |1           |1                  |100.0      |
|web.facebook.com           |1           |1                  |100.0      |
|online.fullsail.edu        |1           |1                  |100.0      |
|search.tb.ask.com          |1           |                   |           |
|ph.search.yahoo.com        |1           |                   |           |
|web.mail.comcast.net       |1           |1                  |100.0      |
|kidrex.org                 |1           |1                  |100.0      |
|gophergala.com             |1           |1                  |100.0      |
|kik.com                    |1           |1                  |100.0      |
|google.ca                  |1           |                   |           |
|newclasses.nyu.edu         |1           |                   |           |
|earth.google.com           |1           |                   |           |
|google.es                  |1           |1                  |100.0      |
|malaysia.search.yahoo.com  |1           |1                  |100.0      |
|aol                        |1           |                   |           |
|google.nl                  |1           |                   |           |
|google.com.br              |1           |                   |           |
|suche.t-online.de          |1           |1                  |100.0      |

Google drives the most traffic but has a bounce rate of 51.56%. Social media sources (YouTube 66.73%, Facebook 64.28%) show low retention, while Reddit (28.57%) attracts engaged users. Some sources indicate poor-quality traffic. It is necessary to optimize landing pages, refine SEO, invest in high-retention sources, and filter low-quality traffic.

**5.3 Revenue by traffic source by week, by month in June 2017**
~~~~sql
WITH 
month_data as(
  SELECT
    "Month" as time_type,
    format_date("%Y%m", parse_date("%Y%m%d", date)) as month,
    trafficSource.source AS source,
    SUM(p.productRevenue)/1000000 AS revenue
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
    UNNEST(hits) hits,
    UNNEST(product) p
  WHERE p.productRevenue IS NOT NULL
  GROUP BY 1,2,3
  ORDER BY revenue DESC
),

week_data as(
  SELECT
    "Week" as time_type,
    format_date("%Y%W", parse_date("%Y%m%d", date)) as week,
    trafficSource.source AS source,
    SUM(p.productRevenue)/1000000 AS revenue
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
    unnest(hits) hits,
    unnest(product) p
  WHERE p.productRevenue IS NOT NULL
  GROUP BY 1,2,3
  ORDER BY revenue DESC
)

SELECT * FROM month_data
UNION ALL
SELECT * FROM week_data
ORDER BY time_type, revenue DESC;
~~~~
| time_type | month  | source               | revenue        |
|-----------|--------|----------------------|---------------:|
| Month     | 201706 | (direct)             | 97333.619695   |
| Month     | 201706 | google               | 18757.17992    |
| Month     | 201706 | dfa                  | 8862.229996    |
| Month     | 201706 | mail.google.com      | 2563.13        |
| Month     | 201706 | search.myway.com     | 105.939998     |
| Month     | 201706 | groups.google.com    | 101.96         |
| Month     | 201706 | chat.google.com      | 74.03          |
| Month     | 201706 | dealspotr.com        | 72.95          |
| Month     | 201706 | mail.aol.com         | 64.849998      |
| Month     | 201706 | phandroid.com        | 52.95          |
| Month     | 201706 | sites.google.com     | 39.17          |
| Month     | 201706 | google.com           | 23.99          |
| Month     | 201706 | yahoo                | 20.39          |
| Month     | 201706 | youtube.com          | 16.99          |
| Month     | 201706 | bing                 | 13.98          |
| Month     | 201706 | l.facebook.com       | 12.48          |
| Week      | 201724 | (direct)             | 30908.909927   |
| Week      | 201725 | (direct)             | 27295.319924   |
| Week      | 201723 | (direct)             | 17325.679919   |
| Week      | 201726 | (direct)             | 14914.80995    |
| Week      | 201724 | google               | 9217.169976    |
| Week      | 201722 | (direct)             | 6888.899975    |
| Week      | 201726 | google               | 5330.569964    |
| Week      | 201726 | dfa                  | 3704.74        |
| Week      | 201724 | mail.google.com      | 2486.86        |
| Week      | 201724 | dfa                  | 2341.56        |
| Week      | 201722 | google               | 2119.38999     |
| Week      | 201722 | dfa                  | 1670.649998    |
| Week      | 201723 | dfa                  | 1145.279998    |
| Week      | 201723 | google               | 1083.949999    |
| Week      | 201725 | google               | 1006.099991    |
| Week      | 201723 | search.myway.com     | 105.939998     |
| Week      | 201725 | mail.google.com      | 76.27          |
| Week      | 201723 | chat.google.com      | 74.03          |
| Week      | 201724 | dealspotr.com        | 72.95          |
| Week      | 201725 | mail.aol.com         | 64.849998      |
| Week      | 201726 | groups.google.com    | 63.37          |
| Week      | 201725 | phandroid.com        | 52.95          |
| Week      | 201725 | groups.google.com    | 38.59          |
| Week      | 201725 | sites.google.com     | 25.19          |
| Week      | 201725 | google.com           | 23.99          |
| Week      | 201726 | yahoo                | 20.39          |
| Week      | 201723 | youtube.com          | 16.99          |
| Week      | 201724 | bing                 | 13.98          |
| Week      | 201722 | sites.google.com     | 13.98          |
| Week      | 201724 | l.facebook.com       | 12.48          |

It can be seen that the two sources, Direct and Google, have the highest revenue during both the weekly and monthly periods.

**5.4 Average number of pageviews by purchaser type**
~~~~sql
WITH
purchaser_data AS(
  SELECT
      FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) month,
      (SUM(totals.pageviews)/COUNT(DISTINCT fullvisitorid)) avg_pageviews_purchase,
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
    ,UNNEST(hits) hits
    ,UNNEST(product) product
  WHERE _table_suffix BETWEEN '0601' AND '0731'
  AND totals.transactions>=1
  AND product.productRevenue IS NOT NULL
  GROUP BY month
),

non_purchaser_data AS(
  SELECT
      FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) month,
      (SUM(totals.pageviews)/COUNT(DISTINCT fullvisitorid)) avg_pageviews_non_purchase,
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
    ,UNNEST(hits) hits
    ,UNNEST(product) product
  WHERE _table_suffix BETWEEN '0601' AND '0731'
  AND totals.transactions IS NULL
  AND product.productRevenue IS NULL
  GROUP BY month
)

SELECT
    pd.*,
    avg_pageviews_non_purchase
FROM purchaser_data pd
FULL JOIN non_purchaser_data USING(month)
ORDER BY pd.month;
~~~~
|month                      |avg_pageviews_purchase|avg_pageviews_non_purchase|
|---------------------------|----------------------|--------------------------|
|201706                     |94.02050113895217     |316.86558846341671        |
|201707                     |124.23755186721992    |334.05655979568053        |


Non-purchasers view significantly more pages on average than purchasers, with an increase in pageviews from June to July. This indicates that they browse more but are less likely to convert.

**5.5 Average number of transactions per user that purchased in July 2017**
~~~~sql
SELECT
    FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) month,
    SUM(totals.transactions)/COUNT(DISTINCT fullvisitorid) Avg_total_transactions_per_user
from `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
    ,UNNEST(hits) hits
    ,UNNEST(product) product
WHERE totals.transactions >= 1
  and product.productRevenue IS NOT NULL
GROUP BY month;
~~~~
|month                      |Avg_total_transactions_per_user|
|---------------------------|-------------------------------|
|201707                     |4.16390041493776               |

The table shows the average total transactions per user in July. 
This data suggests that, during July 2017, the typical user conducted about 4.16 transactions on average. This could be useful for understanding user behavior, tracking user engagement with your platform, or evaluating the effectiveness of marketing campaigns or promotions during that specific month.

**5.6 Average amount of money spent per session. Only include purchaser data in 2017**
~~~~sql

WITH revenue_by_user AS (
  SELECT
    FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) month
    ,SUM(product.productRevenue) AS total_revenue
    ,SUM(totals.visits) total_visits
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
    ,UNNEST(hits) hits
    ,UNNEST(hits.product) product
  WHERE 
    product.productRevenue IS NOT NULL
    AND totals.transactions IS NOT NULL
  GROUP BY month
)

SELECT 
  month
  ,ROUND((total_revenue / total_visits)/1000000, 2) Avg_revenue_by_user_per_visit
FROM revenue_by_user
ORDER BY month;

~~~~
|month                      |Avg_revenue_by_user_per_visit|
|---------------------------|-----------------------------|
|201707                     |43.86                        |

The average total transactions per user for July 2017 is 43.86. This suggests that, on average, each user conducted approximately 44 transactions during that month. This could be an important metric for businesses to measure user engagement and activity.


**5.7 Other products purchased by customers who purchased product” Youtube Men’s Vintage Henley” in July 2017**
~~~~sql
WITH
buyer_list AS(
    SELECT
        distinct fullVisitorId  
    FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
    , UNNEST(hits) hits
    , UNNEST(hits.product) product
    WHERE product.v2ProductName = "YouTube Men's Vintage Henley"
    AND totals.transactions >=1
    AND product.productRevenue IS NOT NULL
)

SELECT
  product.v2ProductName other_purchased_products,
  SUM(product.productQuantity) quantity
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
, UNNEST(hits) hits
, UNNEST(hits.product) product
JOIN buyer_list USING(fullVisitorId)
WHERE product.v2ProductName != "YouTube Men's Vintage Henley"
  AND product.productRevenue IS NOT NULL
GROUP BY other_purchased_products
ORDER BY quantity DESC;
~~~~
|other_purchased_products                                 |quantity|
|---------------------------------------------------------|--------|
|Google Sunglasses                                        |20      |
|Google Women's Vintage Hero Tee Black                    |7       |
|SPF-15 Slim & Slender Lip Balm                           |6       |
|Google Women's Short Sleeve Hero Tee Red Heather         |4       |
|YouTube Men's Fleece Hoodie Black                        |3       |
|Google Men's Short Sleeve Badge Tee Charcoal             |3       |
|Android Men's Vintage Henley                             |2       |
|22 oz YouTube Bottle Infuser                             |2       |
|Android Wool Heather Cap Heather/Black                   |2       |
|Crunch Noise Dog Toy                                     |2       |
|Google Men's Short Sleeve Hero Tee Charcoal              |2       |
|Android Women's Fleece Hoodie                            |2       |
|Google Doodle Decal                                      |2       |
|Recycled Mouse Pad                                       |2       |
|YouTube Twill Cap                                        |2       |
|Red Shine 15 oz Mug                                      |2       |
|Google Men's  Zip Hoodie                                 |1       |
|Google Men's 100% Cotton Short Sleeve Hero Tee Red       |1       |
|Android Men's Vintage Tank                               |1       |
|Google Men's Vintage Badge Tee White                     |1       |
|Android Men's Short Sleeve Hero Tee White                |1       |
|Android Men's Pep Rally Short Sleeve Tee Navy            |1       |
|YouTube Men's Short Sleeve Hero Tee Black                |1       |
|Google Men's Pullover Hoodie Grey                        |1       |
|YouTube Men's Short Sleeve Hero Tee White                |1       |
|Google Slim Utility Travel Bag                           |1       |
|YouTube Women's Short Sleeve Hero Tee Charcoal           |1       |
|Google Men's Performance Full Zip Jacket Black           |1       |
|26 oz Double Wall Insulated Bottle                       |1       |
|8 pc Android Sticker Sheet                               |1       |
|Google Women's Long Sleeve Tee Lavender                  |1       |
|YouTube Men's Long & Lean Tee Charcoal                   |1       |
|Android Men's Short Sleeve Hero Tee Heather              |1       |
|YouTube Women's Short Sleeve Tri-blend Badge Tee Charcoal|1       |
|Google Men's Performance 1/4 Zip Pullover Heather/Black  |1       |
|YouTube Hard Cover Journal                               |1       |
|Android BTTF Moonshot Graphic Tee                        |1       |
|Google Men's Airflow 1/4 Zip Pullover Black              |1       |
|Google Men's Bike Short Sleeve Tee Charcoal              |1       |
|Google 5-Panel Cap                                       |1       |
|Google Toddler Short Sleeve T-shirt Grey                 |1       |
|Android Sticker Sheet Ultra Removable                    |1       |
|Google Men's Long & Lean Tee Charcoal                    |1       |
|Google Twill Cap                                         |1       |
|Google Men's Long & Lean Tee Grey                        |1       |
|Google Men's Vintage Badge Tee Black                     |1       |
|Google Men's Long Sleeve Raglan Ocean Blue               |1       |
|YouTube Custom Decals                                    |1       |
|Four Color Retractable Pen                               |1       |
|Google Laptop and Cell Phone Stickers                    |1       |
                                                                      

Overall, the data provides valuable insights into customer preferences, product popularity, and potential areas for marketing and merchandising strategies. Further analysis of historical data and integration with customer demographics could provide a more comprehensive understanding of these trends.
 
 **5.8 Calculate cohort map from product view to add_to_cart/number_product_view.**
 ~~~~sql
WITH
product_view AS(
  SELECT
    FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d", date)) month
    ,COUNT(product.productSKU) num_product_view
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_*`
  , UNNEST(hits) hits
  , UNNEST(hits.product) product
  WHERE _TABLE_SUFFIX BETWEEN '20170101' AND '20170331'
  AND hits.eCommerceAction.action_type = '2'
  GROUP BY 1
),

add_to_cart AS(
  SELECT
    FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d", date)) month,
    COUNT(product.productSKU) num_addtocart
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_*`
  , UNNEST(hits) hits
  , UNNEST(hits.product) product
  WHERE _TABLE_SUFFIX BETWEEN '20170101' AND '20170331'
  AND hits.eCommerceAction.action_type = '3'
  GROUP BY 1
),

purchase AS(
  SELECT
    FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d", date)) month,
    COUNT(product.productSKU) num_purchase
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_*`
  , UNNEST(hits) hits
  , UNNEST(hits.product) product
  WHERE _TABLE_SUFFIX BETWEEN '20170101' AND '20170331'
    AND hits.eCommerceAction.action_type = '6'
    AND product.productRevenue IS NOT NULL   --phải thêm điều kiện này để đảm bảo có revenue
  group by 1
)

SELECT
    pv.*,
    num_addtocart,
    num_purchase,
    ROUND(num_addtocart*100/num_product_view,2) add_to_cart_rate,
    ROUND(num_purchase*100/num_product_view,2) purchase_rate
FROM product_view pv
LEFT JOIN add_to_cart a ON pv.month = a.month
LEFT JOIN purchase p ON pv.month = p.month
ORDER BY pv.month;
~~~~
|month                                                    |num_product_view|num_addtocart|num_purchase|add_to_cart_rate|purchase_rate|
|---------------------------------------------------------|----------------|-------------|------------|----------------|-------------|
|201701                                                   |25787           |7342         |2143        |28.47           |8.31         |
|201702                                                   |21489           |7360         |2060        |34.25           |9.59         |
|201703                                                   |23549           |8782         |2977        |37.29           |12.64        |



The table illustrates five various metrics and rates related to user behavior from January to March 2017.

Overall, the number of product views from January 2017 to March 2017 increased gradually. The add-to-cart rate and purchase rate also increased over the same period, indicating improved user engagement and conversion.

However, the add-to-cart rate and purchase rate are notably higher in March 2017, suggesting potential improvements in the website's user experience or marketing efforts.

<div id='cau6'/>
  
## 6. Conclusion

Analyzing the eCommerce dataset using SQL in Google BigQuery provided valuable insights into customer behavior, including visits, pageviews, transactions, bounce rates, and revenue per traffic source. By examining referral sources, I identified key marketing channels driving traffic and sales, highlighting opportunities to optimize underperforming ones for better ROI. This project offered a deeper understanding of the customer journey and marketing effectiveness. The next step involves visualizing these insights using tools like Power BI or Tableau to enhance strategic decision-making, optimize operations, and drive revenue growth.
