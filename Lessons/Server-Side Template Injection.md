Server-Side Template Injection (SSTI) is a vulnerability that occurs when an application allows users to input data into a server-side template, which is then executed by the server. This can lead to various security risks, including remote code execution (RCE) and data leakage.

Here's how it generally works:

1. **Input Injection**: An attacker injects malicious code or expressions into a template variable or parameter. This can often be done through input fields, query parameters, or other user-controlled data sources.

2. **Template Processing**: The server processes the template, including the injected code, without proper validation or sanitization.

3. **Execution**: The injected code is executed within the server's context, allowing the attacker to perform actions such as accessing sensitive data, executing arbitrary commands, or even taking control of the server.

To test for SSTI vulnerabilities, you can follow these steps:

1. **Identify Input Points**: Determine where user input is being incorporated into server-side templates. This could be in URL parameters, form fields, or any other input mechanism.

2. **Inject Payloads**: Inject template expressions or code snippets into these input points. These payloads can include template syntax specific to the template engine being used (e.g., Jinja2, Twig, FreeMarker).

3. **Observe Behavior**: Monitor the server's response to the injected payloads. Look for any unexpected behavior, such as error messages containing template syntax or changes in the rendered page.

Here's a basic example of a Bash script to test for SSTI using curl:

```bash
#!/bin/bash

# Set the target URL and payload
TARGET_URL="http://example.com/page"
PAYLOAD="{{7*7}}"

# Send a request with the payload injected
curl -X POST -d "input=$PAYLOAD" $TARGET_URL
```

This script sends a POST request to the target URL with the payload injected into an input field named "input".

To prevent SSTI vulnerabilities, follow these best practices:

1. **Input Validation and Sanitization**: Always validate and sanitize user input before incorporating it into server-side templates. Ensure that only safe and expected input is accepted.

2. **Use Safe Templating Engines**: Choose templating engines that have built-in protections against SSTI, such as Jinja2's autoescape feature, which automatically escapes potentially dangerous content.

3. **Avoid Mixing Code and Data**: Separate code from data in templates whenever possible. Avoid directly embedding user input into code constructs or expressions.

4. **Whitelisting**: If possible, use whitelists to specify allowed inputs and reject anything outside of the whitelist.

5. **Regular Updates**: Keep all software, including templating engines and frameworks, up-to-date with the latest security patches to mitigate known vulnerabilities.

6. **Security Audits**: Regularly audit your codebase for potential SSTI vulnerabilities, either manually or using automated scanning tools.

Regarding the types of SSTI vulnerabilities, they can vary based on the templating engine being used. Some common types include:

- **Basic SSTI**: This involves injecting simple template expressions or variables into the template.
- **Nested SSTI**: In some cases, it's possible to nest template expressions within each other to bypass basic protections.
- **Filter Bypass**: Attackers may attempt to bypass filters or sanitization mechanisms by using alternative syntax or encoding techniques.
- **Contextual SSTI**: The behavior of SSTI can vary based on the context in which it occurs, such as within HTML attributes or JavaScript code.

Understanding these variations can help in both testing for and preventing SSTI vulnerabilities effectively.

## Corresponding Tools

- [**Bash**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_bash/server_side_template_injection.sh)
- [**C++**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_c%2B%2B/server_side_template_injection.cpp)
- [**JavaScript**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_javascript/server_side_template_injection.js)
- [**Perl**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_perl/server_side_template_injection.pl)
- [**PHP**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_php/server_site_includes_injection.php)
- [**Python**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_python/server_side_template_injection.py)
- [**Ruby**](https://github.com/saidehossain/Hacking_Tools/blob/main/hacking_with_ruby/server_side_template_injection.rb)

