### What is CSRF?

CSRF is a type of attack that occurs when a malicious website, email, or other means of communication tricks a user's web browser into executing an unwanted action on a trusted site where the user is authenticated. The attacker essentially forges a request to a trusted site on behalf of the victim.

### How does CSRF work?

1. **User Authentication**: The victim user is authenticated on a trusted website, usually by logging in and receiving a session cookie.
2. **Malicious Request**: The attacker tricks the victim into visiting a malicious website or clicking on a malicious link.
3. **Forgery**: The malicious website sends a request to the trusted website in the background, exploiting the user's active session without their knowledge.
4. **Execution**: The trusted website receives and executes the forged request, thinking it's coming from the legitimate user.

### Testing for CSRF Vulnerabilities

To test for CSRF vulnerabilities, you typically follow these steps:

1. **Identify Sensitive Actions**: Determine which actions on the target website (e.g., changing passwords, making transactions) are sensitive and should require user consent.
2. **Create a Test Harness**: Develop a webpage with malicious HTML and JavaScript that sends forged requests to the target site.
3. **Execute the Attack**: Trick the victim into visiting the malicious webpage or clicking on a link.
4. **Check for Success**: Verify if the forged request was executed on the target website without the victim's knowledge.

### Bash Script to Test CSRF

Here's a simple Bash script to demonstrate a CSRF attack:

```bash
#!/bin/bash

# Send a forged request to change the victim's email address
curl -X POST -d "new_email=attacker@example.com" https://targetwebsite.com/change-email
```

This script simulates sending a POST request to change the victim's email address on the target website.

### Preventing CSRF Attacks

Several countermeasures can mitigate CSRF attacks:

1. **CSRF Tokens**: Include unique, randomly generated tokens with each request and verify them on the server side.
2. **SameSite Cookies**: Set cookies to "SameSite" mode to prevent them from being sent in cross-origin requests.
3. **Referrer Policy**: Implement a strict referrer policy to control which URLs can send requests.
4. **Double Submit Cookies**: Require the client to send a cookie and a token in the request, checking if they match on the server side.

### Types of CSRF Bypass Techniques

1. **Referer Header Manipulation**: Some browsers allow attackers to manipulate the Referer header to trick the server into accepting the forged request.
2. **Login CSRF**: Exploits authentication mechanisms where the session is established before validating the CSRF token.
3. **GET-Based CSRF**: Exploits GET requests that have side effects, such as changing data or state on the server.
4. **Script-Initiated Attacks**: Use JavaScript to initiate the CSRF attack, bypassing some security mechanisms.

By understanding how CSRF attacks work and implementing proper prevention techniques, you can significantly reduce the risk of exploitation. Regular testing and staying updated on emerging CSRF bypass techniques are also crucial for maintaining robust security posture.

Below is a simple Go program to demonstrate a Cross-Site Request Forgery (CSRF) vulnerability:

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    // Server setup
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        // Render HTML form with a CSRF token
        fmt.Fprintf(w, `
            <html>
            <head>
                <title>CSRF Test</title>
            </head>
            <body>
                <h1>CSRF Test</h1>
                <form action="/transfer" method="POST">
                    <input type="hidden" name="amount" value="1000">
                    <input type="hidden" name="to" value="attacker">
                    <input type="hidden" name="csrf_token" value="CSRF_TOKEN_HERE">
                    <input type="submit" value="Transfer Money">
                </form>
            </body>
            </html>
        `)
    })

    // Handler for the transfer endpoint
    http.HandleFunc("/transfer", func(w http.ResponseWriter, r *http.Request) {
        // Validate CSRF token
        csrfToken := r.FormValue("csrf_token")
        if csrfToken != "CSRF_TOKEN_HERE" {
            http.Error(w, "CSRF token validation failed", http.StatusBadRequest)
            return
        }

        // Process transfer (for demonstration purposes)
        amount := r.FormValue("amount")
        to := r.FormValue("to")
        fmt.Fprintf(w, "Transfer $%s to %s is successful!\n", amount, to)
    })

    // Start server
    fmt.Println("Server listening on port 8080")
    http.ListenAndServe(":8080", nil)
}
```

This program creates a simple HTTP server with two endpoints:

1. `/`: Displays a form with a hidden CSRF token.
2. `/transfer`: Accepts a POST request to simulate a money transfer, but it requires a correct CSRF token.

To test the CSRF vulnerability:

1. Run the program.
2. Open a browser and go to `http://localhost:8080`.
3. Submit the form. This will perform a POST request to `/transfer` with the predefined CSRF token.
4. You should see a message confirming the money transfer.

To exploit the CSRF vulnerability:

1. Create a malicious HTML page with a form that sends a POST request to `http://localhost:8080/transfer` with arbitrary data, excluding the CSRF token.
2. Trick a user into visiting the malicious page while they are logged into your application.

When the user submits the form on the malicious page, it will perform a POST request to `/transfer` without the valid CSRF token, but the server will still process the request, demonstrating the CSRF vulnerability.
