---
layout: post
title: Creating the Register and Login Methods
date: 2019-02-03
---

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
