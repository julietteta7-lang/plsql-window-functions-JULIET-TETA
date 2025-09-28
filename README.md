# Business Analytics with PL/SQL

## Objective
This project shows how to use PL/SQL window functions to solve a real business problem.  
It uses easy examples to explain how ranking, adding totals, comparing months, grouping customers, and moving averages work.

## Business Context
An online make_up store sells make_up cosmetics and other beauty products to customers across different regions. The Marketing team wants to improve sales by understanding which products sell the best and which type of customers buy the most 

## Data Challenge
The store has many sales, but it is hard to find the best products that attract customers, see the sales growth each month or group customers by how much they spend and their most attractive market.

## Expected Outcome
Find the best products in each place, check how much of their products is sold every month, see how sales grow each month and group customers due to their spendings and identify the market attracted the most.

## Project Goals
1. **Top 5 Products per Region/Quarter** → Use `RANK()`
we use RANK() to find the top 5 products in each region and time so the store  knows what sells best. This helps the store make better decision, improve sales planning, understand customer preferences and focus on the most popular products to increase profit for the business and not just rely on what is not selling.

(For example we have foundation, lipstick and lip balm, when RANK() is applied this will help us know the mostly sold out product that customers seem to consume a lot.)

3. **Running Monthly Sales Totals** → Use `SUM() OVER()`
  SUM() OVER() adds sales from each month so we can see how the total grows. This helps the store know how sales change, compare months, plan better, keep enough products and make more money and profits for the business.

For example in;
( January we had foundation sales -100,000frw lipstick sales-100,000frw and lip balm sales- 100,000frw total sums up to 300,000frw.   February we had foundation sales-50,000frw, lipstick sales-150,000frw and lip balm sales-150,000frw total sums up to 350,000frw.   In March we have concealer sales-120,000, lip balm sales-200,000frw , cleanser-100,000frw and contour-50,000frw total sums up to 470,000frw.
This goes on and on for every month we need to know the total sales so there can be comparison of the products and sales per month also has a huge impact in revealing top selling products. )
   
4. **Month-over-Month Growth** → Use `LAG()` / `LEAD()`
LAG() and LEAD() help us compare sales from one month to the next. LAG() shows the previous month’s sales, and LEAD() shows the next month’s sales. This helps the store see if sales go up or down.
This is different from SUM() OVER() because SUM() OVER() adds sales month by month to see a running total, while LAG() and LEAD() only compare one month to another. This helps the store plan better.
For example for LAG() / LEAD(), if January sales are 10, February is 12, and March is 15,LAG() can show February grew by 2 and March grew by 3. LEAD() can show January will grow by 2 in February and February will grow by 3 in March while for `SUM() OVER()` it adds the sales from each month.
 
5. **Customer Quartiles** → Use `NTILE(4)`
`NTILE(4)`is a tool that helps the store split customers into four equal groups based on how much they spend. Group 1 has the lowest spenders and Group 4 has the highest spenders. This helps the store understand customers better and plan marketing. For example, if we have 8 customers who spend $10, $20, $30, $40, $50, $60, $70, and $80 , NTILE(4) will put the lowest spenders in Group 1 and the highest spenders in Group 4.

6. **3-Month Moving Averages** → Use `AVG() OVER()`
`AVG() OVER()` is a tool that helps the store find the average sales over a certain time, such as three months. This is called a moving average. It looks at the current month and the months before it to see the average sales. This helps the store understand sales trends and plan better. For example,  The online makeup store has January sales of 10, February sales of 12, March sales of 15, and April sales of 20, the 3-month average for March will be (10 + 12 + 15) ÷ 3 = 12.33, and for April it will be (12 + 15 + 20) ÷ 3 = 15.67. This helps the store know if sales are increasing or decreasing.

## Database Schema
In this category i am going to be showing how i was ables to create three tables using codes i wrote
The first table was of customers
* purpose ( Customer Info)
* Key Columns ( Customer_id, name, region)
* Example Row ( 1001, John Doe, Kigali)
Below are the codes that helped me create the table of CUSTOMERS

CREATE TABLE customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    region VARCHAR(50) NOT NULL
);
![CUSTOMER DETAILS](https://github.com/user-attachments/assets/ff9b6998-5a1d-4bbf-a66a-35628c2010ac)

KET TERMS uUSED EXPLAINED;

*INT - Means the column will only store whole numbers (like 1, 2, 3).
*AUTO_INCREMENT - The number goes up by itself each time you add a new row (1, 2, 3, …). You don’t need to type it.
*PRIMARY KEY - This column is the main ID for the table. It makes each row unique (no two rows can have the same ID).
*VARCHAR(100) → means the column stores text (like names, emails, cities). The 100 means it can hold up to 100 characters.
*NOT NULL → means this column cannot be left empty. You must put something there.

Below is an example row for the customers table (codes).

INSERT INTO customers (customer_id, name, region)
VALUES (1001, 'John Doe', 'Kigali');
![CUSTOMER EXAMPLE](https://github.com/user-attachments/assets/5a148c5d-985e-42ec-9619-154af34b8567)


KEY TERMS USED EXPLAINED;

INSERT INTO customers (...) - tells the database we are putting new data into the customers table.
(customer_id, name, region) - lists the columns where the data will go.
VALUES (1001, 'John Doe', 'Kigali') - gives the actual data: the customer ID is 1001, the name is John Doe, and the region is Kigali

TABLE 2;
PRODUCTS 
* purpose (Product Catalog )
* Key Columns (Product_id(PK), name, category).
* Example Row ( 2001, coffee beans, beverages).

Below are the codes for this table

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    category VARCHAR(50) NOT NULL
);

![PRODUCT DETAILS](https://github.com/user-attachments/assets/66032ee8-d53f-432c-abb8-90457b793497)

(Terms used are explained above in the table concept)

 Now the codes for inserting details into the table like the example row has shown us what to insert into this table we have created above.
 
 INSERT INTO products (product_id, name, category) 
VALUES (2001, 'Coffee Beans', 'Beverages');

![PRODUCT EXAMPLE](https://github.com/user-attachments/assets/84ae3cc2-5a4e-4b37-b640-9da3fd03f900)

This image shows that the information of the example row have been successfully inserted.

TABLE 3
TRANSCATIONS

* purpose ( Sales Records)
* Key Columns ( transcation_id(PK), Customer_id(FK), Product_id(FK), sale_date, amount).
* Example Row (3001, 1001, 2001, 2024-01-15, 25000).

Below are the codes for the creation of this table.

CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    sale_date DATE NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

![TRANSCATION DETAIL](https://github.com/user-attachments/assets/c508aa39-f25b-49da-9d55-50229b682c97)

In the transactions table, transaction_id is the unique ID for each sale, customer_id shows which customer bought the product, product_id shows which product was bought, sale_date is the date of the sale, and amount is the money spent. The FOREIGN KEYS link customer_id to the customers table and product_id to the products table, making sure every sale is tied to a real customer and a real product. 

Now below is the entry done successfully of transcation example row.

![TRANSCATION EXAMPLE](https://github.com/user-attachments/assets/ef264a48-8db0-497f-bec9-99ffd731739e)


in conclusion below is the database showing all the 3 created tables,

<img width="1341" height="632" alt="database tables" src="https://github.com/user-attachments/assets/f03aea73-022f-4007-bda7-c96a3d811b8c" />




## SQL Queries
(Add your SQL queries here with explanations)

## Results
(Add screenshots or summaries of your results here)

## Conclusion
This project shows how PL/SQL window functions helps understand business data, improve sales, and make better marketing decisions for their enterprises.

---

**Created by: TETA JULIET  
**Date:09/27/2025
