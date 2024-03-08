### What is LFI Vulnerability?

LFI is a type of security vulnerability that occurs when a web application allows a user to input a file path into the application with the intention of accessing or executing files on the server. If the application does not properly validate or sanitize user input, it can lead to the inclusion of arbitrary files from the server's filesystem, including sensitive system files, configuration files, or even code files.

### How LFI Works:

1. **Input Point**: The web application takes user input, typically via URL parameters or form fields, containing a file path.
2. **File Inclusion**: The application includes the file specified by the user input without proper validation.
3. **Execution**: The server processes the request, including the file specified by the user, leading to the execution of arbitrary code or disclosure of sensitive information.

### Types of LFI Vulnerabilities:

1. **Direct LFI**: Directly includes files specified by the user input.
2. **Indirect LFI**: Includes files indirectly through other files or scripts.
3. **Null Byte Injection**: Exploits languages or systems that use null bytes to terminate strings, allowing attackers to bypass input validation.

### Testing for LFI Vulnerabilities:

1. **Manual Testing**: Manually inject file paths into input fields or URL parameters to observe server responses.
2. **Automated Tools**: Utilize tools like Burp Suite, OWASP ZAP, or custom scripts to automate scanning for LFI vulnerabilities.

### Bash Script to Test LFI:

```bash
#!/bin/bash

# Change the URL and parameters as per your target
URL="http://example.com/vulnerable.php?page="

# File paths to test
FILES=(
    "/etc/passwd"
    "/etc/hosts"
    "/var/log/apache2/access.log"
)

for FILE in "${FILES[@]}"
do
    echo "Testing: $URL$FILE"
    curl -s -o /dev/null -w "%{http_code}\n" "$URL$FILE"
done
```

### Preventing LFI Vulnerabilities:

1. **Input Validation**: Validate and sanitize all user input to ensure it only contains expected values.
2. **Restrict File Access**: Limit file inclusion to specific directories and prevent access to sensitive system files.
3. **Use Whitelists**: Define a whitelist of allowed files and only include files from this list.
4. **Disable Dynamic File Inclusion**: Avoid dynamic file inclusion whenever possible, especially when it involves user-controlled input.

### Example Mitigation (in PHP):

```php
<?php
$page = $_GET['page'];
$allowed_pages = array('page1.php', 'page2.php', 'page3.php');

if (in_array($page, $allowed_pages)) {
    include($page);
} else {
    // Redirect to an error page or handle the error gracefully
    echo "Invalid page!";
}
?>
```

This script only includes files specified in the `$allowed_pages` array, preventing arbitrary file inclusion.

By understanding how LFI vulnerabilities work, testing methods, prevention measures, and examples provided, you're better equipped to identify, mitigate, and prevent these vulnerabilities in web applications.
