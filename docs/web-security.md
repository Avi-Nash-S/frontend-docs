# 🔐 Web security

## Summary

By adhering to these best practices, you can significantly enhance the security of your frontend application and protect against a wide range of attacks. Remember that security is an ongoing process, and regular audits and updates are essential to maintaining a secure application.

```text
- Input Sanitize
- Prevent Cross-Site-Scripting XSS
- Add Content security policy CSP (via headers)
- Prevent Cross-Site-Request-Forgery CSRF (with csrf tokens)
- Always use HTTPs and WSS (secure only)
- Cookies with sameSite and secure 
- Evaluate third party dependencies in regular interval
- IFrame - X-Frame-Options (to block embedding your site) 
- IFrame - frame-ancestors in CSP (to block embedding your site)
- Secure client side logic and proper error handling
```

## 1. **Input Validation and Sanitization**

- **Validate Input**: Always validate user input on the client side, though remember that client-side validation is not a substitute for server-side validation.
- **Sanitize Input**: Remove or encode potentially harmful characters from user input to prevent injection attacks like Cross-Site Scripting (XSS).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Input Sanitization Example</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/dompurify/2.3.4/purify.min.js"></script>
</head>
<body>
    <div id="output"></div>
    <script>
        function sanitizeInput(input) {
            return DOMPurify.sanitize(input);
        }

        const userInput = '<img src=x onerror=alert("XSS Attack!") />';
        document.getElementById('output').innerHTML = sanitizeInput(userInput);
    </script>
</body>
</html>
```

## 2. **Cross-Site Scripting (XSS) Prevention**

- **Output Encoding**: Encode data before rendering it in the HTML to prevent XSS attacks. Use libraries like DOMPurify to sanitize HTML.
- **Avoid `eval`**: Avoid using `eval()` or similar functions (`setTimeout`, `setInterval` with string arguments) which can execute arbitrary code.
- **CSP (Content Security Policy)**: Implement CSP headers to restrict the sources from which content (scripts, styles, etc.) can be loaded.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>XSS Prevention Example</title>
</head>
<body>
    <div id="output"></div>
    <script>
        function escapeHTML(input) {
            const div = document.createElement('div');
            div.appendChild(document.createTextNode(input));
            return div.innerHTML;
        }

        const userInput = '<script>alert("XSS Attack!")</script>';
        document.getElementById('output').innerHTML = escapeHTML(userInput);
    </script>
</body>
</html>
```

## 3. **Cross-Site Request Forgery (CSRF) Prevention**

- **CSRF Tokens**: Use CSRF tokens to ensure that requests made on behalf of users are legitimate.
- **SameSite Cookies**: Set cookies with the `SameSite` attribute to prevent them from being sent along with cross-site requests.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSRF Prevention Example</title>
</head>
<body>
    <button id="submitBtn">Submit</button>
    <script>
        document.getElementById('submitBtn').addEventListener('click', () => {
            const csrfToken = 'your-csrf-token';

            fetch('/your-api-endpoint', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'CSRF-Token': csrfToken
                },
                body: JSON.stringify({ data: 'example data' })
            }).then(response => {
                if (response.ok) {
                    return response.json();
                }
                throw new Error('Network response was not ok.');
            }).then(data => {
                console.log(data);
            }).catch(error => {
                console.error('There has been a problem with your fetch operation:', error);
            });
        });
    </script>
</body>
</html>
```

## 4. **Authentication and Authorization**

- **Use Secure Authentication Methods**: Implement strong authentication mechanisms (e.g., OAuth, JWT) and ensure that passwords are hashed and salted before storing.
- **Access Control**: Implement proper access control checks to ensure users can only access resources they are authorized to.

## 5. **Secure Communication**

- **HTTPS**: Always use HTTPS to encrypt data transmitted between the client and server. Use HSTS (HTTP Strict Transport Security) to enforce HTTPS.
- **Secure WebSockets**: If using WebSockets, ensure they are secure (wss://) to protect against man-in-the-middle attacks.

## 6. **Secure Storage**

- **Avoid Storing Sensitive Data**: Avoid storing sensitive data (like passwords or tokens) in local storage or session storage, as they are accessible through JavaScript.
- **Secure Cookies**: Store tokens and other sensitive information in cookies with `HttpOnly` and `Secure` flags.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Secure Cookie Example</title>
</head>
<body>
    <script>
        function setSecureCookie(name, value, days) {
            let expires = '';
            if (days) {
                const date = new Date();
                date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
                expires = '; expires=' + date.toUTCString();
            }
            document.cookie = `${name}=${value || ''}${expires}; path=/; Secure; HttpOnly; SameSite=Strict`;
        }

        setSecureCookie('sessionId', '123456', 7);
    </script>
</body>
</html>
```

## 7. **Third-Party Dependencies**

- **Review and Monitor Dependencies**: Regularly review and update third-party libraries to ensure they do not introduce vulnerabilities.
- **Use Subresource Integrity (SRI)**: Use SRI to ensure that the content of external scripts and stylesheets has not been tampered with.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SRI Example</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"
            integrity="sha384-KyZXEAg3QhqLMpG8r+Knujsl5+5hb7O9l5m5V6zr9bU8pmM/sG4XfrdeL8wo/fFz"
            crossorigin="anonymous"></script>
</head>
<body>
    <script>
        $(document).ready(function() {
            console.log('jQuery loaded with SRI.');
        });
    </script>
</body>
</html>
```

## 8. **Error Handling**

- **Generic Error Messages**: Avoid displaying detailed error messages to users. Instead, log errors on the server and show generic messages to users.
- **Catch Errors**: Use try-catch blocks and error handling mechanisms to prevent application crashes and exposure of stack traces.

## 9. **Clickjacking Protection**

- **X-Frame-Options**: Use the `X-Frame-Options` header to prevent your site from being embedded in iframes, which can prevent clickjacking attacks.
- **CSP Frame Ancestors**: Use the `frame-ancestors` directive in CSP to control which sites can embed your pages.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' https://cdnjs.cloudflare.com">
    <title>Content Security Policy Example</title>
</head>
<body>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            console.log('jQuery is loaded and CSP is in effect.');
        });
    </script>
</body>
</html>
```

## 10. **Secure Client-Side Logic**

- **Minify and Obfuscate**: Minify and obfuscate JavaScript code to make it harder for attackers to understand and tamper with.
- **Avoid Sensitive Logic on Client Side**: Avoid putting sensitive logic or business rules on the client side where they can be easily inspected and manipulated.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Avoid Eval Example</title>
</head>
<body>
    <script>
        // Instead of using eval
        // let result = eval('2 + 2');
        // Use function constructors or direct code
        let result = (function() {
            return 2 + 2;
        })();
        console.log(result); // Output: 4
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Secure Event Handlers Example</title>
</head>
<body>
    <button id="submitBtn">Submit</button>
    <script>
        document.getElementById('submitBtn').addEventListener('click', () => {
            console.log('Button clicked!');
        });
    </script>
</body>
</html>
```

[Home](../README.md)
