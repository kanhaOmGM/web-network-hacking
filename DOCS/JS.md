# JavaScript (JS) – Essentials & Security Notes

## Overview

JavaScript is an **interpreted language** executed directly in the browser without prior compilation. It's used for dynamic interaction and UI logic on web pages.

**Key Security Consideration:** When pen-testing web applications, check whether JS is internal or external, as this affects attack surfaces (e.g., XSS, malicious external libraries).

## Internal vs External JavaScript

### Internal JS

**Definition:** JavaScript code embedded directly inside `<script>` tags in HTML.

**Characteristics:**
- Good for small scripts or demo code
- Script can be placed in `<head>` section (loaded before page content renders) or `<body>` section (interacts with elements as they load)

**Example:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script>
        let x = 5;
        let y = 10;
        let result = x + y;
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```

### External JS

**Definition:** JavaScript stored in separate `.js` files and referenced with `<script src="...">`.

**Characteristics:**
- Keeps HTML clean and scalable
- Can be hosted on the same web server or external servers (e.g., cloud)

**Example:**

**script.js:**
```javascript
let x = 5;
let y = 10;
let result = x + y;
document.getElementById("result").innerHTML = "The result is: " + result;
```

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <!-- Link to the external JS file -->
    <script src="script.js"></script>
</body>
</html>
```

**How it works:** The `src` attribute in the `<script>` tag loads the JS from an external file. When the browser loads the page, it looks for `script.js` and loads its content into the HTML document.

## Identifying Internal vs External JS (Pen-Testing)

When pen-testing a web application, verify whether the site uses internal or external JS by viewing the page's source code.

**Indicators:**
- **Internal JS:** Code directly inside `<script>` tags, no `src` attribute
- **External JS:** `<script src="filename.js">` or `<script src="external_url">` with a `src` attribute

## User Interaction Functions

JS provides built-in functions for user interaction. If not implemented securely, attackers may exploit these features to execute attacks like Cross-Site Scripting (XSS).

### alert()
Displays a modal alert box.

**Example - Alert Loop:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Hacked</title>
</head>
<body>
    <script>
        for (let i = 0; i < 3; i++) {
            alert("Hacked");
        }
    </script>
</body>
</html>
```

### prompt()
Displays a dialog box requesting user input.

```javascript
let username = prompt("Enter your username:");
```

### confirm()
Displays a dialog with OK/Cancel options.

```javascript
let userChoice = confirm("Are you sure?");
```

## Control Flow in JavaScript

Control flow refers to the order in which statements and code blocks are executed based on conditions. JS provides:
- **Conditional statements:** `if-else`, `switch`
- **Loops:** `for`, `while`, `do...while`

Proper use of control flow ensures programs can handle various conditions effectively.

### Bypassing Client-Side Authentication Example

**Vulnerable Login Code:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Login Page</title>
</head>
<body>
    <h2>Login Authentication</h2>

    <script>
        let username = prompt("Enter your username:");
        let password = prompt("Enter your password:");

        if (username === "admin" && password === "ComplexPassword") {
            document.write("You are successfully authenticated!");
        } else {
            document.write("Authentication failed. Incorrect username or password.");
        }
    </script>
</body>
</html>
```

**Credentials:** `admin` / `ComplexPassword`

**Vulnerability:** Authentication logic is client-side and credentials are visible in source code. Attackers can view source, disable JS, or modify the logic to bypass authentication.

## Minification and Obfuscation

### Minification

**Purpose:** Compress JS files by removing unnecessary characters (spaces, line breaks, comments) and shortening variable names. This reduces file size and improves loading time, especially in production.

**Result:** Makes code more compact and harder to read for humans, but functions exactly the same.

### Obfuscation

**Purpose:** Make JS harder to understand by:
- Adding undesired code
- Renaming variables/functions to meaningless names
- Inserting dummy code

**Note:** Obfuscation makes code difficult to analyze, but attackers can eventually reverse engineer it with effort.

## Best Practices

### 1. Avoid Relying on Client-Side Validation Only

**Problem:** JS performs client-side validation, but users can disable or manipulate JS on the client side.

**Solution:** Always perform validation on the server side as well. Client-side validation is for user experience only, not security.

### 2. Refrain from Adding Untrusted Libraries

**Problem:** JS allows including external scripts using the `src` attribute in `<script>` tags. Bad actors have uploaded malicious libraries with names resembling legitimate ones.

**Solution:** Only include libraries from trusted sources. Verify the library source before adding it to your application.

### 3. Avoid Hardcoded Secrets

**Problem:** Users can easily check the source code and see any hardcoded passwords, API keys, access tokens, or credentials.

**Solution:** Never hardcode sensitive data like API keys, access tokens, or credentials into your JS code.

### 4. Minify and Obfuscate Your JavaScript Code

**When:** Always minify and obfuscate JS code when using it in production.

**Benefits:**
- Reduces file size
- Improves load time
- Makes it harder for attackers to understand code logic

**Note:** Attackers can still reverse engineer it, but it takes more effort to get the original code.

## Common Security Issues

When pen-testing JS in web applications, look for:

- **Hardcoded credentials** in source code
- **Client-side validation** that can be bypassed
- **Malicious external libraries** loaded from untrusted sources
- **Sensitive data exposure** (API keys, tokens, passwords)
- **Logic flaws** that can be exploited by manipulating JS
- **XSS vulnerabilities** in user interaction functions

## Key Takeaways

1. **Internal JS** is embedded in HTML; **External JS** is in separate `.js` files
2. Always check page source to identify JS type when pen-testing
3. Client-side validation provides **no security** – only user experience
4. **Never hardcode secrets** in JS – source is always visible
5. Use **minification and obfuscation** in production to make analysis harder
6. Only use **trusted libraries** to avoid supply chain attacks
7. Always validate inputs on the **server side**, not just client side
