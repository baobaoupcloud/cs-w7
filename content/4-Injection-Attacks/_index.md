---
title : "Injection Attacks"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---
One of the problems that can arise in real-world applications of SQL is what is called an injection attack. An injection attack is where a malicious actor could input malicious SQL code.
For example, consider a login screen as follows:

![login](https://raw.githubusercontent.com/baobaoupcloud/cs-w7/main/static/images/4.injectionattacks/injectionattacks1.png)

You should never trust the users, do not use formatted strings as below to query:
```python
rows = db.execute(f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'")
```

Without the proper protections in our own code, a bad actor could run malicious code. Consider the following:

```python
rows = db.execute("SELECT COUNT(*) FROM users WHERE username = ? AND password = ?", username, password)
```
Notice that because the `?` is in place, validation can be run on favorite before it is blindly accepted by the query.
You never want to utilize formatted strings in queries as above or blindly trust the user’s input.
Utilizing the CS50 Library, the library will sanitize and remove any potentially malicious characters.

That's all of week 7 ^^