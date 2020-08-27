# SQL

## Connecting a Database

First set up a hosted SQL database on the cloud (AWS etc. )

Login to the database using your favourite SQL client (Datagrip etc.)

We first need to create a database
```
CREATE DATABASE <DBNAME>;
```

Open the SQL file to seed data

Select the database to be used
Edit the SQL file and insert this line on top
```
USE <DBNAME>;
```

Run the SQL file, with the database set as the target

SQL Documentation and Cheatsheet: https://popsql.com/learn-sql

#### SQL clauses
FROM, WHERE, ORDER BY, LIMIT, GROUP BY, HAVING, DISTINCT

## Select

Select Everything from a Table
```
SELECT * FROM users
```

Select a Column from a Table
```
SELECT user_login FROM users
```

Select Multiple Columns
```
SELECT user_login, user_pass FROM users
```

Limit number of rows of data
```
SELECT * FROM users LIMIT 2 
```

Offset row selection
```
SELECT user_login FROM users OFFSET 5
```

Sort Data
```
Select * FROM users Order By users.user_email DESC
```

## Filter
`WHERE` is used to filter rows of data
```
SELECT * FROM products WHERE price = 11.99
SELECT * FROM products WHERE price < 9.99
SELECT * FROM products WHERE book_name = "WILD"
```

List of operators
```
= equality 
<> non-equality 
!= non-equality 
< less than 
<= less than or equal 
> greater than 
>= greater than or equal 
!> not greater than 
BETWEEN 
IS NULL 
AND 
OR
```

## IN
- In allows us to filter rows based on multiple values
- Similar to .isin() in Python
```
SELECT users.user_email 
FROM users 
WHERE id IN "caraadams@gmail.com, jamie@gmail.com"
```

## Pattern Search
`LIKE` allows us to search for patterns. The % wildcard represents unknown characters. 
```
SELECT * FROM products WHERE author LIKE "%Miller%"
```

## Aggregation
```
SELECT SUM(price) FROM products
```

List of aggregate functions
```
AVG() 
COUNT()
MAX()
MIN()
SUM()
```

## Operations
We can perform operations on multiple columns. 

For example, we can multiply the values in 2 columns:
```
SELECT price*quantity FROM products
```

## Adding Data
`INSERT INTO` is used to add data into a database. 
If you aren’t adding values for all the columns in a table, you have to specify the columns you want values to be added to. 
```
INSERT INTO states (state, drink, year, image) VALUES ("New Jersey", "Vodka", 2018, "vodka.jpg")
```

## Update Data
Always use WHERE with UPDATES. 
If not, all records in the table will be updated. 
```
UPDATE states SET drink = "Chocolate Oat Milk" WHERE state = "Delaware" 
```

## Delete Data
```
DELETE FROM states WHERE state = "Florida"
```
## CREATE Table
### Basic Definitions
**Primary Key**
A field in a table that uniquely identifies each row/record in a database table. Each row must contain unique values, and cannot have NULL values. Each table can only have one primary key. 

**Unsigned**
An integer that can only hold positive numbers 

**Signed**
An integer that can hold both positive and negative numbers

**Collation**
Defines your character data. Is your data in English? Japanese? Russian? Defining your collation helps your database understand the type of data that it will be holding, in order to more efficiently store and process requests. The default collation in MYSQL is "latin1_swedish_ci" — which is fine to use. Although, in most cases (and assuming you are creating a primarily English language website), you could switch it to "utf8_general_ci" for a slight improvement in efficiency. 

**AI (Auto-increment)**
Tells a table in your database to numerically increase by one (+1) every time a new row is created. 

### SQL Datatypes
**TINYINT(Size)**
A very small integer. Signed range is from -128 to 127. Unsigned range is from 0 to 255. "Size" specifies the maximum display width (which is 255)

**INT(Size)**
A medium integer. Signed range is from -2147483648 to 2147483647. Unsigned range is from 0 to 4294967295. "Size" specifies the maximum display width (which is 255).

**BIGINT(Size)**
A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. Unsigned range is from 0 to 1844674407370955

**VARCHAR(256)**
A VARIABLE length string that can contain letters, numbers, and special characters. The length can be from 0 to 65535

**LONGTEXT**
A string with up to 4,294,967,295 characters

**DECIMAL(size, d)**
"Size" is the total number of digitals. "d" is the number of digits after the decimal point. The maximum number for size is 65. The maximum number for d is 30. The default value for size is 10. The default value for d is 0. For money, a suitable recommendation is to use DECIMAL(10, 2)

**DATETIME**
Used to set the date, Google instructions for more info on this!

**TIMESTAMP**
The current timestamp (in UNIX time) when the row was created.

### Sample Use Case
If we were an e-commerce site, we would create a `users` table, a `products` table, and a `purchases` table. The purchases table would have a Primary ID, a user id, a products id, quantity and order number. 

## SQL Joins
- SQL Join allows us to combine data from different tables
- Similar to Horizontal Concat in Python
```
SELECT posts.post_title, users.user_nicename FROM posts
JOIN users ON users.ID = posts.post_author
```

### Types of Joins
- INNER JOIN
- FULL OUTER JOIN
- LEFT JOIN
- RIGHT JOIN

## Union
Union allows us to concat data vertically from different tables

Example: find all emails from both tables
```
SELECT users.user_email FROM users
UNION 
SELECT comments.comment_author_email FROM comments
```

## Groupby
Example: Display the number of comments each post has
```
SELECT posts.post_title, count(comments.ID) as "# of Comments" FROM posts 
LEFT JOIN comments ON posts.ID = comments.post_id
Group BY posts.ID
```

## Subquery
A subquery is basically a query within a query

Example:  Find the emails of users which have blog posts in draft mode
```
SELECT users.user_email FROM users 
WHERE id IN (SELECT post_author FROM posts WHERE post_status = "draft")
```


## AS Alias
AS alias is used to give a table or column a temporary name

Table Alias Example:
```
SELECT p.post_title, u.user_nicename FROM posts AS p
JOIN users AS u ON u.ID = p.post_author
```

Column Alias Example: 
```
SELECT p.post_title as "Post Title", u.user_nicename, count(*) as "Total Posts" FROM posts AS p
JOIN users AS u ON u.ID = p.post_author
```

## Change Table Schema
We can use ALTER TABLE to add new columns
```
ALTER TABLE comments ADD post_id INTEGER NULL;
```

#### Adding Foreign key
```
ALTER TABLE comments ADD FOREIGN KEY (post_id) REFERENCES posts(ID);
```

## Delete Tables
### TRUNCATE
Truncate will delete all the rows of any given table (but the table will still exist)
```
TRUNCATE TABLE facebook_users
```

### DROP
DROP deletes all the rows of data, and the table will also be deleted.
```
DROP TABLE facebook_users
```
