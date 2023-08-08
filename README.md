### Dannys_Diner_Case_Study <html> <a href="https://8weeksqlchallenge.com/case-study-1/">Danny's Diner</a> </html>

Using data to answer questions about Danny's customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favorite       
Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:
   - sales
   - menu
   - members
#### Case study questions
Each of the following case study questions can be answered using a single SQL statement:
1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

#### Bonus Questions:
1. Create joined tables with membership status
2. Ranking member products

### Solutions
1. What is the total amount each customer spent at the restaurant?

| customer_id 	| Amount_spent 	|
|-------------	|--------------	|
| B           	| $74          	|
| A           	| $76          	|
| C           	| $36          	|
 
2. How many days has each customer visited the restaurant?

| customer_id 	| Days_visit 	|
|-------------	|------------	|
| A           	| 4          	|
| B           	| 6          	|
| C           	| 2          	|

3. What was the first item from the menu purchased by each customer?

| customer_id 	| first_purchased_item 	|
|-------------	|----------------------	|
| A           	| sushi                	|
| A           	| curry                	|
| B           	| curry                	|
| C           	| ramen                	|

4. What is the most purchased item on the menu and how many times was it purchased by all customers?

| product_name 	| total_purchases 	|
|--------------	|-----------------	|
| ramen        	| 8               	|

5. Which item was the most popular for each customer?
   
| customer_id 	| most_popular_item 	| purchase_count 	|
|-------------	|-------------------	|----------------	|
| A           	| ramen             	| 3              	|
| B           	| curry             	| 2              	|
| C           	| ramen             	| 3              	|

6. Which item was purchased first by the customer after they became a member?
| customer_id 	| first_purchased_item 	| first_purchase_date 	|
|-------------	|----------------------	|---------------------	|
| A           	| curry                	| 2021-01-07          	|
| B           	| sushi                	| 2021-01-11          	|

7. Which item was purchased just before the customer became a member?

| customer_id 	| purchase_before_membership 	| join_date  	| purchase_date 	|
|-------------	|----------------------------	|------------	|---------------	|
| B           	| sushi                      	| 2021-01-09 	| 2021-01-04    	|
| A           	| sushi                      	| 2021-01-07 	| 2021-01-01    	|
| A           	| curry                      	| 2021-01-07 	| 2021-01-01    	|


8. What is the total items and amount spent for each member before they became a member?

| customer_id 	| total_items_purchased 	| total_amount_spent 	|
|-------------	|-----------------------	|--------------------	|
| A           	| 2                     	| $25                	|
| B           	| 3                     	| $40                	|


9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

| customer_id 	| total_points 	|
|-------------	|--------------	|
| A           	| 860          	|
| B           	| 940          	|
| C           	| 360          	|

10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

| customer_id 	| total_points 	|
|-------------	|--------------	|
| A           	| 1520         	|
| B           	| 1360         	|

### Answers to the bonus questions.
1. Create joined tables with membership status

| customer_id 	| order_date 	| product_name 	| price 	| memberr 	|
|-------------	|------------	|--------------	|-------	|---------	|
| A           	| 2021-01-01 	| sushi        	| 10    	| N       	|
| A           	| 2021-01-01 	| curry        	| 15    	| N       	|
| A           	| 2021-01-07 	| curry        	| 15    	| Y       	|
| A           	| 2021-01-10 	| ramen        	| 12    	| Y       	|
| A           	| 2021-01-11 	| ramen        	| 12    	| Y       	|
| A           	| 2021-01-11 	| ramen        	| 12    	| Y       	|
| B           	| 2021-01-01 	| curry        	| 15    	| N       	|
| B           	| 2021-01-02 	| curry        	| 15    	| N       	|
| B           	| 2021-01-04 	| sushi        	| 10    	| N       	|
| B           	| 2021-01-11 	| sushi        	| 10    	| Y       	|
| B           	| 2021-01-16 	| ramen        	| 12    	| Y       	|
| B           	| 2021-02-01 	| ramen        	| 12    	| Y       	|
| C           	| 2021-01-01 	| ramen        	| 12    	| N       	|
| C           	| 2021-01-01 	| ramen        	| 12    	| N       	|
| C           	| 2021-01-07 	| ramen        	| 12    	| N       	|

2. Ranking member products
   
| customer_id 	| order_date 	| product_name 	| price 	| memberr 	| ranking 	|
|-------------	|------------	|--------------	|-------	|---------	|---------	|
| A           	| 2021-01-01 	| sushi        	| 10    	| N       	| null    	|
| A           	| 2021-01-01 	| curry        	| 15    	| N       	| null    	|
| A           	| 2021-01-07 	| curry        	| 15    	| Y       	| 1       	|
| A           	| 2021-01-10 	| ramen        	| 12    	| Y       	| 2       	|
| A           	| 2021-01-11 	| ramen        	| 12    	| Y       	| 3       	|
| A           	| 2021-01-11 	| ramen        	| 12    	| Y       	| 3       	|
| B           	| 2021-01-01 	| curry        	| 15    	| N       	| null    	|
| B           	| 2021-01-02 	| curry        	| 15    	| N       	| null    	|
| B           	| 2021-01-04 	| sushi        	| 10    	| N       	| null    	|
| B           	| 2021-01-11 	| sushi        	| 10    	| Y       	| 1       	|
| B           	| 2021-01-16 	| ramen        	| 12    	| Y       	| 2       	|
| B           	| 2021-02-01 	| ramen        	| 12    	| Y       	| 3       	|
| C           	| 2021-01-01 	| ramen        	| 12    	| N       	| null    	|
| C           	| 2021-01-01 	| ramen        	| 12    	| N       	| null    	|
| C           	| 2021-01-07 	| ramen        	| 12    	| N       	| null    	|

#### Note;
- Data was gotten from   <a href="https://8weeksqlchallenge.com//">Danny's Diner's SQL Challenge</a> </html>
- It’s recommended to save all of your code in a separate IDE or text editor as you are trying to solve the problems in the provided SQL Fiddle instance.

  









