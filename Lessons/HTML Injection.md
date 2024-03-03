HTML Injection, also known as Cross-Site Scripting (XSS), is a vulnerability that occurs when an attacker is able to inject malicious HTML or JavaScript code into a web application, which is then executed by the victim's browser. This can lead to various security risks, including session hijacking, data theft, and malware distribution.

### How HTML Injection Works:
HTML Injection typically occurs when a web application fails to properly validate or sanitize user input before rendering it onto a webpage. Attackers exploit this vulnerability by injecting malicious code into input fields or parameters that are later displayed on the webpage.

There are several types of HTML Injection vulnerabilities:

1. **Reflected XSS**: The malicious script is injected as part of a request to the server and reflected back in the response, often in error messages or search results.

2. **Stored XSS**: The injected script is permanently stored on the server (e.g., in a database) and is served to all users who view the affected page.

3. **DOM-based XSS**: The vulnerability arises in client-side JavaScript code rather than in server-side code. The attacker manipulates the DOM environment to execute malicious code.

### Testing for HTML Injection:
To test for HTML Injection vulnerabilities, you can follow these steps:

1. Identify input fields or parameters where user-supplied data is reflected on the webpage.
2. Input some basic HTML or JavaScript code (e.g., `<script>alert('XSS')</script>`) into these fields.
3. Submit the form or request and observe whether the injected code is executed on the webpage.

### Bash Script to Test for HTML Injection:
Here's a simple bash script to test for HTML Injection vulnerabilities:

```bash
#!/bin/bash

# Replace the URL and parameters with the target webpage and input fields
TARGET_URL="http://example.com/login"
USERNAME="<script>alert('XSS')</script>"
PASSWORD="password123"

# Send a POST request with the injected payload
curl -X POST $TARGET_URL \
     -d "username=$USERNAME" \
     -d "password=$PASSWORD"
```

### Prevention Measures:
To prevent HTML Injection vulnerabilities, follow these best practices:

1. **Input Validation**: Validate and sanitize all user input before using it in HTML output. Use input validation libraries or frameworks to ensure data integrity.

2. **Output Encoding**: Encode user input when rendering it onto the webpage. This prevents the browser from interpreting it as HTML or JavaScript code.

3. **Content Security Policy (CSP)**: Implement CSP headers to restrict the sources from which browsers can load content, reducing the risk of executing injected scripts.

4. **HTTPOnly Flag**: Use the HTTPOnly flag for cookies to prevent client-side scripts from accessing them, reducing the risk of session hijacking.

5. **Regular Security Audits**: Conduct regular security audits and penetration testing to identify and mitigate vulnerabilities in the web application.

By understanding how HTML Injection vulnerabilities work and following these prevention measures, you can effectively secure your web applications against such attacks. Let me know if you need further clarification or assistance!
