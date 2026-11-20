[Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft)  


# SQL Injection – Querying the Database Version (MySQL)

## Steps to Solve

1. **Launch the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Click on the **Accessories** category and intercept the category filter request using Burp Suite.

3. **Test for SQL Injection:** Add a single quote (`'`) to the `category` parameter and observe the server response for an SQL error.

4. **Determine the Number of Columns:** Use `ORDER BY` to identify the number of columns.

```sql
' ORDER BY 1--  
' ORDER BY 2--  
' ORDER BY 3--  
```

- `ORDER BY 1` – Success
- `ORDER BY 2` – Success
- `ORDER BY 3` – Error

**Result:** The query returns two columns.

5. **Identify Text-Compatible Columns:** Use the following payload to verify that both columns accept text data.

```sql
' UNION SELECT 'abc', 'def'--  
```

**Result:** Both columns accept string data.

6. **Retrieve the Database Version:** Use the following payload:

```sql
' UNION SELECT @@version, NULL--  
```

Send the modified request using Burp Repeater.

7. **Verify Lab Completion:** Check the response for the MySQL database version. If displayed successfully, the lab is solved.

## Key Takeaways

- **UNION SELECT:** Combines results from multiple SQL queries.
- **ORDER BY:** Determines the number of columns.
- **--  :** MySQL comment syntax. [Note the space after the double dash]
- **@@version:** Returns the MySQL database version.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Database:** MySQL
- **Tool:** Burp Suite Community Edition
