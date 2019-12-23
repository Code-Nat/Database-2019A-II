# Stored Function
A stored function is a special kind stored program that returns a single value. Typically, you use stored functions to encapsulate common formulas or business rules that are reusable among SQL statements or stored programs.


## Differences between procedures and functions
* Procedure parameters can be defined as input-only, output-only, or both. This means that a procedure can pass values back to the caller by using output parameters. These values can be accessed in statements that follow the CALL statement. Functions have only input parameters. As a result, although both procedures and functions can have parameters, procedure parameter declaration differs from that for functions.
* Functions return value, so there must be a RETURNS clause in a function definition to indicate the data type of the return value. Also, there must be at least one RETURN statement within the function body to return a value to the caller. RETURNS and RETURN do not appear in procedure definitions.
* A procedure is invoked using a CALL statement, and can only pass back values using output variables. A function can be called from inside a statement just like any other function (that is, by invoking the function's name), and can return a scalar value.
* Stored procedures and functions do not share the same namespace. It is possible to have a procedure and a function with the same name in a database

## Creating a stored function 
To create a stored function, you use the CREATE FUNCTION statement.
The following illustrates the basic syntax for creating a new stored function:

```sql
DELIMITER $$
 
CREATE FUNCTION function_name(
    param1,
    param2,…
)
RETURNS datatype
[NOT] DETERMINISTIC
BEGIN
 -- statements
END $$
 
DELIMITER ;
```
In this syntax:
* First, specify the name of the stored function that you want to create after `CREATE FUNCTION`  keywords.
* Second, list all parameters of the stored function inside the parentheses followed by the function name. By default, all parameters are the IN parameters. You cannot specify IN , OUT or INOUT modifiers to parameters
* Third, specify the data type of the return value in the RETURNS statement, which can be any valid MySQL data types.
* Fourth, specify if a function is deterministic or not using the DETERMINISTIC keyword. (A deterministic function always returns the same result for the same input parameters whereas a non-deterministic function returns different results for the same input parameters). If you don’t use DETERMINISTIC or NOT DETERMINISTIC, MySQL uses the NOT DETERMINISTIC option by default.
* Fifth, write the code in the body of the stored function in the BEGIN END block. Inside the body section, you need to specify at least one RETURN statement. The RETURN statement returns a value to the calling programs. Whenever the RETURN statement is reached, the execution of the stored function is terminated immediately.

Let’s take the example of creating a stored function. We will use the customers table in the sample database for the demonstration.
The following CREATE FUNCTION statement creates a function that returns the customer level based on credit:
```sql
DELIMITER $$
 
CREATE FUNCTION CustomerLevel(
    credit DECIMAL(10,2)
) 
RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN
    DECLARE customerLevel VARCHAR(20);
 
    IF credit > 50000 THEN
        SET customerLevel = 'PLATINUM';
    ELSEIF (credit >= 50000 AND credit <= 10000) THEN
        SET customerLevel = 'GOLD';
    ELSEIF credit < 10000 THEN
        SET customerLevel = 'SILVER';
    END IF;
    -- return the customer level
    RETURN (customerLevel);
END$$
DELIMITER ;
```
## Calling a stored function in an SQL statement
The following statement uses the CustomerLevel stored function:
```sql
SELECT 
    customerName, 
    CustomerLevel(creditLimit)
FROM customers
ORDER BY customerName;

```
## Calling a stored function in a stored procedure
The following statement creates a new stored procedure that calls the CustomerLevel() stored function:
```sql

DELIMITER $$
 
CREATE PROCEDURE GetCustomerLevel(
    IN  customerNo INT,  
    OUT customerLevel VARCHAR(20)
)
BEGIN
 
    DECLARE credit DEC(10,2) DEFAULT 0;
    
    -- get credit limit of a customer
    SELECT creditLimit 
    INTO credit
    FROM customers
    WHERE customerNumber = customerNo;
    
    -- call the function 
    SET customerLevel = CustomerLevel(credit);
END$$
 
DELIMITER ;
```
The following illustrates how to call the GetCustomerLevel() stored procedure:
```
CALL GetCustomerLevel(-131,@customerLevel);
SELECT @customerLevel;
```

## MySQL DROP FUNCTION statement
The DROP FUNCTION statement drops a stored function. Here is the syntax of the DROP FUNCTION statement:
```sql
DROP FUNCTION [IF EXISTS] function_name;
```
In this syntax, you specify the name of the stored function that you want to drop after the DROP FUNCTION keywords.

The IF EXISTS option allows you to conditionally drop a stored function if it exists. It prevents an error from arising if the function does not exist.

We’ll use the orders table in the sample database for the demonstration.
First, create a new function called OrderLeadTime that calculates the number of days between ordered date and required date:

```sql
DELIMITER $$
 
CREATE FUNCTION OrderLeadTime (
    orderDate DATE,
    requiredDate DATE
) 
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN requiredDate - orderDate;
END$$
 
DELIMITER ;
```
Then, use the DROP FUNCTION statement to drop the function OrderLeadTime:

```sql
DROP FUNCTION OrderLeadTime;
```

## Listing stored functions 
MySQL data dictionary has a routines table that stores information about the stored functions of all databases in the current MySQL server.

This query finds all stored functions in a particular database:
```sql
SELECT routine_name
FROM information_schema.routines
WHERE routine_type = 'FUNCTION' AND routine_schema = '<database_name>';
```
For example, the following statement returns all stored functions in the classicmodels database:
```sql
SELECT routine_name
FROM information_schema.routines
WHERE routine_type = 'FUNCTION'  AND routine_schema = 'classicmodels';
```