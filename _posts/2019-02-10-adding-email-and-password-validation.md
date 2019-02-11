---
layout: post
title: Adding Email And Password Validation
date: 2019-02-10
---

### Modifying the Login and Register Page Layouts
The login and register pages' layout were a complete mess so we used the *GridBagLayout* to keep all the GUI elements in order based on a customisable grid. This makes the pages easier for the user to navigate improving the usability of our program. Moreover, we have an error message label which the text can be changed based on the error and should pop up when an error occurs e.g. email address is not valid. The problem was though, it was visible on the login/register page from the start when the user hadn't even typed anything in. To rectify this, we hid the error label at the start of the program and only made it visible when the error occurred.
### Adding Email and Password Validation
When users register to use our program, we need accurate information and not any random nonsense. To ensure as much accuracy as possible, we need to use validation. By using validation we can check if what the user has entered matches a specific format. To do this, we used Regex (regular expression). By using regex, we can check if a string matches a certain pattern. Here's an example of how we used regex to check if the password the user created is valid:
```java
public static boolean isPasswordValid(char[] password) {
    String regex = "((?=.*\\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[!\"£$€%^&*()@#/?]).{8,})";
    Pattern pattern = Pattern.compile(regex);
    Matcher matcher = pattern.matcher(new String(password));
    boolean isValid = matcher.matches();
    
    return isValid;
}
```

To break it down, the regex string above means for a password to be valid it must:
 - `(?=.*\\d)` contain at least one number
 - `(?=.*[a-z])` contain at least one lowercase letter
 - `(?=.*[A-Z])` contain at least one uppercase letter
 - `(?=.*[!\"£$€%^&*()@#/?])` contain at least one of the following special characters: !\"£$€%^&*()@#/?
 - `.{8,}` be at least eight characters long

### Creating the Home Page
With the login sorted, we now need a home page. To do this, we simply add another panel to the *rootPanel* and as we have a *CardLayout*, we can simply use `cardLayout.show(rootPanel, "home");` to display the home page once the user has logged in. The home page is blank for now but will contain a list of all the user's usernames and passwords for various sites and the option to add another password to the list.
