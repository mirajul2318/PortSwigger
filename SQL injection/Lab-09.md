[Lab: SQL injection UNION attack, retrieving data from other tables](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables)  


## Goal
Retrieve the administrator's password from the `users` table and log in as administrator.

## Steps to Solve

1. **Launch the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Click on any product category and intercept the category filter request using Burp Suite.

3. **Determine the Number of Columns:** Use `ORDER BY` to identify the column count.

- `ORDER BY 1` – Success
- `ORDER BY 2` – Success
- `ORDER BY 3` – Error

**Result:** The query returns two columns.

4. **Identify Text-Compatible Columns:** Test both columns using:

```sql
' UNION SELECT 'abc','def'--
```

**Result:** Both columns accept text data.

5. **Identify Database Information:**

- **Table Name:** `users`
- **Column Names:** `username`, `password`

6. **Retrieve User Credentials:** Extract usernames and passwords.

```sql
' UNION SELECT username,password FROM users--
```

**Result:** Retrieved the administrator credentials.

- **Username:** `administrator`
- **Password:** `c7em5dvqbggw4l2ys8s0`

7. **Login as Administrator:** Navigate to **My Account** and log in using the retrieved credentials.

**Result:** Successfully logged in as administrator and completed the lab.

## Key Takeaways

- **UNION SELECT:** Retrieves data from additional database queries.
- **Credential Extraction:** Retrieves usernames and passwords from database tables.
- **Authentication Bypass:** Stolen credentials can provide unauthorized account access.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Tool:** Burp Suite Community Edition
