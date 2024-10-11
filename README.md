App Trader Analysis
Welcome to the App Trader project! This SQL-based analysis explores app market data from both the Apple App Store and Android Play Store. Our team was tasked with helping App Trader, a company that purchases app rights to market and offer in-app purchases. Using a combination of SQL queries and data manipulation, we analyzed app pricing, revenue potential, and investment returns to provide App Trader with actionable insights.

Project Overview
App Trader wanted to understand which apps were worth purchasing and how their pricing, availability, and user ratings impacted potential returns. The challenge lay in combining two separate datasets—one from the Apple App Store and the other from the Android Play Store—with differing metrics and no referential integrity.

Our team worked collaboratively, using key SQL techniques like UNION, FULL OUTER JOIN, and CASE statements to consolidate the data and generate meaningful insights. This project demonstrates a combination of leadership, problem-solving, and analytical skills—qualities often sharpened in complex team-based activities like Dungeons & Dragons.

Skills Applied
Leadership and Team Building
Just like a Dungeon Master organizes a party of adventurers, leading this project required coordinating efforts among teammates, assigning tasks, and uniting diverse skill sets to achieve the common goal. Each of us brought our strengths to the table, helping to motivate and inspire our group toward success.

Problem-Solving and Mediation
Merging datasets with differing structures and resolving discrepancies in pricing and metrics mirrored the challenges D&D players face when solving intricate puzzles or overcoming tactical hurdles. Like defusing conflicts in-game, we mediated between conflicting data types, transforming obstacles into opportunities for creative problem-solving.

Data Visualization and Experimentation
Visualizing data was akin to conjuring up epic battle scenes—a blend of experimentation and planning to map out complex scenarios. By analyzing trends and generating insights, we developed strategies to guide App Trader in its app acquisition efforts.

Key Queries and Solutions
1. Combining App Store Data
We began by combining the Apple App Store and Play Store data into a single dataset. Using FULL OUTER JOIN, we created a unified view of apps available in one or both stores. The highest price between the two stores was used for valuation purposes.

sql
Copy code
WITH CombinedApps AS (
    SELECT
        COALESCE(a.name, p.name) AS name,
        GREATEST(CAST(a.price AS NUMERIC), CAST(REPLACE(p.price, '$', '') AS NUMERIC)) AS max_price
    FROM app_store_apps AS a
    FULL OUTER JOIN play_store_apps AS p ON a.name = p.name
)
2. Calculating Purchase Price
Using the combined data, we calculated the purchase price based on app cost. Free apps or those priced under $1.00 were set at the minimum purchase price of $10,000. Higher-priced apps were scaled accordingly.

sql
Copy code
SELECT 
    name,
    max_price,
    CASE
        WHEN max_price <= 1 THEN 10000
        ELSE max_price * 10000
    END AS purchase_price
FROM CombinedApps
3. Monthly Revenue and Longevity
We calculated monthly revenue per app, distinguishing between apps available on both stores ($10,000 per month) and those on a single store ($5,000 per month). App longevity was based on ratings, with higher-rated apps projected to have longer lifespans.

sql
Copy code
SELECT 
    name,
    purchase_price,
    CASE
        WHEN availability = 'Both' THEN 10000
        ELSE 5000
    END AS revenue_per_month,
    ROUND(average_rating * 2) / 2 AS rounded_rating
FROM CombinedApps
4. Recouping Investment
We estimated how many quarters it would take to recoup App Trader’s investment in each app. This involved calculating the app’s monthly revenue minus marketing costs and dividing the purchase price by the net profit.

sql
Copy code
SELECT 
    name,
    purchase_price,
    CEIL((purchase_price / (revenue_per_month - marketing_cost_per_month)) / 3.0) AS quarters_to_recoup_investment
FROM RevenueCalc
ORDER BY quarters_to_recoup_investment, purchase_price DESC;
5. Top 10 Apps to Buy
We generated a Top 10 list of apps that App Trader should consider purchasing, based on a combination of price, revenue potential, and how quickly the investment could be recouped.

sql
Copy code
SELECT 
    name, 
    purchase_price, 
    revenue_per_month,
    quarters_to_recoup_investment
FROM InvestmentReturn
ORDER BY quarters_to_recoup_investment ASC
LIMIT 10;
Deliverables
General Recommendations:

Apps priced between $0.99 and $10 (labeled "affordable" or "niche") with higher ratings and availability on both stores tend to have the best return on investment.
Top 10 List:

We provided a ranked list of the top 10 apps App Trader should purchase, based on purchase price, revenue potential, and how quickly the investment can be recouped.
Report:

All analysis was done in PostgreSQL, and query results were exported for further visualization in Excel to create charts supporting the findings.
Technologies Used
PostgreSQL: Primary database for storing, querying, and analyzing app data.
Excel: Used for data visualization and reporting.
SQL: Queries to combine datasets, calculate revenue and costs, and project investment returns.
Conclusion
This project showcases our ability to integrate and analyze disparate datasets, applying strategic thinking to provide App Trader with actionable insights. Much like navigating complex in-game challenges in Dungeons & Dragons, the skills of leadership, problem-solving, and visualization played a crucial role in achieving success for App Trader.

