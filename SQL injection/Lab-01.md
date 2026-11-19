[Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)  


## Steps to Solve

1. **Visit the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Open Burp Suite, select any product category (e.g., Gifts), and intercept the category filter request.

3. **Modify the Category Parameter:** Change the `category` parameter using this SQL injection payload:

```sql
Gifts' OR 1=1--
```

4. **Forward the Request:** Send the modified request to the server.

5. **Verify the Result:** Check the response for unreleased products. If they appear, the lab is solved.

## SQL Query Explanation

**Modified SQL Query:**
```sql
SELECT * FROM products
WHERE category = 'Gifts' OR 1=1--'
AND released = 1
```

- `'` closes the original string.
- `OR 1=1` creates an always-true condition.
- `--` comments out the remaining SQL query.

## Key Takeaways

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** Retrieving Hidden Data
- **Tool:** Burp Suite
- **Impact:** Bypasses application filters and exposes unreleased products.
