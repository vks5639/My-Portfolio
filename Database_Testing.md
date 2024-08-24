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

## Schema Testing



DESCRIBE information_schema.columns; provides the structure of the columns table in the information_schema database.

This table contains metadata about all the columns in the database, including details such as column names, data types, nullability, keys, and default values.

This command is useful for understanding the schema and attributes of columns across all tables in the database.













TC001	Check table presence in database schema





TC002	Check table name conventions







TC003	Check number of columns in a table





TC004	Check column names in a table







TC005	Check data type of columns in table





TC006	Check size of the columns in a table





TC007	Check nulls fields in a table





TC008	Check column keys in a table





Tests Passed





## Stored Procedures

- A stored procedure is a set of SQL statements that can be stored in the database and executed as a single unit. They can accept input parameters, return output parameters, and contain programmatic constructs such as loops and conditionals.


## Benefits of Stored Procedures

- Performance: Reduced network traffic by sending a single call to execute multiple statements.
Execution plans are cached and reused, reducing parsing time.



- Security:
Enhanced security through encapsulation of business logic.

Permissions can be granted on the procedure instead of individual tables.



- Maintainability:
Centralized logic, making it easier to update and maintain.

Reduces duplication of code across multiple applications.



- Consistency:
Ensures consistent implementation of business rules.

Reduces risk of errors from complex SQL statements.



- Modularity:
Breaks down complex operations into manageable, reusable code blocks.

Facilitates structured and organized programming.

These benefits make stored procedures a powerful tool for efficient, secure, and maintainable database management.



Stored Procedure: SelectAllCustomers

delimiter //

create procedure SelectAllCustomers()
Begin
    select * from customers;
End //

delimiter ;

call SelectAllCustomers();

Stored Procedure: SelectAllCustomersByCity

delimiter //

create procedure SelectAllCustomersByCity(IN mycity varchar(50))
begin
    select * from customers where city=mycity;
end //

delimiter ;

call SelectAllCustomersByCity('Singapore');

Stored Procedure: SelectAllCustomersByCityAndPin

delimiter //

create procedure SelectAllCustomersByCityAndPin(IN mycity varchar(50), IN pcode varchar(15))
begin
    select * from customers where city=mycity and postalCode=pcode;
end //

delimiter ;

call SelectAllCustomersByCityAndPin('Singapore', '079903');

Stored Procedure: get_order_by_cust

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

Stored Procedure: GetCustomerShipping

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

Stored Procedure: InsertSupplierProduct

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

Stored Procedures Testing













Automated Database Testing (Maven Project)

Add dependencies in pom.xml

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





Test Script

```java
package storedProcedureTesting;
```





```java
import java.sql.CallableStatement;
```

```java
import java.sql.Connection;
```

```java
import java.sql.DriverManager;
```

```java
import java.sql.ResultSet;
```

```java
import java.sql.SQLException;
```

```java
import java.sql.Statement;
```

```java
import java.sql.Types;
```



```java
import org.apache.commons.lang3.StringUtils;
```

```java
import org.testng.Assert;
```

```java
import org.testng.annotations.AfterClass;
```

```java
import org.testng.annotations.BeforeClass;
```

```java
import org.testng.annotations.Test;
```





```java
public class SPTesting {
```



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





## Stored Function in SQL



A Stored Function in SQL is a reusable set of SQL statements that returns a single value. It can accept parameters, perform calculations, and return the result. Stored functions are often used to encapsulate business logic that can be used in SQL queries.



Stored Procedure vs. Stored Function

In summary, stored procedures are used for performing actions and can return multiple results, while stored functions are designed for calculations and always return a single value, which can be used directly in SQL queries.





## Creating a Stored Function



```java
DELIMITER //
```



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



```java
DELIMITER ;
```



Calling the Stored Function



```java
SELECT CustomerLevel(creditLimit) FROM customers;
```







## Calling Stored Function using Stored Procedure



```java
DELIMITER //
```



```java
CREATE PROCEDURE GetCustomerLevel(
```

IN customerNo INT,

OUT customerLevel VARCHAR(20)

)

BEGIN

DECLARE credit DEC(10,2) DEFAULT 0;



-- get credit limit of a customer

```java
SELECT creditLimit INTO credit FROM customers WHERE customerNumber = customerNo;
```



-- call the function

SET customerLevel = CustomerLevel(credit);

END //



```java
DELIMITER ;
```





```java
CALL GetCustomerLevel(131, @customerLevel);
```

```java
SELECT @customerLevel;
```







## Test Cases to verify StoredFunction







```java
package storedFunctionTesting;
```



```java
import static org.testng.Assert.assertEquals;
```



```java
import java.sql.CallableStatement;
```

```java
import java.sql.Connection;
```

```java
import java.sql.DriverManager;
```

```java
import java.sql.ResultSet;
```

```java
import java.sql.SQLException;
```

```java
import java.sql.Statement;
```

```java
import java.sql.Types;
```



```java
import org.apache.commons.lang3.StringUtils;
```

```java
import org.testng.Assert;
```

```java
import org.testng.annotations.AfterClass;
```

```java
import org.testng.annotations.BeforeClass;
```

```java
import org.testng.annotations.Test;
```



```java
public class SFTesting {
```



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





