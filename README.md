# Stocks Analysis Using MySQL

## Project Overview

**Project Title**: Stocks Analysis  
**Level**: Intermediate  
**Database**: `mysql_project`

This project demonstrates SQL skills and techniques for exploring, analyzing, and gaining insights from historical stock market data. Using MySQL, this analysis includes setting up a stocks database, performing exploratory data analysis (EDA), and answering key business questions through SQL queries. This project is ideal for data enthusiasts aiming to build strong SQL skills through practical financial data analysis.

## Objectives

1. **Database Setup**: Created and populated a stock market database using historical S&P 500 data.
2. **Data Cleaning**: Identified and removed records with missing or null values to ensure data accuracy.
3. **Exploratory Data Analysis (EDA)**: Conducted initial data exploration to uncover trading patterns and stock performance insights.
4. **Business Insights**: Leveraged SQL queries to answer business-specific questions and generate actionable insights about trading volume, stock volatility, and historical returns.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `stocks_db`.
- **Table Creation**: A table named `stocks` is created to store daily trading data for S&P 500 companies. The table structure includes columns for ticker, trade date, volume, high, low, open, and close prices.

```sql
CREATE DATABASE stocks_db;

CREATE TABLE stocks (
    symbol VARCHAR(10),
    date DATE,
    volume BIGINT,
    high FLOAT,
    low FLOAT,
    open FLOAT,
    close FLOAT,
    PRIMARY KEY symbol
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Unique Tickers**: Find out how many unique stocks (tickers) are included.
- **Null Value Check**: Check for null values in the dataset and delete records with missing data to maintain quality.

```sql
SELECT COUNT(*) FROM stocks;
SELECT COUNT(DISTINCT ticker) FROM stocks;

SELECT * FROM stocks
WHERE volume IS NULL OR high IS NULL OR low IS NULL OR open IS NULL OR close IS NULL;

DELETE FROM stocks
WHERE volume IS NULL OR high IS NULL OR low IS NULL OR open IS NULL OR close IS NULL;
```

### 3. Data Analysis & Findings

1. **Which date saw the largest total trading volume across all S&P 500 companies, and which two stocks had the highest individual volumes on that day?**:
   ```sql
-- Find the date with the highest total trading volume
SELECT date, sum(volume) as Total_Volume
FROM stocks
GROUP BY date
ORDER BY SUM(volume) desc LIMIT 1;

-- Find the two stocks with the highest volumes on that date
SELECT symbol,volume
FROM stocks
WHERE date = '2015-08-24'
ORDER BY volume desc limit 2;
```

2. **Identify Which Day of the Week Has the Highest and Lowest Trading Volume**:
   ```sql
--week does trading volume tend to be the highest
SELECT dayname(date) as Day_Name, 
avg(volume) as Avg_Volume
FROM stocks
GROUP BY dayname(date)
ORDER BY avg(volume) desc limit 1;

--week does trading volume tend to be the lowest
SELECT dayname(date) as Day_Name, 
avg(volume) as Avg_Volume
FROM stocks
GROUP BY dayname(date)
ORDER BY avg(volume) asc limit 1;
```

3. **Identify Which Day of the Week Has the Highest and Lowest Trading Volume**:
```sql
   SELECT DAYNAME(trade_date) AS day_of_week, AVG(volume) AS avg_volume
   FROM stocks
   GROUP BY day_of_week
   ORDER BY avg_volume DESC;
```

4. **Find the Day with the Highest Volatility for Amazon (AMZN)**:
   ```sql
   SELECT trade_date, (high - low) AS volatility
   FROM stocks
   WHERE ticker = 'AMZN'
   ORDER BY volatility DESC
   LIMIT 1;
   ```

5. **Calculate the Stock with the Highest Percentage Gain from 2014 to 2017**:
   ```sql
   WITH price_changes AS (
       SELECT ticker,
              MAX(CASE WHEN trade_date = '2014-01-02' THEN open END) AS open_price,
              MAX(CASE WHEN trade_date = '2017-12-29' THEN close END) AS close_price
       FROM stocks
       GROUP BY ticker
   )
   SELECT ticker, ((close_price - open_price) / open_price) * 100 AS percentage_gain
   FROM price_changes
   ORDER BY percentage_gain DESC
   LIMIT 1;
   ```

6. **Identify the Five Most Volatile Stocks by Average Daily Volatility**:
   ```sql
   SELECT ticker, AVG(high - low) AS avg_volatility
   FROM stocks
   GROUP BY ticker
   ORDER BY avg_volatility DESC
   LIMIT 5;
   ```

7. **Calculate Monthly Trading Volume Patterns**:
   ```sql
   SELECT MONTHNAME(trade_date) AS month, AVG(volume) AS avg_volume
   FROM stocks
   GROUP BY month
   ORDER BY avg_volume DESC;
   ```

## Findings

- **Highest Trading Volume Date**: Identified the day with the largest trading volume and the top two traded stocks.
- **Weekly Volume Patterns**: Determined which day of the week generally has the highest and lowest trading volume.
- **Amazon's Volatility**: Found the date of highest volatility for Amazon (AMZN), indicating significant price fluctuation.
- **Top Investment Opportunity**: Analyzed the stock with the highest percentage gain from 2014 to 2017, useful for retrospective investment insights.
- **Stock Volatility**: Identified the most volatile stocks over the given period, providing insights into risk levels.

## Reports

- **Trading Volume Summary**: Reports on highest-volume days, day-of-week patterns, and month-wise trends.
- **Stock Performance**: Analysis of stocks with the highest historical returns and volatility.
- **Investment Insights**: A summary of stocks with potential for gains based on historical performance.

## Conclusion

This project serves as a comprehensive exploration of stock market data using SQL, covering database setup, data cleaning, exploratory analysis, and advanced business queries. The insights generated from this project can support investment strategies and provide valuable trading insights based on historical data.

Analyzed by: Hardik Thakur  
Date: 5, November 2024
