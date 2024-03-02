### What is CRLF Injection Vulnerability?

CRLF injection is a type of injection attack where an attacker can insert malicious CRLF sequences into input fields or parameters of a web application. These sequences consist of special characters like Carriage Return (CR - '\r') and Line Feed (LF - '\n'). 

### How Does CRLF Injection Work?

1. **Understanding CRLF Characters**:
   - Carriage Return (`\r`) moves the cursor to the beginning of the line.
   - Line Feed (`\n`) moves the cursor down to the next line.

2. **Injection Points**:
   - CRLF injections usually target input fields that are used to construct HTTP responses, such as headers or cookies.

3. **Exploiting Vulnerabilities**:
   - Attackers inject CRLF sequences to manipulate the response generated by the web server.
   - By inserting CRLF sequences, attackers can perform various malicious actions such as HTTP header injection, HTTP response splitting, and session fixation.

### Examples of CRLF Injection Vulnerabilities:

Let's consider a simple example of a login form where the username is echoed back in the response header without proper validation:

```html
<form action="/login" method="post">
  Username: <input type="text" name="username"><br>
  Password: <input type="password" name="password"><br>
  <input type="submit" value="Login">
</form>
```

And the corresponding server-side code in a Node.js application:

```javascript
app.post('/login', (req, res) => {
  const username = req.body.username;
  res.setHeader('Set-Cookie', `username=${username}`);
  res.send('Login successful');
});
```

In this example, the value of the `username` input field is directly used to set a cookie without proper validation. An attacker can exploit this vulnerability by injecting CRLF sequences in the username field.



**Bash Script for Testing:**
```bash
#!/bin/bash

# Target URL vulnerable to CRLF injection
target_url="http://example.com/vulnerable_endpoint"

# Injected payload
payload="%0D%0AInjected_Header: Malicious_Content%0D%0A"

# Send a request with the payload
curl -X GET "$target_url" -H "User-Agent: Mozilla/5.0" -H "Accept-Language: en" -H "Cookie: session=123456" -H "Referer: http://example.com" -H "$payload"
```

**Preventing CRLF Injection:**
1. **Input Validation**: Validate user inputs to ensure they contain only expected data and do not contain any malicious characters.
2. **Output Encoding**: Encode special characters before outputting them to prevent them from being interpreted as control characters.
3. **HTTP Headers Sanitization**: Properly sanitize and validate all user-controllable data before including it in HTTP headers.

### Broken Access Control Vulnerabilities:

Broken access control refers to flaws in access control mechanisms that allow unauthorized access to restricted resources. Here are some types:

1. **Insecure Direct Object References (IDOR)**: Occurs when an application exposes direct references to internal implementation objects, such as files or database keys, in URLs or parameters, allowing attackers to manipulate these references to access unauthorized data.

2. **Missing Function Level Access Control**: This happens when developers fail to enforce proper access controls at the function or API level, allowing unauthorized users to execute privileged actions.

3. **Privilege Escalation**: Involves attackers gaining unauthorized access to elevated privileges, typically by exploiting vulnerabilities in authentication mechanisms or by manipulating session tokens.

4. **Horizontal and Vertical Access Control Issues**: Horizontal access control issues involve one user accessing another user's data (e.g., via insufficient session management), while vertical access control issues occur when a user with limited privileges can access data or functionality intended for users with higher privileges (e.g., by manipulating URL parameters).

To prevent broken access control vulnerabilities, adhere to the principle of least privilege, implement proper authentication and authorization mechanisms, enforce access controls at both the function and data levels, and regularly audit and review access control policies and configurations.

