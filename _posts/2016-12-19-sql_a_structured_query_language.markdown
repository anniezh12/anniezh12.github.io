---
layout: post
title:  "SQL A Structured Query Language"
date:   2016-12-19 06:41:17 +0000
---


SQL (Structured Query Language) is everywhere, and in today's digitalized world with massive amounts of data being gathered every day and stored into a database, knowing how to program with SQL is very important. 
# What is SQL ?
SQL stands for Structured Query Language. SQL is used to communicate with a database. According to ANSI (American National Standards Institute), it is the standard language for relational database management systems.

# Storing Data
Relational Databases like SQL store data in a structure we refer to as a table. You can think of a table in a database a lot like you would a spreadsheet. We define specific columns in our table, and then we store any number of what we refer to as 'records' as rows in our database.
For Example we have  our student's record table as follows
```

ID        -   Name        -       Age        -    Gender    -    Grade   -    Score 
----------------------------------------------------------------------------------------------------------

01        - Qasim         -      10         -       M         -     5TH      -    87

02        - Hunniya       -      10         -       F         -     5TH      -    95

03        - Abiya         -       6         -       F         -      1ST     -    90

04        - Ahmed         -      13         -       M         -     7TH      -    87

05        - Zara          -      9          -       F         -     5TH      -    87
----------------------------------------------------------------------------------------------------------


```
SQL supports a number of statements that can build, manipulate and update such data tables called queries which are  terminated with a semicolon (;).
 
# TABLE CREATION 
We can easily create a table using "CREATE TABLE" query as follows


```
CREATE TABLE  students(ID INTEGER PRIMARY KEY , Name TEXT,Age INTEGER,
Gender CHAR(1),	Grade TEXT,Score INTEGER);

 											
```								

Above statement will create a table name "students" with six fields of data specifying their data types as
text, integer and char(character) .Here it is important to know that SQL supports only four data types i.e
TEXT,INTEGER,CHAR  and BOOLEAN and doesnt support Arrays or Hashes.
Also we have specify the ID as a PRIMARY KEY which means its a uniques value which will be used to identify each record associated with it . 
Now we can apply different queries to insert, retrieve and update data in this table .

# DATA INSERTION 
Now we have our "students" table which is empty right now we can insert data using INSERT INTO query


INSERT INTO students(Name,Age,Gender,Grade,Score) VALUES ("Qasim",10,"M","5TH",87);


Above statement will insert record for the first student and will automatically give it the ID as 1
since a primary key will automatically be incremented and assigned. we can add the rest of records as well.


# DATA SELECTION

To select data from the above table we can use the following query using wildcard character(*)

SELECT * FROM students;

will give us 


1 | Qasim | 10 | M| 5TH| 87

2 | Hunniya|10 | F|5TH| 95

3  | Abiya   | 6 |   F  |1ST|   90

4  | Ahmed | 13 |  M | 7TH | 87

5  | Zara   | 9  |  F  |5TH | 87

we can get names only as follows

SELECT Name FROM students;

will give us name only as follows

Qasim
Hunniya
Abiya
Ahmed
Zara

We can also filter our result using WHERE with SELECT as follows 
For Example I want to get the students names who have a Score over 90 

SELECT Name FROM students WHERE Score>90;

will out put as follows

Hunniya

# UPDATING A TABLE
NOW we want to add a new column to our students table which gives us information about Teacher,
we can do that using following query
			
			ALTER students ADD COLUMN Teacher TEXT;

And we can insert the value for teacher as follows
	 
	    UPDATE students SET Teacher = "MRS Rice" WHERE ID = 1;
	 
The UPDATE statement uses a WHERE clause to grab the row you want to update. It identifies the table name you are looking in and resets the data in a particular column to a new value.


