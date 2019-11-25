# Create table and insert data
```sql

CREATE DATABASE `northwind`;
USE `northwind` ;


CREATE TABLE `products` (
  `supplier_ids` LONGTEXT NULL DEFAULT NULL,
  `id` INT(11) NOT NULL PRIMARY KEY,
  `product_name` VARCHAR(50) NULL DEFAULT NULL,
  `list_price` DECIMAL(19,4) NOT NULL DEFAULT '0.0000',
  `reorder_level` INT(11) NULL DEFAULT NULL,
  `target_level` INT(11) NULL DEFAULT NULL,
  `quantity_per_unit` VARCHAR(50) NULL DEFAULT NULL,
  `category` VARCHAR(50) NULL DEFAULT NULL
   );


USE `northwind`;

INSERT INTO `products` VALUES ('4', 1, 'Chai',18, 10, 40, '10 boxes x 20 bags', 'Beverages');
INSERT INTO `products` VALUES ('10', 3,  'Syrup', 10, 25, 100, '12 - 550 ml bottles', 'Condiments');
INSERT INTO `products` VALUES ('10', 4, 'Cajun Seasoning', 22, 10, 40, '48 - 6 oz jars', 'Condiments');
INSERT INTO `products` VALUES ('10', 5, 'Olive Oil', 21.35, 10, 40, '36 boxes', 'Oil');
INSERT INTO `products` VALUES ('2;6', 6, 'Boysenberry Spread', 25, 25, 100, '12 - 8 oz jars', 'Jams, Preserves');
INSERT INTO `products` VALUES ('2', 7, 'Dried Pears', 30, 10, 40, '12 - 1 lb pkgs.', 'Dried Fruit & Nuts');
INSERT INTO `products` VALUES ('8', 8, 'Curry Sauce',  40, 10, 40, '12 - 12 oz jars',  'Sauces');
INSERT INTO `products` VALUES ('2;6', 14, 'Walnuts',  23.25, 10, 40, '40 - 100 g pkgs.',  'Dried Fruit & Nuts');
INSERT INTO `products` VALUES ('6', 17, 'Fruit Cocktail',  39, 10, 40, '15.25 OZ',  'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('1', 19, 'Chocolate Biscuits Mix', 9.2, 5, 20, '10 boxes x 12 pieces', 'Baked Goods & Mixes');
INSERT INTO `products` VALUES ('2;6', 20, 'Marmalade',  81, 10, 40, '30 gift boxes',  'Jams, Preserves');
INSERT INTO `products` VALUES ('1', 21, 'Scones',  10, 5, 20, '24 pkgs. x 4 pieces',  'Baked Goods & Mixes');
INSERT INTO `products` VALUES ('4', 34, 'Beer',  14, 15, 60, '24 - 12 oz bottles',  'Beverages');
INSERT INTO `products` VALUES ('7', 40, 'Crab Meat',  18.4, 30, 120, '24 - 4 oz tins',  'Canned Meat');
INSERT INTO `products` VALUES ('6', 41, 'Clam Chowder',  9.65, 10, 40, '12 - 12 oz cans',  'Soups');
INSERT INTO `products` VALUES ('3;4', 43, 'Coffee',  46, 25, 100, '16 - 500 g tins', 'Beverages');
INSERT INTO `products` VALUES ('10', 48,  'Chocolate',  12.75, 25, 100, '10 pkgs',  'Candy');
INSERT INTO `products` VALUES ('2', 51,  'Dried Apples',  53, 10, 40, '50 - 300 g pkgs.',  'Dried Fruit & Nuts');
INSERT INTO `products` VALUES ('1', 52,  'Long Grain Rice',  7, 25, 100, '16 - 2 kg boxes',  'Grains');
INSERT INTO `products` VALUES ('1', 56,  'Gnocchi',  38, 30, 120, '24 - 250 g pkgs.',  'Pasta');
INSERT INTO `products` VALUES ('1', 57, 'Ravioli',  19.5, 20, 80, '24 - 250 g pkgs.', 'Pasta');
INSERT INTO `products` VALUES ('8', 65, 'Hot Pepper Sauce',  21.05, 10, 40, '32 - 8 oz bottles',  'Sauces');
INSERT INTO `products` VALUES ('8', 66,  'Tomato Sauce',  17, 20, 80, '24 - 8 oz jars',  'Sauces', '');
INSERT INTO `products` VALUES ('5', 72, 'Mozzarella',  34.8, 10, 40, '24 - 200 g pkgs.',  'Dairy products');
INSERT INTO `products` VALUES ('2;6', 74, 'Almonds',  10, 5, 20, '5 kg pkg.', 'Dried Fruit & Nuts');
INSERT INTO `products` VALUES ('10', 77, 'Mustard',  13, 15, 60, '12 boxes',  'Condiments');
INSERT INTO `products` VALUES ('2', 80, 'Dried Plums',  3.5, 50, 75, '1 lb bag',  'Dried Fruit & Nuts');
INSERT INTO `products` VALUES ('3', 81, 'Green Tea', 2.99, 100, 125, '20 bags per box',  'Beverages');
INSERT INTO `products` VALUES ('9', 83, 'Potato Chips', 1.8, 30, 200, NULL, 'Chips, Snacks');
INSERT INTO `products` VALUES ('1', 85, 'Brownie Mix',  12.49, 10, 20, '3 boxes',  'Baked Goods & Mixes');
INSERT INTO `products` VALUES ('1', 86, 'Cake Mix',  15.99, 10, 20, '4 boxes',  'Baked Goods & Mixes');
INSERT INTO `products` VALUES ('7', 87,'Tea',  4, 20, 50, '100 count per box',  'Beverages');
INSERT INTO `products` VALUES ('6', 88, 'Pears',  1.3, 10, 40, '15.25 OZ',  'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('6', 89, 'Peaches', 1.5, 10, 40, '15.25 OZ',  'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('6', 90, 'Pineapple', 1.8, 10, 40, '15.25 OZ',  'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('6', 91, 'Cherry Pie Filling',  2, 10, 40, '15.25 OZ',  'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('6', 92, 'Green Beans',  1.2, 10, 40, '14.5 OZ',  'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('6', 93, 'Corn',  1.2, 10, 40, '14.5 OZ', 'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('6', 94, 'Peas', 1.5, 10, 40, '14.5 OZ', 'Canned Fruit & Vegetables');
INSERT INTO `products` VALUES ('7', 95, 'Tuna Fish', 2, 30, 50, '5 oz',  'Canned Meat');
INSERT INTO `products` VALUES ('7', 96, 'Smoked Salmon', 4, 30, 50, '5 oz', 'Canned Meat');
INSERT INTO `products` VALUES ('1', 97, 'Hot Cereal',  5, 50, 200, NULL,  'Cereal');
INSERT INTO `products` VALUES ('6', 98, 'Vegetable Soup',  1.89, 100, 200, NULL,  'Soups');
INSERT INTO `products` VALUES ('6', 99, 'Chicken Soup',  1.95, 100, 200, NULL,  'Soups');
```
# Basic select
```sql
/*
************Basic SELECT*************
The SELECT statement is used to select data from a database
The data returned is stored in a result table, called the result-set.
--SQL requires single quotes around text values
*/


-- Select specific columns from the table 
SELECT list_price, product_name 
FROM products;


-- Select all columns from the table 
SELECT *
FROM products;

--SQL aliases for column in a table, 
--An alias is temporary name - only exists for the duration of the query
SELECT list_price AS 'Price', product_name AS 'Name'
FROM products;


SELECT list_price AS 'Oeiginal Price',list_price*0.18 AS 'tax Price',list_price*1.18 AS 'Market Price', product_name AS 'Product Name'
FROM products;


--The SELECT DISTINCT statement is used to return only distinct (different) UnitPrices
SELECT DISTINCT list_price
FROM products;

--when we use SELECT DISTINCT with few columns, only distinct combinations will be returned  
SELECT DISTINCT list_price, product_name
FROM products

```


# Where

```sql

/*
The WHERE clause is used to filter records with a specified condition.
 
Common WHERE Operators (for numbers and dates):
-----------------------------------
=			|	Equal
<>			|	Not equal
!=			|	Not equal
>			|	Greater than
<			|	Less than
>=			|	Greater than or equal
<=			|	Less than or equal

Common WHERE Operators (for strings etc):
-----------------------------------
=			|	Equal
<>			|	Not equal
!=			|	Not equal

Advanced conditions operators:
------------------------------------
AND			|	displays a record if all the conditions separated by AND is TRUE. (like &&)
OR			|	displays a record if any of the conditions separated by OR is TRUE. (like ||)
NOT			|	displays a record if the condition(s) is NOT TRUE. (like !)
*/

/*
==========================================================
number condition
==========================================================
*/

--************************* = *************************
--select all Products with id equal to 5 
SELECT * 
FROM products
WHERE id=5; 

--************************* <> *************************
--select all Products with id not equal to 5 
SELECT * 
FROM products
WHERE id<>5;

--************************* != *************************
--select all Products with id not equal to 5 
SELECT * 
FROM products
WHERE id!=5;

--************************* > *************************
--select all Products with id greater than 5 
SELECT * 
FROM products
WHERE id>5;

--************************* < *************************
--select all Products with id less than 5 
SELECT * 
FROM products
WHERE id<5; 

--************************* >= *************************
--select all Products with id greater than 5 or equal to 5 
SELECT * 
FROM products
WHERE id>=5;

--************************* <= *************************
--select all Products with ProductID less than 5 or equal to 5 
SELECT * 
FROM products
WHERE id<=5;




/*
==========================================================
string condition
==========================================================
*/

--************************* = *************************
--select all Products with product_name equal to 'Corn' 
SELECT * 
FROM products
WHERE product_name='Corn';

--************************* <> *************************
--select all Products with product_name not equal to 'Corn' 
SELECT * 
FROM products
WHERE product_name<>'Corn';

--************************* != *************************
--select all Products with product_name not equal to 'Corn'  
SELECT * 
FROM products
WHERE product_name!='Corn';



/*
==========================================================
combined condition
==========================================================
*/

--************************* AND *************************
--select all Products with id greater than 5 AND less than 10
SELECT * 
FROM products
WHERE id>5 AND id<10;

--************************* OR *************************
--select all Products with id greater than 5 OR less than 10
SELECT * 
FROM products
WHERE id=5 OR id=8;

--************************* NOT *************************
--select all Products with ProductID than is NOT 5 
SELECT * 
FROM products
WHERE  NOT id=5;

```


# Between
```sql
/*
--************************* BETWEEN *************************
The WHERE clause is used to filter records with a specified condition.
 
Common WHERE Operator:
-----------------------------------
BETWEEN		|	Between an inclusive range
*/



--select all products with a list_price BETWEEN 10 and 18:
--(including 10 and including 18)
SELECT id,list_price,product_name 
FROM products
WHERE list_price BETWEEN 10 AND 18;



--select all products with a list_price outside the range of 10 and 18:
SELECT id,list_price,product_name 
FROM products
WHERE list_price NOT BETWEEN 10 AND 18;



--select all products with a product_name BETWEEN 'A' AND 'C'
SELECT id,list_price,product_name 
FROM products
WHERE product_name BETWEEN 'A' AND 'C';

--select all products with a product_name not BETWEEN 'A' AND 'C'
SELECT id,list_price,product_name 
FROM products
WHERE product_name NOT BETWEEN 'A' AND 'C';

```


# in
```sql
/*
--************************* IN *************************
The WHERE clause is used to filter records with a specified condition.
 
Common WHERE Operator:
-----------------------------------
IN			|	Specify multiple possible values
*/


--select all products that are named 'Peas' or 'Chicken Soup'
SELECT product_name 
FROM products
WHERE product_name IN ('Peas', 'Chicken Soup');



--select all products that are not named 'Peas' or 'Chicken Soup'
SELECT product_name 
FROM products
WHERE product_name IN ('Peas', 'Chicken Soup');
```


# Like
```sql
/*
--************************* LIKE *************************
The WHERE clause is used to filter records with a specified condition.
 
Common WHERE Operator:
-----------------------------------
LIKE		|	Specify pattern
Wildcard characters used with the LIKE operator:
-----------------------------------
%				|	The percent represents zero or more characters
_				|	The underscore represents a single character 
*/


--selects all Products with a product_name starting with the pattern "C"
SELECT product_name 
FROM products
WHERE product_name LIKE 'C%';

--selects all Products with a product_name ending with the pattern "p"
SELECT product_name 
FROM products
WHERE product_name LIKE '%p';

--selects all Products with a product_name ending with the pattern "ou" + 1 char
SELECT product_name 
FROM products
WHERE product_name LIKE '%ou_';
```

# is null
```sql
/*
--************************* IS NULL *************************
The WHERE clause is used to filter records with a specified condition.
A field with a NULL value is a field with no value.
It is not possible to test for NULL values with comparison operators, such as =, <, or <>.
We will have to use the IS NULL and IS NOT NULL operators instead.
 
Common WHERE Operators:
-----------------------------------
IS NULL			|	displays a record if the specified column element contains NULL
IS NOT NULL		|	displays a record if the specified column element is not NULL
*/

--select all products that have no quantity_per_unit
SELECT product_name, quantity_per_unit
FROM products
WHERE quantity_per_unit IS NULL;


--select all products that do have quantity_per_unit
SELECT product_name, quantity_per_unit
FROM products
WHERE quantity_per_unit IS NOT NULL;
```


# order by
```sql
/*
The ORDER BY is used to sort the result-set in ascending or descending order.
The ORDER BY keyword sorts the records in ascending order by default. 
Common ORDER BY keywords:
-----------------------------------
DESC	|	sort the records in descending order
ASC		|	sort the records in ascending order
*/


/*
==========================================================
ascending order
==========================================================
*/

--select all Products with id greater than 5,  in ascending order by the "product_name" column
SELECT id,list_price,product_name
FROM products
WHERE id>5 
ORDER BY product_name;

--select all Products with ProductID greater than 5,  in ascending order by the "product_name" column
SELECT id,list_price,product_name
FROM products
WHERE id>5 
ORDER BY product_name ASC;


--select all Products with ProductID greater than 5,  in descending order by the "product_name" column
SELECT id,list_price,product_name
FROM products
WHERE id>5 
ORDER BY product_name DESC;


--"list_price" - will be the main indexer in the order, and the "id" will be the sub indexer
SELECT id,list_price,product_name
FROM products
ORDER BY list_price, id;

--"list_price" - will be the main indexer in the order, and the "id" will be the sub indexer by descending
SELECT id,list_price,product_name
FROM products
ORDER BY list_price ASC, id DESC;



/*
==========================================================
order by column number
==========================================================
*/

--select all Products in ascending order by the "product_name" column (the third in the select column list)
SELECT id,list_price,product_name
FROM products
ORDER BY 3

```


# SQL NUMERIC FUNCTIONS
```sql
/*
SQL NUMERIC FUNCTIONS
*/


/*
==========================================================
AVG
----------------------------------------------------------
AVG requires A numeric value and returns the average value
==========================================================
*/

--Select the average value for the "list_price" column in the "products" table
SELECT AVG(list_price) AS 'Average Price' 
FROM products;



/*
==========================================================
COUNT
----------------------------------------------------------
COUNT requires A field or a string value as a parameter
returns the number of records in a select query (counts only NOT NULL values)
==========================================================
*/

--return the number of products in the "products" table
SELECT COUNT(id) AS 'Number Of Products' 
FROM products;


/*
==========================================================
SUM
----------------------------------------------------------
SUM requires A numeric value (can be a field or a formula)  
returns the summed value of an expression.
==========================================================
*/

--SUM of the "list_price" field in the "products" table
SELECT SUM(list_price) AS 'Sum Unit Price' FROM products;



/*
==========================================================
MAX
----------------------------------------------------------
MAX equires A numeric value (can be a field or a formula)  
returns the maximum value of an expression
==========================================================
*/

--returns the price of the most expensive product in the "products" table
SELECT MAX(list_price) AS 'Expensive Product ' 
FROM products;



/*
==========================================================
MIN
----------------------------------------------------------
MIN equires A numeric value (can be a field or a formula)  
returns the minimum value of an expression
==========================================================
*/

--returns the price of the most low cost product in the "Products" table
SELECT MIN(list_price) AS 'Low Cost Product ' 
FROM products;


/*
==========================================================
ABS
----------------------------------------------------------
ABS requires A numeric value and returns the absolute value of a number
==========================================================
*/

--select from all Products the list_price as positive and negative
SELECT list_price, (-list_price) AS 'Neg UnitPrice', ABS(-list_price) AS 'Pos UnitPrice'
FROM products;


/*
==========================================================
CEILING
----------------------------------------------------------
CEILING requires A numeric value 
and returns the smallest integer value that is greater than or equal to a number
==========================================================
*/

--select from all Products the list_price and the CEILING(list_price)
SELECT list_price, CEILING(list_price) AS 'Ceil UnitPrice'
FROM products;



/*
==========================================================
FLOOR
----------------------------------------------------------
FLOOR requires A numeric value 
and returns the largest integer value that is equal to or less than the number
==========================================================
*/

--select from all Products the UnitPrice and the FLOOR(UnitPrice)
SELECT list_price, FLOOR(list_price) AS 'Floor UnitPrice'
FROM products;



/*
==========================================================
ROUND
----------------------------------------------------------
ROUND requires the following parameters:
1.	number - Required - The number to round
2.	decimal_places - Required - The number of decimal places to round to
3.	operation - Optional - Can be any other numeric value - Default value is 0
(When 0,  ROUND will round the result to the number of decimal_places. 
When another value than 0, ROUND will truncate the result to the number of decimal_places. )
and returns a number rounded to a certain number of decimal places.
==========================================================
*/

--select from all Products the UnitPrice and the CEILING(UnitPrice)
SELECT list_price, ROUND(list_price,1) AS 'Round UnitPrice'
FROM products;



/*
==========================================================
RAND
----------------------------------------------------------
RAND has an Optional parameter that can take A numeric value (seed) 
RAND will return a value between 0 (inclusive) and 1 (exclusive) if no seed is provided, 
and a repeatable sequence of random numbers if a seed value is used 
==========================================================
*/

--Return a random decimal number (no seed value - so it returns a random number >= 0 and <1)
SELECT RAND() AS 'Round Num';

--Return a random decimal number >= 0 and <5
SELECT RAND(5) AS 'Round Num';

--Return a random decimal number >= 5 and <10
SELECT RAND()*(10-5)+5 AS 'Round Num';

--Return a random number >= 5 and <=10
SELECT FLOOR(RAND()*(10-5+1)+5) AS 'Round Num';



/*
==========================================================
SIGN
----------------------------------------------------------
SIGN requires A numeric value and returns a value indicating the sign of a number.
This function will return one of the following:
If number > 0, it returns 1
If number = 0, it returns 0
If number < 0, it returns -1
==========================================================
*/

--returns : 1	0	-1

SELECT SIGN(3) AS 'Sign Pos', SIGN(0) AS 'Sign Zero', SIGN(-3) AS 'Sign Neg';
```


# Group by
```sql
/*
GROUP BY works with:
	-AVG
	-COUNT
	-SUM
	-MAX
	-MIN
    etc...
*/
--GET 45 ROWS 
SELECT supplier_ids, list_price
FROM products;


--GET 12 ROWS OF DISTINCT supplier_ids
SELECT DISTINCT supplier_ids
FROM products;

--GET 12 ROWS OF DISTINCT supplier_ids and show their avrage UnitPrice
SELECT supplier_ids, AVG(list_price) AS 'Supplier Average'
FROM products
GROUP BY supplier_ids;

```