# SQL Queries – Mountain Logistics Analytics Dashboard

This document contains the SQL queries used to analyze the Mountain Logistics dataset before creating the interactive dashboard in Looker Studio.

Each query focuses on a different business question and provides insights into logistics performance, profitability, customer satisfaction, and operational efficiency.

---

# Query 1 – Total Revenue, Cost, Profit & Profit Margin

### Objective
Calculate the overall business performance by finding total revenue, total cost, total profit, and average profit margin.

```sql
SELECT
    ROUND(SUM(revenue),2) AS Total_Revenue,
    ROUND(SUM(total_cost),2) AS Total_Cost,
    ROUND(SUM(profit),2) AS Total_Profit,
    ROUND(AVG(profit_margin) * 100,2) AS Avg_Margin_Pct
FROM logistics_data;
```

### Insight
Provides high-level financial KPIs used in the Executive Dashboard.

---

# Query 2 – Delivery Status Distribution

### Objective
Count shipments based on delivery status.

```sql
SELECT
    delivery_status,
    COUNT(*) AS Total_Shipments
FROM logistics_data
GROUP BY delivery_status;
```

### Insight
Used to visualize On-Time vs Delayed deliveries.

---

# Query 3 – Profit by Mountain Region

### Objective
Analyze profitability across different mountain regions.

```sql
SELECT
    mountain_region,
    ROUND(SUM(profit),2) AS Total_Profit,
    ROUND(SUM(revenue),2) AS Total_Revenue,
    ROUND(AVG(profit_margin) * 100,2) AS Avg_Profit_Margin
FROM logistics_data
GROUP BY mountain_region
ORDER BY Total_Profit DESC;
```

### Insight
Identifies the highest and lowest performing regions.

---

# Query 4 – Company Performance Scorecard

### Objective
Evaluate logistics companies based on shipments, profit, customer satisfaction, and average delay.

```sql
SELECT
    company_name,
    COUNT(*) AS Shipments,
    ROUND(SUM(profit),2) AS Total_Profit,
    ROUND(AVG(customer_satisfaction),2) AS Satisfaction,
    ROUND(AVG(delay_hours),2) AS Avg_Delay
FROM logistics_data
GROUP BY company_name
ORDER BY Total_Profit DESC;
```

### Insight
Ranks logistics companies by operational performance.

---

# Query 5 – Weather Impact Analysis

### Objective
Measure how weather conditions affect logistics performance.

```sql
SELECT
    weather_condition,
    COUNT(*) AS Shipments,
    ROUND(AVG(delay_hours),2) AS Avg_Delay,
    ROUND(AVG(profit),2) AS Avg_Profit,
    ROUND(AVG(customer_satisfaction),2) AS Satisfaction
FROM logistics_data
GROUP BY weather_condition
ORDER BY Avg_Delay DESC;
```

### Insight
Shows which weather conditions create the highest delivery delays.

---

# Query 6 – Top Performing Regions

### Objective
Compare revenue, profit, customer satisfaction, and delays across regions.

```sql
SELECT
    mountain_region,
    ROUND(SUM(revenue),2) AS Revenue,
    ROUND(SUM(profit),2) AS Profit,
    ROUND(AVG(customer_satisfaction),2) AS Satisfaction,
    ROUND(AVG(delay_hours),2) AS Avg_Delay
FROM logistics_data
GROUP BY mountain_region
ORDER BY Profit DESC;
```

### Insight
Highlights regions generating the strongest financial performance.

---

# Query 7 – Logistics Upgrade ROI Analysis

### Objective
Compare upgraded and non-upgraded logistics operations.

```sql
SELECT
    logistics_upgrade,
    COUNT(*) AS Shipments,
    ROUND(AVG(profit),2) AS Avg_Profit,
    ROUND(AVG(customer_satisfaction),2) AS Satisfaction,
    ROUND(AVG(delay_hours),2) AS Avg_Delay
FROM logistics_data
GROUP BY logistics_upgrade
ORDER BY Avg_Profit DESC;
```

### Insight
Measures the business impact of logistics infrastructure upgrades.

---

# Query 8 – Risk Score Analysis

### Objective
Understand the relationship between operational risk and business performance.

```sql
SELECT
    risk_score,
    COUNT(*) AS Shipments,
    ROUND(AVG(profit),2) AS Avg_Profit,
    ROUND(AVG(delay_hours),2) AS Avg_Delay
FROM logistics_data
GROUP BY risk_score
ORDER BY risk_score;
```

### Insight
Supports the Risk vs Profit bubble chart.

---

# Query 9 – Regional Profit Ranking

### Objective
Rank mountain regions based on total profit.

```sql
SELECT
    mountain_region,
    SUM(profit) AS Total_Profit,
    RANK() OVER(
        ORDER BY SUM(profit) DESC
    ) AS Profit_Rank
FROM logistics_data
GROUP BY mountain_region;
```

### Insight
Assigns a ranking to every region according to profitability.

---

# Query 10 – Profit Contribution by Region

### Objective
Calculate each region's contribution to total company profit.

```sql
SELECT
    mountain_region,
    ROUND(SUM(profit),2) AS Total_Profit,
    ROUND(
        SUM(profit) * 100 /
        (SELECT SUM(profit) FROM logistics_data),
        2
    ) AS Contribution_Percentage
FROM logistics_data
GROUP BY mountain_region
ORDER BY Contribution_Percentage DESC;
```

### Insight
Shows which regions contribute the largest share of total profit.

---

# Summary

These SQL queries were used to prepare the dataset before building the interactive Looker Studio dashboard.

The analysis focuses on:

- Revenue and profitability
- Regional performance
- Delivery efficiency
- Weather impact
- Customer satisfaction
- Operational risk
- Logistics upgrade effectiveness
- Company benchmarking
- Profit contribution analysis

The processed results were then connected to Looker Studio for interactive visualization and filtering.
