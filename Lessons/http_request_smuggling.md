HTTP request smuggling is a web security vulnerability that occurs when an intermediary server (such as a proxy or a load balancer) interprets the HTTP requests from the client and forwards them to the backend servers. This vulnerability arises due to inconsistencies in how different servers interpret and process HTTP requests, leading to the smuggling of multiple requests within a single HTTP request or responses being interpreted differently by the frontend and backend servers.

### How It Works:

1. **Discrepancies in Parsing**: The vulnerability often stems from differences in how frontend and backend servers parse HTTP requests. For example, one server may interpret line terminators (such as LF, CR, or CRLF) differently than another server.

2. **Splitting Requests**: An attacker crafts a malicious HTTP request with specially crafted headers or payloads that exploit these discrepancies. By manipulating the request headers (such as "Content-Length" or "Transfer-Encoding"), the attacker can split a single request into multiple requests, tricking the backend server into processing them separately.

3. **Collusion Attack**: In a collusion attack, the attacker sends a request that appears valid to the frontend server but is interpreted differently by the backend server. This allows the attacker to bypass security mechanisms and perform unauthorized actions.

### Types of HTTP Request Smuggling:

1. **CL.TE Smuggling**: This involves sending a "Content-Length" header with a value that is interpreted differently by the frontend and backend servers. For example, the frontend server may interpret the header as the total length of the request, while the backend server may interpret it as the length of the body content only.

2. **TE.CL Smuggling**: This occurs when the "Transfer-Encoding" header is manipulated to trick the frontend and backend servers into processing the request differently. For instance, the frontend server may interpret the "Transfer-Encoding" header as chunked encoding, while the backend server interprets it as a content-length.

3. **CL.CL Smuggling**: In this scenario, the attacker manipulates both the "Content-Length" and "Transfer-Encoding" headers to confuse the servers into processing the request inconsistently.

### Testing for HTTP Request Smuggling:

To test for HTTP request smuggling, you can use various tools and techniques:

1. **Manual Testing**: Craft HTTP requests with different headers and payloads to observe how the frontend and backend servers interpret them differently.

2. **Automated Scanners**: Tools like Burp Suite, OWASP ZAP, or HTTP Request Smuggler can automate the process of sending crafted requests and identifying vulnerabilities.

3. **Custom Scripts**: You can write custom scripts in scripting languages like Python or Bash to send crafted requests and analyze the responses.

### Example Bash Script for Testing:

```bash
#!/bin/bash

# Replace these variables with your target server details
TARGET_HOST="example.com"
TARGET_PORT="80"

# Craft a malicious request
MALICIOUS_REQUEST="POST / HTTP/1.1\r\nHost: $TARGET_HOST\r\nContent-Length: 4\r\nTransfer-Encoding: chunked\r\n\r\n7\r\nX: X\r\n\r\n0\r\n\r\n"

# Send the request and display the response
echo -e $MALICIOUS_REQUEST | nc $TARGET_HOST $TARGET_PORT
```

### Preventing HTTP Request Smuggling:

To mitigate HTTP request smuggling vulnerabilities, follow these best practices:

1. **Consistent Parsing**: Ensure that both frontend and backend servers parse HTTP requests consistently, especially regarding headers like "Content-Length" and "Transfer-Encoding."

2. **Request Normalization**: Normalize incoming requests to eliminate ambiguities and inconsistencies that could be exploited by attackers.

3. **Security Headers**: Implement security headers like "Content-Security-Policy" and "X-XSS-Protection" to enhance the security of web applications.

4. **Regular Updates**: Keep servers and web application frameworks up to date with the latest security patches to prevent known vulnerabilities.

5. **WAFs and Proxies**: Use Web Application Firewalls (WAFs) and reverse proxies to filter and sanitize incoming requests, protecting against malicious payloads.

6. **Security Testing**: Conduct regular security assessments, including penetration testing and code reviews, to identify and remediate vulnerabilities proactively.

By understanding how HTTP request smuggling works and adopting preventive measures, you can safeguard web applications against this critical security risk.

## Corresponding Tools

- [**Bash**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_bash/http_request_smuggling.sh)
- [**Go**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_go/http_request_smuggling.go)
- [**JavaScript**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_javascript/http_request_smuggling.js)
- [**Perl**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_perl/http_request_smuggling.pl)
- [**PHP**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_php/http_request_smuggling.php)
- [**Python**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_python/http_request_smuggling.py)
- [**Ruby**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_ruby/http_request_smuggling.rb)
