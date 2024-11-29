# SQL_Ecommerce_Exploring

## Table of Contents:
1. [Introduction and Motivation](#data)
2. [The goal of creating this project](#clean_data)
3. [Import raw data](#cau3)
4. [Read and explain dataset](#cau4)
5. [ Data Processing & Exploratory Data Analysis](#cau5)
6. [Ask questions and solve it](#cau6)
7. [Conclusion](#cau7)

<div id='data'/>
  
## 1. Introduction and Motivation

The eCommerce dataset is stored in a public Google BigQuery dataset. This dataset contains information about user sessions on a website collected from Google Analytics in 2017.

Based on the eCommerce dataset, the author perform queries to analyze website activity in 2017, such as calculating bounce rate, identifying days with the highest revenue, analyzing user behavior on pages, and various other types of analysis. This project aims to have an outlook on the business situation, marketing activity efficiency analyzing the products.

To query and work with this dataset, the author uses the Google BigQuery tool to write and execute SQL queries.

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
  
## 5. Data Processing & Exploratory Data Analysis


~~~~sql
SELECT COUNT(fullVisitorId) row_num,
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`
~~~~

| row_num |
|---------|
| 71812   |

~~~~sql
SELECT COUNT(fullVisitorId) row_num,
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
~~~~

| row_num |
|---------|
| 467260  |

~~~~sql
SELECT EXTRACT(MONTH FROM PARSE_DATE("%Y%m%d",date)) month
,COUNT(*) AS counts
,ROUND((COUNT(*)/(SELECT COUNT(*) 
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`))*100,1) pct
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
GROUP BY EXTRACT(MONTH FROM PARSE_DATE("%Y%m%d",date))
~~~~

| month | counts | pct  |
|-------|--------|------|
| 6     | 63578  | 13.6 |
| 3     | 69931  | 15.0 |
| 8     | 2556   | 0.5  |
| 2     | 62192  | 13.3 |
| 4     | 67126  | 14.4 |
| 1     | 64694  | 13.8 |
| 7     | 71812  | 15.4 |
| 5     | 65371  | 14.0 |



**UNNEST hits and products**
~~~~sql
SELECT date, 
fullVisitorId,
eCommerceAction.action_type,
product.v2ProductName,
product.productRevenue,
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
UNNEST(hits) AS hits,
UNNEST(hits.product) as product
~~~~

| date     | fullVisitorId       | action_type | v2ProductName                         | productRevenue |
|----------|---------------------|-------------|---------------------------------------|----------------|
| 20170712 | 4080810487624198636 | 1           | YouTube Custom Decals                 |                |
| 20170712 | 4080810487624198636 | 2           | YouTube Custom Decals                 |                |
| 20170712 | 7291695423333449793 | 1           | Keyboard DOT Sticker                  |                |
| 20170712 | 7291695423333449793 | 2           | Keyboard DOT Sticker                  |                |
| 20170712 | 3153380067864919818 | 2           | Google Baby Essentials Set            |                |
| 20170712 | 3153380067864919818 | 1           | Google Baby Essentials Set            |                |
| 20170712 | 5615263059272956391 | 0           | Android Lunch Kit                     |                |
| 20170712 | 5615263059272956391 | 0           | Android Rise 14 oz Mug                |                |
| 20170712 | 5615263059272956391 | 0           | Android Sticker Sheet Ultra Removable |                |
| 20170712 | 5615263059272956391 | 0           | Windup Android                        |                |

<div id='cau6'/>
  
## 6. Ask questions and solve it
**6.1 alculate total visit, pageview, transaction for Jan, Feb and March 2017**

~~~~sql
   SELECT 
    FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) month
    ,SUM(totals.visits) visits
    ,SUM(totals.pageviews) pageviews
    ,SUM(totals.transactions) transactions
    ,ROUND(SUM(totals.totalTransactionRevenue)/POW(10,6),2) revenue
   FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
   WHERE _table_suffix BETWEEN '0101' AND '0331'
   GROUP BY month
   ORDER BY month;
~~~~

| month  | visits | pageviews | transactions | revenue   |
|--------|--------|-----------|--------------|-----------|
| 201701 | 64694  | 257708    | 713          | 106248.15 |
| 201702 | 62192  | 233373    | 733          | 116111.6  |
| 201703 | 69931  | 259522    | 993          | 150224.7  |

The table provides a snapshot of The eCommerce website performance over the first few months of 2017, including visits, pageviews, transactions, and revenue metrics. The author derive some insights from the provided monthly data:

 **Traffic and Engagement Patterns**: The number of visits and pageviews increased from January (`201701`) to March (`201703`), suggesting a growing website interest over time. Besides that, the increase in pageviews is a positive sign, indicating that users explore multiple pages during their visits, potentially finding the content engaging.
 
  **Conversion and Revenue Trends**:The number of transactions and total revenue also steadily increased from January to March. This demonstrates that more users are engaging with the site, and a growing proportion of them are also making transactions, contributing to increased revenue.
Moreover, the substantial jump in transactions and revenue from January to March (`201703`) suggests that efforts made during this period might have been particularly effective in driving user conversions.

  **Seasonal or Marketing Influence**:The consistent growth in both transactions and revenue over the three months could indicate the influence of seasonality, marketing campaigns, or optimizations implemented during this time.
  
  **Opportunities for Improvement**: While the data shows positive growth, there might be opportunities to optimize user engagement and conversions further. Analyzing specific user journeys, popular landing pages, and exit points could help identify areas for improvement.
  
 **Future Strategy and Focus**: Consider further investigating what strategies, campaigns, or changes were implemented between January and March that contributed to the significant increase in transactions and revenue. These insights can inform future marketing and optimization efforts.

**6.2 Bounce rate per traffic source in July 2017**
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

The table provides an overview of website traffic from various sources and key metrics that help evaluate user engagement and behavior based on four elements: source, total_visits, total_no_of_bounces, bounce_rate. Google website received the most traffic to the website at 38,400 visits in the first row. Of these, 19,798 were single-page visits (bounces), resulting in a bounce rate of approximately 51.56%. Following that Direct website took second place at 19,891 visits from direct traffic, with 8,606 bounces, leading to a bounce rate of about 43.27%. Thereafter, the Youtube.com website had 6,351 visits, with 4,238 bounces, resulting in a bounce rate of approximately 66.73%, higher than Google and Direct. However, search.mysearch.com website had the lowest total visits but the highest bounce rate. It had 12 visits with 11 bounces, resulting in a bounce rate of approximately 91.67%. This indicates that most users who visited the website from this source left without interacting with other pages. A high bounce rate from a source like search.mysearch.com suggests that users arriving from this source might not find the content they are looking for or that the landing page experience may not be engaging enough to encourage further exploration. It's important to note that while addressing high bounce rates is essential, the context of the source and user behavior should be thoroughly analyzed to determine the most effective strategies for improvement. Because a lower bounce rate generally indicates that users are more engaged with the website content, while a higher bounce rate may suggest that users are leaving the site after viewing only a single page.

**6.3 Revenue by traffic source by week, by month in June 2017**
~~~~sql
WITH 
month_data AS(
  SELECT
    "Month" time_type,
    FORMAT_DATE("%Y%m", parse_date("%Y%m%d", date)) month,
    trafficSource.source source,
    ROUND(SUM(p.productRevenue)/1000000, 2) revenue
  FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`,
    UNNEST(hits) hits,
    UNNEST(product) p
  WHERE p.productRevenue IS NOT NULL
  GROUP BY 1,2,3
  ORDER BY revenue DESC
),

week_data AS (
  SELECT
    "Week" time_type,
    FORMAT_DATE("%Y%W", PARSE_DATE("%Y%m%d", date)) week,
    trafficSource.source source,
    ROUND(SUM(p.productRevenue)/1000000, 2) revenue
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
ORDER BY revenue DESC;
~~~~
|time_type|month |source           |revenue |
|---------|------|-----------------|--------|
|Month    |201706|(direct)         |97333.62|
|Week     |201724|(direct)         |30908.91|
|Week     |201725|(direct)         |27295.32|
|Month    |201706|google           |18757.18|
|Week     |201723|(direct)         |17325.68|
|Week     |201726|(direct)         |14914.81|
|Week     |201724|google           |9217.17 |
|Month    |201706|dfa              |8862.23 |
|Week     |201722|(direct)         |6888.9  |
|Week     |201726|google           |5330.57 |
|Week     |201726|dfa              |3704.74 |
|Month    |201706|mail.google.com  |2563.13 |
|Week     |201724|mail.google.com  |2486.86 |
|Week     |201724|dfa              |2341.56 |
|Week     |201722|google           |2119.39 |
|Week     |201722|dfa              |1670.65 |
|Week     |201723|dfa              |1145.28 |
|Week     |201723|google           |1083.95 |
|Week     |201725|google           |1006.1  |
|Month    |201706|search.myway.com |105.94  |
|Week     |201723|search.myway.com |105.94  |
|Month    |201706|groups.google.com|101.96  |
|Week     |201725|mail.google.com  |76.27   |
|Month    |201706|chat.google.com  |74.03   |
|Week     |201723|chat.google.com  |74.03   |
|Week     |201724|dealspotr.com    |72.95   |
|Month    |201706|dealspotr.com    |72.95   |
|Month    |201706|mail.aol.com     |64.85   |
|Week     |201725|mail.aol.com     |64.85   |
|Week     |201726|groups.google.com|63.37   |
|Week     |201725|phandroid.com    |52.95   |
|Month    |201706|phandroid.com    |52.95   |
|Month    |201706|sites.google.com |39.17   |
|Week     |201725|groups.google.com|38.59   |
|Week     |201725|sites.google.com |25.19   |
|Month    |201706|google.com       |23.99   |
|Week     |201725|google.com       |23.99   |
|Month    |201706|yahoo            |20.39   |
|Week     |201726|yahoo            |20.39   |
|Month    |201706|youtube.com      |16.99   |
|Week     |201723|youtube.com      |16.99   |
|Week     |201724|bing             |13.98   |
|Month    |201706|bing             |13.98   |
|Week     |201722|sites.google.com |13.98   |
|Week     |201724|l.facebook.com   |12.48   |
|Month    |201706|l.facebook.com   |12.48   |

This table represents revenue data collection with various attributes, including time type, time period, source, and revenue amount. Each row corresponds to a specific time period (week or month) and provides information about the revenue generated from different sources during that time period. The author found the insights bellow through the table above.

 **Direct Revenue and Source Breakdown:** The dataset includes revenue from various sources, such as (direct), Google, DFA, mail.google.com, and search engines like myway.com. Revenue comes from different sources, including direct website visits, organic search traffic, and referrals from various websites.
 
**Time Period Analysis:**
Revenue is reported on a weekly and monthly basis. It seems like the data spans multiple months, including the month labeled "201706."

**6.4 Average number of pageviews by purchaser type**
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


The table shows user behavior in a tabular format with two columns  "avg_pageviews_purchase," and "avg_pageviews_non_purchase"  in two consecutive months, June 2017 (201706) and July 2017 (201707).
 
 **Engagement Patterns:** The higher average pageviews for purchasers suggest that users who are more engaged with the website's content are more likely to make a purchase. This could indicate a positive correlation between engagement and conversion.
 
 **Behavioral Insights:** The data reflect different user behaviors. Users interested in making a purchase might spend more time exploring product details, reading reviews, and comparing options, leading to a higher number of pageviews.
 
 **Conversion Optimization:** The business can leverage this data to optimize its website's design and content to encourage higher engagement among users likely to purchase. This could involve improving navigation, showcasing relevant products, and providing valuable content to guide users toward conversion.

**6.5 Average number of transactions per user that purchased in July 2017**
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

**6.6 Average amount of money spent per session. Only include purchaser data in 2017**
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


**6.7 Other products purchased by customers who purchased product” Youtube Men’s Vintage Henley” in July 2017**
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
 
 **6.8 Calculate cohort map from product view to add_to_cart/number_product_view.**
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


<div id='cau7'/>
  
## 7. Conclusion

- This is the author's opportunity to learn about the marketing industry and the customer journey through this e-commerce dataset

- In analyzing the e-commerce dataset using BigQuery, the author understands customer behavior through the bounce rate, transaction, revenue, visit, and purchase.
  
- The author gained insights into which marketing channels drive traffic and sales by examining referral sources. Investing resources in effective channels and optimizing underperforming ones can improve marketing ROI.

- In conclusion, exploring the e-commerce dataset on BigQuery unearthed a wealth of insights critical for strategic decision-making to help the business can optimize operations, enhance customer experiences, and drive revenue.
