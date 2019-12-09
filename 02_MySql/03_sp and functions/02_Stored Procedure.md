# Stored Procedure
When you use MySQL Workbench or mysql shell to issue the query to MySQL Server, MySQL processes the query and returns the result set.

If you want to save this query on the database server for execution later, one way to do it is to use a stored procedure.

By definition, a stored procedure is a segment of declarative SQL statements stored inside the MySQL Server.

The first time you invoke a stored procedure, MySQL looks up for the name in the database catalog, compiles the stored procedure’s code, place it in a memory area known as a cache, and execute the stored procedure.

If you invoke the same stored procedure in the same session again, MySQL just executes the stored procedure from the cache without having to recompile it.

* A stored procedure can have parameters so you can pass values to it and get the result back.
* A stored procedure may contain control flow statements such as IF, CASE, and LOOP that allow you to implement the code in the procedural way.
* A stored procedure can call other stored procedures or stored functions, which allows you to modulize your code.


## MySQL stored procedures advantages
The following are the advantages of stored procedures.

* *Reduce network traffic* - Stored procedures help reduce the network traffic between applications and MySQL Server. Because instead of sending multiple lengthy SQL statements, applications have to send only the name and parameters of stored procedures.

* *Centralize business logic in the database* - You can use the stored procedures to implement business logic that is reusable by multiple applications. The stored procedures help reduce the efforts of duplicating the same logic in many applications and make your database more consistent.

* *Make database more secure* - The database administrator can grant appropriate privileges to applications that only access specific stored procedures without giving any privileges on the underlying tables.

## MySQL stored procedures disadvantages
* *Resource usages* - If you use many stored procedures, the memory usage of every connection will increase substantially. Besides, overusing a large number of logical operations in the stored procedures will increase the CPU usage because the MySQL is not well-designed for logical operations.

* *Troubleshooting* - It’s difficult to debug stored procedures. Unfortunately, MySQL does not provide any facilities to debug stored procedures like other enterprise database products such as Oracle and SQL Server.

* *Maintenances* - Developing and maintaining stored procedures often requires a specialized skill set that not all application developers possess. This may lead to problems in both application development and maintenance.


## DELIMITER command.

When you write SQL statements, you use the semicolon (`;`) to separate two statements.

To redefine the default delimiter, you use the DELIMITER command:
```sql
DELIMITER delimiter_character
```
The delimiter_character may consist of a single character or multiple characters (e.g., `//` or `$$`). However, you should avoid using the backslash (`\`) because this is the escape character in MySQL.

For example, this statement changes the delimiter to `//`
```sql
DELIMITER //
```
Once change the delimiter, you can use the new delimiter to end a statement as follows:

```sql
SELECT * FROM customers //
SELECT * FROM products //
```
To change the delimiter back to semicolon, you use this statement:

```sql
DELIMITER ;
```


## Using MySQL DELIMITER for stored procedures
A stored procedure, however, consists of multiple statements separated by a semicolon (`;`).

If you use a MySQL client program to define a stored procedure that contains semicolon characters, the MySQL client program will not treat the whole stored procedure as a single statement, but many statements.

Therefore, you must redefine the delimiter from the semicolon (`;`) to anther delimiters temporarily so that you can pass the whole stored procedure to the server as a single statement.


```sql
DELIMITER $$
 
CREATE PROCEDURE sp_name()
BEGIN
  -- statements
END $$
 
DELIMITER ;
```

In this code:

First, change the default delimiter to `$$`
Second, use (`;`) in the body of the stored procedure and $$ after the END keyword to end the stored procedure.
Third, change the default delimiter back to a semicolon (`;`)
In this tutorial, you have learned how to use the MySQL DELIMITER command to change the default delimiter (`;`) to another.

## MySQL CREATE PROCEDURE statement
This query returns all products in the products table from the sample database.

```sql
SELECT * FROM products;
```
The following statement creates a new stored procedure that wraps the query:

```sql
DELIMITER //
 
CREATE PROCEDURE GetAllProducts()
BEGIN
    SELECT *  FROM products;
END //
 
DELIMITER ;
```


Let’s examine the syntax of the stored procedure:
The first and last DELIMITER commands are not a part of the stored procedure. The first DELIMITER command changes the default delimiter to `//` and the last DELIMITER command changes the delimiter back to the default one which is semicolon (`;`).

To create a new stored procedure, you use the `CREATE PROCEDURE` statement.

Here is the basic syntax of the CREATE PROCEDURE statement:
```sql
CREATE PROCEDURE procedure_name(parameter_list)
BEGIN
   statements;
END //
```
In this syntax:
* First, specify the name of the stored procedure that you want to create after the `CREATE PROCEDURE` keywords.
* Second, specify a list of comma-separated parameters for the stored procedure in parentheses after the procedure name.
* Third, write the code between the `BEGIN` `END` block. 
* After the `END` keyword, you place the delimiter character to end the procedure statement.


## Executing a stored procedure
To execute a stored procedure, you use the `CALL` statement:

```sql
CALL stored_procedure_name(argument_list);
```
In this syntax, you specify the name of the stored procedure after the `CALL` keyword. If the stored procedure has parameters, you need to pass arguments inside parentheses following the stored procedure name.

This example illustrates how to call the `GetAllProducts()` stored procedure:

```sql
CALL GetAllProducts();
```

## DROP PROCEDURE 
The DROP PROCEDURE deletes a stored procedure from the database.

The following shows the syntax of the DROP PROCEDURE statement:

```mysql
DROP PROCEDURE [IF EXISTS] stored_procedure_name;
```
In this syntax:

* First, specify the name of of the stored procedure that you want to remove after the `DROP PROCEDURE` keywords.
* Second, use `IF EXISTS` option to conditionally drop the stored procedure if it exists only. (When you drop a procedure that does not exist without using the `IF EXISTS` option, MySQL issues an error. In this case, if you use the IF EXISTS option, MySQL issues a warning instead).

Note that you must have the `ALTER ROUTINE` privilege for the stored procedure to remove it.


Let’s take some examples of using the DROP PROCEDURE statement.
First, create a new stored procedure that returns employee and office information:

```sql
DELIMITER $$
 
CREATE PROCEDURE GetEmployees()
BEGIN
    SELECT 
        firstName, 
        lastName, 
        city, 
        state, 
        country
    FROM employees
    INNER JOIN offices using (officeCode);
    
END$$
 
DELIMITER ;
```
Second, use the `DROP PROCEDURE` to delete the `GetEmployees()` stored procedure:

```sql
DROP PROCEDURE GetEmployees;
```


## ALTER PROCEDURE 
Fortunately, MySQL does not have any statement that allows you to directly modify the parameters and body of the stored procedure.

To make such changes, you must drop ad re-create the stored procedure using the DROP PROCEDURE and CREATE PROCEDURE statements.

## Listing stored procedures
The routines table in the information_schema database contains all information on the stored procedures and stored functions of all databases in the current MySQL server.

To show all stored procedures of a particular database, you use the following query:
```sql
SELECT 
    routine_name
FROM
    information_schema.routines
WHERE
    routine_type = 'PROCEDURE'
        AND routine_schema = '<database_name>';
```
For example, this statement lists all stored procedures in the classicmodels database:
```sql
SELECT 
    routine_name
FROM
    information_schema.routines
WHERE
    routine_type = 'PROCEDURE'
        AND routine_schema = 'classicmodels';

```

## Declaring variables
To declare a variable inside a stored procedure, you use the `DECLARE` statement as follows:

```sql
DECLARE variable_name datatype(size) [DEFAULT default_value];
```
In this syntax:
* First, specify the name of the variable after the DECLARE keyword. The variable name must follow the naming rules of MySQL table column names.
* Second, specify the data type and length of the variable. A variable can have any MySQL data types such as INT, VARCHAR , and DATETIME.
* Third, assign a variable a default value using the DEFAULT option.  If you declare a variable without specifying a default value, its value is NULL.
The following example declares a variable named totalSale with the data type DEC(10,2) and default value 0.0  as follows:

```
DECLARE totalSale DEC(10,2) DEFAULT 0.0;
```
MySQL allows you to declare two or more variables that share the same data type using a single DECLARE statement. The following example declares two integer variables  x and  y, and set their default values to zero.

```sql
DECLARE x, y INT DEFAULT 0;
```
## Assigning variables
Once a variable is declared, it is ready to use. To assign a variable a value, you use the SET statement:

```sql
SET variable_name = value;
```
For example:

```sql
DECLARE total INT DEFAULT 0;
SET total = 10;
```
The value of the total variable is 10  after the assignment.

In addition to the SET statement, you can use the SELECT INTO statement to assign the result of a query to a variable.

The following example illustrates how to declare and use a variable in a stored procedure:

```sql
DELIMITER $$
 
CREATE PROCEDURE GetTotalOrder()
BEGIN
    DECLARE totalOrder INT DEFAULT 0;
    
    SELECT COUNT(*) 
    INTO totalOrder
    FROM orders;
    
    SELECT totalOrder;
END$$
 
DELIMITER ;
```

How it works:
* First, declare a variable totalOrder with a default value of zero. This variable will hold the number of orders from the orders table.
* Second, use the `SELECT INTO`  statement to assign the variable totalOrder the number of orders selected from the orders table
* Third, select the value of the variable totalOrder.


This statement calls the stored procedure GetTotalOrder():
```sql
CALL GetTotalOrder();
```
Here is the output:
```
+------------+
| totalOrder |
+------------+
|        326 |
+------------+
```

## Variable scopes
A variable has its own scope that defines its lifetime. If you declare a variable inside a stored procedure, it will be out of scope when the END statement of stored procedure reaches.

When you declare a variable inside the block BEGIN END, it will be out of scope if the END is reached.

MySQL allows you to declare two or more variables that share the same name in different scopes. Because a variable is only effective in its scope. However, declaring variables with the same name in different scopes is not good programming practice.

A variable whose name begins with the `@` sign is a session variable. It is available and accessible until the session ends.


## stored procedure parameters
Almost stored procedures that you develop require parameters. The parameters make the stored procedure more flexible and useful.

In MySQL, a parameter has one of three modes: 
* *IN parameters* - IN is the default mode. When you define an IN parameter in a stored procedure, the calling program has to pass an argument to the stored procedure. In addition, the value of an IN parameter is protected. It means that even the value of the IN parameter is changed inside the stored procedure, its original value is retained after the stored procedure ends. In other words, the stored procedure only works on the copy of the IN parameter.

* *OUT parameters* - The value of an OUT parameter can be changed inside the stored procedure and its new value is passed back to the calling program. Notice that the stored procedure cannot access the initial value of the OUT parameter when it starts.

* *INOUT parameters* - An INOUT  parameter is a combination of IN  and OUT  parameters. It means that the calling program may pass the argument, and the stored procedure can modify the INOUT parameter, and pass the new value back to the calling program.

## Defining a parameter
Here is the basic syntax of defining a parameter in stored procedures:

```sql
[IN | OUT | INOUT] parameter_name datatype[(length)]
```
In this syntax,

* First, specify the parameter mode, which can be IN , OUT or INOUT , depending on the purpose of the parameter in the stored procedure.
* Second, specify the name of the parameter. The parameter name must follow the naming rules of the column name in MySQL.
* Third, specify the data type and maximum length of the parameter.

## IN parameter example
The following example creates a stored procedure that finds all offices that locate in a country specified by the input parameter countryName:
```sql
DELIMITER //
 
CREATE PROCEDURE GetOfficeByCountry(
    IN countryName VARCHAR(255)
)
BEGIN
    SELECT * 
     FROM offices
    WHERE country = countryName;
END //
 
DELIMITER ;
```
In this example, the countryName is the IN parameter of the stored procedure.

Suppose that you want to find offices locating in the USA, you need to pass an argument (USA) to the stored procedure as shown in the following query:

```sql
CALL GetOfficeByCountry('USA');
```
the output is:
```
+------------+---------------+-----------------+----------------------+--------------+-------+---------+------------+-----------+
| officeCode | city          | phone           | addressLine1         | addressLine2 | state | country | postalCode | territory |
+------------+---------------+-----------------+----------------------+--------------+-------+---------+------------+-----------+
| 1          | San Francisco | +1 650 219 4782 | 100 Market Street    | Suite 300    | CA    | USA     | 94080      | NA        |
| 2          | Boston        | +1 215 837 0825 | 1550 Court Place     | Suite 102    | MA    | USA     | 02107      | NA        |
| 3          | NYC           | +1 212 555 3000 | 523 East 53rd Street | apt. 5A      | NY    | USA     | 10022      | NA        |
+------------+---------------+-----------------+----------------------+--------------+-------+---------+------------+-----------+
```

Because the countryName is the IN parameter, you must pass an argument. Fail to do so will result in an error:

```sql
CALL GetOfficeByCountry();
```
Here is the error:
```
Error Code: 1318. Incorrect number of arguments for PROCEDURE classicmodels.GetOfficeByCountry; expected 1, got 0
```


## OUT parameter example
The following stored procedure returns the number of orders by order status.

```sql
DELIMITER $$
 
CREATE PROCEDURE GetOrderCountByStatus (
    IN  orderStatus VARCHAR(25),
    OUT total INT
)
BEGIN
    SELECT COUNT(orderNumber)
    INTO total
    FROM orders
    WHERE status = orderStatus;
END$$
 
DELIMITER ;
```
The stored procedure GetOrderCountByStatus() has two parameters:
* orderStatus : is the IN parameter specifies the status of orders to return.
* total : is the OUT parameter that stores the number of orders in a specific status.

To find the number of orders that already shipped, you call GetOrderCountByStatus  and pass the order status as of Shipped, and also pass a session variable ( `@total` ) to receive the return value.

```sql
CALL GetOrderCountByStatus('Shipped',@total);
SELECT @total;
```
the output is:
```
+--------+
| @total |
+--------+
|    303 |
+--------+

```


## INOUT parameter example
The following example demonstrates how to use an INOUT parameter in the stored procedure.
```sql
DELIMITER $$
 
CREATE PROCEDURE SetCounter(
    INOUT counter INT,
    IN inc INT
)
BEGIN
    SET counter = counter + inc;
END$$
 
DELIMITER ;
```
In this example, the stored procedure SetCounter()  accepts one INOUT  parameter ( counter ) and one IN parameter ( inc ). It increases the counter by the value of specified by the inc parameter.

These statements illustrate how to call the SetSounter  stored procedure:

```sql
SET @counter = 1;
CALL SetCounter(@counter,5); 
SELECT @counter; 
```
Here is the output:
```
+----------+
| @counter |
+----------+
|        6 |
+----------+
```

if we call the  SetCounter() with the same '@counter' parameter, it will contain the previous value (6).
```sql
CALL SetCounter(@counter,5); 
SELECT @counter; 
```
Here is the output:
```
+----------+
| @counter |
+----------+
|       11 |
+----------+
```


## MySQL simple IF-THEN statement
* Note that MySQL has an IF() function that is different from the IF statement described in this tutorial.

The IF-THEN statement allows you to execute a set of SQL statements based on a specified condition. The following illustrates the syntax of the IF-THEN statement:
```sql
IF condition THEN 
   statements;
END IF;
```
In this syntax:
* First, specify a condition to execute the code between the IF-THEN and END IF . If the condition evaluates to TRUE, the statements between `THEN` and `END IF` will execute. Otherwise, the control is passed to the next statement following the END IF.
* Second, specify the code that will execute if the condition evaluates to TRUE.

We’ll use the customers table from the sample database for the demonstration:
```sql
DELIMITER $$
 
CREATE PROCEDURE GetCustomerLevel(
    IN  pCustomerNumber INT, 
    OUT pCustomerLevel  VARCHAR(20))
BEGIN
    DECLARE credit DECIMAL(10,2) DEFAULT 0;
 
    SELECT creditLimit 
    INTO credit
    FROM customers
    WHERE customerNumber = pCustomerNumber;
 
    IF credit > 50000 THEN
        SET pCustomerLevel = 'PLATINUM';
    END IF;
END$$
 
DELIMITER ;
```
The stored procedure GetCustomerLevel() accepts two parameters: pCustomerNumber and pCustomerLevel.

These statements call the GetCustomerLevel() stored procedure for customer 141 and show the value of the OUT parameter pCustomerLevel:
```sql
CALL GetCustomerLevel(141, @level);
SELECT @level;
```
Because the customer 141 has a credit limit greater than 50,000, its level is set to PLATINUM as expected.
```
+----------+
| @level   |
+----------+
| PLATINUM |
+----------+

```
## MySQL IF-THEN-ELSE statement
In case you want to execute other statements when the condition in the IF branch does not evaluate to TRUE, you can use the IF-THEN-ELSE statement as follows:
```sql
IF condition THEN
   statements;
ELSE
   statements;
END IF;
```
In this syntax, if the condition evaluates to TRUE, the statements between IF-THEN and ELSE execute. Otherwise, the statements between the ELSE and END IF execute.

Let’s modify the GetCustomerLevel() stored procedure. First, drop the GetCustomerLevel() stored procedure:
```sql
DROP PROCEDURE GetCustomerLevel;
```
Then, create the GetCustomerLevel() stored procedure with the new code:
```sql
DELIMITER $$
 
CREATE PROCEDURE GetCustomerLevel(
    IN  pCustomerNumber INT, 
    OUT pCustomerLevel  VARCHAR(20))
BEGIN
    DECLARE credit DECIMAL DEFAULT 0;
 
    SELECT creditLimit 
    INTO credit
    FROM customers
    WHERE customerNumber = pCustomerNumber;
 
    IF credit > 50000 THEN
        SET pCustomerLevel = 'PLATINUM';
    ELSE
        SET pCustomerLevel = 'NOT PLATINUM';
    END IF;
END$$
 
DELIMITER ;
```
In this new stored procedure, we include the ELSE branch. If the credit is not greater than 50,000, we set the customer level to NOT PLATINUM in the block between ELSE and END IF.

The following statements call the stored procedure for customer number 447  and show the value of the OUT parameter pCustomerLevel:

```sql
CALL GetCustomerLevel(447, @level);
SELECT @level;
```
mysql if else statement - output:
```
+--------------+
| @level       |
+--------------+
| NOT PLATINUM |
+--------------+

```

The credit limit of the customer 447 is less than 50,000, therefore, the statement in the ELSE branch executes and sets the value of the OUT parameter pCustomerLevel to NOT PLATINUM.

## MySQL IF-THEN-ELSEIF-ELSE statement
If you want to execute statements conditionally based on multiple conditions, you use the following IF-THEN-ELSEIF-ELSE statement:

```sql
IF condition THEN
   statements;
ELSEIF elseif-condition THEN
   elseif-statements;
...
ELSE
   else-statements;
END IF;
```
In this syntax, if the condition evaluates to TRUE , the statements in the IF-THEN branch executes; otherwise, the next elseif-condition is evaluated.
* If the elseif-condition evaluates to TRUE, the elseif-statement executes; otherwise, the next elseif-condition is evaluated.
* The IF-THEN-ELSEIF-ELSE statement can have multiple ELSEIF branches.
* If no condition in the IF and ELSE IF evaluates to TRUE, the else-statements in the ELSE branch will execute.

We will modify the GetCustomerLevel() stored procedure to use the IF-THEN-ELSEIF-ELSE statement. First, drop the GetCustomerLevel() stored procedure:
```sql
DROP PROCEDURE GetCustomerLevel;
```
Then, create the new GetCustomerLevel() stored procedure that uses the the IF-THEN-ELSEIF-ELSE statement.
```sql
DELIMITER $$
 
CREATE PROCEDURE GetCustomerLevel(
    IN  pCustomerNumber INT, 
    OUT pCustomerLevel  VARCHAR(20))
BEGIN
    DECLARE credit DECIMAL DEFAULT 0;
 
    SELECT creditLimit 
    INTO credit
    FROM customers
    WHERE customerNumber = pCustomerNumber;
 
    IF credit > 50000 THEN
        SET pCustomerLevel = 'PLATINUM';
    ELSEIF credit <= 50000 AND credit > 10000 THEN
        SET pCustomerLevel = 'GOLD';
    ELSE
        SET pCustomerLevel = 'SILVER';
    END IF;
END $$
 
DELIMITER ;
```
In this stored procedure:
* If the credit is greater than 50,000, the level of the customer is PLATINUM.
* If the credit is less than or equal 50,000 and greater than 10,000, then the level of customer is GOLD.
* Otherwise, the level of the customer is SILVER.

These statements call the stored procedure GetCustomerLevel() and show the level of the customer 447:

```sql
CALL GetCustomerLevel(447, @level); 
SELECT @level;
```
the output is:
```
+--------+
| @level |
+--------+
| GOLD   |
+--------+
```

If you test the stored procedure with the customer that has a credit limit of 10000 or less, you will get the output as SILVER.

## CASE statements
The CASE statements make the code more readable and efficient.
Note that if you want to add the if-else logic to an SQL statement, you use the CASE expression which is different from the CASE statement described in this tutorial.

## Simple CASE statement
The following is the basic syntax of the simple CASE statement:
```sql
CASE case_value
   WHEN when_value1 THEN statements
   WHEN when_value2 THEN statements
   ...
   [ELSE else-statements]
END CASE;
```
In this syntax, the simple CASE statement sequentially compares the case_value is with the when_value1, when_value2, … until it finds one is equal. When the CASE finds a case_value equal to a when_value, it executes statements in the corresponding THEN clause.

If CASE cannot find any when_value equal to the case_value, it executes the else-statements in the ELSE clause if the ELSE clause is available.

When the ELSE clause does not exist and the CASE cannot find any when_value equal to the case_value, it issues an error:
```
Case not found for CASE statement
```
Note that the case_value can be a literal value or an expression. The statements can be one or more SQL statements, and cannot have zero statement.

To avoid the error when the  case_value does not equal any when_value, you can use an empty BEGIN END block in the ELSE clause as follows:
```sql
CASE case_value
    WHEN when_value1 THEN ...
    WHEN when_value2 THEN ...
    ELSE 
        BEGIN
        END;
END CASE;
```
The simple CASE statement tests for equality (`=`), you cannot use it to test equality with NULL; because `NULL = NULL` returns `FALSE`.

The following stored procedure illustrates how to use the simple CASE statement:
```sql
DELIMITER $$
 
CREATE PROCEDURE GetCustomerShipping(
    IN  pCustomerNUmber INT, 
    OUT pShipping       VARCHAR(50)
)
BEGIN
    DECLARE customerCountry VARCHAR(100);
 
    SELECT country
    INTO customerCountry 
    FROM customers
    WHERE customerNumber = pCustomerNUmber;
 
    CASE customerCountry
        WHEN  'USA' THEN
           SET pShipping = '2-day Shipping';
        WHEN 'Canada' THEN
           SET pShipping = '3-day Shipping';
        ELSE
           SET pShipping = '5-day Shipping';
    END CASE;
END$$
 
DELIMITER ;
```
This statement calls the stored procedure and passes the customer number 112:
```sql
CALL GetCustomerShipping(112,@shipping);
```
The following statement returns the shipping time of the customer 112:
```
SELECT @shipping;
```
Here is the output:
```
+----------------+
| @shipping      |
+----------------+
| 2-day Shipping |
+----------------+
```

## Searched CASE statement
The simple CASE statement only allows you to compare a value with a set of distinct values.

To perform more complex matches such as ranges, you use the searched CASE statement. The searched CASE statement is equivalent to the IF  statement, however, it’s much more readable than the IF statement.

Here is the basic syntax of the searched CASE statement:

```sql
CASE
    WHEN search_condition1 THEN statements
    WHEN search_condition1 THEN statements
    ...
    [ELSE else-statements]
END CASE;
```
In this syntax, searched CASE evaluates each search_condition in the WHEN clause until it finds a condition that evaluates to TRUE , then it executes the corresponding THEN clause statements.

If no search_condition evaluates to TRUE, the CASE will execute else-statements in the ELSE clause if an ELSE clause is available.

Similar to the simple CASE statement, if you don’t specify an ELSE clause and no condition is TRUE, MySQL raises the same error:
```
Case not found for CASE statement
```
MySQL also does not allow you to have empty statements in the THEN or ELSE clause. If you don’t want to handle the logic in the ELSE clause while preventing MySQL from raising an error in case no search_condition is true, you can use an empty BEGIN END  block in the ELSE clause.

The following example demonstrates how to use a searched CASE statement to find customer level SILVER , GOLD or PLATINUM based on customer’s credit limit.

```sql
DELIMITER $$
 
CREATE PROCEDURE GetDeliveryStatus(
    IN pOrderNumber INT,
    OUT pDeliveryStatus VARCHAR(100)
)
BEGIN
    DECLARE waitingDay INT DEFAULT 0;
    SELECT 
        DATEDIFF(requiredDate, shippedDate)
    INTO waitingDay
    FROM orders
    WHERE orderNumber = pOrderNumber;
    
    CASE 
        WHEN waitingDay = 0 THEN 
            SET pDeliveryStatus = 'On Time';
        WHEN waitingDay >= 1 AND waitingDay < 5 THEN
            SET pDeliveryStatus = 'Late';
        WHEN waitingDay >= 5 THEN
            SET pDeliveryStatus = 'Very Late';
        ELSE
            SET pDeliveryStatus = 'No Information';
    END CASE;    
END$$
DELIMITER ;
```
This statement uses the stored procedure GetDeliveryStatus() to get the delivery status of the order 10100 :
```sql
CALL GetDeliveryStatus(10100,@delivery);
```
The following statement returns the delivery status of the order 10100 :
```
SELECT @delivery;
```
Here is the result:
```
+-----------+
| @delivery |
+-----------+
| Late      |
+-----------+

```


## MySQL CASE vs. IF
Both IF and CASE statements allow you to execute a block of code based on a specific condition. Choosing between IF or CASE sometimes is just a matter of personal preference. Here are some guidelines:
* A simple CASE statement is more readable and efficient than an IF statement when you compare a single expression against a range of unique values.
* When you check complex expressions based on multiple values, the IF statement is easier to understand.
* If you use the CASE statement, you have to make sure that at least one of the CASE condition is matched. Otherwise, you need to define an error handler to catch the error. Note that you do not have to do this with the IF statement.



## Introduction to MySQL LOOP statement
The LOOP statement allows you to execute one or more statements repeatedly.
Here is the basic syntax of the LOOP statement:
```sql
[begin_label:] LOOP
    statement_list
END LOOP [end_label]
```
The LOOP can have optional labels at the beginning and end of the block.

The LOOP executes the statement_list repeatedly. The statement_list may have one or more statements, each terminated by a semicolon (;) statement delimiter.

Typically, you terminate the loop when a condition is satisfied by using the LEAVE statement.

This is the typical syntax of the LOOP statement used with LEAVE statement:

```sql
[label]: LOOP
    ...
    -- terminate the loop
    IF condition THEN
        LEAVE [label];
    END IF;
    ...
END LOOP;
```
The LEAVE statement immediately exits the loop. It works like the break statement in other programming languages like C/C++, and Java.

In addition to the LEAVE statement, you can use the ITERATE statement to skip the current loop iteration and start a new iteration. The ITERATE is similar to the continue statement in C/C++, and Java.


The following statement creates a stored procedure that uses a LOOP loop statement:
```sql
DELIMITER $$
CREATE PROCEDURE LoopDemo()
BEGIN
    DECLARE x  INT;
    DECLARE str  VARCHAR(255);
        
    SET x = 1;
    SET str =  '';
        
    loop_label:  LOOP
        IF  x > 10 THEN 
            LEAVE  loop_label;
        END  IF;
            
        SET  x = x + 1;
        IF  (x mod 2) THEN
            ITERATE  loop_label;
        ELSE
            SET  str = CONCAT(str,x,',');
        END  IF;
    END LOOP;
    SELECT str;
END$$
 
DELIMITER ;
```
In this example:
* The stored procedure constructs a string from the even numbers e.g., 2, 4, and 6.
* The loop_label  before the LOOP statement is in order to use it with the ITERATE and LEAVE statements.
* If the value of  x is greater than 10, the loop is terminated because of the LEAVE statement.
* If the value of the x is an odd number, the ITERATE ignores everything below it and starts a new loop iteration.
* If the value of the x is an even number, the block in the ELSE statement will build the result string from even numbers.

The following statement calls the stored procedure:
```sql
CALL LoopDemo();
```
Here is the output:

```
+-------------+
| str         |
+-------------+
| 2,4,6,8,10, |
+-------------+
```



## MySQL WHILE Loop
The WHILE loop is a loop statement that executes a block of code repeatedly as long as a condition is true.

Here is the basic syntax of the WHILE statement:
```sql
[begin_label:] WHILE search_condition DO
    statement_list
END WHILE [end_label]
```
In this syntax:
* First, specify a search condition after the WHILE keyword.
    * The WHILE checks the search_condition at the beginning of each iteration.
    * If the search_condition evaluates to TRUE, the WHILE executes the statement_list as long as the search_condition is TRUE.
    * The WHILE loop is called a pretest loop because it checks the search_condition before the statement_list executes.
* Second, specify one or more statements that will execute between the DO and END WHILE keywords.
* Third, specify optional labels for the WHILE statement at the beginning and end of the loop construct.

MySQL WHILE loop statement example
First, create a table namedcalendars which stores dates and derived date information such as day, month, quarter, and year:

```sql
CREATE TABLE calendars(
    id INT AUTO_INCREMENT,
    fulldate DATE UNIQUE,
    day TINYINT NOT NULL,
    month TINYINT NOT NULL,
    quarter TINYINT NOT NULL,
    year INT NOT NULL,
    PRIMARY KEY(id)
);
```
Second, create a new stored procedure to insert a date into the calendars table:
```sql
DELIMITER $$
 
CREATE PROCEDURE InsertCalendar(dt DATE)
BEGIN
    INSERT INTO calendars(
        fulldate,
        day,
        month,
        quarter,
        year
    )
    VALUES(
        dt, 
        EXTRACT(DAY FROM dt),
        EXTRACT(MONTH FROM dt),
        EXTRACT(QUARTER FROM dt),
        EXTRACT(YEAR FROM dt)
    );
END$$
 
DELIMITER ;
```
Third, create a new stored procedure LoadCalendars() that loads a number of days starting from a start date into the calendars table.
```sql
DELIMITER $$
 
CREATE PROCEDURE LoadCalendars(
    startDate DATE, 
    day INT
)
BEGIN
    
    DECLARE counter INT DEFAULT 1;
    DECLARE dt DATE DEFAULT startDate;
 
    WHILE counter <= day DO
        CALL InsertCalendar(dt);
        SET counter = counter + 1;
        SET dt = DATE_ADD(dt,INTERVAL 1 day);
    END WHILE;
 
END$$
 
DELIMITER ;
```
The stored procedure LoadCalendars() accepts two arguments:
* startDate is the start date inserted into the calendars table.
* day is the number of days that will be loaded starting from the startDate.

Lets run this query:
```sql
SELECT * FROM calendars;
```
the result is:
```
Empty set (0.00 sec)
```
Then, Lets run the following statement taht calls the stored procedure LoadCalendars() to load 31 days into the calendars table starting from January 1st 2019.
```sql
CALL LoadCalendars('2019-01-01',31);
```
Now, Lets run again this query:
```sql
SELECT * FROM calendars;
```
the result is:
```
+----+------------+-----+-------+---------+------+
| id | fulldate   | day | month | quarter | year |
+----+------------+-----+-------+---------+------+
|  1 | 2019-01-01 |   1 |     1 |       1 | 2019 |
|  2 | 2019-01-02 |   2 |     1 |       1 | 2019 |
|  3 | 2019-01-03 |   3 |     1 |       1 | 2019 |
|  4 | 2019-01-04 |   4 |     1 |       1 | 2019 |
|  5 | 2019-01-05 |   5 |     1 |       1 | 2019 |
|  6 | 2019-01-06 |   6 |     1 |       1 | 2019 |
|  7 | 2019-01-07 |   7 |     1 |       1 | 2019 |
|  8 | 2019-01-08 |   8 |     1 |       1 | 2019 |
|  9 | 2019-01-09 |   9 |     1 |       1 | 2019 |
| 10 | 2019-01-10 |  10 |     1 |       1 | 2019 |
| 11 | 2019-01-11 |  11 |     1 |       1 | 2019 |
| 12 | 2019-01-12 |  12 |     1 |       1 | 2019 |
| 13 | 2019-01-13 |  13 |     1 |       1 | 2019 |
| 14 | 2019-01-14 |  14 |     1 |       1 | 2019 |
| 15 | 2019-01-15 |  15 |     1 |       1 | 2019 |
| 16 | 2019-01-16 |  16 |     1 |       1 | 2019 |
| 17 | 2019-01-17 |  17 |     1 |       1 | 2019 |
| 18 | 2019-01-18 |  18 |     1 |       1 | 2019 |
| 19 | 2019-01-19 |  19 |     1 |       1 | 2019 |
| 20 | 2019-01-20 |  20 |     1 |       1 | 2019 |
| 21 | 2019-01-21 |  21 |     1 |       1 | 2019 |
| 22 | 2019-01-22 |  22 |     1 |       1 | 2019 |
| 23 | 2019-01-23 |  23 |     1 |       1 | 2019 |
| 24 | 2019-01-24 |  24 |     1 |       1 | 2019 |
| 25 | 2019-01-25 |  25 |     1 |       1 | 2019 |
| 26 | 2019-01-26 |  26 |     1 |       1 | 2019 |
| 27 | 2019-01-27 |  27 |     1 |       1 | 2019 |
| 28 | 2019-01-28 |  28 |     1 |       1 | 2019 |
| 29 | 2019-01-29 |  29 |     1 |       1 | 2019 |
| 30 | 2019-01-30 |  30 |     1 |       1 | 2019 |
| 31 | 2019-01-31 |  31 |     1 |       1 | 2019 |
+----+------------+-----+-------+---------+------+
```

## MySQL REPEAT Loop
The REPEAT statement executes one or more statements until a search condition is true.
Here is the basic syntax of the REPEAT loop statement:

```sql
[begin_label:] REPEAT
    statement
UNTIL search_condition
END REPEAT [end_label]
```
* The REPEAT executes the statement until the search_condition evaluates to true.
* The REPEAT checks the search_condition after the execution of statement, therefore, the statement always executes at least once. This is why the REPEAT is also known as a post-test loop.
* The REPEAT statement can have labels at the beginning and at the end. These labels are optional.


This statement creates a stored procedure called RepeatDemo  that uses the REPEAT statement to concatenate numbers from 1 to 9:
```sql
DELIMITER $$
 
CREATE PROCEDURE RepeatDemo()
BEGIN
    DECLARE counter INT DEFAULT 1;
    DECLARE result VARCHAR(100) DEFAULT '';
    
    REPEAT
        SET result = CONCAT(result,counter,',');
        SET counter = counter + 1;
    UNTIL counter >= 10
    END REPEAT;
    
    -- display result
    SELECT result;
END$$
 
DELIMITER ;
```
In this stored procedure:
* First, declare two variables counter and result and set their initial values to 1 and blank. The counter variable is used for counting from 1 to 9 in the loop. And the result variable is used for storing the concatenated string after each loop iteration.
* Second, append counter value to the result variable using the CONCAT() function until the counter is greater than or equal to 10.

The following statement calls the RepeatDemo() stored procedure:

```sql
CALL RepeatDemo();
```
Here is the output:

```
+--------------------+
| result             |
+--------------------+
| 1,2,3,4,5,6,7,8,9, |
+--------------------+
```
 
Query OK, 0 rows affected (0.02 sec)
## MySQL Error Handling in Stored Procedures
When an error occurs inside a stored procedure, it is important to handle it appropriately, such as continuing or exiting the current code block’s execution, and issuing a meaningful error message.

MySQL provides an easy way to define handlers that handle from general conditions such as warnings or exceptions to specific conditions e.g., specific error codes.

## Declaring a handler
To declare a handler, you use the  DECLARE HANDLER statement as follows:

```sql
DECLARE action HANDLER FOR condition_value statement;
```
If a condition whose value matches the  condition_value , MySQL will execute the statement and continue or exit the current code block based on the action .

* The action accepts one of the following values:
    * CONTINUE :  the execution of the enclosing code block ( BEGIN … END ) continues.
    * EXIT : the execution of the enclosing code block, where the handler is declared, terminates.
* The  condition_value specifies a particular condition or a class of conditions that activate the handler. The  condition_value accepts one of the following values:
    * A MySQL error code.
    * A standard SQLSTATE value. Or it can be an SQLWARNING , NOTFOUND or SQLEXCEPTION condition, which is shorthand for the class of SQLSTATE values. The NOTFOUND condition is used for a cursor or  SELECT INTO variable_list statement.
    * A named condition associated with either a MySQL error code or SQLSTATE value.
* The statement could be a simple statement or a compound statement enclosing by the BEGIN and END keywords.

Let’s take some examples of declaring handlers.

The following handler set the value of the  hasError variable to 1 and continue the execution if an SQLEXCEPTION occurs
```sql
DECLARE CONTINUE HANDLER FOR SQLEXCEPTION 
SET hasError = 1;
```
The following handler sets the value of the  RowNotFound variable to 1 and continues execution if there is no more row to fetch in case of a SELECT INTO statement:
```sql
DECLARE CONTINUE HANDLER FOR NOT FOUND 
SET RowNotFound = 1;
```
If a duplicate key error occurs, the following handler issues an error message and continues execution.
```sql
DECLARE CONTINUE HANDLER FOR 1062
SELECT 'Error, duplicate key occurred';
```
## MySQL handler example in stored procedures
First, create a new table named SupplierProductsfor the demonstration:
```sql
CREATE TABLE SupplierProducts (
    supplierId INT,
    productId INT,
    PRIMARY KEY (supplierId , productId)
);
```
The table SupplierProducts stores the relationships between the table suppliers and products. Each supplier may provide many products and each product can be provided by many suppliers. For the sake of simplicity, we don’t create Products and Suppliers tables, as well as the foreign keys in the  SupplierProducts table.

Second, create a stored procedure that inserts product id and supplier id into the SupplierProducts table:
```sql
CREATE PROCEDURE InsertSupplierProduct(
    IN inSupplierId INT, 
    IN inProductId INT
)
BEGIN
    -- exit if the duplicate key occurs
    DECLARE EXIT HANDLER FOR 1062
    BEGIN
     SELECT CONCAT('Duplicate key (',inSupplierId,',',inProductId,') occurred') AS message;
    END;
    
    -- insert a new row into the SupplierProducts
    INSERT INTO SupplierProducts(supplierId,productId)
    VALUES(inSupplierId,inProductId);
    
    -- return the products supplied by the supplier id
    SELECT COUNT(*) 
    FROM SupplierProducts
    WHERE supplierId = inSupplierId;
    
END$$
 
DELIMITER ;
```
How it works.

The following exit handler terminates the stored procedure whenever a duplicate key occurs (with code 1062). In addition, it returns an error message.
```sql
DECLARE EXIT HANDLER FOR 1062
BEGIN
    SELECT CONCAT('Duplicate key (',supplierId,',',productId,') occurred') AS message;
END;
```
This statement inserts a row into the SupplierProducts table. If a duplicate key occurs, the code in the handler section will execute.
```sql
INSERT INTO SupplierProducts(supplierId,productId) 
VALUES(supplierId,productId);
```
Third, call the InsertSupplierProduct() to insert some rows into the SupplierProducts table:
```sql
CALL InsertSupplierProduct(1,1);
CALL InsertSupplierProduct(1,2);
CALL InsertSupplierProduct(1,3);
```
Fourth, attempt to insert a row whose values already exist in the SupplierProducts table:
```sql
CALL InsertSupplierProduct(1,3);
```
Here is the error message:
```
+------------------------------+
| message                      |
+------------------------------+
| Duplicate key (1,3) occurred |
+------------------------------+
```
Because the handler is an EXIT handler, the last statement does not execute:
```
SELECT COUNT(*) 
FROM SupplierProducts
WHERE supplierId = inSupplierId;
```
If  you change the EXIT in the handler declaration to CONTINUE , you will also get the number of products provided by the supplier:
```sql
DROP PROCEDURE IF EXISTS InsertSupplierProduct;
 
DELIMITER $$
 
CREATE PROCEDURE InsertSupplierProduct(
    IN inSupplierId INT, 
    IN inProductId INT
)
BEGIN
    -- exit if the duplicate key occurs
    DECLARE CONTINUE HANDLER FOR 1062
    BEGIN
    SELECT CONCAT('Duplicate key (',inSupplierId,',',inProductId,') occurred') AS message;
    END;
    
    -- insert a new row into the SupplierProducts
    INSERT INTO SupplierProducts(supplierId,productId)
    VALUES(inSupplierId,inProductId);
    
    -- return the products supplied by the supplier id
    SELECT COUNT(*) 
    FROM SupplierProducts
    WHERE supplierId = inSupplierId;
    
END$$
 
DELIMITER ;
```
Finally, call the stored procedure again to see the effect of the CONTINUE handler:
```sql
CALL InsertSupplierProduct(1,3);
```
Here is the output:
```
+----------+
| COUNT(*) |
+----------+
|        3 |
+----------+
```
## MySQL handler precedence
In case you have multiple handlers that handle the same error, MySQL will call the most specific handler to handle the error first based on the following rules:
* An error always maps to a MySQL error code because in MySQL it is the most specific.
* An SQLSTATE may map to many MySQL error codes, therefore, it is less specific.
* An SQLEXCPETION or an SQLWARNING is the shorthand for a class of SQLSTATES values so it is the most generic.
Based on the handler precedence rules,  MySQL error code handler, SQLSTATE handler and SQLEXCEPTION takes the first, second and third precedence.

Suppose that we have three handlers in the handlers in the stored procedure insert_article_tags_3 :
```sql
DROP PROCEDURE IF EXISTS InsertSupplierProduct;
 
DELIMITER $$
 
CREATE PROCEDURE InsertSupplierProduct(
    IN inSupplierId INT, 
    IN inProductId INT
)
BEGIN
    -- exit if the duplicate key occurs
    DECLARE EXIT HANDLER FOR 1062 SELECT 'Duplicate keys error encountered' Message; 
    DECLARE EXIT HANDLER FOR SQLEXCEPTION SELECT 'SQLException encountered' Message; 
    DECLARE EXIT HANDLER FOR SQLSTATE '23000' SELECT 'SQLSTATE 23000' ErrorCode;
    
    -- insert a new row into the SupplierProducts
    INSERT INTO SupplierProducts(supplierId,productId)
    VALUES(inSupplierId,inProductId);
    
    -- return the products supplied by the supplier id
    SELECT COUNT(*) 
    FROM SupplierProducts
    WHERE supplierId = inSupplierId;
    
END$$
 
DELIMITER ;
```
Call the stored procedure to insert a duplicate key:
```sql
CALL InsertSupplierProduct(1,3);
```
Here is the output:
```sql
+----------------------------------+
| Message                          |
+----------------------------------+
| Duplicate keys error encountered |
+----------------------------------+
```
As you see the MySQL error code handler is called.

## Using a named error condition
Let’s start with an error handler declaration.

```sql
DELIMITER $$
 
CREATE PROCEDURE TestProc()
BEGIN
 
    DECLARE EXIT HANDLER FOR 1146 
    SELECT 'Please create table abc first' Message; 
        
    SELECT * FROM abc;
END$$
 
DELIMITER ;
```
What does the number 1146 really mean? Imagine you have stored procedures polluted with these numbers all over places; it will be difficult to understand and maintain the code.

Fortunately, MySQL provides you with the DECLARE CONDITION statement that declares a named error condition, which associates with a condition.

Here is the syntax of the DECLARE CONDITION statement:
```sql
DECLARE condition_name CONDITION FOR condition_value;
```
The condition_value  can be a MySQL error code such as 1146 or a SQLSTATE value. The condition_value is represented by the condition_name .

After the declaration, you can refer to condition_name instead of condition_value .

So you can rewrite the code above as follows:

```sql
DROP PROCEDURE IF EXISTS TestProc;
 
DELIMITER $$
 
CREATE PROCEDURE TestProc()
BEGIN
    DECLARE TableNotFound CONDITION for 1146 ; 
 
    DECLARE EXIT HANDLER FOR TableNotFound 
    SELECT 'Please create table abc first' Message; 
    SELECT * FROM abc;
END$$
 
DELIMITER ;
```
As you can see, the code is more obviously and readable than the previous one. Notice that the condition declaration must appear before handler or cursor declarations.



## Raising Error Conditions with MySQL SIGNAL  Statements
You use the SIGNAL statement to return an error or warning condition to the caller from a stored program e.g., stored procedure, stored function, trigger or event. The SIGNAL  statement provides you with control over which information for returning such as value and messageSQLSTATE.

The following illustrates syntax of the SIGNAL statement:
```sql
SIGNAL SQLSTATE | condition_name;
SET condition_information_item_name_1 = value_1,
    condition_information_item_name_1 = value_2, etc;
```
Following the SIGNAL keyword is a SQLSTATE value or a condition name declared by the  DECLARE CONDITION statement. Notice that the SIGNAL statement must always specify a SQLSTATE value or a named condition that defined with an  SQLSTATE value.

To provide the caller with information, you use the SET clause. If you want to return multiple condition information item names with values, you need to separate each name/value pair by a comma.

The  condition_information_item_name can be MESSAGE_TEXT, MYSQL_ERRORNO, CURSOR_NAME , etc.

The following stored procedure adds an order line item into an existing sales order. It issues an error message if the order number does not exist.

```sql
DELIMITER $$
 
CREATE PROCEDURE AddOrderItem(
                 in orderNo int,
             in productCode varchar(45),
             in qty int, 
                         in price double, 
                         in lineNo int )
BEGIN
    DECLARE C INT;
 
    SELECT COUNT(orderNumber) INTO C
    FROM orders 
    WHERE orderNumber = orderNo;
 
    -- check if orderNumber exists
    IF(C != 1) THEN 
        SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = 'Order No not found in orders table';
    END IF;
    -- more code below
    -- ...
END
```
First, it counts the orders with the input order number that we pass to the stored procedure.

Second, if the number of order is not 1, it raises an error with  SQLSTATE 45000 along with an error message saying that order number does not exist in the orders table.

Notice that 45000 is a generic SQLSTATE value that illustrates an unhandled user-defined exception.

If we call the stored procedure  AddOrderItem() and pass a nonexistent order number, we will get an error message.

```sql
CALL AddOrderItem(10,'S10_1678',1,95.7,1);
```

## Raising Error Conditions with MySQL RESIGNAL  Statements
Besides the SIGNAL  statement, MySQL also provides the RESIGNAL  statement used to raise a warning or error condition.

The RESIGNAL  statement is similar to SIGNAL  statement in term of functionality and syntax, except that:

You must use the RESIGNAL  statement within an error or warning handler, otherwise, you will get an error message saying that “RESIGNAL when handler is not active”. Notice that you can use SIGNAL  statement anywhere inside a stored procedure.
You can omit all attributes of the RESIGNAL statement, even the SQLSTATE value.
If you use the RESIGNAL statement alone, all attributes are the same as the ones passed to the condition handler.

The following stored procedure changes the error message before issuing it to the caller.
```sql
DELIMITER $$
 
CREATE PROCEDURE Divide(IN numerator INT, IN denominator INT, OUT result double)
BEGIN
    DECLARE division_by_zero CONDITION FOR SQLSTATE '22012';
 
    DECLARE CONTINUE HANDLER FOR division_by_zero 
    RESIGNAL SET MESSAGE_TEXT = 'Division by zero / Denominator cannot be zero';
    -- 
    IF denominator = 0 THEN
        SIGNAL division_by_zero;
    ELSE
        SET result := numerator / denominator;
    END IF;
END
```
Let’s call the  Divide() stored procedure.

```sql
CALL Divide(10,0,@result);
```

## Introduction to MySQL cursor
To handle a result set inside a stored procedure, you use a cursor. A cursor allows you to iterate a set of rows returned by a query and process each row individually.
You can use MySQL cursors in stored procedures, stored functions, and triggers.
*MySQL cursor is read-only, non-scrollable and asensitive*:
* Read-only: you cannot update data in the underlying table through the cursor.
* Non-scrollable: you can only fetch rows in the order determined by the SELECT statement. You cannot fetch rows in the reversed order. 
    * In addition, you cannot skip rows or jump to a specific row in the result set.
* Asensitive: there are two kinds of cursors: asensitive cursor and insensitive cursor. An asensitive cursor points to the actual data, whereas an insensitive cursor uses a temporary copy of the data. An asensitive cursor performs faster than an insensitive cursor because it does not have to make a temporary copy of data. However, any change that made to the data from other connections will affect the data that is being used by an asensitive cursor, therefore, it is safer if you do not update the data that is being used by an asensitive cursor. MySQL cursor is asensitive.

## Working with MySQL cursor
First, declare a cursor by using the DECLARE statement:
```sql
DECLARE cursor_name CURSOR FOR SELECT_statement;
```
The cursor declaration must be after any variable declaration. If you declare a cursor before the variable declarations, MySQL will issue an error. A cursor must always associate with a SELECT statement.

Next, open the cursor by using the OPEN statement. The OPEN statement initializes the result set for the cursor, therefore, you must call the OPEN statement before fetching rows from the result set.

```sql
OPEN cursor_name;
```
Then, use the FETCH statement to retrieve the next row pointed by the cursor and move the cursor to the next row in the result set.

```sql
FETCH cursor_name INTO variables list;
```
After that, check if there is any row available before fetching it.

Finally, deactivate the cursor and release the memory associated with it  using the CLOSE statement:

```sql
CLOSE cursor_name;
```
It is a good practice to always close a cursor when it is no longer used.

When working with MySQL cursor, you must also declare a NOT FOUND handler to handle the situation when the cursor could not find any row.

Because each time you call the FETCH statement, the cursor attempts to read the next row in the result set. When the cursor reaches the end of the result set, it will not be able to get the data, and a condition is raised. The handler is used to handle this condition.

To declare a NOT FOUND handler, you use the following syntax:

```sql
DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = 1;
```
The finished is a variable to indicate that the cursor has reached the end of the result set. Notice that the handler declaration must appear after variable and cursor declaration inside the stored procedures.

We’ll develop a stored procedure that creates an email list of all employees in the employees table in the sample database.
First, declare some variables, a cursor for looping over the emails of employees, and a NOT FOUND handler:
```sql
DECLARE finished INTEGER DEFAULT 0;
DECLARE emailAddress varchar(100) DEFAULT "";

-- declare cursor for employee email
DEClARE curEmail CURSOR FOR SELECT email FROM employees;

-- declare NOT FOUND handler
DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = 1;
```
Next, open the cursor by using the OPEN statement:

```sql
OPEN curEmail;
```
Then, iterate the email list, and concatenate all emails where each email is separated by a semicolon(;):

```sql
getEmail: LOOP
    FETCH curEmail INTO emailAddress;
    IF finished = 1 THEN 
        LEAVE getEmail;
    END IF;
    -- build email list
    SET emailList = CONCAT(emailAddress,";",emailList);
END LOOP getEmail;
```
After that, inside the loop, we used the finished variable to check if there is an email in the list to terminate the loop.
Finally, close the cursor using the CLOSE statement:
```sql
CLOSE email_cursor;
```
The createEmailList stored procedure is as follows:
```sql
DELIMITER $$
CREATE PROCEDURE createEmailList (
    INOUT emailList varchar(4000)
)
BEGIN
    DECLARE finished INTEGER DEFAULT 0;
    DECLARE emailAddress varchar(100) DEFAULT "";
 
    -- declare cursor for employee email
    DEClARE curEmail 
        CURSOR FOR 
            SELECT email FROM employees;
 
    -- declare NOT FOUND handler
    DECLARE CONTINUE HANDLER 
        FOR NOT FOUND SET finished = 1;
 
    OPEN curEmail;
 
    getEmail: LOOP
        FETCH curEmail INTO emailAddress;
        IF finished = 1 THEN 
            LEAVE getEmail;
        END IF;
        -- build email list
        SET emailList = CONCAT(emailAddress,";",emailList);
    END LOOP getEmail;
    CLOSE curEmail;
 
END$$
DELIMITER ;
```
You can test the createEmailList stored procedure using the following script:
```sql
SET @emailList = ""; 
CALL createEmailList(@emailList); 
SELECT @emailList;
```