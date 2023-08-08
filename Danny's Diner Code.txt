--# Database creation
CREATE DATABASE dannys_diner;
USE dannys_diner;

--# Creation of sales table
CREATE TABLE sales (
  customer_id VARCHAR(1),
  order_date DATE,
  product_id INTEGER
);

INSERT INTO sales
  (customer_id, order_date, product_id)
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
 

 --# Creation of menu table
CREATE TABLE menu (
  product_id INTEGER,
  product_name VARCHAR(5),
  price INTEGER
);

INSERT INTO menu
  (product_id, product_name, price)
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
  

 --# Creation of members table
CREATE TABLE members (
  customer_id VARCHAR(1),
  join_date DATE
);

INSERT INTO members
  (customer_id, join_date)
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
  
 ---#case study
 
 ---#Question 1. What is the total amount each customer spent at the restaurant?
 
SELECT customer_id, CONCAT('$', SUM(price)) as Amount_spent
FROM menu
LEFT JOIN sales ON sales.product_id = menu.product_id
GROUP BY customer_id;
 
 ---#Question 2. How many days has each customer visited the restaurant?
 SELECT customer_id, COUNT(DISTINCT order_date) AS Days_visit
FROM sales
GROUP BY customer_id;
 
 
---#Question 3. What was the first item from the menu purchased by each customer?
SELECT DISTINCT s.customer_id, m.product_name AS first_purchased_item
FROM sales s
JOIN menu m ON s.product_id = m.product_id
WHERE s.order_date = (
    SELECT MIN(order_date)
    FROM sales
    WHERE customer_id = s.customer_id
);

---#Question 4. What is the most purchased item on the menu and how many times was it purchased by all customers?

SELECT m.product_name, COUNT(*) AS total_purchases
FROM sales s
JOIN menu m ON s.product_id = m.product_id
GROUP BY m.product_name
ORDER BY total_purchases DESC
LIMIT 1;

---#Question 5. Which item was the most popular for each customer?
SELECT customer_id, product_name AS most_popular_item, purchase_count
FROM (
    SELECT s.customer_id, m.product_name, COUNT(*) AS purchase_count,
           ROW_NUMBER() OVER (PARTITION BY s.customer_id ORDER BY COUNT(*) DESC) AS item_rank
    FROM sales s
    JOIN menu m ON s.product_id = m.product_id
    GROUP BY s.customer_id, m.product_name
) ranked_items
WHERE item_rank = 1;

---#Question 6. Which item was purchased first by the customer after they became a member?
SELECT s.customer_id, m.product_name AS first_purchased_item, s.order_date AS first_purchase_date
FROM sales s
JOIN menu m ON s.product_id = m.product_id
WHERE s.order_date = (
    SELECT MIN(order_date)
    FROM sales
    WHERE customer_id = s.customer_id
      AND order_date >= (
          SELECT join_date
          FROM members m2 
          WHERE customer_id = s.customer_id
      )
);

---#Question 7. Which item was purchased just before the customer became a member?
SELECT s.customer_id,
       m.product_name AS purchased_before_membership,
       m2.join_date,
       s.order_date AS purchase_date
FROM sales s
JOIN menu m ON s.product_id = m.product_id
JOIN members m2  ON s.customer_id = m2.customer_id
WHERE s.order_date = (
    SELECT MAX(order_date)
    FROM sales
    WHERE customer_id = s.customer_id
      AND order_date < m2.join_date
);

---#Question 8.What is the total items and amount spent for each member before they became a member?

SELECT me.customer_id,
       COUNT(s.product_id) AS total_items_purchased,
       CONCAT('$',SUM(m.price)) AS total_amount_spent
FROM sales s
JOIN menu m ON s.product_id = m.product_id
JOIN members me  ON s.customer_id = me.customer_id
WHERE s.order_date < me.join_date
GROUP BY me.customer_id, me.join_date
order by me.customer_id ;

---#Question 9.If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
 
SELECT s.customer_id,
       SUM(CASE WHEN m.product_name  = 'sushi' THEN m.price  * 20 ELSE m.price  * 10 END) AS total_points
FROM sales s
JOIN menu m ON s.product_id = m.product_id
GROUP BY s.customer_id;


---#Question 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

SELECT s.customer_id,
       SUM(CASE 
               WHEN s.order_date <= DATE_ADD(me.join_date, INTERVAL 7 DAY) THEN m.price * 20 
               ELSE m.price  * 10 
           END) AS total_points
FROM sales s
LEFT JOIN members me ON me.customer_id = s.customer_id
LEFT JOIN menu m  ON m.product_id  = s.product_id
WHERE (s.customer_id = 'A' OR s.customer_id = 'B')
  AND s.order_date <= '2023-01-31'
GROUP BY s.customer_id;

---#Bonus Questions
---#1. The following questions are related creating basic data tables that Danny and his team can use to quickly derive insights without needing to join the underlying tables using SQL.

select s.customer_id, s.order_date, m.product_name,m.price,
(case when order_date < join_date OR join_date is null then 'N' else 'Y' end ) as memberr
from sales s 
LEFT JOIN members me ON me.customer_id = s.customer_id
LEFT JOIN menu m  ON m.product_id  = s.product_id
order by s.customer_id;

---#2. Danny also requires further information about the ranking of customer products, 
#but he purposely does not need the ranking for non-member purchases so he expects null ranking values for the records when customers are not yet part of the loyalty program.

WITH memeber_status AS (
		SELECT s.customer_id
		   , s.order_date
		   , m.product_name
		   , m.price
		   , CASE 
				WHEN order_date < join_date OR join_date is null
					THEN 'N'
				ELSE 'Y'END as memberr
		FROM sales s
		LEFT JOIN members me ON me.customer_id = s.customer_id
        LEFT JOIN menu m  ON m.product_id  = s.product_id
		ORDER BY 1, 2
		)

SELECT *
       , CASE
	   WHEN memberr = 'N'
	     THEN null
	   ELSE DENSE_RANK() OVER (PARTITION BY customer_id, memberr ORDER BY order_date)							 
	 END ranking
FROM memeber_status;
