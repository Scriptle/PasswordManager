# Developer Blog
## 27th January 2019
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
![enter image description here](https://raw.githubusercontent.com/Scriptle/PasswordManager/master/GUI.png)
### Creating the Register Method
With the GUI made, we now need users to be able to register so that they can use the program. This involves the user entering their email and creating a password. We need to store this data in the database on the *users* table. Before we can do that, we need to hash the password with a salt for security reasons. Once hashed, we use another SQL statement to store the data.
```SQL
INSERT INTO users(email,password,salt) VALUES(?,?,?)
```
`VALUES(?,?,?)` The question marks act as a placeholder. This means that, using the *sqlite-jdbc* library, we can replace them with the corresponding data.
`INSERT INTO users(email,password,salt)` means add the corresponding data as specified by `VALUES` to the columns (listed in between the brackets) in the table called *users*. So now the user's data is stored in the database and they will be able to login.
### Creating the Login Method
Now that users can register with the program, they have to be able to log in. This involves the user entering their email and password on the GUI and clicking the login button. On clicked, the program uses yet another SQL statement to search for the entered email in the database.
```SQL
SELECT password,salt FROM users WHERE email=?
```
This returns the corresponding hashed password and salt, if the entered email exists in the table *users*. Otherwise, it returns nothing. Notice the `?` which works in the same way as the statement above.

If the email is found, we have to hash the entered password using the salt retrieved from the database and compare it to the hashed password retrieved from the database. If the hashes match we log the user in, otherwise we display a suitable error message.
