# Joins

## Data Model

![Alt text](/images/data_model_udemy_course.png?raw=true "Data Model")

## Questions

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
