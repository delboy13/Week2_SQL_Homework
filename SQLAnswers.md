# Week2 - MYSQL Homework answer (1-3) + Stretch (4-7)

## Walkthrough of creating tables and inserting data including creating the database!

### create database for the shop
	> create database shop;
	> use shop

	>create table category (CategoryID int, CategoryName char(50));
		>insert into Category values (1,'Food');
	+------------+--------------+
	| CategoryID | CategoryName |
	+------------+--------------+
	|          1 | Food         |
	|          2 | Drink        |
	|          3 | Clothes      |
	+------------+--------------+

	> create table Subcategory (SubcategoryID int, CategoryID int, SubcategoryName char (50));
		> insert into Subcategory values (20, 1,'Frozen' );
	+---------------+------------+-----------------+
	| SubcategoryID | CategoryID | SubcategoryName |
	+---------------+------------+-----------------+
	|            20 |          1 | Frozen          |
	|            21 |          1 | Fresh           |
	|            22 |          1 | Tinned          |
	|            30 |          2 | Squash          |
	|            31 |          2 | Alchohol        |
	+---------------+------------+-----------------+

	>create table Product (SubcategoryID int, ProductID int, ProductName char (50));
		> insert into product values (20, 100,'Peas' );
	+---------------+-----------+-------------+
	| SubcategoryID | ProductID | ProductName |
	+---------------+-----------+-------------+
	|            20 |       100 | Peas        |
	|            20 |       101 | Pizza       |
	|            20 |       102 | Icecream    |
	+---------------+-----------+-------------+

	> create table Sales (SalesID int,ProductID int, Quantity int, UnitPrice float, DateOfSales date);
		>insert into sales values (1000, 20, 376, 1.30, 20200102);
	+---------+-----------+----------+-----------+-------------+
	| SalesID | ProductID | Quantity | UnitPrice | DateOfSales |
	+---------+-----------+----------+-----------+-------------+
	|    1000 |       100 |      238 |       1.3 | 2020-01-01  |
	|    1001 |       100 |       20 |       1.3 | 2020-01-02  |
	|    1002 |       100 |     2320 |       1.9 | 2020-04-02  |
	|    1003 |       101 |     2920 |       4.9 | 2020-04-05  |
	|    1004 |       102 |       20 |       9.9 | 2020-07-05  |
	|    1005 |       110 |        9 |      39.9 | 2020-07-07  |
	|    1006 |       111 |       99 |       9.9 | 2019-07-07  |
	|    1007 |       112 |      199 |      19.9 | 2019-01-07  |
	|    1008 |       100 |       10 |       1.6 | 2020-09-24  |
	|    1009 |       100 |       10 |       1.6 | 2020-09-24  |
	|    1010 |       100 |       10 |       1.6 | 2020-09-24  |
	|    1011 |       101 |       90 |       1.6 | 2020-09-24  |
	|    1013 |       101 |       99 |       1.6 | 2020-09-24  |
	|    1014 |       101 |        1 |       1.6 | 2020-09-24  |
	+---------+-----------+----------+-----------+-------------+


# ANSWERS
---------
1. The most popular product sold on a specific date.

	> select product.productname, sales.productid, count(*) as popularity, sales.dateofsales from product, sales where sales.productid = product.productid and dateofsales = 20200924 group by dateofsales, sales.productID having count(*) = (select max(r) from (select count(*) as r, productid from sales group by dateofsales, productid)as mytable);

2. The most popular product sold last week. 

	>select Product.ProductName, Sales.ProductID, Count(*) as "popularity", week(Sales.DateofSales) from Product, Sales where Sales.ProductID = Product.ProductID and week(DateofSales) = week(curdate())-1  group by week(DateofSales), Sales.ProductID having count(*) = (select max(r) from (select count(*) as r, ProductID from Sales where week(Dateofsales) = week(curdate())-1 group by DateofSales, ProductID) as myTable);

3. The most popular product sold on a specific month.

	>select Product.ProductName, Sales.ProductID, Count(*) as "popularity", month(Sales.DateofSales) from Product, Sales where Sales.ProductID = Product.ProductID and month(DateofSales) = "07" group by month(DateofSales), Sales.ProductID having count(*) = (select max(r) from (select count(*) as r, ProductID from Sales where month(Dateofsales) = "07" group by DateofSales, ProductID) as myTable);

4. The most popular subcategory for a specific date.

	>select subcategoryname, count(*) as popularity, dateofsales from product join sales on product.productid = sales.productid join subcategory on subcategory.subcategoryID = product.subcategoryID where dateofsales = 20200924 group by dateofsales, subcategoryname having count(*) = (select max(r) from (select count(*) as r, subcategory.subcategoryid from product join sales on product.productid = sales.productid join subcategory on subcategory.subcategoryid = product.subcategoryid where dateofsales = 20200924 group by dateofsales, subcategory.subcategoryid) as mytable); 

5. The most popular subcategory for last week.
 
	>select subcategoryname, count(*) as popularity, week(dateofsales) from product join sales on product.productid = sales.productid join subcategory on subcategory.subcategoryID = product.subcategoryID where week(DateofSales) = week(curdate())-1 group by month(DateofSales), subcategoryname having count(*) = (select max(r) from (select count(*) as r, subcategory.subcategoryid from product join sales on product.productid = sales.productid join subcategory on subcategory.subcategoryid = product.subcategoryid where week(DateofSales) = week(curdate())-1 group by month(DateofSales), subcategory.subcategoryid) as mytable);

6. The most popular subcategory for a specific month.
	>select subcategoryname, count(*) as popularity, dateofsales from product join sales on product.productid = sales.productid join subcategory on subcategory.subcategoryID = product.subcategoryID where month(DateofSales) = "07" group by month(DateofSales), subcategoryname having count(*) = (select max(r) from (select count(*) as r, subcategory.subcategoryid from product join sales on product.productid = sales.productid join subcategory on subcategory.subcategoryid = product.subcategoryid where month(DateofSales) = "07" group by month(DateofSales), subcategory.subcategoryid) as mytable);

