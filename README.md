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
| hits.eCommerceAction.action_type | STRING    | The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0.Usually this action type applies to all the products in a hit, with the following exception: when hits.product.isImpression = TRUE, the corresponding product is a product impression that is seen while the product action is taking place (i.e., a product in list view).Example query to calculate number of products in list views:SELECTCOUNT(hits.product.v2ProductName)FROM [foo-160803:123456789.ga_sessions_20170101]WHERE hits.product.isImpression == TRUEExample query to calculate number of products in detailed view:SELECTCOUNT(hits.product.v2ProductName),FROM[foo-160803:123456789.ga_sessions_20170101]WHEREhits.ecommerceaction.action_type = 2AND ( BOOLEAN(hits.product.isImpression) IS NULL OR BOOLEAN(hits.product.isImpression) == FALSE ) |
| hits.product                     | RECORD    | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| hits.product.productQuantity     | INTEGER   | The quantity of the product purchased.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| hits.product.productRevenue      | INTEGER   | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| hits.product.productSKU          | STRING    | Product SKU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| hits.product.v2ProductName       | STRING    | Product Name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| fullVisitorId                    | STRING    | The unique visitor ID.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

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
**6.1 calculate total visits, pageview, transaction, and revenue for Jan, Feb, and March 2017**

~~~~sql
   SELECT 
    FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) month_extract
    ,SUM(totals.visits) visits
    ,SUM(totals.pageviews) pageviews
    ,SUM(totals.transactions) transactions
    ,ROUND(SUM(totals.totalTransactionRevenue)/POW(10,6),2) revenue
   FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
   WHERE _table_suffix BETWEEN '0101' AND '0331'
   GROUP BY month_extract
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
| source                      | total_visits | total_no_of_bounces | bounce_rate |
|-----------------------------|--------------|---------------------|-------------|
| google                      | 38400        | 19798               | 51.56       |
| (direct)                    | 19891        | 8606                | 43.27       |
| youtube.com                 | 6351         | 4238                | 66.73       |
| analytics.google.com        | 1972         | 1064                | 53.96       |
| Partners                    | 1788         | 936                 | 52.35       |
| m.facebook.com              | 669          | 430                 | 64.28       |
| google.com                  | 368          | 183                 | 49.73       |
| dfa                         | 302          | 124                 | 41.06       |
| sites.google.com            | 230          | 97                  | 42.17       |
| facebook.com                | 191          | 102                 | 53.4        |
| reddit.com                  | 189          | 54                  | 28.57       |
| qiita.com                   | 146          | 72                  | 49.32       |
| baidu                       | 140          | 84                  | 60          |
| quora.com                   | 140          | 70                  | 50          |
| bing                        | 111          | 54                  | 48.65       |
| mail.google.com             | 101          | 25                  | 24.75       |
| yahoo                       | 100          | 41                  | 41          |
| blog.golang.org             | 65           | 19                  | 29.23       |
| l.facebook.com              | 51           | 45                  | 88.24       |
| groups.google.com           | 50           | 22                  | 44          |
| t.co                        | 38           | 27                  | 71.05       |
| google.co.jp                | 36           | 25                  | 69.44       |
| m.youtube.com               | 34           | 22                  | 64.71       |
| dealspotr.com               | 26           | 12                  | 46.15       |
| productforums.google.com    | 25           | 21                  | 84          |
| ask                         | 24           | 16                  | 66.67       |
| support.google.com          | 24           | 16                  | 66.67       |
| int.search.tb.ask.com       | 23           | 17                  | 73.91       |
| optimize.google.com         | 21           | 10                  | 47.62       |
| docs.google.com             | 20           | 8                   | 40          |
| lm.facebook.com             | 18           | 9                   | 50          |
| l.messenger.com             | 17           | 6                   | 35.29       |
| adwords.google.com          | 16           | 7                   | 43.75       |
| duckduckgo.com              | 16           | 14                  | 87.5        |
| google.co.uk                | 15           | 7                   | 46.67       |
| sashihara.jp                | 14           | 8                   | 57.14       |
| lunametrics.com             | 13           | 8                   | 61.54       |
| search.mysearch.com         | 12           | 11                  | 91.67       |
| tw.search.yahoo.com         | 10           | 8                   | 80          |
| outlook.live.com            | 10           | 7                   | 70          |
| phandroid.com               | 9            | 7                   | 77.78       |
| connect.googleforwork.com   | 8            | 5                   | 62.5        |
| plus.google.com             | 8            | 2                   | 25          |
| m.yz.sm.cn                  | 7            | 5                   | 71.43       |
| google.co.in                | 6            | 3                   | 50          |
| search.xfinity.com          | 6            | 6                   | 100         |
| google.ru                   | 5            | 1                   | 20          |
| online-metrics.com          | 5            | 2                   | 40          |
| hangouts.google.com         | 5            | 1                   | 20          |
| s0.2mdn.net                 | 5            | 3                   | 60          |
| m.sogou.com                 | 4            | 3                   | 75          |
| in.search.yahoo.com         | 4            | 2                   | 50          |
| googleads.g.doubleclick.net | 4            | 1                   | 25          |
| away.vk.com                 | 4            | 3                   | 75          |
| getpocket.com               | 3            |                     |             |
| m.baidu.com                 | 3            | 2                   | 66.67       |
| siliconvalley.about.com     | 3            | 2                   | 66.67       |
| msn.com                     | 2            | 1                   | 50          |
| google.it                   | 2            | 1                   | 50          |
| google.co.th                | 2            | 1                   | 50          |
| wap.sogou.com               | 2            | 2                   | 100         |
| calendar.google.com         | 2            | 1                   | 50          |
| github.com                  | 2            | 2                   | 100         |
| plus.url.google.com         | 2            |                     |             |
| myactivity.google.com       | 2            | 1                   | 50          |
| centrum.cz                  | 2            | 2                   | 100         |
| search.1and1.com            | 2            | 2                   | 100         |
| uk.search.yahoo.com         | 2            | 1                   | 50          |
| au.search.yahoo.com         | 2            | 2                   | 100         |
| m.sp.sm.cn                  | 2            | 2                   | 100         |
| google.cl                   | 2            | 1                   | 50          |
| moodle.aurora.edu           | 2            | 2                   | 100         |
| amp.reddit.com              | 2            | 1                   | 50          |
| newclasses.nyu.edu          | 1            |                     |             |
| google.es                   | 1            | 1                   | 100         |
| google.ca                   | 1            |                     |             |
| malaysia.search.yahoo.com   | 1            | 1                   | 100         |
| kidrex.org                  | 1            | 1                   | 100         |
| gophergala.com              | 1            | 1                   | 100         |
| aol                         | 1            |                     |             |
| google.nl                   | 1            |                     |             |
| kik.com                     | 1            | 1                   | 100         |
| earth.google.com            | 1            |                     |             |
| ph.search.yahoo.com         | 1            |                     |             |
| web.mail.comcast.net        | 1            | 1                   | 100         |
| google.bg                   | 1            | 1                   | 100         |
| news.ycombinator.com        | 1            | 1                   | 100         |
| es.search.yahoo.com         | 1            | 1                   | 100         |
| it.pinterest.com            | 1            | 1                   | 100         |
| mx.search.yahoo.com         | 1            | 1                   | 100         |
| images.google.com.au        | 1            | 1                   | 100         |
| search.tb.ask.com           | 1            |                     |             |
| arstechnica.com             | 1            |                     |             |
| web.facebook.com            | 1            | 1                   | 100         |
| online.fullsail.edu         | 1            | 1                   | 100         |
| google.com.br               | 1            |                     |             |
| suche.t-online.de           | 1            | 1                   | 100         |


The table provides an overview of website traffic from various sources and key metrics that help evaluate user engagement and behavior based on four elements: source, total_visits, total_no_of_bounces, bounce_rate. Google website received the most traffic to the website at 38,400 visits in the first row. Of these, 19,798 were single-page visits (bounces), resulting in a bounce rate of approximately 51.56%. Following that Direct website took second place at 19,891 visits from direct traffic, with 8,606 bounces, leading to a bounce rate of about 43.27%. Thereafter, the Youtube.com website had 6,351 visits, with 4,238 bounces, resulting in a bounce rate of approximately 66.73%, higher than Google and Direct. However, search.mysearch.com website had the lowest total visits but the highest bounce rate. It had 12 visits with 11 bounces, resulting in a bounce rate of approximately 91.67%. This indicates that most users who visited the website from this source left without interacting with other pages. A high bounce rate from a source like search.mysearch.com suggests that users arriving from this source might not find the content they are looking for or that the landing page experience may not be engaging enough to encourage further exploration. It's important to note that while addressing high bounce rates is essential, the context of the source and user behavior should be thoroughly analyzed to determine the most effective strategies for improvement. Because a lower bounce rate generally indicates that users are more engaged with the website content, while a higher bounce rate may suggest that users are leaving the site after viewing only a single page.

**6.3 Revenue by traffic source by week, by month in June 2017**
~~~~sql
WITH GET_RE_MONTH AS 
(
    SELECT DISTINCT
        CASE WHEN 1=1 THEN "Month" END time_type,
        FORMAT_DATE("%Y%m", PARSE_DATE("%Y%m%d", date)) AS time ,
        trafficSource.source AS source,
        ROUND(SUM(totals.totalTransactionRevenue/1000000) OVER(PARTITION BY trafficSource.source),2) revenue
    FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`
),

GET_RE_WEEK AS 
(
    SELECT
        CASE WHEN 1=1 THEN "WEEK" END time_type,
        FORMAT_DATE("%Y%W", PARSE_DATE("%Y%m%d", date)) AS time,
        trafficSource.source AS source,
        SUM(totals.totalTransactionRevenue)/1000000 revenue
    FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
    WHERE _table_suffix BETWEEN '0601' AND '0630'
    GROUP BY 1,2,3
)

SELECT * FROM GET_RE_MONTH
UNION ALL 
SELECT * FROM GET_RE_WEEK
ORDER BY revenue DESC;
~~~~
| time_type | time   | source                        | revenue  |
|-----------|--------|-------------------------------|----------|
| Month     | 201706 | (direct)                      | 97231.62 |
| WEEK      | 201724 | (direct)                      | 30883.91 |
| WEEK      | 201725 | (direct)                      | 27254.32 |
| Month     | 201706 | google                        | 18757.18 |
| WEEK      | 201723 | (direct)                      | 17302.68 |
| WEEK      | 201726 | (direct)                      | 14905.81 |
| WEEK      | 201724 | google                        | 9217.17  |
| Month     | 201706 | dfa                           | 8841.23  |
| WEEK      | 201722 | (direct)                      | 6884.9   |
| WEEK      | 201726 | google                        | 5330.57  |
| WEEK      | 201726 | dfa                           | 3704.74  |
| Month     | 201706 | mail.google.com               | 2563.13  |
| WEEK      | 201724 | mail.google.com               | 2486.86  |
| WEEK      | 201724 | dfa                           | 2341.56  |
| WEEK      | 201722 | google                        | 2119.39  |
| WEEK      | 201722 | dfa                           | 1670.65  |
| WEEK      | 201723 | dfa                           | 1124.28  |
| WEEK      | 201723 | google                        | 1083.95  |
| WEEK      | 201725 | google                        | 1006.1   |
| WEEK      | 201723 | search.myway.com              | 105.94   |
| Month     | 201706 | search.myway.com              | 105.94   |
| Month     | 201706 | groups.google.com             | 101.96   |
| WEEK      | 201725 | mail.google.com               | 76.27    |
| Month     | 201706 | chat.google.com               | 74.03    |
| WEEK      | 201723 | chat.google.com               | 74.03    |
| WEEK      | 201724 | dealspotr.com                 | 72.95    |
| Month     | 201706 | dealspotr.com                 | 72.95    |
| WEEK      | 201725 | mail.aol.com                  | 64.85    |
| Month     | 201706 | mail.aol.com                  | 64.85    |
| WEEK      | 201726 | groups.google.com             | 63.37    |
| Month     | 201706 | phandroid.com                 | 52.95    |
| WEEK      | 201725 | phandroid.com                 | 52.95    |
| Month     | 201706 | sites.google.com              | 39.17    |
| WEEK      | 201725 | groups.google.com             | 38.59    |
| WEEK      | 201725 | sites.google.com              | 25.19    |
| Month     | 201706 | google.com                    | 23.99    |
| WEEK      | 201725 | google.com                    | 23.99    |
| Month     | 201706 | yahoo                         | 20.39    |
| WEEK      | 201726 | yahoo                         | 20.39    |
| WEEK      | 201723 | youtube.com                   | 16.99    |
| Month     | 201706 | youtube.com                   | 16.99    |
| WEEK      | 201724 | bing                          | 13.98    |
| Month     | 201706 | bing                          | 13.98    |
| WEEK      | 201722 | sites.google.com              | 13.98    |
| WEEK      | 201724 | l.facebook.com                | 12.48    |
| Month     | 201706 | l.facebook.com                | 12.48    |
| WEEK      | 201724 | t.co                          |          |
| WEEK      | 201724 | groups.google.com             |          |
| WEEK      | 201724 | Partners                      |          |
| WEEK      | 201724 | qiita.com                     |          |
| WEEK      | 201724 | sashihara.jp                  |          |
| WEEK      | 201724 | yahoo                         |          |
| WEEK      | 201724 | youtube.com                   |          |
| WEEK      | 201724 | l.messenger.com               |          |
| WEEK      | 201723 | sites.google.com              |          |
| WEEK      | 201723 | mail.google.com               |          |
| WEEK      | 201723 | baidu                         |          |
| WEEK      | 201723 | productforums.google.com      |          |
| WEEK      | 201723 | google.com                    |          |
| WEEK      | 201723 | facebook.com                  |          |
| WEEK      | 201723 | groups.google.com             |          |
| WEEK      | 201723 | l.facebook.com                |          |
| WEEK      | 201723 | ask                           |          |
| WEEK      | 201725 | ask                           |          |
| WEEK      | 201725 | keep.google.com               |          |
| WEEK      | 201725 | t.co                          |          |
| WEEK      | 201726 | optimize.google.com           |          |
| WEEK      | 201726 | bing                          |          |
| WEEK      | 201726 | google.co.jp                  |          |
| WEEK      | 201726 | analytics.google.com          |          |
| WEEK      | 201726 | sites.google.com              |          |
| WEEK      | 201726 | quora.com                     |          |
| WEEK      | 201726 | lm.facebook.com               |          |
| WEEK      | 201726 | t.co                          |          |
| WEEK      | 201726 | m.facebook.com                |          |
| WEEK      | 201726 | search.xfinity.com            |          |
| WEEK      | 201726 | plus.google.com               |          |
| WEEK      | 201726 | search.incredibar.com         |          |
| WEEK      | 201726 | m.youtube.com                 |          |
| WEEK      | 201726 | suche.t-online.de             |          |
| WEEK      | 201726 | businessinsider.com           |          |
| WEEK      | 201724 | connect.googleforwork.com     |          |
| WEEK      | 201724 | google.com.ua                 |          |
| WEEK      | 201724 | m.youtube.com                 |          |
| WEEK      | 201724 | search.mysearch.com           |          |
| WEEK      | 201724 | phandroid.com                 |          |
| WEEK      | 201722 | analytics.google.com          |          |
| WEEK      | 201722 | s0.2mdn.net                   |          |
| WEEK      | 201722 | Partners                      |          |
| WEEK      | 201722 | qiita.com                     |          |
| WEEK      | 201722 | groups.google.com             |          |
| WEEK      | 201722 | reddit.com                    |          |
| WEEK      | 201722 | fr.search.yahoo.com           |          |
| WEEK      | 201722 | google.ca                     |          |
| WEEK      | 201722 | yahoo                         |          |
| WEEK      | 201722 | mail.google.com               |          |
| WEEK      | 201722 | ask                           |          |
| WEEK      | 201723 | google.com.ua                 |          |
| WEEK      | 201723 | siliconvalley.about.com       |          |
| WEEK      | 201723 | m.youtube.com                 |          |
| WEEK      | 201725 | productforums.google.com      |          |
| WEEK      | 201724 | docs.google.com               |          |
| WEEK      | 201724 | outlook.live.com              |          |
| WEEK      | 201724 | ask                           |          |
| WEEK      | 201723 | connect.googleforwork.com     |          |
| WEEK      | 201723 | au.search.yahoo.com           |          |
| WEEK      | 201723 | google.it                     |          |
| WEEK      | 201723 | m.baidu.com                   |          |
| WEEK      | 201723 | lm.facebook.com               |          |
| WEEK      | 201726 | phandroid.com                 |          |
| WEEK      | 201726 | myactivity.google.com         |          |
| WEEK      | 201726 | google.co.th                  |          |
| WEEK      | 201723 | lunametrics.com               |          |
| WEEK      | 201723 | datastudio.google.com         |          |
| WEEK      | 201723 | businessinsider.com           |          |
| WEEK      | 201723 | int.search.mywebsearch.com    |          |
| WEEK      | 201726 | dealspotr.com                 |          |
| WEEK      | 201726 | google.com.au                 |          |
| WEEK      | 201724 | search.tb.ask.com             |          |
| WEEK      | 201726 | hangouts.google.com           |          |
| WEEK      | 201726 | linkedin.com                  |          |
| WEEK      | 201726 | search.tb.ask.com             |          |
| WEEK      | 201724 | admin.globalaccess.com        |          |
| WEEK      | 201724 | search.xfinity.com            |          |
| WEEK      | 201725 | optimize.google.com           |          |
| WEEK      | 201725 | lunametrics.com               |          |
| WEEK      | 201725 | it.search.yahoo.com           |          |
| WEEK      | 201725 | google.co.in                  |          |
| WEEK      | 201725 | m.baidu.com                   |          |
| WEEK      | 201725 | dealspotr.com                 |          |
| WEEK      | 201722 | lm.facebook.com               |          |
| WEEK      | 201722 | sashihara.jp                  |          |
| WEEK      | 201725 | googleads.g.doubleclick.net   |          |
| WEEK      | 201724 | adwords.google.com            |          |
| WEEK      | 201722 | search.tb.ask.com             |          |
| WEEK      | 201722 | l.messenger.com               |          |
| WEEK      | 201722 | away.vk.com                   |          |
| WEEK      | 201722 | msn.com                       |          |
| WEEK      | 201722 | google.co.uk                  |          |
| Month     | 201706 | google.fr                     |          |
| Month     | 201706 | googleads.g.doubleclick.net   |          |
| Month     | 201706 | github.com                    |          |
| Month     | 201706 | google.it                     |          |
| Month     | 201706 | away.vk.com                   |          |
| Month     | 201706 | malaysia.search.yahoo.com     |          |
| WEEK      | 201724 | sites.google.com              |          |
| WEEK      | 201724 | reddit.com                    |          |
| WEEK      | 201724 | analytics.google.com          |          |
| WEEK      | 201724 | optimize.google.com           |          |
| WEEK      | 201724 | facebook.com                  |          |
| WEEK      | 201724 | quora.com                     |          |
| WEEK      | 201724 | baidu                         |          |
| WEEK      | 201724 | m.facebook.com                |          |
| WEEK      | 201724 | plus.url.google.com           |          |
| WEEK      | 201724 | google.co.jp                  |          |
| WEEK      | 201723 | m.facebook.com                |          |
| WEEK      | 201723 | analytics.google.com          |          |
| WEEK      | 201723 | msn.com                       |          |
| WEEK      | 201723 | bing                          |          |
| WEEK      | 201723 | duckduckgo.com                |          |
| WEEK      | 201723 | sashihara.jp                  |          |
| WEEK      | 201723 | quora.com                     |          |
| WEEK      | 201723 | t.co                          |          |
| WEEK      | 201723 | desktop.google.com.ua         |          |
| WEEK      | 201723 | search.mysearch.com           |          |
| WEEK      | 201723 | reddit.com                    |          |
| WEEK      | 201723 | docs.google.com               |          |
| WEEK      | 201725 | analytics.google.com          |          |
| WEEK      | 201725 | bing                          |          |
| WEEK      | 201725 | baidu                         |          |
| WEEK      | 201725 | yahoo                         |          |
| WEEK      | 201725 | duckduckgo.com                |          |
| WEEK      | 201725 | Partners                      |          |
| WEEK      | 201725 | sashihara.jp                  |          |
| WEEK      | 201725 | google.co.jp                  |          |
| WEEK      | 201725 | dfa                           |          |
| WEEK      | 201725 | l.facebook.com                |          |
| WEEK      | 201725 | msn.com                       |          |
| WEEK      | 201725 | au.search.yahoo.com           |          |
| WEEK      | 201725 | youtube.com                   |          |
| WEEK      | 201726 | qiita.com                     |          |
| WEEK      | 201726 | l.facebook.com                |          |
| WEEK      | 201726 | docs.google.com               |          |
| WEEK      | 201726 | reddit.com                    |          |
| WEEK      | 201726 | mail.google.com               |          |
| WEEK      | 201726 | pinterest.com                 |          |
| WEEK      | 201726 | productforums.google.com      |          |
| WEEK      | 201726 | github.com                    |          |
| WEEK      | 201726 | google.fr                     |          |
| WEEK      | 201724 | es.search.yahoo.com           |          |
| WEEK      | 201724 | getpocket.com                 |          |
| WEEK      | 201724 | google.co.th                  |          |
| WEEK      | 201722 | google.com                    |          |
| WEEK      | 201722 | youtube.com                   |          |
| WEEK      | 201722 | bing                          |          |
| WEEK      | 201722 | search.mysearch.com           |          |
| WEEK      | 201722 | m.youtube.com                 |          |
| WEEK      | 201724 | gsuite.google.com             |          |
| WEEK      | 201724 | staging.talkgadget.google.com |          |
| WEEK      | 201723 | nl.search.yahoo.com           |          |
| WEEK      | 201723 | phandroid.com                 |          |
| WEEK      | 201723 | kidrex.org                    |          |
| WEEK      | 201723 | google.es                     |          |
| WEEK      | 201726 | google.co.uk                  |          |
| WEEK      | 201723 | google.nl                     |          |
| WEEK      | 201723 | in.search.yahoo.com           |          |
| WEEK      | 201723 | support.google.com            |          |
| WEEK      | 201723 | github.com                    |          |
| WEEK      | 201723 | search.xfinity.com            |          |
| WEEK      | 201723 | google.com.tw                 |          |
| WEEK      | 201726 | googleads.g.doubleclick.net   |          |
| WEEK      | 201726 | getiriver.com                 |          |
| WEEK      | 201726 | google.ca                     |          |
| WEEK      | 201724 | linkedin.com                  |          |
| WEEK      | 201723 | outlook.live.com              |          |
| WEEK      | 201723 | search.tb.ask.com             |          |
| WEEK      | 201726 | m.yz.sm.cn                    |          |
| WEEK      | 201726 | nl.search.yahoo.com           |          |
| WEEK      | 201724 | google.com.au                 |          |
| WEEK      | 201724 | lm.facebook.com               |          |
| WEEK      | 201725 | support.google.com            |          |
| WEEK      | 201725 | google.es                     |          |
| WEEK      | 201725 | search.xfinity.com            |          |
| WEEK      | 201722 | dealspotr.com                 |          |
| WEEK      | 201722 | tw.search.yahoo.com           |          |
| WEEK      | 201725 | l.messenger.com               |          |
| WEEK      | 201725 | google.de                     |          |
| WEEK      | 201722 | search.earthlink.net          |          |
| WEEK      | 201724 | plus.google.com               |          |
| WEEK      | 201725 | google.co.uk                  |          |
| WEEK      | 201725 | linkedin.com                  |          |
| Month     | 201706 | kidrex.org                    |          |
| Month     | 201706 | m.yz.sm.cn                    |          |
| Month     | 201706 | google.co.uk                  |          |
| Month     | 201706 | gsuite.google.com             |          |
| Month     | 201706 | search.tb.ask.com             |          |
| Month     | 201706 | adwords.google.com            |          |
| Month     | 201706 | google.com.au                 |          |
| Month     | 201706 | myactivity.google.com         |          |
| Month     | 201706 | int.search.tb.ask.com         |          |
| Month     | 201706 | plus.url.google.com           |          |
| Month     | 201706 | s0.2mdn.net                   |          |
| Month     | 201706 | google.ru                     |          |
| Month     | 201706 | google.com.tw                 |          |
| Month     | 201706 | search.mysearch.com           |          |
| Month     | 201706 | search.incredibar.com         |          |
| Month     | 201706 | google.nl                     |          |
| Month     | 201706 | reddit.com                    |          |
| Month     | 201706 | l.messenger.com               |          |
| Month     | 201706 | businessinsider.com           |          |
| Month     | 201706 | quora.com                     |          |
| Month     | 201706 | siliconvalley.about.com       |          |
| Month     | 201706 | productforums.google.com      |          |
| Month     | 201706 | m.facebook.com                |          |
| Month     | 201706 | Partners                      |          |
| Month     | 201706 | msn.com                       |          |
| Month     | 201706 | suche.t-online.de             |          |
| Month     | 201706 | desktop.google.com.ua         |          |
| Month     | 201706 | m.youtube.com                 |          |
| Month     | 201706 | mg.mail.yahoo.com             |          |
| Month     | 201706 | support.google.com            |          |
| Month     | 201706 | docs.google.com               |          |
| Month     | 201706 | lunametrics.com               |          |
| Month     | 201706 | gophergala.com                |          |
| Month     | 201706 | m.baidu.com                   |          |
| Month     | 201706 | linkedin.com                  |          |
| Month     | 201706 | search.earthlink.net          |          |
| Month     | 201706 | it.search.yahoo.com           |          |
| Month     | 201706 | connect.googleforwork.com     |          |
| Month     | 201706 | google.es                     |          |
| Month     | 201706 | getiriver.com                 |          |
| Month     | 201706 | online.fullsail.edu           |          |
| Month     | 201706 | sashihara.jp                  |          |
| Month     | 201706 | centrum.cz                    |          |
| Month     | 201706 | analytics.google.com          |          |
| Month     | 201706 | qiita.com                     |          |
| Month     | 201706 | google.co.jp                  |          |
| Month     | 201706 | facebook.com                  |          |
| Month     | 201706 | aol                           |          |
| Month     | 201706 | au.search.yahoo.com           |          |
| Month     | 201706 | getpocket.com                 |          |
| Month     | 201706 | google.de                     |          |
| Month     | 201706 | google.com.ua                 |          |
| Month     | 201706 | keep.google.com               |          |
| WEEK      | 201724 | mg.mail.yahoo.com             |          |
| WEEK      | 201724 | google.com                    |          |
| WEEK      | 201724 | blog.golang.org               |          |
| WEEK      | 201724 | productforums.google.com      |          |
| WEEK      | 201724 | int.search.tb.ask.com         |          |
| WEEK      | 201724 | duckduckgo.com                |          |
| WEEK      | 201724 | support.google.com            |          |
| WEEK      | 201723 | yahoo                         |          |
| WEEK      | 201723 | Partners                      |          |
| WEEK      | 201723 | plus.google.com               |          |
| WEEK      | 201723 | fr.search.yahoo.com           |          |
| WEEK      | 201723 | google.co.jp                  |          |
| WEEK      | 201723 | googleads.g.doubleclick.net   |          |
| WEEK      | 201723 | qiita.com                     |          |
| WEEK      | 201723 | tw.search.yahoo.com           |          |
| WEEK      | 201723 | int.search.tb.ask.com         |          |
| WEEK      | 201725 | reddit.com                    |          |
| WEEK      | 201725 | quora.com                     |          |
| WEEK      | 201725 | connect.googleforwork.com     |          |
| WEEK      | 201725 | qiita.com                     |          |
| WEEK      | 201725 | blog.golang.org               |          |
| WEEK      | 201725 | m.facebook.com                |          |
| WEEK      | 201725 | docs.google.com               |          |
| WEEK      | 201726 | malaysia.search.yahoo.com     |          |
| WEEK      | 201726 | int.search.tb.ask.com         |          |
| WEEK      | 201726 | baidu                         |          |
| WEEK      | 201726 | duckduckgo.com                |          |
| WEEK      | 201726 | Partners                      |          |
| WEEK      | 201726 | google.com                    |          |
| WEEK      | 201726 | datastudio.google.com         |          |
| WEEK      | 201726 | blog.golang.org               |          |
| WEEK      | 201726 | facebook.com                  |          |
| WEEK      | 201726 | ask                           |          |
| WEEK      | 201726 | youtube.com                   |          |
| WEEK      | 201726 | l.messenger.com               |          |
| WEEK      | 201726 | search.mysearch.com           |          |
| WEEK      | 201726 | sashihara.jp                  |          |
| WEEK      | 201726 | m.baidu.com                   |          |
| WEEK      | 201726 | adwords.google.com            |          |
| WEEK      | 201724 | google.co.uk                  |          |
| WEEK      | 201724 | googleads.g.doubleclick.net   |          |
| WEEK      | 201724 | hangouts.google.com           |          |
| WEEK      | 201724 | google.ru                     |          |
| WEEK      | 201724 | lunametrics.com               |          |
| WEEK      | 201722 | duckduckgo.com                |          |
| WEEK      | 201722 | google.co.jp                  |          |
| WEEK      | 201722 | optimize.google.com           |          |
| WEEK      | 201722 | quora.com                     |          |
| WEEK      | 201722 | blog.golang.org               |          |
| WEEK      | 201722 | l.facebook.com                |          |
| WEEK      | 201722 | facebook.com                  |          |
| WEEK      | 201722 | productforums.google.com      |          |
| WEEK      | 201722 | baidu                         |          |
| WEEK      | 201722 | support.google.com            |          |
| WEEK      | 201722 | t.co                          |          |
| WEEK      | 201722 | m.facebook.com                |          |
| WEEK      | 201722 | plus.google.com               |          |
| WEEK      | 201722 | search.myway.com              |          |
| WEEK      | 201723 | sg.search.yahoo.com           |          |
| WEEK      | 201723 | dealspotr.com                 |          |
| WEEK      | 201723 | optimize.google.com           |          |
| WEEK      | 201723 | google.co.in                  |          |
| WEEK      | 201725 | google.com.pe                 |          |
| WEEK      | 201725 | facebook.com                  |          |
| WEEK      | 201723 | google.co.uk                  |          |
| WEEK      | 201723 | l.messenger.com               |          |
| WEEK      | 201723 | blog.golang.org               |          |
| WEEK      | 201723 | gophergala.com                |          |
| WEEK      | 201726 | msn.com                       |          |
| WEEK      | 201726 | centrum.cz                    |          |
| WEEK      | 201726 | away.vk.com                   |          |
| WEEK      | 201723 | online.fullsail.edu           |          |
| WEEK      | 201723 | es.search.yahoo.com           |          |
| WEEK      | 201723 | myactivity.google.com         |          |
| WEEK      | 201723 | getpocket.com                 |          |
| WEEK      | 201723 | aol                           |          |
| WEEK      | 201726 | support.google.com            |          |
| WEEK      | 201726 | lunametrics.com               |          |
| WEEK      | 201726 | google.com.ua                 |          |
| WEEK      | 201726 | google.it                     |          |
| WEEK      | 201724 | github.com                    |          |
| WEEK      | 201726 | keep.google.com               |          |
| WEEK      | 201724 | s0.2mdn.net                   |          |
| WEEK      | 201725 | search.tb.ask.com             |          |
| WEEK      | 201725 | google.ca                     |          |
| WEEK      | 201725 | m.youtube.com                 |          |
| WEEK      | 201725 | google.co.th                  |          |
| WEEK      | 201722 | phandroid.com                 |          |
| WEEK      | 201722 | int.search.tb.ask.com         |          |
| WEEK      | 201725 | online-metrics.com            |          |
| WEEK      | 201722 | google.es                     |          |
| WEEK      | 201722 | desktop.google.com.ua         |          |
| WEEK      | 201722 | chat.google.com               |          |
| WEEK      | 201722 | meetup.com                    |          |
| WEEK      | 201725 | google.com.au                 |          |
| WEEK      | 201725 | int.search.tb.ask.com         |          |
| Month     | 201706 | nl.search.yahoo.com           |          |
| Month     | 201706 | t.co                          |          |
| Month     | 201706 | online-metrics.com            |          |
| Month     | 201706 | staging.talkgadget.google.com |          |
| Month     | 201706 | lm.facebook.com               |          |
| Month     | 201706 | admin.globalaccess.com        |          |
| Month     | 201706 | blog.golang.org               |          |
| Month     | 201706 | pinterest.com                 |          |
| Month     | 201706 | hangouts.google.com           |          |
| Month     | 201706 | google.ca                     |          |
| Month     | 201706 | ask                           |          |
| Month     | 201706 | fr.search.yahoo.com           |          |
| Month     | 201706 | int.search.mywebsearch.com    |          |
| Month     | 201706 | search.xfinity.com            |          |
| Month     | 201706 | sg.search.yahoo.com           |          |
| Month     | 201706 | in.search.yahoo.com           |          |
| Month     | 201706 | duckduckgo.com                |          |
| Month     | 201706 | tw.search.yahoo.com           |          |
| Month     | 201706 | optimize.google.com           |          |
| Month     | 201706 | outlook.live.com              |          |
| Month     | 201706 | google.com.pe                 |          |
| Month     | 201706 | google.co.in                  |          |
| Month     | 201706 | baidu                         |          |
| Month     | 201706 | es.search.yahoo.com           |          |
| Month     | 201706 | datastudio.google.com         |          |
| Month     | 201706 | google.co.th                  |          |
| Month     | 201706 | meetup.com                    |          |
| Month     | 201706 | plus.google.com               |          |

This table represents revenue data collection with various attributes, including time type, time period, source, and revenue amount. Each row corresponds to a specific time period (week or month) and provides information about the revenue generated from different sources during that time period. The author found the insights bellow through the table above.

 **Direct Revenue and Source Breakdown:** The dataset includes revenue from various sources, such as (direct), Google, DFA, mail.google.com, and search engines like myway.com. Revenue comes from different sources, including direct website visits, organic search traffic, and referrals from various websites.
 
**Time Period Analysis:**
Revenue is reported on a weekly and monthly basis. It seems like the data spans multiple months, including the month labeled "201706."

**6.4 Average number of pageviews by purchaser type**
~~~~sql
WITH GET_AVG_6_MONTH AS (SELECT
CASE WHEN 1 = 1 THEN "201706" END AS Month,
SUM(CASE WHEN totals.transactions >=1 THEN totals.transactions END ) AS total_transactions,
COUNT(DISTINCT(CASE WHEN totals.transactions >=1 THEN fullVisitorId END )) AS NUM_USER
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`)

SELECT 
Month,
ROUND(total_transactions/NUM_USER,2) as Avg_total_transactions_per_user
FROM GET_AVG_6_MONTH;
~~~~
| month  | avg_pageviews_purchase | avg_pageviews_non_purchase |
|--------|------------------------|----------------------------|
| 201706 | 25.73            | 4.07               |
| 201707 | 27.72            | 4.19               |


The table shows user behavior in a tabular format with two columns  "avg_pageviews_purchase," and "avg_pageviews_non_purchase"  in two consecutive months, June 2017 (201706) and July 2017 (201707).

 **Pageviews for Users Making a Purchase vs. Non-Purchasers:** The data highlights a significant difference in average pageviews between users who made a purchase and those who did not. On average, users who made a purchase tend to view more pages on the website than those who did not. In June 2017, for instance, the average pageviews for purchasers (25.74) were much higher compared to non-purchasers (4.07).
 
 **Engagement Patterns:** The higher average pageviews for purchasers suggest that users who are more engaged with the website's content are more likely to make a purchase. This could indicate a positive correlation between engagement and conversion.
 
 **Behavioral Insights:** The data reflect different user behaviors. Users interested in making a purchase might spend more time exploring product details, reading reviews, and comparing options, leading to a higher number of pageviews.
 
 **Conversion Optimization:** The business can leverage this data to optimize its website's design and content to encourage higher engagement among users likely to purchase. This could involve improving navigation, showcasing relevant products, and providing valuable content to guide users toward conversion.

**6.5 Average number of transactions per user that purchased in July 2017**
~~~~sql
WITH GET_AVG_7_MONTH AS (SELECT
CASE WHEN 1 = 1 THEN "201707" END AS Month,
SUM(CASE WHEN totals.transactions >=1 THEN totals.transactions END ) AS total_transactions,
COUNT(DISTINCT(CASE WHEN totals.transactions >=1 THEN fullVisitorId END )) AS NUM_USER
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201706*`)

SELECT 
Month,
ROUND(total_transactions/NUM_USER,2) as Avg_total_transactions_per_user
FROM GET_AVG_7_MONTH;
~~~~
| Month  | Avg_total_transactions_per_user |
|--------|---------------------------------|
| 201707 | 1.11                   |

The table shows the average total transactions per user in July. 
This data suggests that, during July 2017, the typical user conducted about 1.11 transactions on average. This could be useful for understanding user behavior, tracking user engagement with your platform, or evaluating the effectiveness of marketing campaigns or promotions during that specific month.

**6.6 Average amount of money spent per session. Only include purchaser data in 2017**
~~~~sql

SELECT 
  FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS month,
  ROUND((SUM(product.productRevenue) / SUM(totals.visits))/1000000,2) AS Avg_revenue_by_user_per_visit
FROM 
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`, 
  UNNEST(hits) AS hits, 
  UNNEST(hits.product) AS product
WHERE 
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170731'
  AND product.productRevenue IS NOT NULL
  AND totals.transactions IS NOT NULL
GROUP BY month;

~~~~
| Month  | Avg_total_transactions_per_user |
|--------|---------------------------------|
| 201707 | 43.86                  |

The average total transactions per user for July 2017 is 43.86. This suggests that, on average, each user conducted approximately 44 transactions during that month. This could be an important metric for businesses to measure user engagement and activity.


**6.7 Other products purchased by customers who purchased product” Youtube Men’s Vintage Henley” in July 2017**
~~~~sql
WITH GET_CUS_ID AS (SELECT DISTINCT fullVisitorId as Henley_CUSTOMER_ID
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*`,
UNNEST(hits) AS hits,
UNNEST(hits.product) as product
WHERE product.v2ProductName = "YouTube Men's Vintage Henley"
AND product.productRevenue IS NOT NULL)

SELECT product.v2ProductName AS other_purchased_products,
       SUM(product.productQuantity) AS quantity
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_201707*` TAB_A 
RIGHT JOIN GET_CUS_ID
ON GET_CUS_ID.Henley_CUSTOMER_ID=TAB_A.fullVisitorId,
UNNEST(hits) AS hits,
UNNEST(hits.product) as product
WHERE TAB_A.fullVisitorId IN (SELECT * FROM GET_CUS_ID)
    AND product.v2ProductName <> "YouTube Men's Vintage Henley"
    AND product.productRevenue IS NOT NULL
GROUP BY product.v2ProductName
ORDER BY QUANTITY DESC;
~~~~
| other_purchased_products                                 | quantity |
|----------------------------------------------------------|----------|
| Google Sunglasses                                        | 20       |
| Google Womens Vintage Hero Tee Black                     | 7        |
| SPF-15 Slim & Slender Lip Balm                           | 6        |
| Google Womens Short Sleeve Hero Tee Red Heather          | 4        |
| YouTube Mens Fleece Hoodie Black                         | 3        |
| Google Mens Short Sleeve Badge Tee Charcoal              | 3        |
| YouTube Twill Cap                                        | 2        |
| Red Shine 15 oz Mug                                      | 2        |
| Google Doodle Decal                                      | 2        |
| Recycled Mouse Pad                                       | 2        |
| Google Mens Short Sleeve Hero Tee Charcoal               | 2        |
| Android Womens Fleece Hoodie                             | 2        |
| 22 oz YouTube Bottle Infuser                             | 2        |
| Android Mens Vintage Henley                              | 2        |
| Crunch Noise Dog Toy	                                   | 2        |
| Android Wool Heather Cap Heather/Black                   | 2        |
| Google Mens Vintage Badge Tee Black                      | 1        |
| Google Twill Cap                                         | 1        |
| Google Mens Long & Lean Tee Grey                         | 1        |
| Google Mens Long & Lean Tee Charcoal                     | 1        |
| Google Laptop and Cell Phone Stickers                    | 1        |
| Google Mens Bike Short Sleeve Tee Charcoal               | 1        |
| Google 5-Panel Cap                                       | 1        |
| Google Toddler Short Sleeve T-shirt Grey                 | 1        |
| Android Sticker Sheet Ultra Removable                    | 1        |
| YouTube Custom Decals                                    | 1        |
| Four Color Retractable Pen                               | 1        |
| Google Mens Long Sleeve Raglan Ocean Blue                | 1        |
| Google Mens Vintage Badge Tee White                      | 1        |
| Google Mens 100% Cotton Short Sleeve Hero Tee Red        | 1        |
| Android Mens Vintage Tank                                | 1        |
| Google Mens Performance Full Zip Jacket Black            | 1        |
| 26 oz Double Wall Insulated Bottle                       | 1        |
| Google Mens  Zip Hoodie                                  | 1        |
| YouTube Womens Short Sleeve Hero Tee Charcoal            | 1        |
| Google Mens Pullover Hoodie Grey                         | 1        |
| YouTube Mens Short Sleeve Hero Tee White                 | 1        |
| Android Mens Short Sleeve Hero Tee White                 | 1        |
| Android Mens Pep Rally Short Sleeve Tee Navy             | 1        |
| YouTube Mens Short Sleeve Hero Tee Black                 | 1        |
| Google Slim Utility Travel Bag                           | 1        |
| Android BTTF Moonshot Graphic Tee                        | 1        |
| Google Mens Airflow 1/4 Zip Pullover Black               | 1        |
| Google Womens Long Sleeve Tee Lavender                   | 1        |
| 8 pc Android Sticker Sheet                               | 1        |
| YouTube Hard Cover Journal                               | 1        |
| Android Mens Short Sleeve Hero Tee Heather               | 1        |
| YouTube Womens Short Sleeve Tri-blend Badge Tee Charcoal | 1        |
| Google Mens Performance 1/4 Zip Pullover Heather/Black   | 1        |
| YouTube Mens Long & Lean Tee Charcoal                    | 1        |
                                                                      

Overall, the data provides valuable insights into customer preferences, product popularity, and potential areas for marketing and merchandising strategies. Further analysis of historical data and integration with customer demographics could provide a more comprehensive understanding of these trends.
 
 **6.8 Calculate cohort map from product view to add_to_cart/number_product_view.**
 ~~~~sql
WITH addtocart AS
(
        SELECT
        FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) AS month
        ,COUNT(eCommerceAction.action_type) AS num_addtocart
        FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`   
                ,UNNEST (hits) AS hits
        WHERE _table_suffix BETWEEN '0101' AND '0331'
                AND eCommerceAction.action_type = '3'
        GROUP BY month 
)
    , productview AS
(
        SELECT
        FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) AS month
        ,COUNT(eCommerceAction.action_type) AS num_product_view
        FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`   
                ,UNNEST (hits) AS hits
        WHERE _table_suffix BETWEEN '0101' AND '0331'
                AND eCommerceAction.action_type = '2'
        GROUP BY month 
)
    , id_purchase_revenue AS -- this is the first step to inspect the purchase step
(
                SELECT
        FORMAT_DATE("%Y%m",PARSE_DATE("%Y%m%d",date)) AS month
        ,fullVisitorId
        ,eCommerceAction.action_type
        ,product.productRevenue -- notice that not every purchase step that an ID made that the revenue was recorded (maybe refund?).
        FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`   
                ,UNNEST (hits) AS hits
                ,UNNEST (hits.product) AS product -- productrevenue 
        WHERE _table_suffix BETWEEN '0101' AND '0331'
                AND eCommerceAction.action_type = '6'
)
    , purchase AS
(
        SELECT 
            month
            ,COUNT(action_type) AS num_purchase
        FROM id_purchase_revenue 
        WHERE productRevenue IS NOT NULL
        GROUP BY month
)
SELECT 
        month
        ,num_product_view
        ,num_addtocart
        ,num_purchase
        ,ROUND(num_addtocart / num_product_view * 100.0, 2) AS add_to_cart_rate
        ,ROUND(num_purchase / num_product_view * 100.0, 2) AS purchase_rate
FROM productview
JOIN addtocart
USING (month)
JOIN purchase
USING (month)
ORDER BY month;
~~~~
| month  | num_product_view | num_addtocart | num_purchase | add_to_cart_rate | purchase_rate |
|--------|------------------|---------------|--------------|------------------|---------------|
| 201701 | 25787            | 7342          | 4328         | 28.47            | 16.78         |
| 201702 | 21489            | 7360          | 4141         | 34.25            | 19.27         |
| 201703 | 23549            | 8782          | 6018         | 37.29            | 25.56         |


The table illustrates five various metrics and rates related to user behavior from January to March 2017.

Overall, the number of product views from January 2017 to March 2017 increased gradually. The add-to-cart rate and purchase rate also increased over the same period, indicating improved user engagement and conversion.

However, the add-to-cart rate and purchase rate are notably higher in March 2017, suggesting potential improvements in the website's user experience or marketing efforts.


<div id='cau7'/>
  
## 7. Conclusion

- This is the author's opportunity to learn about the marketing industry and the customer journey through this e-commerce dataset

- In analyzing the e-commerce dataset using BigQuery, the author understands customer behavior through the bounce rate, transaction, revenue, visit, and purchase.
  
- The author gained insights into which marketing channels drive traffic and sales by examining referral sources. Investing resources in effective channels and optimizing underperforming ones can improve marketing ROI.

- In conclusion, exploring the e-commerce dataset on BigQuery unearthed a wealth of insights critical for strategic decision-making to help the business can optimize operations, enhance customer experiences, and drive revenue.
