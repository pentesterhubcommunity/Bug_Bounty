XPath Injection is a vulnerability that occurs when user input is improperly sanitized or validated in an XPath query used by an application to retrieve data from XML-based data sources. XPath (XML Path Language) is a query language used for selecting nodes from an XML document.

### How XPath Injection Works:
1. **User Input:** The vulnerability arises when user input is directly concatenated into an XPath query without proper sanitization or validation.
2. **Manipulation:** An attacker can manipulate the input to change the intended behavior of the XPath query.
3. **Injection:** By injecting specially crafted input, the attacker can alter the XPath query to retrieve unauthorized data or perform unintended actions.
4. **Impact:** This can lead to data exposure, authentication bypass, and potentially compromise the entire system.

### Example:
Consider a web application that uses XPath queries to authenticate users:

```xml
//XPath query used for authentication:
"//user[@username='$username' and @password='$password']"
```

If the application directly inserts user-supplied values into this query without proper validation, an attacker could input something like:

```plaintext
Username: admin' or '1'='1
Password: anything
```

The resulting XPath query would become:

```xml
"//user[@username='admin' or '1'='1' and @password='anything']"
```

This altered query always evaluates to true because of the `or '1'='1'` condition, effectively bypassing authentication.

### Testing XPath Injection:
1. **Identify Input Points:** Find where user input is used in XPath queries.
2. **Inject Payloads:** Insert payloads like `' or '1'='1` or `'; DROP TABLE users--` to test for injection.
3. **Observe Results:** Analyze the application's response to determine if injection was successful.

### Bash Script to Test XPath Injection:
Here's a simple bash script to test XPath Injection using curl:

```bash
#!/bin/bash

URL="http://example.com/login"
USERNAME="admin' or '1'='1"
PASSWORD="password"

curl -X POST $URL \
-d "username=$USERNAME&password=$PASSWORD"
```

### Prevention Techniques:
1. **Parameterization:** Use parameterized XPath queries to separate user input from the query structure.
2. **Input Validation:** Validate and sanitize user input to ensure it conforms to expected formats.
3. **Least Privilege:** Limit the privileges of accounts used by the application to access XML data.
4. **Error Handling:** Implement proper error handling to avoid leaking sensitive information.

### Types of XPath Injection Vulnerabilities:
1. **Authentication Bypass:** Manipulating XPath queries to bypass authentication mechanisms.
2. **Data Exposure:** Modifying queries to retrieve unauthorized data.
3. **Denial of Service:** Injecting payloads to disrupt or overload the application's processing capabilities.

Understanding and mitigating XPath Injection vulnerabilities is crucial for securing applications that rely on XPath queries to interact with XML data sources. Regular security testing and code reviews are essential to detect and address such vulnerabilities effectively.
