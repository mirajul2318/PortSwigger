[Lab: SQL injection vulnerability allowing login bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass)


## Steps to Solve

1. **Launch the Lab:** Click **ACCESS THE LAB** and open the challenge.
2. **Navigate to Login:** Go to the **My Account** page.
3. **Intercept the Request:** Open Burp Suite and enable Proxy Intercept. Submit the login form with any username and password.
4. **Modify the Username Parameter:** Change the `username` parameter to:

```sql
administrator' OR 1=1 --
```

5. **Forward the Request:** Send the modified request to the server.
6. **Verify the Result:** If successful, you will be logged in as the administrator and the lab will be marked as solved.

## SQL Query Explanation

**Modified SQL Query:**

```sql
SELECT * FROM users
WHERE username = 'administrator'OR 1=1 --
AND password = '...';
```

- `'` closes the username string.
- `--` comments out the remaining SQL query, including the password condition.

## Key Takeaways

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** Authentication Bypass
- **Tool:** Burp Suite
- **Impact:** Unauthorized access to administrator accounts.
