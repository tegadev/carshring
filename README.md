https://docs.google.com/spreadsheets/d/14AfMX2mHjXuRMobyH_9ipvQFHQUvnvaa4Iye56vrtgM/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/1qK3X3xEPuyuuu26rLHaYal32YUKLg35_ogvnTqpqihQ/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/1xs5iDR_txs_9GcbNXQLY6wVJLmins_a78yE4R8lGoeI/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/11xCiaZKFkS9kGmk0hV1PS4qdinBKO9-o3jznnnZnPXg/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/1mIzWSvIBPzxPMPCImiJRFS4dFlqAHxQV8Z2k99VdSmg/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/1GilOeXkYE5YvUEXIZUHJNbxEJGV0-bZaZpa0qPh2MFk/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/1lh4RXRbXpTagnLwm2elIwHwZi7-bYgbMbbeP2TuL9dQ/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/1H1f4TrUXJQRdqPxtMQ0zIcJWGEF1j7u1HKSDQuB4EaE/edit?usp=drive_link
https://docs.google.com/spreadsheets/d/1pkIWtYluyyLcdXVle1CbL_Mm-D4b3DNNXhzyPHWRo4Q/edit?usp=drive_link




(d) Please tell me what the weather was like in 2017. Was it mostly cold, mild, or hot? which weather condition (shown in the weather column) was the most prevalent in 2017? 

SELECT
    temp_category AS Temperature_Category,
    COUNT(*) AS Record_Count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percentage_of_Total
FROM siwes_project
GROUP BY temp_category
ORDER BY Record_Count DESC;


SELECT
    weather AS Weather_Condition,
    COUNT(*) AS Record_Count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percentage_of_Total
FROM siwes_project
GROUP BY weather
ORDER BY Record_Count DESC;






(b) Give me a table containing the name of the weekday, month, and season in which we had the highest and lowest average demand throughout 2017. Please include the calculated average demand values as wel


SELECT
    weekday_name AS Weekday,
    AVG(demand) AS Avg_Demand,
    COUNT(*) AS Row_Count
FROM siwes_project
GROUP BY weekday_name
ORDER BY Avg_Demand DESC;

-- 2. Average demand by Month
SELECT
    month_name AS Month,
    AVG(demand) AS Avg_Demand,
    COUNT(*) AS Row_Count
FROM siwes_project
GROUP BY month_name
ORDER BY Avg_Demand DESC;

-- 3. Average demand by Season
SELECT
    season AS Season,
    AVG(demand) AS Avg_Demand,
    COUNT(*) AS Row_Count
FROM siwes_project
GROUP BY season
ORDER BY Avg_Demand DESC;

-- 4. Combined query: Highest and Lowest average demand across Weekday, Month, and Season
WITH weekday_stats AS (
    SELECT
        'Weekday' AS Category,
        weekday_name AS Name,
        AVG(demand) AS Avg_Demand,
        RANK() OVER (ORDER BY AVG(demand) DESC) AS Rank_Highest,
        RANK() OVER (ORDER BY AVG(demand) ASC) AS Rank_Lowest
    FROM siwes_project
    GROUP BY weekday_name
),
month_stats AS (
    SELECT
        'Month' AS Category,
        month_name AS Name,
        AVG(demand) AS Avg_Demand,
        RANK() OVER (ORDER BY AVG(demand) DESC) AS Rank_Highest,
        RANK() OVER (ORDER BY AVG(demand) ASC) AS Rank_Lowest
    FROM siwes_project
    GROUP BY month_name
),
season_stats AS (
    SELECT
        'Season' AS Category,
        season AS Name,
        AVG(demand) AS Avg_Demand,
        RANK() OVER (ORDER BY AVG(demand) DESC) AS Rank_Highest,
        RANK() OVER (ORDER BY AVG(demand) ASC) AS Rank_Lowest
    FROM siwes_project
    GROUP BY season
)
SELECT
    Category,
    Name,
    ROUND(Avg_Demand, 4) AS Avg_Demand,
    CASE
        WHEN Rank_Highest = 1 THEN 'Highest'
        WHEN Rank_Lowest = 1 THEN 'Lowest'
        ELSE 'Other'
    END AS Metric
FROM (
    SELECT * FROM weekday_stats
    UNION ALL
    SELECT * FROM month_stats
    UNION ALL
    SELECT * FROM season_stats
) combined
WHERE Rank_Highest = 1 OR Rank_Lowest = 1
ORDER BY Category, Metric DESC;





(d3)

What was the average, highest, and lowest humidity for each month in 2017?


SELECT
    month_name AS Month,
    ROUND(AVG(humidity), 2) AS Avg_Humidity,
    MAX(humidity) AS Highest_Humidity,
    COUNT(*) AS Record_Count
FROM siwes_project
GROUP BY month_name
ORDER BY month_name;





(d2)

What was the average, highest, and lowest wind speed


SELECT
    temp_category AS Temperature_Category,
    COUNT(*) AS Record_Count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percentage
FROM siwes_project
GROUP BY temp_category
ORDER BY Record_Count DESC;

-- 2. Most prevalent weather condition
SELECT
    weather AS Weather_Condition,
    COUNT(*) AS Record_Count,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS Percentage
FROM siwes_project
GROUP BY weather
ORDER BY Record_Count DESC
LIMIT 5;

-- 3. Wind Speed Statistics by Month in 2017
SELECT
    month_name AS Month,
    ROUND(AVG(windspeed), 2) AS Avg_Wind_Speed,
    MAX(windspeed) AS Max_Wind_Speed,
    MIN(windspeed) AS Min_Wind_Speed,
    COUNT(*) AS Record_Count
FROM siwes_project
GROUP BY month_name
ORDER BY month_name;



















(d4)


table showing the average demand for each cold, mild, and hot weather in 2017 sorted in descending order based on their average demand.




SELECT
    temp_category AS Temperature_Category,
    ROUND(AVG(demand), 6) AS Avg_Demand,
    COUNT(*) AS Record_Count
FROM siwes_project
GROUP BY temp_category
ORDER BY Avg_Demand DESC;





(b) Give me a table containing the name of the weekday, month, and season in which we had the highest and lowest average demand throughout 2017. Please include the calculated average demand values as well.



SELECT
    weekday_name AS Weekday,
    AVG(demand) AS Avg_Demand,
    COUNT(*) AS Row_Count
FROM siwes_project
GROUP BY weekday_name
ORDER BY Avg_Demand DESC;

-- 2. Average demand by Month
SELECT
    month_name AS Month,
    AVG(demand) AS Avg_Demand,
    COUNT(*) AS Row_Count
FROM siwes_project
GROUP BY month_name
ORDER BY Avg_Demand DESC;

-- 3. Average demand by Season
SELECT
    season AS Season,
    AVG(demand) AS Avg_Demand,
    COUNT(*) AS Row_Count
FROM siwes_project
GROUP BY season
ORDER BY Avg_Demand DESC;

-- 4. Combined query: Highest and Lowest average demand across Weekday, Month, and Season
WITH weekday_stats AS (
    SELECT
        'Weekday' AS Category,
        weekday_name AS Name,
        AVG(demand) AS Avg_Demand,
        RANK() OVER (ORDER BY AVG(demand) DESC) AS Rank_Highest,
        RANK() OVER (ORDER BY AVG(demand) ASC) AS Rank_Lowest
    FROM siwes_project
    GROUP BY weekday_name
),
month_stats AS (
    SELECT
        'Month' AS Category,
        month_name AS Name,
        AVG(demand) AS Avg_Demand,
        RANK() OVER (ORDER BY AVG(demand) DESC) AS Rank_Highest,
        RANK() OVER (ORDER BY AVG(demand) ASC) AS Rank_Lowest
    FROM siwes_project
    GROUP BY month_name
),
season_stats AS (
    SELECT
        'Season' AS Category,
        season AS Name,
        AVG(demand) AS Avg_Demand,
        RANK() OVER (ORDER BY AVG(demand) DESC) AS Rank_Highest,
        RANK() OVER (ORDER BY AVG(demand) ASC) AS Rank_Lowest
    FROM siwes_project
    GROUP BY season
)
SELECT
    Category,
    Name,
    ROUND(Avg_Demand, 4) AS Avg_Demand,
    CASE
        WHEN Rank_Highest = 1 THEN 'Highest'
        WHEN Rank_Lowest = 1 THEN 'Lowest'
        ELSE 'Other'
    END AS Metric
FROM (
    SELECT * FROM weekday_stats
    UNION ALL
    SELECT * FROM month_stats
    UNION ALL
    SELECT * FROM season_stats
) combined
WHERE Rank_Highest = 1 OR Rank_Lowest = 1
ORDER BY Category, Metric DESC;




(e) Give me another table showing the information requested in (d) for the month we had the highest average demand in 2017 so that I can compare it with other months.


SELECT
    MONTHNAME(STR_TO_DATE(timestamp, '%d/%m/%Y %H:%i')) AS month_name,
    AVG(demand) AS avg_demand
FROM siwes_project
WHERE YEAR(STR_TO_DATE(timestamp, '%d/%m/%Y %H:%i')) = 2017
GROUP BY
    MONTH(STR_TO_DATE(timestamp, '%d/%m/%Y %H:%i')),
    MONTHNAME(STR_TO_DATE(timestamp, '%d/%m/%Y %H:%i'))
ORDER BY avg_demand DESC
LIMIT 1;





(a) Please tell me which date and time we had the highest demand rate in 2017.

SELECT timestamp, demand
FROM siwes_project
WHERE YEAR(STR_TO_DATE(timestamp, '%d/%m/%Y %H:%i')) = 2017
ORDER BY demand DESC
LIMIT 1;































