Client-side template Injection (CSTI) is a vulnerability that occurs when an attacker can inject and execute malicious code within templates processed on the client side, typically within a web browser. This vulnerability arises when user-controlled input is directly interpolated into a template without proper sanitization or validation. CSTI attacks are particularly dangerous because they can lead to Cross-Site Scripting (XSS) or Remote Code Execution (RCE) depending on the context and capabilities of the templating engine.

Here's how CSTI works, how to test for it, and how to prevent it:

### How CSTI Works:
1. **Injection Point**: The attacker identifies a point in the application where user input is directly embedded into a client-side template.
2. **Payload Injection**: The attacker crafts a payload containing malicious code, usually using the syntax recognized by the templating engine.
3. **Template Processing**: When the application renders the template on the client side, the injected payload gets executed in the context of the application, leading to various consequences such as XSS or RCE.

### Testing for CSTI:
To test for CSTI, follow these steps:

1. Identify user input points where templates are being processed.
2. Inject a simple payload containing template engine syntax.
3. Observe the output to see if the payload is executed.

### Example Bash Script for Testing CSTI:
```bash
#!/bin/bash

# Replace <URL> with the target URL vulnerable to CSTI
TARGET_URL="<URL>"

# Replace <PAYLOAD> with your payload containing template engine syntax
PAYLOAD="{{ 7 * 7 }}"

# Send a request with the payload injected
curl -X POST -d "input=$PAYLOAD" $TARGET_URL
```

### Prevention of CSTI:
To prevent CSTI vulnerabilities, follow these best practices:

1. **Input Validation and Sanitization**: Always validate and sanitize user inputs before incorporating them into templates.
2. **Context-Aware Escaping**: Apply context-aware escaping to user input to ensure that it is treated as data, not code.
3. **Use Safe Templating Engines**: Choose templating engines that offer built-in protections against CSTI, or use frameworks that automatically handle input sanitization and escaping.
4. **Content Security Policy (CSP)**: Implement CSP to restrict the sources from which scripts can be loaded, thereby mitigating the impact of XSS attacks.

### Types of CSTI:
CSTI vulnerabilities can manifest in various forms depending on the templating engine and context. Some common types include:

1. **String Interpolation**: Directly embedding user input within strings without proper escaping.
2. **Expression Injection**: Injecting expressions that get evaluated within the template.
3. **Code Injection**: Injecting executable code that gets executed within the client-side environment.
4. **Attribute Injection**: Injecting values into HTML attributes that can lead to XSS vulnerabilities.

Understanding and mitigating CSTI vulnerabilities is crucial for ensuring the security of web applications, as they can lead to severe consequences such as data breaches, unauthorized access, and manipulation of sensitive information. Regular security assessments, code reviews, and staying updated on emerging threats are essential for effectively addressing CSTI and other vulnerabilities.

## Corresponding Tools

- [**Bash**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_bash/CSTI.sh)
- [**Go**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_go/CSTI.go)
- [**Perl**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_perl/CSTI.pl)
- [**PHP**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_php/CSTI.php)
- [**Ruby**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_ruby/CSTI.rb)

