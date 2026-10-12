# Pizza-Sales-Analytics-Dashboard
An interactive **Power BI dashboard** built to analyze pizza sales,
product performance, and order patterns. The report uses a dark theme
with orange and yellow accents.

##  Project Overview

This project explores: - Which pizzas generate the most revenue - Which
pizzas sell the most units and which perform weakest - Revenue
contribution by pizza category and size - Peak ordering hours and
busiest days - Daily and monthly order trends

##  Tools Used

-   **Power BI** --- dashboard and data visualization
-   **DAX** --- KPI measures and calculations
-   **Power Query** --- data preparation and transformation
-   **PostgreSQL / SQL** --- querying and sales analysis

##  Dashboard Pages

### 1. Overview

-   Total Revenue
-   Average Order Value
-   Total Pizzas Sold
-   Total Orders
-   Average Pizzas per Order
-   Revenue and order trends
-   Revenue contribution by category and size

### 2. Product Performance

-   Top 10 pizzas by revenue, quantity sold, and orders
-   Bottom 5 pizzas by revenue, quantity sold, and orders
-   Revenue contribution by category and size
-   Average selling price by pizza

### 3. Order Analysis

-   Hourly order trend
-   Orders by day of week
-   Monthly order trend
-   Orders by day and hour matrix
-   Peak order hour and peak order day

##  SQL Analysis

The SQL script creates the `pizza_data` table and includes queries for
sales KPIs, time trends, category/size contribution, and
top/bottom-performing pizzas.

### Table Schema

| Column | Description |
|---|---|
| `pizza_id` | Pizza row identifier |
| `order_id` | Order identifier |
| `pizza_name_id` | Pizza product code |
| `quantity` | Quantity sold |
| `order_date` | Date of order |
| `order_time` | Time of order |
| `unit_price` | Price per pizza |
| `total_price` | Total line value |
| `pizza_size` | Pizza size |
| `pizza_category` | Pizza category |
| `pizza_ingredients` | Ingredient details |
| `pizza_name` | Pizza name |

### SQL Queries

The full SQL queries are included below for easy viewing directly on
GitHub.

#### 1. Create Table

``` sql
CREATE TABLE pizza_data (
    pizza_id INT,
    order_id INT,
    pizza_name_id VARCHAR(50),
    quantity INT,
    order_date DATE,
    order_time TIME,
    unit_price DECIMAL(10,2),
    total_price DECIMAL(10,2),
    pizza_size VARCHAR(5),
    pizza_category VARCHAR(20),
    pizza_ingredients VARCHAR(255),
    pizza_name VARCHAR(50)
);
```

#### 2. Total Revenue

``` sql
SELECT SUM(total_price) AS total_revenue
FROM pizza_data;
```

#### 3. Average Order Value

``` sql
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS avg_order_value
FROM pizza_data;
```

#### 4. Total Pizzas Sold

``` sql
SELECT SUM(quantity) AS total_pizza_sold
FROM pizza_data;
```

#### 5. Total Orders

``` sql
SELECT COUNT(DISTINCT order_id) AS total_orders
FROM pizza_data;
```

#### 6. Average Pizzas per Order

``` sql
SELECT CAST(
    CAST(SUM(quantity) AS DECIMAL(10,2)) /
    CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2))
    AS DECIMAL(10,2)
) AS avg_pizza_per_order
FROM pizza_data;
```

#### 7. Daily Trend for Total Orders

``` sql
SELECT TO_CHAR(order_date, 'DAY') AS order_day,
       COUNT(DISTINCT order_id) AS total_orders
FROM pizza_data
GROUP BY order_day;
```

#### 8. Monthly Trend for Orders

``` sql
SELECT TO_CHAR(order_date, 'MONTH') AS order_month,
       COUNT(DISTINCT order_id) AS total_orders
FROM pizza_data
GROUP BY order_month;
```

#### 9. Percentage of Sales by Pizza Category

``` sql
SELECT pizza_category,
       CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
       CAST(
           SUM(total_price) * 100 /
           (SELECT SUM(total_price) FROM pizza_data)
           AS DECIMAL(10,2)
       ) AS pct
FROM pizza_data
GROUP BY pizza_category;
```

#### 10. Percentage of Sales by Pizza Size

``` sql
SELECT pizza_size,
       CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
       CAST(
           SUM(total_price) * 100 /
           (SELECT SUM(total_price) FROM pizza_data)
           AS DECIMAL(10,2)
       ) AS pct
FROM pizza_data
GROUP BY pizza_size
ORDER BY pizza_size;
```

#### 11. Total Pizzas Sold by Category

``` sql
SELECT pizza_category,
       SUM(quantity) AS total_qty
FROM pizza_data
GROUP BY pizza_category
ORDER BY total_qty DESC;
```

#### 12. Top 5 Pizzas by Revenue

``` sql
SELECT pizza_name,
       SUM(total_price) AS total_revenue
FROM pizza_data
GROUP BY pizza_name
ORDER BY total_revenue DESC
LIMIT 5;
```

#### 13. Bottom 5 Pizzas by Revenue

``` sql
SELECT pizza_name,
       SUM(total_price) AS total_revenue
FROM pizza_data
GROUP BY pizza_name
ORDER BY total_revenue ASC
LIMIT 5;
```

#### 14. Top 5 Pizzas by Quantity

``` sql
SELECT pizza_name,
       SUM(quantity) AS total_qty
FROM pizza_data
GROUP BY pizza_name
ORDER BY total_qty DESC
LIMIT 5;
```

#### 15. Bottom 5 Pizzas by Quantity

``` sql
SELECT pizza_name,
       SUM(quantity) AS total_qty
FROM pizza_data
GROUP BY pizza_name
ORDER BY total_qty ASC
LIMIT 5;
```

#### 16. Top 5 Pizzas by Total Orders

``` sql
SELECT pizza_name,
       COUNT(DISTINCT order_id) AS total_order
FROM pizza_data
GROUP BY pizza_name
ORDER BY total_order DESC
LIMIT 5;
```

#### 17. Bottom 5 Pizzas by Total Orders

``` sql
SELECT pizza_name,
       COUNT(DISTINCT order_id) AS total_order
FROM pizza_data
GROUP BY pizza_name
ORDER BY total_order ASC
LIMIT 5;
```

##  How to Run

1.  Open `SQL QUERY.sql` in pgAdmin or another PostgreSQL client.
2.  Connect to the database you want to use.
3.  Run the `CREATE TABLE` statement if the table does not already
    exist.
4.  Import the pizza sales dataset into `pizza_data`.
5.  Run the analysis queries and compare their outputs with the Power BI
    dashboard.
6.  Open the `.pbix` file in Power BI Desktop to explore the interactive
    report.

##  Interactive Features

-   Slicers for order date, pizza category, pizza size, pizza name, and
    order day
-   Cross-filtering across charts
-   Navigation between report pages
-   Product rankings and time-based comparisons

##  Business Value

The dashboard can help identify high-performing products, review
weaker-selling pizzas, understand category and size contribution, and
recognize peak ordering periods for operational planning.

## 🖼️ Screenshots

Add exported report screenshots to an `images/` folder, for example:

``` text
images/
├── overview.png
├── product-performance.png
└── order-analysis.png
```

##  Suggested Repository Structure

``` text
pizza-sales-analytics/
├── pizza.pbix
├── SQL QUERY.sql
├── README.md
└── images/
    ├── overview.png
    ├── product-performance.png
    └── order-analysis.png
```

## Validation Notes

-   KPI values change with slicer selections.
-   Confirm that order counts use the intended distinct order
    identifier.
-   Keep the definition of **Average Pizzas per Order** consistent
    across report pages.
-   Validate displayed results against the underlying dataset before
    making business decisions.
-   Do not publish confidential or restricted data in a public
    repository.

## Author

**Rohit Kumar**\
*Data Analytics Portfolio*

------------------------------------------------------------------------

*Portfolio project demonstrating SQL analysis and interactive Power BI
reporting.*
