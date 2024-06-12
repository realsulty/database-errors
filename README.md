# Database Errors :

## - Illogical Error
CREATE TABLE dbo.src(i int, a varchar(20)); 
INSERT dbo.src VALUES(1,'aaaaa'),(2,'bbbbbbbbbbbbbbb');
DECLARE @t table(a varchar(10));
INSERT @t SELECT a FROM dbo.src WHERE i = 1; 
DROP TABLE dbo.src;

This will be the illogical part -> If you change that to WHERE i = 2;
You would get this* error message:

> Msg 2628, Level 16, State 1
String or binary data would be truncated in table 'tempdb.dbo.#A60B3C91', column 'a'. 
Truncated value: 'bbbbbbbbbb'.
has context menu

## - Wrong data type: 

SELECT * FROM table1 WHERE DOJ = '19-Feb-14'; 

Data type mismatch in criteria expression.
Fix : SELECT * FROM table1 WHERE DOJ = '2014-02-19'; 

## - Wrong code :
CREATE TABLE model (
name VARCHAR(3),
age int(25) ,
);
FIX: in line 2 no comma needed


## - Missing data (ARERR 556)
 It is apparent that for some columns no value is provided. Missing data can have different representations. It is common to use the special NULL value for representing a missing value for a record. String columns can contain NULL values but may also use the empty string ('') to represent a missing value.

 example:
 >  CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);

INSERT INTO Persons
VALUES(2);

INSERT INTO people Persons(2, '', 'Doe',28);

The exact error message may vary depending on the database system, but it typically indicates that a NOT NULL constraint is violated. Here are examples of what you might see in MySQL or MariaDB:
> ERROR 1048 (23000): Column 'name' cannot be null

An empty string ('') is technically a valid value for a NOT NULL column, but it might not be meaningful for certain data fields, and it's generally good practice to ensure meaningful data is inserted.

## - Duplicate Data :
CREATE UNIQUE NONCLUSTERED INDEX [ID_UIDX] ON [dbo].[REPLT_100]([newIDs] ASC)ON [PRIMARY]GO

This is generated when you try to insert duplicate values into a column or columns with a unique index. In the earlier primary key violation tip, the error number was 2627.

 ### You might face the exception “data exception - invalid character value for cast “ if you are:
trying to process a number value
given as varchar/string
which includes numeric characters only and a group separator and/or decimal character.
INSERT INTO T VALUES ('9,25'); <-- The comma should be fixed
## - Censored or truncated data:
 insert Customers(CustomerID, CompanyName, Phone) 
Values('101','Southwinds','19126602729')

> 1>2- >3>4>5>....<1 row affected>Msg 815 Censored or truncated data
 Level 16, State 4, Server SP1001, Line 1string or binary data would be truncated.

 Table structure for the customers table that the length of one or more fields is NOT big enough to hold, placing  BIGINT value in a predefined SMLLINT DATA type.

## 2. Task 
INSERT INTO country
VALUES('BRA', ‘Brazil’,'South America','South America',8,547,403.00);
