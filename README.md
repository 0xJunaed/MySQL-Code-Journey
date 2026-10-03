# MySQL Code Journey Full

🔗 **[View Live Notion Document](https://deadpan-hacksaw-7b3.notion.site/MySQL-Code-Journey-Full-9ba1c023d9a4835c9d1281a4a58cb459)**

# Introduction

- MySQL has its own REPL console and we can use this to run our code.
- To enter the MySQL environment from our terminal, we have to use the following command structure:
    - `mysql -u root -p` : User-Name : `root`, Password : xxxxxxx
    - `u root`: Tells the system your username is "root" (the default admin).
    - `p`: Prompts the system to ask for your password.
    - For Linux distro : don’t need to use `u root -p` , it doesn’t require password on the linux , by Just `sudo mysql` command we can get into the DB !
- **SQL Language** are Declarative Programming - MySQL, PostgreSQL etc.
Mean you do not have to write logic or step-by-step instructions. Instead, You tell the database *what* you want. The Database Management System (DBMS) figures out the internal logic of *how* to grab it for you.
    
    Once you enter the correct password, your terminal changes into the MySQL REPL console. You are no longer directly controlling your machine's operating system; you are now talking directly to the MySQL Server.
    

# Code

### **Programming Part** -

### To clear the MySQL console

- There `\` backslash is to execute bash query, so we can run any bash command with backslash & bang `\!`.

```sql
\! clear
// FOR WINDOWS POWERSHELL
\! cls

// bash
\! ls

// Another short curt to clean terminal using keyword
CTRL + L
```

### 1 List all the databases

```sql
show databases;
```

### 2 Create a new Database

```sql
CREATE DATABASE <name of the database>;

// Ex :
CREATE DATABASE Blogging;
```

### 3 Start working on a Database

```sql
USE <name of the DB>;

// Ex :
USE Blogging;
```

### 4 Delete the whole database

```sql
DROP DATABASE <name of the DB>;

// Ex :
DROP DATABASE Blogging;
```

### 5 List all the tables of a database

```sql
SHOW TABLES;
```

### 6 Create A table

```sql
CREATE TABLE <name> (attribute1name attribute1type extra, attribute2name attribute2type extra . . . . );

// Ex :
CREATE TABLE Blogs (id INT, title VARCHAR(20), content VARCHAR(1200));

CREATE TABLE Actor (
 Name varchar(20),
 Gender ENUM("Male", "Female", "Transgender"),
  Charges Decimal );
```

### 7 Drop A Table

```sql
DROP TABLE <name>;

// EX :
DROP TABLE Blogs;
```

### To Describe A Table

- It will show all details about the DB include type of the Colum if have any extra field/properties. ( Upper case or lower case will not effect the code bcz we know that SQL NON-Case sensitive Language )

```sql
DESCRIBE <DB NAME>;

//EX :
Describe Actors;
//or
DES Actors;
```

### Example A table with extra properties -

```sql
// FIRST TRY
CREATE TABLE Actors (
Name Varchar(20) NOT NULL ,
 Gender ENUM("Male", "Female") NOT NULL ,
 Charges Decimal,
 Id int AUTO_INCREMENT
);
ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key

// SECOND TRY
CREATE TABLE Actors (
Name Varchar(20) NOT NULL ,
 Gender ENUM("Male", "Female") NOT NULL ,
 Charges Decimal,
 Id int AUTO_INCREMENT,
 PRIMARY KEY (Id)
);

// Now if we do
Desc Actors;

+---------+-----------------------+------+-----+---------+----------------+
| Field   | Type                  | Null | Key | Default | Extra          |
+---------+-----------------------+------+-----+---------+----------------+
| Name    | varchar(20)           | NO   |     | NULL    |                |
| Gender  | enum('Male','Female') | NO   |     | NULL    |                |
| Charges | decimal(10,0)         | YES  |     | NULL    |                |
| Id      | int                   | NO   | PRI | NULL    | auto_increment |
+---------+-----------------------+------+-----+---------+----------------+
4 rows in set (0.07 sec)

// we will get this OutPut
```

- `Name VARCHAR(20) NOT NULL,` : After writing the `Name` of the column (`Name` is the column name (the attribute) ), we define its data type `VARCHAR(20)`, which means it can store text up to 20 characters. Then we add an extra property `NOT NULL`. This means the `Name` attribute must always have a value and cannot be empty (`NULL`). So every record must have a name, otherwise the database will not allow the entry.
- Also, for the `Gender` column we have added the same extra property `NOT NULL`. This means `Gender` can never be `NULL`, so every record must include a value for it.
- `ID` is used to uniquely identify each actor. To automatically increase the ID number, we use a property called `AUTO_INCREMENT`. What this does is, whenever you create a new entry in the actor table, it takes the last used ID and adds +1 to it. So each new record gets a unique ID automatically without manual input.
- `ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key`
- This error occurs because `AUTO_INCREMENT` can only be used on one column, and that column must be defined as a key.
- There is a concept of keys in a table. As we know, `ID` is used to uniquely identify every actor or record. A `PRIMARY KEY` is a special type of key that ensures each value is unique and not `NULL`, so every record can be identified individually. In simple terms, the `ID` column should be set as the primary key of the table. Without setting it as `PRIMARY KEY`, we cannot use `AUTO_INCREMENT` on it.

### Re-Declare Or Defined A Table

- If we try to re-declare or create a table that already exists in the database, it will throw an error:

```sql
CREATE TABLE Actors (
Name Varchar(20) NOT NULL ,
Gender ENUM("Male", "Female") NOT NULL ,
Charges Decimal,
Id int AUTO_INCREMENT,
PRIMARY KEY (Id)
);
ERROR 1050 (42S01): Table 'actors' already exists
```

- `ERROR 1050 (42S01): Table 'actors' already exists` This happens because the database does not allow creating a table with the same name twice.
- But there is a way to avoid this error. We can use an extra part of the query called `IF NOT EXISTS` after the table name `Actors`. This means the table will only be created if a table with the name `Actors` does not already exist. If it already exists, the query will simply do nothing and will not throw any error.

```sql
CREATE TABLE Actors IF NOT EXISTS (
Name Varchar(20) NOT NULL ,
Gender ENUM("Male", "Female") NOT NULL ,
Charges Decimal,
Id int AUTO_INCREMENT,
 PRIMARY KEY (Id)
);
```

### 8 INSERT Data in a Table

- To insert data into a table in MySQL, you use the `INSERT INTO` statement followed by the **table name**. After that, you can optionally specify the *column* names in *parentheses*. This step is important because it defines the exact order in which values will be inserted. The values you provide must match both the order and the data types of these columns.
- Next, you use the `VALUES` keyword to provide the actual data. Inside parentheses, you list the values in the same order as the columns you specified. Each value corresponds directly to one column.
- If you want to insert multiple rows at once, you can include multiple sets of values. Each set is placed inside its own parentheses and separated by commas. The order of values must remain consistent for every row, matching the column list each time.

```sql
INSERT INTO <table name> ( Name, Gender, Charges) VALUES (VAL1, VAL2, VAL3);

//EX : For One Entries
INSERT INTO Actors ( Name, Gender, Charges) VALUES ("Hrithik Roshan", "Male", 155360);

// For Multiple Entries
INSERT INTO Actors ( Name, Gender, Charges) VALUES ("Hrithik Roshan", "Male", 155360), ("Chris Evaes", "Male", 567979), ("RDJ", "Male", 497971), ("Julia", "Female", 67791);
```

- If you forget or miss to give any row value then it will throw a error like this :

```sql
INSERT INTO Actors ( Name, Gender, Charges) VALUES ("Shiba Sila", "Male");
ERROR 1136 (21S01): Column count doesn't match value count at row 1
```

### 9 Retrieve everything from the table

- To retrieve data from a table , wen can use the `SELECT` statement. After `SELECT`, you specify the column **names (attributes)** you want to **retrieve**. If you need multiple columns, you separate them with commas, and the result will include only those columns.
- If you want to retrieve all the data from a table, you can also use  after `SELECT`. This tells MySQL to return every column for each row in the table. Then you specify the **table name** to indicate where the data should come from.
- You may also notice an `Id` column where values increase automatically. This usually means the column is set as auto-increment. In that case, you don’t need to provide a value for it when inserting data—MySQL will generate it automatically

```sql
SELECT <attribute1>, <attribute2> FROM <NAME OF THE TABLE>;

//EX :
SELECT NAME, GENDER FROM ACTORS; // AS WELL THERE CASE DOESN`T MATTER

// FOR GETTING ALL THE ATTRIBUTE
SELECT * FRMOM <Table Name>;

// EX :
SELECT * FROM ACTORS;
+----------------+--------+---------+----+
| Name           | Gender | Charges | Id |
+----------------+--------+---------+----+
| Hrithik Roshan | Male   |  155360 |  1 |
| Chris Evaes    | Male   |  567979 |  2 |
| RDJ            | Male   |  497971 |  3 |
| Julia          | Female |   67791 |  4 |
+----------------+--------+---------+----+
```

- There  `SELECT` can also usable as a print command to print text on the terminal
    
    ```sql
    SELECT 'What is your name Brother ?' // there it will print the string 
    //under the single quotation
    ```
    

### 10 Where clause

- Whenever we query data from a table with something like `SELECT *`, all the rows get printed every time, right? But we don't need all the rows every single time. What we can do instead is filter out the rows using the **`WHERE` clause** . You can think of a clause as an extra expression that you can add to your query to do some specific computation.
- As we know, **SQL is a declarative language**. This means we are just going to tell the database exactly what you want! For example, you just tell it, "Give me all the *properties* of those actors who have **charges greater** than this *value*, " and it will **fetch** them for you. You don't have to tell it *how* to loop through the data; you just declare your conditions!
- Now, to do this **filtering**, there are a lot of basic **operators** you can use inside MySQL Technically, you don't need to remember all of these because whenever you have a real use case, you can just Google it. But it's good to know how they work -
    - **Greater than** (`>`), **Less than** (`<`), **Greater than or equal to** (`>=`), **Less than or equal to** (`<=`), **Not equal to** (`!=`), **Equality operator** (`=`) — remember, inside MySQL queries, the equality operator is determined by just a *single* equals sign, not double equals like you'd see in JavaScript or C !

```sql
 select * from actors WHERE Charges > 500000;
 select * from actors WHERE ID > 3;
 select * from actors WHERE Name = "chris evaes";

 // OPERATORS
 >, < >=, <=, !=, <> (not equals), = (equals), IN, etc
```

### 11 Like Query

Now, we are already querying the data with the `WHERE` clause, but there is a very interesting operator that exists called the **`LIKE` operator**.

```sql
//syntax
SELECT Col1, Col2, ... FROM Table Where Column1 Like %Srting%;

//Ex;
select * from actors WHERE Name like "Chris%"; // prefix match
// Here it denotes prefix matching i.e. get me all the rows where the name
// Starts from Chris and after that it can have anything.

 select * from actors WHERE Name like "%Roshan"; // suffix match
 // Here it denotes that give me all the rows where Name might start with
 // Anything but it always ends with Roshan.

 select * from actors WHERE Name like "%R%"; // Substring match
 
 // REAL LIFE EXAMPLE ,IN QUESTION IT HAS SAID ME TO PRINT ONLY THOSE NAME THAT
 // START WITH VOWEL'S ALSO UNIQUELY!  SO HOW GONNNA WE CHECK 5 VOWEL CHARACTER FOR EACH NAME ?
 
SELECT DISTINCT CITY   
FROM STATION 
WHERE CITY LIKE 'A%' 
   OR CITY LIKE 'E%' 
   OR CITY LIKE 'I%' 
   OR CITY LIKE 'O%' 
   OR CITY LIKE 'U%';
// We are going to learn about the DISTINCT query later. For now, just think of it as a Query that prints unique entities
   
// More example 
SELECT DISTINCT CITY 
FROM STATION 
WHERE (CITY LIKE 'A%' OR CITY LIKE 'E%' OR CITY LIKE 'I%' OR CITY LIKE 'O%' OR  CITY LIKE 'U%')
  AND (CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR  CITY LIKE '%u');

```

- Let's say you want to get actors whose names **start** with **"Chris"**, end with **"Roshan"**, or have **"he"** somewhere inside their *name*. For these kinds of situations, you do **string matching** using the `LIKE` query. There are three main types of string matching you can do:
    - **Prefix Matching:** This is when you want your string to start with a specific word. You use the `%` wildcard after your prefix. For example, if you write `WHERE name LIKE 'Chris%'`, this denotes prefix matching. It means "get me all the rows where the **Name column** name starts with *Chris*, and after that, it can have *anything*
    - **Suffix Matching:** What if you want to find names that end with something specific? You put the `%` wildcard at the beginning. If you do `LIKE '%Roshan'`, this tells the database that the string might start with anything, but it *always* ends with *Roshan* . (And don't worry, by default in MySQL, this is case-insensitive so it will still find it!)
    - **Substring Matching:** If you want to find an actor having the letter **"R"** *somewhere* in their name—maybe at the start, in the middle, or at the end—you just wrap your string in wildcards: `LIKE '%R%'` . This is substring matching, meaning as long as the string has **"R"** as a substring inside it, it will match!

**And here is the best part :** This `LIKE` query isn't only limited to `SELECT` queries  You can use it pretty much anywhere you want to list down data! For instance, if you want to list all of your databases that end with **"schema"**, you can just type `SHOW DATABASES LIKE '%schema'`. So there can be some pretty advanced use cases for `LIKE` queries!

```sql
show databases Like "%schema";
```

### NOT LIKE Query

- There is a **`NOT LIKE`** query that works as the opposite of a **`LIKE`** query. Now, let’s say you want to get names that do not start with a specific character, like a **vowel**. How can you find those names? For this kind of situation, we use a different type of string matching called a **`NOT LIKE**` query.
    - Same as a **`LIKE`** query, there are three main types of string matching we can do: **prefix matching, suffix matching, and substring matching**. We already know the details of these three types and how they work. The logic is the same, with no difference, so we are not going into the details. ****

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY NOT LIKE 'A%'
AND CITY NOT LIKE 'E%'
AND CITY NOT LIKE 'I%'
AND CITY NOT LIKE 'O%'
AND CITY NOT LIKE 'U%';
```

- From this example, we can guess how a **`NOT LIKE`** query works! Imagine there is a **table** filled with lots of data. In this table, there is a column called **City** containing hundreds of names, including duplicate entries. From this column, we need to print only unique entities for city names that do not start with a **vowel**. For these types of cases, we use a **`NOT LIKE`** query, which helps us filter out only the elements that do not match our condition.
    - This question is actually from **HackerRank**, where you can practice solving similar problems.

### Combing query filters

Now, a lot of times you might need to **combine conditions**, right? For example, what if you want to get all the actors which are having an ID less than 4 *and* their charges should be greater than or equal to a specific value

You can combine these query filters very easily inside your `WHERE` clause using logical operators :

```sql
select * from actors WHERE Charges >= 50000 AND ID < 4;

select * from actors WHERE Charges >= 500000 AND ID < 4;

select * from actors WHERE NOT(Charges >= 500000 AND ID < 4);
```

- Explanation -
    - **`AND` & `OR` Operators:** Just like you would expect, you can write something like `SELECT * FROM actors WHERE charges >= 50000 AND id < 4`. If you only want records that meet *both* conditions, use `AND`. If you want records that meet *either* condition, you can just put an `OR` there as simple as that!
    - **`NOT` Operator:** In fact, there exists a `NOT` operator also! You use this if you want to completely negate a condition. Let's say your `AND` condition from before only returned exactly one row that satisfied it. If you put a `NOT` in front of it, then *all* the other rows which are *not* satisfying this condition will come up! It basically says **"apart from this condition, give me everything else."**
    - **`XOR` Operator:** Similar to this, if at any point of time in any condition you need an exclusive OR, you even have a `XOR` operator available to use.

So, whatever field or `WHERE` clause you are writing, you can combine the output of the conditions using `AND`, `OR`, `NOT`, and `XOR` to get exactly the corresponding output you need!

- **A great question:** *"What if I have three conditions?"* The answer is going to be the exact same as in normal programming languages like JavaScript or Java! If you are going to have three or more conditions, you just keep chaining them together with these operators. It's really that simple!

### 12 Order By, Limit, Offset

*Now I'm going to show you a very interesting thing! Technically, this is asked a lot in interviews, but more importantly, it is going to be very relevant to actual software development!*

**How many of you guys have seen Google search? Whenever you search for something, you see maybe 10 or 15 results on one page, and then at the bottom, you have 1, 2, 3, 4, 5 like a pagination where you click on page number two to get the next results.**

How do you actually implement **pagination** if all the data is stored inside a relational database? This is very easily done using three clauses together: **`ORDER BY`**, **`LIMIT`**, and **`OFFSET`**!

Let's break them down:

```sql
select * from actors order by charges DESC, name;
select * from actors order by charges DESC, name DESC;
select * from actors order by charges DESC, name DESC Limit 2;
select * from actors order by charges DESC, name DESC Limit 2 OFFSET 1;
select * from actors order by charges DESC, name DESC Limit 2 OFFSET 2;
select * from actors order by charges DESC, name DESC Limit 2 OFFSET 0;
```

- **`ORDER BY` (Sorting the Data):** Think about normal C++ programming. If I want to print an array of products sorted by price, I can write a custom comparator to arrange them. In SQL, `ORDER BY` acts as that comparator! You just say `ORDER BY charges`.
    - By default, it will arrange them in ascending order (`ASC`) *(smallest to largest)*.
    - If you manually specify `DESC`, it arranges them in descending order *(largest to smallest)*.
    - **Pro Tip:** You can even combine comparators! Let's say you want to arrange everyone in decreasing order of their charges, but if two actors have the *exact same* charges, you want to sort them in **lexicographically** increasing order of their names. You just write `ORDER BY charges DESC, name ASC`. Simple as that!
        - **lexicographically -** *Arranging data, words, or symbols in alphabetical or dictionary order.*
        
- **`LIMIT` (Restricting the Output):** Now let's say I don't want *all* the records at one point of time. Let's say I only want the first two records. There is a clause called `LIMIT`. If you just say `LIMIT 2`, you are only getting the first two records!

- **`OFFSET` (Skipping Records):** But what if I don't want the first two records? What if I want the 3rd and the 4th record? This is where `OFFSET` comes in. `OFFSET` basically means **"from where do we need to start fetching the next records?"**
    - If you say `LIMIT 2 OFFSET 2`, it skips the first two records and gives you the next two!
    - If you say `OFFSET 4`, it gives you the 5th and 6th records.
    - `OFFSET 0` gives you the very first records.
        - That's mean `OFFSET` start it counting from *0th Index .*

**Bringing it all together for Pagination!** At any point in time, anywhere you see pagination—Amazon, Book_My_Show, Google, or even infinite scrolling in WhatsApp or Facebook—this is what they do! If you have **1 million records**, you can't just show them all at once because there will be a massive load on the UI. Instead, you *divide* them into smaller *chunks*.

If you want to show 50 records per page:

1. **Page 1:** You say `LIMIT 50 OFFSET 0`.
2. **Page 2:** You just update the offset to skip the first 50! `LIMIT 50 OFFSET 50`.
3. **Page 3:** `LIMIT 50 OFFSET 100`, and so on!
- **Question**—is it fetching the whole 1 million records and then filtering? No! It's not like the whole data is fetched. If you put a limit of 50, it is *only* going to go and fetch those 50 records. It is highly optimized!

---

### **13 Updating Existing Data (`UPDATE` clause)**

Let's say we make a spelling mistake in our database.

**EX:** The name is currently stored as "*Chris Evaes*", but we want to update it to "*Chris Evans*". How can you update the data?

we can use `UPDATE`, then the name of the table `actors`, then `SET` `name =`  `''new values''`, and finally use a `WHERE` clause to tell SQL exactly which row to update

```sql
// EX :
UPDATE actors SET name = 'Chris Evans' WHERE name = 'Chris Evaes`;
UPDATE Actors Set charges = 500000 Where Name = "Chris Evans";
```

You can do the exact same thing to update someone's charges. Just say

- `UPDATE table_name SET whatever_values_you_want WHERE condition`.

### **14 Modifying Table Structure (`ALTER` command)**

Now, suppose you want to add an entirely extra column to your table. You can't use `UPDATE` because `UPDATE` is always for a *row* inside a table. If you want to make changes to the actual *structure* of the table itself, you use the `ALTER` command!

- **Adding a column:**

```sql
ALTER TABLE actors ADD date_of_birth datetime;
```

- Boom, now a Date of Birth column has been added to your table structure!
    - There `datetime` is a data type of sql, to represent date and time format !
- **Removing a column:** If you want to remove it, you just say

```sql
ALTER TABLE actors DROP date_of_birth
```

Using `ALTER`, you can completely manipulate things, **update schemas**, and **change** the structure of the data!

## **15 Data Replication & Foreign Keys**

Let's say we create a `movies` table and we want to list the lead actor for each movie. If we store the name "Chris Evans" directly inside the movies table, do you see the problem? The problem is **data replication**! Actor is an **entity**, and you are trying to create a *copy* of that **entity**. If Chris Evans changes his name later, you have to go update it in the actors table *and* in every single row in the movies table where his name appears. *That is a very bad design*!

```sql
// create a table movies
 CREATE TABLE Movies (Id int AUTO_INCREMENT, Name varchar(20) NOT NULL, Primary Key(Id));
+------------------+
| Tables_in_movies |
+------------------+
| actors           |
| movies           |
+------------------+
```

- A very *simple* approach to go for is instead of storing the actor *details directly*, we just store an `actor_id`.
- This `actor_id` column acts as a **Foreign Key**.
- What is a foreign key? It is simply a column which is actually a **primary key** in some other *table*! By just **mentioning** the foreign key, we are able to uniquely identify the record. Using these techniques, we reduce data *redundancy* (this is often called *normalization*). Data *replication* is never going to be a good thing to do

```sql
ALTER TABLE movies ADD COLUMN ACTOR INT;
INSERT INTO MOVIES (NAME, ACTOR) VALUES ("CAPTAIN AMERICA", 3), ("IRON MAN 1", 5), ("IRON MAN 2", 5), ("THOR", 4);

// the output
select * from movies;
+----+-----------------+-------+
| Id | Name            | ACTOR |
+----+-----------------+-------+
|  1 | CAPTAIN AMERICA |     3 |
|  2 | IRON MAN 1      |     5 |
|  3 | IRON MAN 2      |     5 |
|  4 | THOR            |     4 |
+----+-----------------+-------+
```

## **16 The Power of `JOIN` Queries**

Now, we have our movies table with `actor_id`s, but if I say `SELECT * FROM movies`, it just shows user the ID numbers. If I tell you "actor with ID 3 was the lead," you will say, "Who is the actor with ID 3?!"

```sql
+----+-----------------+-------+
| Id | Name            | ACTOR |
+----+-----------------+-------+
|  1 | CAPTAIN AMERICA |     3 |
|  2 | IRON MAN 1      |     5 |
|  3 | IRON MAN 2      |     5 |
|  4 | THOR            |     4 |
+----+-----------------+-------+
```

This is where the concept of **Joins** comes into the picture! We have an **actors** table, and we have a **movies** table. There will be some common intersection of data between them. SQL helps us fetch that exact intersection!

simple syntax:

```sql
SELECT * FROM movies JOIN actors ON movies.actor = actors.id
```

- You say `JOIN` and put the name of the second table, and then you use `ON` to specify exactly what foreign key you want to intersect them on. Once you press enter, you get all the details of the movies right alongside the actual details of the actors!
- So there `Movies.actor` column working as a *foreign* key !
- EX :

```sql
SELECT * FROM movies JOIN actors ON movies.actor = actors.id;
+----+-----------------+-------+--------------+--------+---------+----+
| Id | Name            | ACTOR | Name         | Gender | Charges | Id |
+----+-----------------+-------+--------------+--------+---------+----+
|  1 | CAPTAIN AMERICA |     3 | RDJ          | Male   |  497971 |  3 |
|  2 | IRON MAN 1      |     5 | Mark ruffalo | Male   |  400000 |  5 |
|  3 | IRON MAN 2      |     5 | Mark ruffalo | Male   |  400000 |  5 |
|  4 | THOR            |     4 | Julia        | Female |  400000 |  4 |
+----+-----------------+-------+--------------+--------+---------+----+
```

## **17 Aliases**

- **Refining Queries with Aliases** You might notice that doing `SELECT *` gives you the ID columns twice. To fix this, you can ask for specific columns like `movies.name` and `actors.name`.

```sql
SELECT MOVIES.ID, MOVIES.NAME, ACTORS.NAME, ACTORS.CHARGES FROM MOVIES JOIN ACTORS ON MOVIES.ACTOR = ACTORS.ID;
+----+-----------------+--------------+---------+
| ID | NAME            | NAME         | CHARGES |
+----+-----------------+--------------+---------+
|  1 | CAPTAIN AMERICA | RDJ          |  497971 |
|  2 | IRON MAN 1      | Mark ruffalo |  400000 |
|  3 | IRON MAN 2      | Mark ruffalo |  400000 |
|  4 | THOR            | Julia        |  400000 |
+----+-----------------+--------------+---------+
```

- But writing the full table name every time gets tedious. So, you can use an **Alias** (which is basically a nickname)! You just say `movies AS m` and `actors AS a`. Then, everywhere else in your query, instead of writing `movies.id`, you just write `m.id`.

```sql
SELECT * FROM MOVIES AS M JOIN ACTORS AS A ON M.ACTOR = A.ID;
```

---

**Other Useful Operators** There are tons and tons of queries out there. You don't need to learn them all, but here are a few handy ones:

- **`BETWEEN`:** To find values within a range (e.g., `population BETWEEN x AND y`).

```sql
SELECT name FROM country WHERE population BETWEEN 1000000 AND 5000000;
SELECT name FROM country WHERE population NOT BETWEEN 1000000 AND 5000000;
```

- **`IN`:** If you have array-like data. You can say `country_id IN (1, 4, 7, 8)` to fetch rows that match any of those exact IDs.

```sql
SELECT name FROM country WHERE country_id IN (1, 4, 7, 8);
SELECT name FROM country WHERE country_id NOT IN (1, 4, 7, 8);
```

- **Nested Queries:** This is a query within a query! Generally, we try to avoid nested queries unless we have a very specific use case, but they exist.
    
    ```sql
    SELECT name FROM country WHERE population > (SELECT AVG(population) FROM country);
    ```
    

*Fun fact:* Most SQL databases have incredibly similar syntax. Limit, offset, and basic queries in PostgreSQL will look exactly the same as in MySQL!

## 18 DISTINCT Query

In a relational database (SQL), table columns often contain duplicate entries. A **`DISTINCT`** query is used in conjunction with the `SELECT` statement to retrieve only unique (distinct) values from a specific column, filtering out any repeating or duplicate rows from the query output

When you execute a standard `SELECT column_name`, the database returns every single row, including repeating values, But when you add the `DISTINCT` keyword, the database scans the specified column, groups identical values together, and returns a single instance of each unique value , Finding unique categories, cities, countries, or states in large datasets (e.g., retrieving all unique districts represented in an addresses table)

```sql
SELECT DISTINCT column_name FROM table_name;
```

Multiple addresses in the database may belong to the same district  To fetch a list containing every district only once:

```sql
SELECT DISTINCT district FROM address;
```

## 19 Aggregation queries

In simple terms, you can assume that an aggregate function is able to **club responses of multiple rows together** and then give you one single final response

Instead of just printing individual rows one by one, MySQL provides a bunch of built-in aggregate functions that crunch the numbers for you—like counting total rows, finding averages, or calculating total sums .

#### Few Key Aggregate Functions Covered in the List

- If you inspect any table where you want to know the total number of entries or total unique records, you use the `COUNT()` function.

```sql
// syntax
SELECT COUNT(column_name) FROM table_name;
// example
SELECT COUNT(*) FROM address;
```

- Fetches the total count of rows present in the **address** table (e.g., total stored addresses).
    - Now, what if you want to find out how many **distinct (unique)** districts are there? Remember how we got distinct districts in the previous query? Now, we just wrap that inside the `COUNT()` function

```sql
// syntax
SELECT COUNT(DISTINCT column_name) FROM table_name;
// example
SELECT COUNT(DISTINCT district) FROM address;
```

- Clubs `COUNT` and `DISTINCT` together to first remove duplicate entries in the `district` column, then counts only the unique districts.

- `MAX()` & `MIN()` – Finding Extremes, If you inspect any table where you see there are numerical list or data are saved as column , from there let's say you want to get the the maximum numerical value , Just like `COUNT`, we have a `MAX()` function .

```sql
// syntax
SELECT MAX(column_name) FROM table_name;

// example
SELECT MAX(rental_duration) FROM film;
```

- Fetches the highest integer value that present in the `rental_duration` column in **film** table

- Finding the Minimum Value:

```sql
// syntax
SELECT MIN(column_name) FROM table_name;
// example
SELECT MIN(rental_duration) FROM film;
```

- Fetches the lowest integer value present in the `rental_duration` column in the **film** table.

- `AVG()` – Calculating the Average If you inspect any table with numerical columns and want to find the overall average across all records, you use the `AVG()` function.

```sql
// syntax
SELECT AVG(column_name) FROM table_name;
// example
SELECT AVG(rental_duration) FROM film;
```

- Adds up all the numbers in the `rental_duration` column and divides by the total number of films to give you the average value.

- `SUM()` – Calculating the Total If you inspect a table with numerical values and want to calculate the grand total by adding every single row's value together, you use the `SUM()` function.

```sql
// syntax
SELECT SUM(column_name) FROM table_name;

// example
SELECT SUM(rental_duration) FROM film;
```

- Adds together every single numerical entry present in the `rental_duration` column in the **film** table and returns the final sum.

## 20 GROUP BY Clause

In simple terms, the **`GROUP BY`** clause is used to **group rows that have the same values** into summary rows.

Unlike `ORDER BY` (which simply sorts all rows), `GROUP BY` collapses multiple matching rows and **returns only one single row per group**. Because of this, it is almost always paired with aggregate functions like `COUNT()`, `SUM()`, `MAX()`, `MIN()`, or `AVG()` to perform calculations on each group.

- `GROUP BY` – Grouping Data by Column
    - If you inspect any table where you want to perform calculations per category (like finding how many total addresses belong to each district), you use `GROUP BY`.

```sql
// syntax
SELECT column_name, AGGREGATE_FUNCTION(column_name)
FROM table_name
GROUP BY column_name;

// example
SELECT district, COUNT(*)
FROM address
GROUP BY district;
```

- Groups all address records by their `district` and calculates the total count of addresses present for every single unique district.

- Alias (`AS`) – Giving Nicknames to Columns
    - When you run queries with aggregate functions like `COUNT(*)`, the default column header in the output looks messy. You can use the **`AS`** keyword to give it a clean, readable nickname (alias).

```sql
// syntax
SELECT column_name, AGGREGATE_FUNCTION(column_name) AS alias_name
FROM table_name
GROUP BY column_name;

// example
SELECT district, COUNT(*) AS number_of_address
FROM address
GROUP BY district;
```

- Replaces the generic `COUNT(*)` header in the result table with the friendly column name `number_of_address`.

- Combining `GROUP BY`, Aggregate Functions, and `ORDER BY`
    - You can chain `GROUP BY`, aliases, and `ORDER BY` together to group the data, calculate values per group, and then sort the grouped results.

```sql
// syntax
SELECT column_name, AGGREGATE_FUNCTION(column_name) AS alias_name
FROM table_name
GROUP BY column_name
ORDER BY alias_name;

// example
SELECT district, COUNT(*) AS number_of_address
FROM address
GROUP BY district
ORDER BY number_of_address;
```

- Groups addresses by district, counts the addresses for each district (naming the result column `number_of_address`), and then sorts the districts from lowest to highest address count.

#### Important Rule to Remember!

- **Group BY returns ONE row per group.**
    - **Common Error:** Trying to select individual unaggregated columns (like `address`) alongside a grouped column (like `district`) will result in an error:
        
        ```sql
        // THIS WILL CAUSE AN ERROR!
        SELECT address, district
        FROM address
        GROUP BY district;
        ```
        
- Since a single district can have multiple addresses, MySQL doesn't know which individual address to display for that single summary row.

## 21 HAVING Clause

In simple terms, the **`HAVING`** clause is used to **filter aggregated groups of data**, just like how the `WHERE` clause filters individual rows.

When you use `GROUP BY`, rows are collapsed into summary groups. If you try to filter these grouped results using a `WHERE` clause after `GROUP BY`, SQL will throw a **syntax error**. To filter groups, you must use `HAVING`.

- **`WHERE`**  Filters individual **rows** *before* grouping happens.
- **`HAVING`**  Filters summarized **groups** *after* `GROUP BY` has collapsed the rows.
- `HAVING` – Filtering Groups by Condition
    - If you inspect any query where you have already grouped data (using `GROUP BY`) and you want to filter down to specific groups or aggregated values, you use `HAVING`.

```sql
// syntax
SELECT column_name, AGGREGATE_FUNCTION(column_name) AS alias_name
FROM table_name
GROUP BY column_name
HAVING condition;

// example
SELECT district, COUNT(*) AS no_of_address
FROM address
GROUP BY district
HAVING district = 'Buenos Aires' OR district = 'Shandong';
```

- Groups all address records by their `district`, counts the addresses per district, and then filters the grouped results to only show the summary rows for `'Buenos Aires'` and `'Shandong'`.
    
    #### Difference Between `WHERE` and `HAVING`
    
    - Incorrect Query (Using `WHERE` to filter groups):
    
    ```sql
    -- THIS WILL CAUSE A SYNTAX ERROR!
    SELECT district, COUNT(*) AS no_of_address
    FROM addresssq
    GROUP BY district
    WHERE district = 'Buenos Aires' OR district = 'Shandong';
    ```
    
    - `WHERE` cannot be placed after `GROUP BY` to filter aggregated groups.
    - Correct Query (Using `HAVING` to filter groups):
    
    ```sql
    // syntax
    SELECT column_name, AGGREGATE_FUNCTION(column_name)
    FROM table_name
    GROUP BY column_name
    HAVING condition;
    
    // example
    SELECT district, COUNT(*) AS no_of_address
    FROM address
    GROUP BY district
    HAVING district = 'Buenos Aires' OR district = 'Shandong';
    ```
    
    - Successfully filters and returns the grouped count output only for the specified districts.

## 22 LENGTH() Query

The **`LENGTH()`** function is a built-in SQL string function used to **calculate the total number of characters** in a text column or string value.
When you pass a column containing text (like names, cities, or titles) into `LENGTH()`, it counts the characters in that string and returns the numerical length for every row !

- Calculating Character Length of a String
    - If you inspect any table where text data is saved as a column and you want to know the character count for each string, you use the `LENGTH()` function.

```sql
// syntax
SELECT column_name, LENGTH(column_name) FROM table_name;

// example
SELECT city, LENGTH(city) FROM station;
```

- Fetches the text value from the `city` column alongside its total character count in the **station** table.
- Giving an Alias to `LENGTH()`
    - Since `LENGTH(column_name)` can be repetitive to write inside complex queries, you can give it a clean nickname using `AS`.

```sql
// syntax
SELECT column_name, LENGTH(column_name) AS alias_name FROM table_name;

// example
SELECT city, LENGTH(city) AS len FROM station;
```

- Calculates the string length of each city and names the output column header `len`.
- Enough knowledge now solve some problem from the Hacker-Rank

## 23 CASE WHEN THEN ELSE END

The **`CASE`** statement is used to **add conditional logic (like `if-else` in programming)** directly inside your SQL query.

Instead of just selecting and printing raw values from a column, `CASE` allows you to inspect values in every row, evaluate conditions, and print specific outputs based on whether those conditions evaluate to `TRUE` or `FALSE`.

- Basic `CASE` Syntax Structure
If you inspect any table where you want to output custom values based on conditions, you use `CASE`. In SQL, `CASE` opens the conditional block and `END` closes it (acting like curly braces `{ }` in programming languages)

```sql
// syntax
SELECT
    CASE
        WHEN condition1 THEN 'output1'
        WHEN condition2 THEN 'output2'
        ELSE 'default_output'
    END AS column_alias
FROM table_name;

// example
SELECT
    CASE
        WHEN a = b AND b = c THEN 'Equilateral'
        ELSE 'Not Equilateral'
    END AS triangle_type
FROM triangles;
```

- Checks every row in the **triangles** table. If sides `a`, `b`, and `c` are all equal, it outputs `'Equilateral'`. Otherwise, it outputs `'Not Equilateral'`.

- Nested `CASE` Statements (Conditional Inside Conditional)
    
    You can nest a `CASE` statement inside another `WHEN...THEN` block to handle multi-step condition logic (such as checking if a shape is first a valid triangle before checking its type).
    

```sql
// syntax
SELECT
    CASE
        WHEN outer_condition THEN
            CASE
                WHEN inner_condition1 THEN 'output1'
                ELSE 'output2'
            END
        ELSE 'outer_fallback_output'
    END AS alias_name
FROM table_name;

// example
SELECT
    CASE
        WHEN (a + b > c) AND (a + c > b) AND (b + c > a) THEN
            CASE
                WHEN a = b AND b = c THEN 'Equilateral'
                WHEN a = b OR b = c OR a = c THEN 'Isosceles'
                ELSE 'Scalene'
            END
        ELSE 'Not A Triangle'
    END
FROM triangles;
```

- Explanation
    - **Outer `CASE`:** First checks if the three side lengths satisfy the fundamental triangle inequality rule (`a + b > c`, `a + c > b`, `b + c > a`). If not, it outputs `'Not A Triangle'`.
        - **Inner `CASE`:** If it is a valid triangle, it evaluates side equality:
            - All 3 sides equal \rightarrow `'Equilateral'`
            - Any 2 sides equal \rightarrow `'Isosceles'`
            - All sides different \rightarrow `'Scalene'`
- Key Rules to Remember
    1. **`CASE` and `END` match:** Every `CASE` statement must have a corresponding `END` to mark where the conditional block finishes.
    2. **`WHEN` and `THEN` pair:** `WHEN` takes the boolean condition and `THEN` defines the output if that condition evaluates to `TRUE`.
    3. **`ELSE` is optional:** Acts as the fallback default if none of the `WHEN` conditions are met. If omitted and no condition matches, SQL returns `NULL`.

## 24 CONCAT(), LOWER() and SUBSTR() Queries

In simple terms, these string functions let you **manipulate and format text outputs directly inside your SQL queries**:

- **`CONCAT()`**: Merges or pastes multiple text strings and column values together into a single string.
- **`LOWER()`**: Converts any uppercase text into lowercase text.
- **`SUBSTR()`** (or `SUBSTRING()`): Extracts a specific portion of a text string based on a starting index and length.
- `CONCAT()` – Combining Multiple Strings
If you inspect any table where you need to combine column values with custom strings (like appending an `'s'` to an occupation or putting parentheses around a letter), you pass multiple arguments into `CONCAT()`.

```sql
// syntax
SELECT CONCAT(string1, string2, string3, ...) FROM table_name;

// example
SELECT CONCAT(name, '(', SUBSTR(occupation, 1, 1), ')') FROM occupations;
```

- Merges the person's `name`, an opening bracket `'('`, the first character of their `occupation`, and a closing bracket `')'` into one string output (e.g., `'Ashley(A)'`).
- `LOWER()` – Converting Text to Lowercase
    
    If you inspect any text column that contains uppercase letters (like `'Doctor'` or `'Actor'`) and you need to convert it into lowercase, you wrap the string or column inside `LOWER()`.
    

```sql
// syntax
SELECT LOWER(column_name) FROM table_name;

// example
SELECT LOWER(occupation) FROM occupations;
```

- Converts uppercase or mixed-case values like `'Doctor'` or `'Actor'` to lowercase `'doctor'` or `'actor'`.
- `SUBSTR()` – Extracting Part of a String
    
    If you need to extract specific characters from a text string, you use `SUBSTR(column_name, start_position, length)`. In MySQL, indexing starts at `1` (not `0`).
    

```sql
// syntax
SELECT SUBSTR(column_name, start_position, length) FROM table_name;

// example
SELECT SUBSTR(occupation, 1, 1) FROM occupations;
```

- Takes the `occupation` text, starts at the 1st character (`1`), and extracts a substring of length `1`. For `'Actor'`, it extracts `'A'`.
- Keep in Mind!
    - **Multi-argument `CONCAT()`:** In MySQL, `CONCAT()` accepts multiple arguments comma-separated. You don't need nested `CONCAT(CONCAT(...))` calls.
    - **1-Based Indexing:** MySQL string functions start counting positions at `1`, so `1, 1` means starting at character 1 for a length of 1.
- Problem-Solving Pattern: Formatting Custom Text Outputs (HackerRank "The PADS")
    - Problem Requirements:
        1. Print all names ordered alphabetically, followed immediately by the first letter of their profession inside parentheses (e.g., `'Ashley(A)'`).
        2. Print the total count of each occupation formatted as `'There are a total of [count] [occupation]s.'` sorted by count ascending.
- Query 1: Formatting Names with Profession Initial

```sql
// syntax
SELECT CONCAT(column_1, '(', SUBSTR(column_2, start_pos, length), ')')
FROM table_name
ORDER BY column_1 ASC;

// example
SELECT CONCAT(name, '(', SUBSTR(occupation, 1, 1), ')')
FROM occupations
ORDER BY name ASC;
```

**Explanation :**

- Uses a single `CONCAT()` to merge `name`, `'('`, the first letter of `occupation` via `SUBSTR(occupation, 1, 1)`, and `')'`.
- `ORDER BY name ASC`: Sorts all formatted names alphabetically (A-Z).
- Query 2: Formatted Aggregation with `CONCAT()` and `LOWER()`

```sql
// syntax
SELECT CONCAT('text_1 ', AGGREGATE_FUNC(column_1), ' ', LOWER(column_2), 's.')
FROM table_name
GROUP BY column_2
ORDER BY AGGREGATE_FUNC(column_1) ASC, LOWER(column_2) ASC;

// example
SELECT CONCAT('There are a total of ', COUNT(occupation), ' ', LOWER(occupation), 's.')
FROM occupations
GROUP BY occupation
ORDER BY COUNT(occupation) ASC, occupation ASC;
```

Explanation :

- Groups the table by `occupation` using `GROUP BY occupation`.
- Uses `COUNT(occupation)` to get the group count.
- Uses `LOWER(occupation)` to convert `'Doctor'` to `'doctor'`.
- Uses `CONCAT()` to assemble the sentence: `'There are a total of 3 doctors.'`
- `ORDER BY COUNT(occupation) ASC, occupation ASC`: Sorts the summary outputs by lowest count first, and breaks ties alphabetically by occupation name.

## 25 Round() Query

The **`ROUND()`** function modifies a numerical value by rounding it to a specific number of decimal places. It takes two arguments as inputs: the first is the column name or the exact number that you want to round, and the second is the number of decimal places you want to keep. In other words, it looks at the decimal point and counts to the right by the number you gave in the second argument, then rounds the final digit up or down based on standard math rules (how many decimal places it keeps will depend on the number we give as the second argument).

- For better understand let's look at the example !
• We have 3 values in a column called **price**:
    ◦ 10.546
    ◦ 25.821
    ◦ 5.198

```sql
ROUND(number, decimal_places)
//example
SELECT ROUND(price, 2) FROM ITEMS;
```

- First, it takes the **PRICE** column from the **ITEMS** table. Then, from `ROUND()` that column, it looks at the numbers and keeps only two characters after the decimal point based on the second argument. For example, for "10.546," the third decimal digit is 6, so it rounds up to **"10.55"**; for "25.821," the third digit is 1, so it stays down at **"25.82"**; and for "5.198," the third digit is 8, so it rounds up to **"5.20."**
- These rounded numbers will then be displayed directly in the final output in the order they exist in the table, showing clean values with exactly two decimal places.
- The moral of the story is that we will get a list of the adjusted prices, but they will all be mathematically rounded up or down to exactly two decimal places.

## 26 Right() Query

The **`RIGHT()`** function slices a string starting from the right side. It takes two arguments as inputs: the first is the column name or text string that you want to slice, and the second is the number of characters you want to pull from the end. In other words, it starts counting from the very last character of the text and moves backward to the left ( how many character it going to move it will depend on the number we give as second arguments ) .

For better understand let’s look at the example ! 

- We have 3 person_name in a column called **name**
    - `Bobby`
    - `Samantha`
    - `Ashley`

```sql
RIGHT(text, number_of_characters)
//example 
SELECT NAME FROM THE STUDENT ORDER BY RIGHT(name, 3); 
```

- So, what is the output going to be? First, it takes the NAME column from the STUDENT table. Then, from `RIGHT()`  that column, it extracts only the last three characters of each name. For example, for "Bobby," the last three letters are "bby"; for "Samantha," they are "tha"; and for "Ashley," they are "ley."
- These names will then be displayed in alphabetical order based on those three trailing characters. Since we didn't specify a sorting direction, it automatically defaults to ascending (ASC) order.
- The moral of the story is that we will get a list of the full names, but they will be ordered alphabetically by their last three letters.

- Similarly, there is a **`LEFT()`** function available, whose syntax and arguments are exactly the same as **`RIGHT()`**. Only the use case is different. We can already guess how **`LEFT()`** is going to work, right?
    
    ```sql
    LEFT(text, number_of_characters)
    ```
    
    - It will take the number of characters defined in the second argument, and then, based on that number, it will start counting from the very first character on the far-left side of the text and move forward to the right to extract that specific amount of characters.

## 27 ABS() and SQRT()

The **`ABS()`** and **`SQRT()`** functions are quick mathematical utilities used to modify or calculate values from a single numeric input. **`ABS()`** strips away any negative signs to return the absolute positive value, while **`SQRT()`** calculates the square root of a positive number. In other words, `ABS()` turns a minus into a plus, and `SQRT()` finds the number that multiplies by itself to equal your input.For better understand let's look at the combined example!
• We have 2 values in a column called **num**:
    ◦ -16
    ◦ 49

```sql
ABS(number)  |  SQRT(number)
//example
SELECT ABS(num), SQRT(num) FROM DATA;
```

- First, it takes the **NUM** column from the **DATA** table. Then, it runs both functions on those numbers. For "-16," `ABS()` removes the negative sign to give **"16"** (while `SQRT(-16)` would return `NULL` since you cannot square root a negative number). For "49," `ABS()` stays positive as **"49"**, and `SQRT()` finds that 7×7=49 to give **"7."**
- These calculated numbers will then be displayed directly in the final output as clean, transformed values side by side.
- The moral of the story is that we get a quick way to clean up negative signs or find square roots whenever a quick math check is required for your records.

---

### RWU-JPA

#Real-World #Job_Practice

**Real-World Usage & Job Practice Advice** Now, you might be wondering, do you need to know *all* different types of raw queries? To start, no. We'll be actually dealing with something else called ORMs later. *However*, you can't just escape from raw queries forever! In highly complex projects—like ones I worked on at LinkedIn and Google—we had to write a lot of raw queries because the use cases get beyond the scope of a basic framework.

#interviews_Practice

So, if you are preparing for interviews, interviewers *can* and *will* ask you SQL queries. Companies like **Postman** or **Grofers**, which have very dev-heavy rounds, will expect you to know this. SQL is the bare minimum taught in almost all college branches!

#challenge_Practice

- **Where should you practice?** I highly recommend you go to **LeetCode**! You can practice some very good SQL queries right there on the platform to get interview-ready.

#SE #Experience_share

**A Quick Note on Low-Level Design (LLD)** I also want to touch on Low-Level Design because people ask about this a lot. In a lot of courses, you will see people teaching LLD from standard O'Reilly books. They keep teaching the exact same examples—like using SOLID principles or the Facade pattern with the same generic "bird" examples. Those courses are great for passing LLD interviews, but actually, it won't help you become a *good engineer* until you write a lot of code yourself! You have to write actual code and understand the use cases of how to prepare classes and structures in real life for LLD to actually help you in your work.

---

#Industry_Experience

**Industry Experience: SWE vs. Data Analytics** Let me share some real-world experience. In my career, the most complex data I worked with was at InterviewBit (because everything was solely inside MySQL) and LinkedIn (because we were handling payments and product allocation in RDBMS).

#Software_engineers

**In the real world, your role determines how you interact with SQL**

- **Software Engineers:** For us, database *design* is 60-70% of the weightage, and raw queries are maybe 30%. We design how the tables connect. (Though, in huge projects at LinkedIn and Google, you absolutely still write a lot of complex raw queries to get the job done!)

#tips_tricks #interviews_trick

And a quick tip for *interviews*: If they ask for a specific function like `CONCAT` and you don't remember the exact syntax, it's okay! They will allow you to Google it. They care more that you understand the concepts!

---

#END
