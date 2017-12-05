# Technical Interview: Database

## Part 1 - General Database Questions

**1.	What is a Tablespace?**

Tablespaces are logical groups of physical databases files, which can be broken down into segments which contain extents. Tablespaces contain one or more data files and allow multiple data files to be manipulated at once by the database administrator.


**2.	What are extents?**

Extents are groups of database blocks. The user has limited control over extents as the lowest unit
the user works with are segments (which are made up of extents).  The user can determine the number of extents
that a segment contains.  The database will automatically add new extents to a segment once it has been filled.

**3.	How are extents and tablespaces related?**

Tablespaces are the logical groups of the database which contain datafiles (the physical grouping of the database) which then contain segments and extents.  

## Part 2 - SQL Queries

**1. Write a SQL query that returns a list of all car makes and models stored in the database. The result should be sorted alphabetically by
make and then by model. Do not display duplicate rows.**

The following query selects from the SC_CAR table and orders alphabetically by make, and then by model. The “DISTINCT” modifier prevents duplicate rows from appearing in the results.  

```mysql
SELECT DISTINCT make, model FROM SC_CAR ORDER BY make, model;
```

**2. Write a SQL query that returns the number of unique models per make. The results should be sorted alphabetically by the make.**

```mysql
SELECT make, COUNT(*) FROM SC_CAR GROUP BY make;
```

**3. Write a SQL query that returns the first and last names of the mechanics and the number of jobs(transactions) that they have performed
on BMWs with over 150000KM. Sort the results by number of such transactions in a descending order.**

```mysql
select first_name, last_name, count(*) as num_transactions from
(SELECT first_name, last_name
FROM SC_MECHANIC 
inner join SC_TRANSACTION on 
	SC_MECHANIC.mechanic_id = SC_TRANSACTION.mechanic_id
    and SC_TRANSACTION.car_odo_km >= 150000
inner join SC_CUSTOMER_CAR on 
	SC_TRANSACTION.customer_car_id = SC_CUSTOMER_CAR.customer_car_id
inner join SC_CAR on
	SC_CAR.car_id = SC_CUSTOMER_CAR.car_id
    and SC_CAR.make = "BMW") as T
group by first_name, last_name
order by num_transactions desc;
```

**4. Write a SQL query that returns the total revenue per mechanic for cars serviced in the 2011 calendar year. Please sort by revenue
descending.**

```mysql
select first_name, last_name, sum(cost) as total_revenue from 
(SELECT first_name, last_name, service_date, cost
FROM SC_MECHANIC 
inner join SC_TRANSACTION on 
	SC_MECHANIC.mechanic_id = SC_TRANSACTION.mechanic_id
    and YEAR(SC_TRANSACTION.service_date) = 2011
inner join SC_TRANSACTION_ITEM on 
	SC_TRANSACTION.transaction_id = SC_TRANSACTION_ITEM.transaction_id) as T
group by first_name, last_name
order by total_revenue desc;
```

**5. Due to poor logging by some mechanics, not all transactions have items. However, the boss wants a list of all transactions that have
been made by mechanics "Tim Ivers", and "Moe Unkle". The query should include a breakdown of a transaction's items if available
(description and price). Sort by Mechanic in alphabetical order and then by the Service Date ascending.**

```mysql
select * from 
(select CONCAT(first_name, ' ', last_name) as mechanic_name, description, cost, car_odo_km, service_date from SC_TRANSACTION
left join SC_TRANSACTION_ITEM
	on SC_TRANSACTION.transaction_id = SC_TRANSACTION_ITEM.transaction_id
inner join SC_MECHANIC
	on SC_MECHANIC.mechanic_id = SC_TRANSACTION.mechanic_id
order by mechanic_name asc) as T
where mechanic_name = 'Tim Ivers' 
or mechanic_name = 'Moe Unkle';
```

## Part 3 - Database Design

**1. In order to send out Receipts and Promotional offers to customers as well as upcoming Job information to mechanics we want to add
both email addresses and physical addresses to the system. An email address is a simple string. An address consists of a Suite, Street, City, Province and Country.**

In order to support email addresses an email_address varchar(255) column should be added to the SC_CUSTOMER table.  To support physical addresses the columns:
 customer_suite varchar(255), 
street_address varchar(255), 
province varchar(255), 
country varchar(255) 
Should be added to the SC_CUSTOMER table.  The address information should be stored in separate columns like this to make searching the database easier, and then concatenated when it needs to be printed for users.  The customer’s postal code should also be recorded to ensure mail is delivered correctly.

***

**2. Currently the SC_TRANSACTION_ITEM table is causing issues for reporting and records keeping. The “description” column is being
populated by free form input by the mechanics, for example, an Oil Change is stored in many different ways: “OIL CHANGE”, “oil
change”, “Oil Change”, “OIL” ...etc.**

**Additionally, mechanics aren't charging a consistent rate for these common jobs either as they are directly entering the cost and have
occasionally been cutting deals or overcharging.**

**a. We want to enhance the application to have a specific set of Services (Ex: Oil Change, Tire Rotation...etc) with defined prices.**

**b. We would still like mechanics to be able to provide notes on the individual transaction_items performed.**

**c. All transaction items should be associated with a Service. We would like to be able to add a new Service (“Ex: Toyota Camry -
Steering Column Warranty Recall”) at run time in the application.**

**d. If a price of the service is increased it should only affect the transaction items created AFTER the change, it should not affect
current or past records.**

***

In order to accommodate these requirements a new table SC_SERVICES should be created with the columns 

service_id INT, 
description varchar(255), 
cost varchar(255)


Furthermore, the column 
service_id INT,
should be added to the SC_TRANSACTION_ITEM table.  

The SC_SERVICES table will allow users to create new services at runtime, and to update prices when required.  The SC_SERVICES.description field eliminates the need for free form input by the mechanics, and allows a selection to be made from the list of available services.  The SC_SERVICES.cost field ensures standardized pricing and eliminates the issues of discounts and overcharging.  The service_id, description, and cost will be populated into the SC_TRANSACTION_ITEM table when the SC_TRANSACTION_ITEM is performed, ensuring that a future update to the SC_SERVICES table will not affect current or past records.  Mechanics can still provide notes in the SC_TRANSACTION_ITEM.notes field on the individual transaction items. 

