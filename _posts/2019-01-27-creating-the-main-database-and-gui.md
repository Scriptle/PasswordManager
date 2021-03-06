---
layout: post
title: Creating the Main Database and GUI
date: 2019-01-27
---

### Creating the Main Database
The first stage of security comes from creating an account on our Password Manager that all your passwords will be tied to. But, to allow the user to be able to register and log in to program, we need a database to store their credentials. We chose to use an SQLite database as it is embedded in the program as oppose to being a client-server database engine. This meant that we didn't need a database server, instead the database is saved locally within the program (for now). In order to work with the SQLite database we needed to use sqlite-jdbc, a Java SQLite library. This library allows us to connect to the database and manipulate it as required e.g. adding a new record to a table.

To keep the code organised and easy to maintain, we created a new class called *DatabaseManager*. We decided to make the class non-static which means it has to be instantiated as a new object before being used. This meant that we could create a new object for every database we wished to connect to (for future use). The constructor code is shown below:
```java
public DatabaseManager(String url) {
        try {
            conn = DriverManager.getConnection("jdbc:sqlite:" + url);
            System.out.println("Connection Sucessful.");
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }
```
What this means is that when we create a new *DatabaseManager*, we have to specify the relative url of the database as parameter as show below:
```java
DatabaseManager db = new DatabaseManager("users.db");
```
Then the program will attempt to connect to the database (or create a new one if it doesn't exist) and save the connection to the variable *conn*. Should any errors occur, they will be outputted to the console.
### Querying the Database: Creating the Table
SQL (Structured Query Language) is a high-level programming language that is used to manipulate databases. In conjunction with *sqlite-jdbc*, we used SQL to create the main table of users using the following SQL statement.
```SQL
CREATE TABLE IF NOT EXISTS users (id integer PRIMARY KEY,email VARCHAR,password VARBINARY, salt VARBINARY)
```
`CREATE TABLE IF NOT EXISTS users` creates a table called *users* if another table with the same name doesn't already exist in the database. We need this because the table will be created when the program is run for the first, but we don't want errors being thrown the next time the program is run.

`(id integer PRIMARY KEY,email VARCHAR,password VARBINARY, salt VARBINARY)` creates 4 columns in the table - *id*, *email*, *password* and *salt*.
`id integer PRIMARY KEY` means that the column called *id* accepts integers (whole numbers) values only and is the primary key which means that the id value automatically generated per record starting from 1.
`VARCHAR` means that the value held must be a string (alphanumeric) and the length can vary.
`VARBINARY` means that the value held must be binary and the length can vary.
### Creating the GUI
Now, that the database has been created, we need some sort of user interface so that the user can interact with the program. We used NetBeans' GUI Builder to create the GUI for a program. It allowed us to easily build the GUI using drag-and-drop tools and not have to worry about the code. A sample is shown below.
![enter image description here](https://raw.githubusercontent.com/Scriptle/PasswordManager/master/images/GUI.png)
