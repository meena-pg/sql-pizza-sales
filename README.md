# sql-pizza-sales

### Question 1

**Problem:-**  Retrieve the total number of orders placed.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Retrieve the total number of orders placed.
SELECT 
    COUNT(order_id) AS total_orders
FROM
    orders;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| total_orders |
| ------------| 
| 21350       | 



</details>

---

### Question 2

**Problem:-** Calculate the total revenue generated from pizza sales.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Calculate the total revenue generated from pizza sales.
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_sales
FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| total_sales | 
| -------- | 
| 817860.05    | 



</details>

---

### Question 3

**Problem:=** Identify the highest-priced pizza.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Identify the highest-priced pizza.
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;

```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>


|name           |price|
|---------------|------|
|The Greek Pizza| 35.95|

</details>

---

### Question 4

**Problem:-** Identify the most common pizza size ordered.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Identify the most common pizza size ordered.

SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| size | order_count |
| ---- | ---- |
|L | 18526|
|M | 15385|
|S  | 14137|
|XL |544|
|XXL | 28|


</details>

---

### Question 5

**Problem:-** List the top 5 most ordered pizza types along with their quantities.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- List the top 5 most ordered pizza types along with their quantities.

SELECT 
    pizza_types.name, SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| name | quantity |
| --- | --- |
|The Classic Deluxe Pizza|	2453| 
|The Barbecue Chicken Pizza	|2432|
|The Hawaiian Pizza |	2422|
|The Pepperoni Pizza	| 2418|
|The Thai Chicken Pizza	| 2371|

</details>

---

### Question 6

**Problem:-** Join the necessary tables to find the total quantity of each pizza category ordered.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Join the necessary tables to find the total quantity of each pizza category ordered.
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY category
ORDER BY quantity DESC;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| category | quantity |
| --- | --- |
|Classic |	14888|
|Supreme	|11987|
|Veggie	|11649|
|Chicken	|11050|

</details>

---

### Question 7

**Problem:-** Determine the distribution of orders by hour of the day.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Determine the distribution of orders by hour of the day.

SELECT 
    HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM
    orders
GROUP BY hour;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| hour | order_count |
| --- | --- |
|11	|1231|
|12	|2520|
|13|	2455|
|14	|1472|
|15	|1468|
|16	|1920|
|17	|2336|
|18	|2399|
|19	|2009|
|20	|1642|
|21	|1198|
|22	|663|
|23	|28|
|10	|8|
|9	|1|

</details>

---

### Question 8

**Problem:-** Join relevant tables to find the category-wise distribution of pizzas.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Join relevant tables to find the category-wise distribution of pizzas.
SELECT 
    category, COUNT(name)
FROM
    pizza_types
GROUP BY category;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| category | COUNT | 
| --- | --- |
|Chicken	|6|
|Classic|	8|
|Supreme	|9|
|Veggie	|9|

</details>

---

### Question 9

**Problem:-** Group the orders by date and calculate the average number of pizzas ordered per day.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Group the orders by date and calculate the average number of pizzas ordered per day.

SELECT 
    ROUND(AVG(quantity), 0) as avg_pizza_ordered_per_day
FROM
    (SELECT 
        orders.order_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date) AS order_quantity;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| avg_pizza_ordered_per_day|
| --- |
|138|


</details>

---

### Question 10

**Problem:-** Determine the top 3 most ordered pizza types based on revenue.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Determine the top 3 most ordered pizza types based on revenue.
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

|name | revenue |
| --- | --- |
|The Thai Chicken Pizza| 43434.25|
|The Barbecue Chicken Pizza| 42768|
|The California Chicken Pizza| 41409.5|

</details>

---

### Question 11

**Problem:-** Calculate the percentage contribution of each pizza type to total revenue.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Calculate the percentage contribution of each pizza type to total revenue.
select pizza_types.category,
round(sum(order_details.quantity*pizzas.price)/ (select
round(sum(order_details.quantity*pizzas.price),2) as total_sales
from order_details join pizzas on pizzas.pizza_id=order_details.pizza_id)*100,2)as revenue
from pizza_types join pizzas
on pizza_types.pizza_type_id=pizzas.pizza_type_id
join order_details
on order_details.pizza_id=pizzas.pizza_id
group by pizza_types.category order by revenue desc;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| category | revenue |
| --- | --- |
|Classic| 26.91|
|Supreme| 25.46|
|Chicken| 23.96|
|Veggie|23.68|


</details>

---

### Question 12

**Problem:-** Analyze the cumulative revenue generated over time.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Analyze the cumulative revenue generated over time.
select order_date,
sum(revenue) over(order by order_date) as cum_revenue
from 
(select orders.order_date,
sum(order_details.quantity*pizzas.price) as revenue
from order_details join pizzas 
on order_details.pizza_id=pizzas.pizza_id
join orders
on orders.order_id=order_details.order_id
group by orders.order_date)as sales;
```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| order_date       | cum_revenue     |
|------------|-----------|
| 2015-01-01 | 2713.85   |
| 2015-01-02 | 5445.75   |
| 2015-01-03 | 8108.15   |
| 2015-01-04 | 9863.60   |
| 2015-01-05 | 11929.55  |
| 2015-01-06 | 14358.50  |
| 2015-01-07 | 16560.70  |
| 2015-01-08 | 19399.05  |
| 2015-01-09 | 21526.40  |
| 2015-01-10 | 23990.35  |

placed only few data fields


</details>

---

### Question 13

**Problem:-** Determine the top 3 most ordered pizza types based on revenue for each pizza category.

<details>
  <summary><strong>Click to expand: SQL Query</strong></summary>

```sql
-- Determine the top 3 most ordered pizza types based on revenue for each pizza category.

select name,revenue from
(select category,name,revenue,
rank() over(partition by category order by revenue desc)as rn
from
(select pizza_types.category,pizza_types.name,
sum((order_details.quantity)*pizzas.price) as revenue
from pizza_types join pizzas
on pizza_types.pizza_type_id=pizzas.pizza_type_id
join order_details
on order_details.pizza_id=pizzas.pizza_id
group by pizza_types.category,pizza_types.name) as a)as b
where rn<=3;

```

</details>

<details>
  <summary><strong>Click to expand: Results Table</strong></summary>

| name | revenue |
| --- | --- |
|The Thai Chicken Pizza	|43434.25|
|The Barbecue Chicken Pizza|	42768|
|The California Chicken Pizza	|41409.5|
|The Classic Deluxe Pizza	|38180.5|
|The Hawaiian Pizza	|32273.25|
|The Pepperoni Pizza|	30161.75|
|The Spicy Italian Pizza|	34831.25|
|The Italian Supreme Pizza	|33476.75|
|The Sicilian Pizza	|30940.5|
|The Four Cheese Pizza|	32265.70000000065|
|The Mexicana Pizza	|26780.75|
|The Five Cheese Pizza|	26066.5|

</details>

---
