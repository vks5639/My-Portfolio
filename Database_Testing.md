## Setting Up the Environment

- Install MAMP:

    - Download and install MAMP, which is a local server environment that allows you to run PHP and MySQL on your computer.
    - Objective: Set up a local development environment that mimics a real-world server setup for testing purposes.

- Create a Database:

    - Start the MAMP servers and open PHPMyAdmin, a tool for managing MySQL databases.
    - Objective: Create a new database named classic_models and import the SQL file classic_models.sql, which contains the initial schema and data.
    - Credentials:
      - Host: localhost
      - Port: 3306
      - User: root
      - Password: root
    - Create a new database called classic_models
    - Import the sql file classic_models in the database

## Schema Testing

- Schema Testing Overview:

    - The purpose of schema testing is to ensure that the database schema is correctly set up. This includes verifying the presence of tables, the correct number of columns, data types, and other constraints.

- SQL Command:

    - DESCRIBE information_schema.columns;
    - Objective: This command provides the structure of the columns in the information_schema database, which contains metadata about all the columns in the database.

- Test Cases (TC001 - TC008):

    - TC001: Check if the required tables are present in the database schema.
    - TC002: Verify that table names follow the correct naming conventions.
    - TC003: Ensure that the correct number of columns exists in each table.
    - TC004: Check that column names adhere to naming conventions.
    - TC005: Verify the data types of columns.
    - TC006: Confirm that columns have the correct size limits.
    - TC007: Check for nullable fields in the table columns.
    - TC008: Validate the presence of primary and foreign keys in the columns.
    - Objective: Validate that the database schema matches the expected structure and constraints.

![Test Cases](https://i.imgur.com/9gD4CdJ.png)

## Stored Procedures

- Overview
    - A stored procedure is a set of SQL statements that can be stored in the database and executed as a single unit. They can accept input parameters, return output parameters, and contain programmatic constructs like loops and conditionals.

## Benefits of Stored Procedures

- Performance: Reduced network traffic by sending a single call to execute multiple statements. Execution plans are cached and reused, reducing parsing time.
- Security: Enhanced security by encapsulating business logic within the database. Permissions can be granted on the procedure instead of individual tables.
- Maintainability: Centralized logic makes it easier to update and maintain, reducing code duplication across multiple applications.
- Consistency: Ensures consistent implementation of business rules, reducing the risk of errors from complex SQL statements.
- Modularity: Breaks down complex operations into manageable, reusable code blocks, facilitating structured and organized programming.

- Example Stored Procedures:

    - SelectAllCustomers: Selects all customers from the customers table.
    - SelectAllCustomersByCity: Selects all customers from a specific city.
    - get_order_by_cust: Retrieves order statistics for a specific customer.
    - Objective: Automate and encapsulate common database operations to improve efficiency and reduce errors.



## Stored Procedure: 

### SelectAllCustomers

delimiter //

create procedure SelectAllCustomers()
Begin
    select * from customers;
End //

delimiter ;

call SelectAllCustomers();

Stored Procedure: SelectAllCustomersByCity

delimiter //

### SelectAllCustomersByCity

create procedure SelectAllCustomersByCity(IN mycity varchar(50))
begin
    select * from customers where city=mycity;
end //

delimiter ;

call SelectAllCustomersByCity('Singapore');

Stored Procedure: SelectAllCustomersByCityAndPin

delimiter //

### SelectAllCustomersByCityAndPin

create procedure SelectAllCustomersByCityAndPin(IN mycity varchar(50), IN pcode varchar(15))
begin
    select * from customers where city=mycity and postalCode=pcode;
end //

delimiter ;

call SelectAllCustomersByCityAndPin('Singapore', '079903');

### get_order_by_cust

delimiter //

CREATE PROCEDURE get_order_by_cust(
    IN cust_no INT,
    OUT shipped INT,
    OUT canceled INT,
    OUT resolved INT,
    OUT disputed INT)
BEGIN
    -- shipped
    SELECT count(*) INTO shipped FROM orders WHERE customerNumber = cust_no AND status = 'Shipped';

    -- canceled
    SELECT count(*) INTO canceled FROM orders WHERE customerNumber = cust_no AND status = 'Canceled';

    -- resolved
    SELECT count(*) INTO resolved FROM orders WHERE customerNumber = cust_no AND status = 'Resolved';

    -- disputed
    SELECT count(*) INTO disputed FROM orders WHERE customerNumber = cust_no AND status = 'Disputed';
END //

DELIMITER ;

call get_order_by_cust(141, @shipped, @canceled, @resolved, @disputed);
select @shipped, @canceled, @resolved, @disputed;

### GetCustomerShipping

delimiter //

CREATE PROCEDURE GetCustomerShipping(
    IN pCustomerNumber INT,
    OUT pShipping VARCHAR(50))
BEGIN
    DECLARE customerCountry VARCHAR(100);

    SELECT country INTO customerCountry FROM customers WHERE customerNumber = pCustomerNumber;

    CASE customerCountry
        WHEN 'USA' THEN
            SET pShipping = '2-day Shipping';
        WHEN 'Canada' THEN
            SET pShipping = '3-day Shipping';
        ELSE
            SET pShipping = '5-day Shipping';
    END CASE;
END //

DELIMITER ;

call GetCustomerShipping(112, @shipping);
select @shipping;

### InsertSupplierProduct

delimiter //

CREATE PROCEDURE InsertSupplierProduct(IN inSupplierId INT, IN inProductId INT)
BEGIN
    -- exit if the duplicate key occurs
    DECLARE EXIT HANDLER FOR 1062 SELECT 'Duplicate keys error encountered' Message;
    DECLARE EXIT HANDLER FOR SQLEXCEPTION SELECT 'SQLException encountered' Message;
    DECLARE EXIT HANDLER FOR SQLSTATE '23000' SELECT 'SQLSTATE 23000' ErrorCode;

    -- insert a new row into the SupplierProducts
    INSERT INTO SupplierProducts(supplierId, productId) VALUES(inSupplierId, inProductId);

    -- return the products supplied by the supplier id
    SELECT COUNT(*) FROM SupplierProducts WHERE supplierId = inSupplierId;
END //

DELIMITER ;

call InsertSupplierProduct(1,1);
call InsertSupplierProduct(1,2);
call InsertSupplierProduct(1,3);

## Stored Procedures Testing

![Test Cases](https://i.imgur.com/xBOvnsX.png)

![Test Cases Final](https://i.imgur.com/Ro1oNpa.png)

# Automated Database Testing (Maven Project)

- Overview:

    - Automated testing ensures that the stored procedures and database operations function correctly and meet the required specifications.
      
- Dependencies:

    - Maven: Used to manage project dependencies.
    - TestNG: A testing framework used to automate the execution of test cases.
    - MySQL Connector/J: A driver that allows Java applications to connect to MySQL databases.

- Test Script Example:

    - Setup: Establish a connection to the database.
    - Test Cases:
        - Verify the existence of stored procedures.
        - Execute stored procedures and compare their results with direct SQL queries to ensure correctness.
        - Test stored procedures that handle input parameters, such as filtering customers by city or retrieving order details by customer.

- Teardown: Close the database connection.

- Objective: Ensure that stored procedures execute correctly, return the expected results, and handle input parameters appropriately.

- Add dependencies in pom.xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>DatabaseTesting</groupId>
	<artifactId>DatabaseTesting</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<dependencies>
		<dependency>
			<groupId>com.mysql</groupId>
			<artifactId>mysql-connector-j</artifactId>
			<version>8.3.0</version>
		</dependency>

		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>3.12.0</version>
		</dependency>

		<dependency>
			<groupId>org.testng</groupId>
			<artifactId>testng</artifactId>
			<version>7.10.2</version>
			<scope>test</scope>
		</dependency>
	</dependencies>

</project>

- Test Script

package storedProcedureTesting;
import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.Types;

import org.apache.commons.lang3.StringUtils;
import org.testng.Assert;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;


public class SPTesting {
	
	Connection con = null;
	Statement stmt = null;
	ResultSet rs;
	CallableStatement cstmt = null;
	ResultSet rs1;
	ResultSet rs2;
	
	
	@BeforeClass
	void setup() throws SQLException {
		con = DriverManager.getConnection("jdbc:mysql://localhost:3306/classic_models","root" ,"root");
		
	}
	
	@AfterClass
	void tearDown() throws SQLException {
		con.close();
	}
	
	@Test(priority=1)
	public void test_storedProcedureExists() throws SQLException {
		stmt=con.createStatement();
		rs=stmt.executeQuery("SHOW PROCEDURE STATUS WHERE Name='SelectAllCustomers'");
		rs.next();
		Assert.assertEquals(rs.getString("Name"), "SelectAllCustomers");
	}
	
	@Test(priority=2)
	void test_SelectAllCustomers() throws SQLException {
		cstmt=con.prepareCall("{CALL SelectAllCustomers()}");
		rs1=cstmt.executeQuery();
		
		Statement stmt=con.createStatement();
		rs2=stmt.executeQuery("SELECT * FROM customers");
		
		Assert.assertTrue(compareResultSets(rs1,rs2));
		
	}
	
	@Test(priority=3)
	void test_customersByCity() throws SQLException {
		Statement stmt = con.createStatement();
		rs1=stmt.executeQuery("SELECT * FROM customers WHERE city='Singapore'");
		
		CallableStatement cstmt = con.prepareCall("{CALL SelectCustomersByCity(?)}");
		cstmt.setString(1, "Singapore");
		rs2=cstmt.executeQuery();
		
		Assert.assertTrue(compareResultSets(rs1, rs2));
	}
	
	@Test(priority=4)
	void test_customersByCityAndPinCode() throws SQLException {
		Statement stmt = con.createStatement();
		rs1=stmt.executeQuery("SELECT * FROM customers WHERE city='Singapore' and postalCode='079903'");
		
		CallableStatement cstmt = con.prepareCall("{CALL customersByCityAndPin(?,?)}");
		cstmt.setString(1, "Singapore");
		cstmt.setString(2, "079903");
		rs2=cstmt.executeQuery();
		
		Assert.assertTrue(compareResultSets(rs1, rs2));
	}
	
	
	@Test(priority=5)
	void test_get_order_by_cust() throws SQLException {
	    cstmt = con.prepareCall("{call get_order_by_cust(?,?,?,?,?)}");
	    cstmt.setInt(1, 141);

	    cstmt.registerOutParameter(2, Types.INTEGER);
	    cstmt.registerOutParameter(3, Types.INTEGER);
	    cstmt.registerOutParameter(4, Types.INTEGER);
	    cstmt.registerOutParameter(5, Types.INTEGER);

	    cstmt.executeQuery();

	    int shipped = cstmt.getInt(2);
	    int canceled = cstmt.getInt(3);
	    int resolved = cstmt.getInt(4);
	    int disputed = cstmt.getInt(5);

	 

	    Statement stmt = con.createStatement();
	    rs = stmt.executeQuery("select (SELECT count(*) as 'shipped' FROM orders WHERE customerNumber = 141 AND status = 'Shipped') as Shipped, " +
	                           "(SELECT count(*) as 'canceled' FROM orders WHERE customerNumber = 141 AND status = 'Canceled') as Canceled, " +
	                           "(SELECT count(*) as 'resolved' FROM orders WHERE customerNumber = 141 AND status = 'Resolved') as Resolved, " +
	                           "(SELECT count(*) as 'disputed' FROM orders WHERE customerNumber = 141 AND status = 'Disputed') as Disputed;");

	    rs.next();

	    int exp_shipped = rs.getInt("shipped");
	    int exp_canceled = rs.getInt("canceled");
	    int exp_resolved = rs.getInt("resolved");
	    int exp_disputed = rs.getInt("disputed");

	    if (shipped == exp_shipped && canceled == exp_canceled && resolved == exp_resolved && disputed == exp_disputed)
	        Assert.assertTrue(true);
	    else
	        Assert.assertTrue(false);
	}
	
	
	@Test(priority=6)
	void test_get_customerShipping() throws SQLException {
	    cstmt = con.prepareCall("{call GetCustomerShipping(?,?)}");
	    cstmt.setInt(1, 112);
	    cstmt.registerOutParameter(2, Types.VARCHAR);

	    cstmt.executeQuery();

	    String shippingTime = cstmt.getString(2);
	    
	    Statement stmt = con.createStatement();
	    rs = stmt.executeQuery("SELECT country, CASE WHEN country='USA' THEN '2-day Shipping' WHEN country='Canada' THEN '3-day Shipping' ELSE '5-day Shipping' END as ShippingTime FROM customers WHERE customerNumber=112");

	    rs.next();

	    String exp_shipping_Time = rs.getString("ShippingTime");
	    
	    Assert.assertEquals(shippingTime, exp_shipping_Time);
	}

	
	public boolean compareResultSets(ResultSet resultSet1, ResultSet resultSet2) throws SQLException {
        while (resultSet1.next()) {
            resultSet2.next();
            int count = resultSet1.getMetaData().getColumnCount();
            for (int i = 1; i <= count; i++) {
                if (!StringUtils.equals(resultSet1.getString(i), resultSet2.getString(i))) {
                    return false;
                }
            }
        }
        return true;
    }
	
	

}


![Console Output](https://i.imgur.com/T2IXAGn.png)

## Stored Functions

- Overview:

	- A stored function in SQL is similar to a stored procedure but returns a single value. It is often used to encapsulate business logic that can be reused in SQL queries.

- Comparison: Stored Procedure vs. Stored Function:

	- Stored Procedure: Can perform multiple operations and may return multiple values. Used for tasks like data manipulation.
	- Stored Function: Performs a calculation or operation and returns a single value. Used within SQL queries.
	- Objective: Understand the difference between procedures and functions to decide when to use each.

![Stored Procedure vs. Stored Function(https://i.imgur.com/kbX8SjY.png)

- Creating a Stored Function Example:

	- CustomerLevel: A function that returns a customer’s level (e.g., PLATINUM, GOLD, SILVER) based on their credit limit.
	- Objective: Implement and test a stored function that categorizes customers based on their credit limit.


DELIMITER //

CREATE FUNCTION CustomerLevel(credit DECIMAL(10,2)) RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN
    DECLARE customerLevel VARCHAR(20);

    IF credit > 50000 THEN
        SET customerLevel = 'PLATINUM';
    ELSEIF credit >= 10000 AND credit <= 50000 THEN
        SET customerLevel = 'GOLD';
    ELSEIF credit < 10000 THEN
        SET customerLevel = 'SILVER';
    END IF;

    RETURN customerLevel;
END //

DELIMITER ;

- Calling the Stored Function
SELECT CustomerLevel(creditLimit) FROM customers;

![](https://i.imgur.com/1v90BRq.png)


Calling Stored Function using Stored Procedure

DELIMITER //

CREATE PROCEDURE GetCustomerLevel(
    IN customerNo INT,
    OUT customerLevel VARCHAR(20)
)
BEGIN
    DECLARE credit DEC(10,2) DEFAULT 0;

    -- get credit limit of a customer
    SELECT creditLimit INTO credit FROM customers WHERE customerNumber = customerNo;

    -- call the function
    SET customerLevel = CustomerLevel(credit);
END //

DELIMITER ;


CALL GetCustomerLevel(131, @customerLevel);
SELECT @customerLevel;



![](https://i.imgur.com/XyaGsSl.png)

- Test Cases to verify StoredFunction

![](https://i.imgur.com/f0qVWvj.png)

package storedFunctionTesting;

import static org.testng.Assert.assertEquals;

import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.Types;

import org.apache.commons.lang3.StringUtils;
import org.testng.Assert;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

public class SFTesting {

	Connection con = null;
	Statement stmt = null;
	ResultSet rs;
	CallableStatement cstmt = null;
	ResultSet rs1;
	ResultSet rs2;

	@BeforeClass
	void setup() throws SQLException {
		con = DriverManager.getConnection("jdbc:mysql://localhost:3306/classic_models", "root", "root");

	}

	@AfterClass
	void tearDown() throws SQLException {
		con.close();
	}

	@Test(priority = 1)
	public void test_storedFunctionExists() throws SQLException {
		stmt = con.createStatement();
		rs = stmt.executeQuery("SHOW FUNCTION STATUS WHERE Name='CustomerLevel'");
		rs.next();
		Assert.assertEquals(rs.getString("Name"), "CustomerLevel");
	}
	
	
	@Test(priority = 2)
	public void test_CustomerLevel_SQL() throws SQLException {
		rs1 = con.createStatement().executeQuery("SELECT customerName, CustomerLevel(creditLimit) FROM customers");
		rs2 = con.createStatement().executeQuery("SELECT customerName,CASE WHEN creditLimit > 50000 THEN 'PLATINUM' WHEN creditLimit >= 10000 AND creditLimit < 50000 THEN 'GOLD' WHEN creditLimit <10000 THEN 'SILVER' END as customerlevel FROM customers");
		Assert.assertEquals(compareResultSets(rs1, rs2), true);
	}
	
	@Test(priority=3)
	void test_CustomerLevel_StoredProcedure() throws SQLException {
	    cstmt = con.prepareCall("{call GetCustomerLevel(?,?)}");
	    cstmt.setInt(1, 131);
	    cstmt.registerOutParameter(2, Types.VARCHAR);

	    cstmt.executeQuery();

	    String customerLevel = cstmt.getString(2);
	    
	    Statement stmt = con.createStatement();
	    rs = stmt.executeQuery("SELECT customerName,\r\n"
	    		+ "CASE\r\n"
	    		+ "WHEN creditLimit > 50000 THEN 'PLATINUM'\r\n"
	    		+ "WHEN creditLimit >= 10000 AND creditLimit < 50000 THEN 'GOLD'\r\n"
	    		+ "WHEN creditLimit <10000 THEN 'SILVER'\r\n"
	    		+ "END as customerlevel FROM customers where customerNumber=131;");

	    rs.next();

	    String exp_customerlevel = rs.getString("customerlevel");
	    
	    Assert.assertEquals(customerLevel, exp_customerlevel);
	}
	
	
	
	public boolean compareResultSets(ResultSet resultSet1, ResultSet resultSet2) throws SQLException {
        while (resultSet1.next()) {
            resultSet2.next();
            int count = resultSet1.getMetaData().getColumnCount();
            for (int i = 1; i <= count; i++) {
                if (!StringUtils.equals(resultSet1.getString(i), resultSet2.getString(i))) {
                    return false;
                }
            }
        }
        return true;
    }

}

![](https://i.imgur.com/tVCW8rB.png)

![](https://i.imgur.com/tVCW8rB.png)



## Triggers in SQL



A Trigger in SQL is a special type of stored procedure that automatically executes (or "fires") in response to specific events on a table or view. Triggers are typically used to enforce data integrity, automatically update related data, or log changes.



Key Points about Triggers:

Automatic Execution: Triggers execute automatically when a specified event occurs, such as INSERT, UPDATE, or DELETE.

Event-Driven: Triggers are associated with specific events on a table (e.g., before or after an INSERT).

Cannot Be Called Directly: Unlike stored procedures, triggers cannot be called directly by a user or application. They are invoked automatically by the database system.





Trigger vs. Stored Procedure

Triggers are event-driven and automatically executed when certain events occur in the database. They are mainly used for maintaining data integrity and automating certain tasks related to data changes.



Stored Procedures are manually invoked, can perform complex operations, and are more versatile in terms of functionality. They are generally used for tasks that require explicit execution by a user or application.



In MySQL, triggers are classified based on the timing of their execution relative to the triggering event and the type of event that activates them. There are two main dimensions to classify triggers: timing and event.

Types of Triggers in MySQL

Timing:

BEFORE Trigger: Executes before the triggering event occurs. Typically used to validate or modify data before it is inserted, updated, or deleted.

AFTER Trigger: Executes after the triggering event occurs. Often used for logging changes, updating related data, or enforcing complex business rules.

Event:

INSERT Trigger: Fired when a new row is inserted into a table.

UPDATE Trigger: Fired when an existing row in a table is modified.

DELETE Trigger: Fired when a row is deleted from a table.

Combining Timing and Event

The timing and event dimensions combine to create six possible types of triggers in MySQL:

BEFORE INSERT Trigger: Executes before a new row is inserted into the table.

AFTER INSERT Trigger: Executes after a new row has been inserted into the table.

BEFORE UPDATE Trigger: Executes before an existing row in the table is updated.

AFTER UPDATE Trigger: Executes after an existing row in the table has been updated.

BEFORE DELETE Trigger: Executes before a row is deleted from the table.

AFTER DELETE Trigger: Executes after a row has been deleted from the table.

Summary:

BEFORE Triggers are useful for validation and pre-processing before the actual event occurs.

AFTER Triggers are typically used for actions that need to occur after the data change has been committed, like logging or cascading updates to other tables.









DROP TABLE IF EXISTS WorkCenters;

DROP TABLE IF EXISTS WorkCenterStats;



CREATE TABLE WorkCenters (

id INT AUTO_INCREMENT PRIMARY KEY,

name VARCHAR(100) NOT NULL,

capacity INT NOT NULL

);



CREATE TABLE WorkCenterStats (

totalCapacity INT NOT NULL

);





```java
DELIMITER //
```

CREATE TRIGGER before_workcenters_insert BEFORE INSERT ON Workcenters FOR EACH ROW

BEGIN

DECLARE rowcount INT;



```java
SELECT COUNT(*) INTO rowcount FROM WorkCenterStats;
```



IF rowcount > 0 THEN

UPDATE WorkCenterStats SET totalCapacity = totalCapacity + new.capacity;

ELSE

INSERT INTO WorkCenterStats(totalCapacity) VALUES(new.capacity);

END IF;

END //



```java
DELIMITER ;
```



SHOW TRIGGERS;





CREATE TABLE members (

id INT AUTO_INCREMENT,

name VARCHAR(100) NOT NULL,

email VARCHAR(255) NOT NULL,

birthDate DATE,

PRIMARY KEY (id)

);



CREATE TABLE reminders (

id INT AUTO_INCREMENT,

memberId INT,

message VARCHAR(255) NOT NULL,

PRIMARY KEY (id, memberId)

);



```java
DELIMITER //
```



CREATE TRIGGER after_members_insert

AFTER INSERT ON members

FOR EACH ROW

BEGIN

IF NEW.birthDate IS NULL THEN

INSERT INTO reminders(memberId, message)

VALUES(NEW.id, CONCAT('Hi ', NEW.name, ', please update your date of birth.'));

END IF;

END //



```java
DELIMITER ;
```





CREATE TABLE sales (

id INT AUTO_INCREMENT,

product VARCHAR(100) NOT NULL,

quantity INT NOT NULL DEFAULT 0,

fiscalYear SMALLINT NOT NULL,

fiscalMonth TINYINT NOT NULL,

CHECK(fiscalMonth >= 1 AND fiscalMonth <= 12),

CHECK(fiscalYear BETWEEN 2000 and 2050),

CHECK(quantity >= 0),

UNIQUE(product, fiscalYear, fiscalMonth),

PRIMARY KEY(id)

);



INSERT INTO sales(product, quantity, fiscalYear, fiscalMonth) VALUES

('2003 Harley-Davidson Eagle Drag Bike', 120, 2020, 1),

('1969 Corvair Monza', 150, 2020, 1),

('1970 Plymouth Hemi Cuda', 200, 2020, 1);



```java
SELECT * FROM sales;
```



```java
DELIMITER //
```



CREATE TRIGGER before_sales_update

BEFORE UPDATE ON sales

FOR EACH ROW

BEGIN

DECLARE errorMessage VARCHAR(255);

SET errorMessage = CONCAT('The new quantity ', NEW.quantity,

' cannot be 3 times greater than the current quantity ',

OLD.quantity);



IF NEW.quantity > OLD.quantity * 3 THEN

SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = errorMessage;

END IF;

END //



```java
DELIMITER ;
```









CREATE TABLE SalesChanges (

id INT AUTO_INCREMENT PRIMARY KEY,

salesId INT,

beforeQuantity INT,

afterQuantity INT,

changedAt TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP

);



```java
DELIMITER //
```



CREATE TRIGGER after_sales_update

AFTER UPDATE ON sales

FOR EACH ROW

BEGIN

IF OLD.quantity <> NEW.quantity THEN

INSERT INTO SalesChanges(salesId, beforeQuantity, afterQuantity)

VALUES(OLD.id, OLD.quantity, NEW.quantity);

END IF;

END //



```java
DELIMITER ;
```







CREATE TABLE Salaries (

employeeNumber INT PRIMARY KEY,

validFrom DATE NOT NULL,

salary DECIMAL(12, 2) NOT NULL DEFAULT 0

);



INSERT INTO salaries(employeeNumber, validFrom, salary) VALUES

(1002, '2000-01-01', 50000),

(1056, '2000-01-01', 60000),

(1076, '2000-01-01', 70000);



CREATE TABLE SalaryArchives (

id INT PRIMARY KEY AUTO_INCREMENT,

employeeNumber INT,

validFrom DATE NOT NULL,

salary DECIMAL(12, 2) NOT NULL,

deletedAt TIMESTAMP DEFAULT NOW()

);



```java
DELIMITER //
```



CREATE TRIGGER before_salaries_delete BEFORE DELETE ON salaries FOR EACH ROW

BEGIN

INSERT INTO SalaryArchives(employeeNumber, validFrom, salary)

VALUES(OLD.employeeNumber, OLD.validFrom, OLD.salary);

END //



```java
DELIMITER ;
```











DROP TABLE IF EXISTS Salaries;



CREATE TABLE Salaries (

employeeNumber INT PRIMARY KEY,

validFrom DATE NOT NULL,

salary DECIMAL(12,2) NOT NULL DEFAULT 0

);



INSERT INTO salaries(employeeNumber, validFrom, salary) VALUES

(1002, '2000-01-01', 50000),

(1056, '2000-01-01', 60000),

(1076, '2000-01-01', 70000);



CREATE TABLE SalaryBudgets (

total DECIMAL(15,2) NOT NULL

);



INSERT INTO SalaryBudgets(total)

```java
SELECT SUM(salary) FROM Salaries;
```



CREATE TRIGGER after_salaries_delete AFTER DELETE ON Salaries FOR EACH ROW

BEGIN

UPDATE SalaryBudgets SET total = total - OLD.salary;

END;





