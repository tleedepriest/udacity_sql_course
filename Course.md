# Joins

## Data Model

![Alt text](/images/data_model_udemy_course.png?raw=true "Data Model")

## Script Questions

1. Provide a table for all web_events associated with account name of Walmart. There should be three columns. Be sure to include the primary_poc, time of the event, and the channel for each event. Additionally, you might choose to add a fourth column to assure only Walmart events were chosen. 

```SQL
SELECT 
        w.occurred_at,
        w.channel, 
        a.primary_poc,
        a.name
FROM web_events w
JOIN accounts a
ON w.account_id=a.id
WHERE a.name='Walmart'


```

- Notes: You must come up with the aliases before they are declared. Somewhat counterintuitive, select all columns from each table with the appropriate alias, define alias in the FROM and JOIN lines.

2. Provide a table that provides the **region** for each **sales_rep** along with their associated **accounts**.  Your final table should include three columns: the region **name**, the sales rep **name**, and the account **name**. Sort the accounts alphabetically (A-Z) according to account name. 

   ```SQL
   SELECT 
   	sr.name sr_name,
   	a.name ac_name,
   	r.name
   FROM sales_reps sr
   JOIN accounts a
   ON a.sales_rep_id=sr.id
   JOIN region r
   ON sr.region_id=r.id
   ORDER BY a.name ASC
   ```

   - Notes

   3. Provide the **name** for each region for every **order**, as well as the account **name** and the **unit price** they paid (total_amt_usd/total) for the order.  Your final table should have 3 columns: **region name**, **account name**, and **unit price**.  A few accounts have 0 for **total**, so I divided by (total + 0.01) to assure not dividing by zero

   ```SQL
   SELECT 
   	o.total_amt_usd/(o.total+0.01) unit_price,
   	a.name ac_name,
   	r.name
   FROM orders o
   JOIN accounts a
   ON a.id=o.account_id
   JOIN sales_reps sr
   ON sr.id=a.sales_rep_id
   JOIN region r
   ON r.id=sr.region_id
   ```

   - Notes: in your SELECT statement you only choose the columns you want to see in the final QUERY. I made the mistake of selecting all the columns, thinking that SQL needed these to perform the joins. In reality, SQL must already have access to all columns and the SELECT statement is just to produce what the user wants in the final output. I also had to create a different name for the accounts_name. It had the same name as the region name column and wasn't showing up in the final query. I will need to go back to question 2 and fix this. I also started with orders and worked my way backwards to perform the necessary joins.

### Fact Questions (All are True)

#### 1 of 2

Mark all the below that are true.  

- The **ON** statement **should** always occur with the foreign key being equal to the primary key.
- **JOIN** statements allow us to pull data from multiple tables in a **SQL** database.
- You can use all of the commands we saw in the first lesson along with **JOIN** statements.

#### 2 of 2

Select all of the below statements that are true.

- Aliasing is common to shorten table names when we start **JOIN**ing multiple tables together.  

#### Solutions

1.

```SQL
SELECT a.primary_poc, w.occurred_at, w.channel, a.name
FROM web_events w
JOIN accounts a
ON w.account_id = a.id
WHERE a.name = 'Walmart';
```

2.

```SQL
SELECT r.name region, s.name rep, a.name account
FROM sales_reps s
JOIN region r
ON s.region_id = r.id
JOIN accounts a
ON a.sales_rep_id = s.id
ORDER BY a.name;
```

3.

```SQL
SELECT r.name region, a.name account, 
       o.total_amt_usd/(o.total + 0.01) unit_price
FROM region r
JOIN sales_reps s
ON s.region_id = r.id
JOIN accounts a
ON a.sales_rep_id = s.id
JOIN orders o
ON o.account_id = a.id;
```

#### Videos

- Joins can have 1-1 or 1-many - some accounts don't have orders. Outer Join includes accounts without orders. May want to do this if we are simply looking to count the number of accounts in a region (even if they have not placed orders).

- Inner Join only returns rows that appear in both tables 

- Left, Right, Full Outer Join - Full Outer Join at least as many rows as Inner Join

- Left Join will include additional rows from left table (Select left, From right) not matched in the right table

- Right join will include additional rows from right table not matched in left table

- Left and right joins are effectively the same, always use left joins as that is the standard.

- ```SQL
  LEFT OUTER JOIN is the same as LEFT JOIN, RIGHT OUTER JOIN is the same as RIGHT JOIN
  FULL OUTER JOIN is the same as OUTER JOIN
  ```

- Use cases for ```FULL OUTER JOINS``` are rare.

#### Questions

### Question 1 of 4

ALL TRUE

- A **LEFT JOIN** and **RIGHT JOIN** do the same thing if we change the tables that are in the **FROM** and **JOIN** statements.
- A **LEFT JOIN** will **at least** return all the rows that are in an **INNER JOIN**.
- **JOIN** and **INNER JOIN** are the same.
- A **LEFT OUTER JOIN** is the same as **LEFT JOIN**

Questions 2-4 asked about various numbers of columns and rows comparing and contrasting inner joins and left joins, I got them all correct the first try.



### Video: Joins and Filters

- Joins are means to do more analysis
- combining allows you to filter and aggregate to perform more advance analysis
- **Pro Tip**: Logic in the ON clause reduces the rows before combining the tables
- **Pro Tip**: Logic in the WHERE clause occurs ofter joining the tables.

- Therefore can change a WHERE to AND to move statement into the ON clause

### QUIZ: Last Check

![Alt text](/images/data_model_udemy_course.png?raw=true "Data Model")

1. Provide a table that provides the **region** for each **sales_rep** along with their associated **accounts**.  This time only for the `Midwest` region.  Your final table should include three columns: the region **name**, the sales rep **name**, and the account **name**.  Sort the accounts alphabetically (A-Z) according to account name.

   ```SQL
   SELECT a.name acc_name, r.name reg_name, sr.name sales_rep_name
   FROM accounts a
   JOIN sales_reps sr
   ON a.sales_rep_id = sr.id
   JOIN region r
   ON sr.region_id = r.id
   AND r.name='Midwest'
   ORDER BY a.name
   ```

   - Notes --> a little confused at first on how to determine which table is in the FROM clause, but ultimately no different. FROM <table_name> JOIN <next_table_name>.... JOIN <next_table_name>
   - Remember single quotations around the Midwest

2. Provide a table that provides the **region** for each **sales_rep** along with their associated **accounts**.  This time only for accounts where the sales rep has a first name starting with `S` and in the `Midwest` region.  Your final table should include three columns: the region **name**, the sales rep **name**, and the account **name**. Sort the accounts alphabetically (A-Z) according to account name. 

   ```SQL
   SELECT a.name acc_name, r.name reg_name, sr.name sales_rep_name
   FROM accounts a
   JOIN sales_reps sr
   ON a.sales_rep_id = sr.id
   JOIN region r
   ON sr.region_id = r.id
   AND r.name='Midwest'
   AND sr.name LIKE 'S%'
   ```

   

- Notes -> Fairly easy extension of the one above, just had to remember like clause...very similar to regular expressions, so natural for me remember notation.

3. Provide a table that provides the **region** for each **sales_rep** along with their associated **accounts**.  This time only for accounts where the sales rep has a **last** name starting with `K` and in the `Midwest` region.  Your final table should include three columns: the region **name**, the sales rep **name**, and the account **name**. Sort the accounts alphabetically (A-Z) according to account name.

   ````SQL
   SELECT a.name acc_name, r.name reg_name, sr.name sales_rep_name
   FROM accounts a
   JOIN sales_reps sr
   ON a.sales_rep_id = sr.id
   JOIN region r
   ON sr.region_id = r.id
   AND r.name='Midwest'
   AND sr.name LIKE '% K%'
   ````

   - first try

4. Provide the **name** for each region for every **order**, as well as the account **name** and the **unit price** they paid (total_amt_usd/total) for the order.  However, you should only provide the results if the **standard order quantity** exceeds `100`. Your final table should have 3 columns: **region name**, **account name**, and **unit price**.  In order to avoid a division by zero error, adding .01 to the denominator here is helpful total_amt_usd/(total+0.01).  

```SQL
SELECT a.name acc_name, r.name reg_name, o.total_amt_usd/(o.total+0.01) unit_price
FROM accounts a
JOIN sales_reps sr
ON a.sales_rep_id = sr.id
JOIN region r
ON sr.region_id = r.id
JOIN orders o
ON o.account_id = a.id
AND o.standard_qty > 100
```

5. Provide the **name** for each region for every **order**, as well as the account **name** and the **unit price** they paid (total_amt_usd/total) for the order.  However, you should only provide the results if the **standard order quantity** exceeds `100` and the **poster order quantity** exceeds `50`.  Your final table should have 3 columns: **region name**, **account name**, and **unit price**. Sort for the smallest **unit price** first. In order to avoid a division by zero error, adding .01 to the denominator here is helpful (total_amt_usd/(total+0.01). 

```SQL 
SELECT a.name acc_name, r.name reg_name, o.total_amt_usd/(o.total+0.01) unit_price
FROM accounts a
JOIN sales_reps sr
ON a.sales_rep_id = sr.id
JOIN region r
ON sr.region_id = r.id
JOIN orders o
ON o.account_id = a.id
AND o.standard_qty > 100
AND o.poster_qty > 50
ORDER BY o.total_amt_usd/(o.total+0.01) ASC
```

- had to recreate the order price column calculation instead of referring to it by the name of unit_price.
- received error when trying ORDER BY o.unit_price, which makes sense as I assume the SELECT may be the last thing to be rendered/compiled by SQL

6. Provide the **name** for each region for every **order**, as well as the account **name** and the **unit price** they paid (total_amt_usd/total) for the order.  However, you should only provide the results if the **standard order quantity** exceeds `100` and the **poster order quantity** exceeds `50`.  Your final table should have 3 columns: **region name**, **account name**, and **unit price**. Sort for the largest **unit price** first. In order to avoid a division by zero error, adding .01 to the denominator here is helpful (total_amt_usd/(total+0.01). 

```SQL
SELECT a.name acc_name, r.name reg_name, o.total_amt_usd/(o.total+0.01) unit_price
FROM accounts a
JOIN sales_reps sr
ON a.sales_rep_id = sr.id
JOIN region r
ON sr.region_id = r.id
JOIN orders o
ON o.account_id = a.id
AND o.standard_qty > 100
AND o.poster_qty > 50
ORDER BY o.total_amt_usd/(o.total+0.01) DESC
```

7. What are the different **channel**s used by **account id** `1001`?  Your final table should have only 2 columns: **account name** and the different **channel**s.  You can try **SELECT DISTINCT** to narrow down the results to only the unique values.

```SQL
SELECT DISTINCT a.name, we.channels
FROM accounts a
JOIN web_events we
ON a.id=we.accounts_id
AND a.id=1001
```

8. Find all the orders that occurred in `2015`.  Your final table should have 4 columns: **occurred_at**, **account name**, **order total**, and **order total_amt_usd**.  

```SQL
```



