HTTP response splitting is a web security vulnerability that occurs when an attacker is able to inject malicious content into the response headers of an HTTP response. This can lead to various attacks, such as cross-site scripting (XSS), session fixation, and cache poisoning. Let's break down how it works, how to test for it, and how to prevent it.

### How it works:

1. **Understanding HTTP Headers**: HTTP responses consist of headers followed by optional content. Headers are separated from the content by a blank line.
2. **Injection Point Identification**: An attacker identifies a point in the application where user input is included in HTTP response headers.
3. **Malicious Injection**: The attacker then crafts a malicious payload containing newline characters (`\r\n` or `\n`) to inject additional headers or manipulate existing ones.
4. **Response Manipulation**: When the server processes the user input, it generates an HTTP response with the injected headers, allowing the attacker to control or manipulate the response received by the client.

### Types of HTTP Response Splitting:

1. **Simple HTTP Response Splitting**: Involves injecting additional headers to split the response into multiple parts.
   
2. **Double Content-Length**: Sending two Content-Length headers can lead to confusion in parsing the response, potentially allowing an attacker to inject content between the two instances of the header.

### Testing for HTTP Response Splitting:

To test for HTTP response splitting, you need to identify input points where user-controlled data is included in HTTP response headers. Common injection points include HTTP headers like `Location`, `Set-Cookie`, or custom headers.

Here's a basic bash script to test for HTTP response splitting:

```bash
#!/bin/bash

# Replace URL with the target vulnerable web application
URL="http://example.com/"

# Injected payload
PAYLOAD="%0d%0aX-Injected-Header: MaliciousContent%0d%0a"

# Send a request with the injected payload
curl -v -H "User-Agent: Mozilla/5.0" -H "Host: example.com" -H "Injected-Header: NormalContent" -X GET "${URL}${PAYLOAD}"
```

This script sends a GET request with a malicious payload injected into the `Injected-Header`.

### Prevention:

1. **Input Validation and Sanitization**: Ensure that user input is properly validated and sanitized before being used in HTTP response headers.
2. **Encode Output**: Encode user-supplied data before including it in HTTP response headers. Use functions like `urlencode` to ensure that any potentially malicious characters are properly encoded.
3. **HTTP Header Library**: Use a trusted HTTP header library that automatically handles encoding and validation of header values.
4. **Security Headers**: Implement security headers like `Content-Security-Policy (CSP)` and `X-XSS-Protection` to mitigate the impact of XSS attacks.

By understanding how HTTP response splitting works, testing for vulnerabilities, and implementing preventive measures, you can protect your web applications from this security risk.
