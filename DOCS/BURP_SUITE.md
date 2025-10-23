# Burp Suite – Overview & Key Features

## Overview

Burp Suite captures and enables manipulation of all HTTP/HTTPS traffic between a browser and a web server. This fundamental capability forms the backbone of the framework. By intercepting requests, users have the flexibility to route them to various components within the Burp Suite framework.

Burp Suite is frequently used when attacking web applications and mobile applications.

## Editions

### Community Edition
Basic features including:
- Proxy
- Repeater
- Decoder
- Basic tools

**Limitations:**
- Rate-limited Intruder
- Cannot save projects

### Professional Edition
Unrestricted version with additional features:
- **Automated vulnerability scanner**
- **Fuzzer/brute-forcer** that isn't rate limited
- **Saving projects** for future use and report generation
- **Built-in API** to allow integration with other tools
- **Unrestricted access** to add new extensions for greater functionality
- **Access to Burp Suite Collaborator** (effectively providing a unique request catcher self-hosted or running on a Portswigger-owned server)

### Enterprise Edition
Primarily utilized for continuous scanning:
- **Automated scanner** that periodically scans web applications for vulnerabilities, similar to how tools like Nessus perform automated infrastructure scanning
- **Server-based:** Unlike Community and Professional editions (which allow manual attacks from a local machine), Enterprise resides on a server and constantly scans target web applications for potential vulnerabilities

## Key Tools

### Proxy
The most renowned aspect of Burp Suite.

**Features:**
- Enables interception and modification of requests and responses while interacting with web applications
- When requests are made through Burp Proxy, they are intercepted and held back from reaching the target server
- Requests appear in the Proxy tab, allowing for further actions such as forwarding, dropping, editing, or sending them to other Burp modules
- To disable intercept and allow requests to pass through without interruption, click the **Intercept is on** button

**Key Points:**
- Taking control: The ability to intercept requests empowers testers to gain complete control over web traffic, making it invaluable for testing web applications
- **Capture and Logging:** Burp Suite captures and logs requests made through the proxy by default, even when interception is turned off. This logging functionality can be helpful for later analysis and review of prior requests
- **WebSocket Support:** Burp Suite also captures and logs WebSocket communication
- **Logs and History:** Captured requests can be viewed in the HTTP history and WebSockets history sub-tabs, allowing for retrospective analysis and sending requests to other Burp modules as needed

### Repeater
Another well-known feature.

**Purpose:** Allows for capturing, modifying, and resending the same request multiple times.

**Use Cases:**
- Particularly useful when crafting payloads through trial and error (e.g., in SQLi - Structured Query Language Injection)
- Testing the functionality of an endpoint for vulnerabilities

### Intruder
Despite rate limitations in Burp Suite Community, Intruder allows for spraying endpoints with requests.

**Common Uses:**
- Brute-force attacks
- Fuzzing endpoints

### Decoder
Offers a valuable service for data transformation.

**Features:**
- Can decode captured information
- Can encode payloads before sending them to the target
- While alternative services exist for this purpose, leveraging Decoder within Burp Suite can be highly efficient

### Comparer
Enables the comparison of two pieces of data at either the word or byte level.

**Benefit:** While not exclusive to Burp Suite, the ability to send potentially large data segments directly to a comparison tool with a single keyboard shortcut significantly accelerates the process.

### Sequencer
Typically employed when assessing the randomness of tokens.

**Purpose:**
- Assess session cookie values or other supposedly randomly generated data
- If the algorithm used for generating these values lacks secure randomness, it can expose avenues for devastating attacks

## Dashboard Features

### Tasks
Allows you to define background tasks that Burp Suite will perform while you use the application.

**Default Task:**
- In Burp Suite Community, the default "Live Passive Crawl" task automatically logs the pages visited, which is sufficient for basic use
- Burp Suite Professional offers additional features like on-demand scans

### Event Log
Provides information about:
- Actions performed by Burp Suite (such as starting the proxy)
- Details about connections made through Burp

### Issue Activity (Professional Only)
Displays the vulnerabilities identified by the automated scanner, ranked by severity and filterable based on the certainty of the vulnerability.

### Advisory (Professional Only)
Provides more detailed information about identified vulnerabilities, including:
- References
- Suggested remediations
- Can be exported into a report

**Note:** In Burp Suite Community, this section may not show any vulnerabilities.

## Target Tab

### Site Map
Allows us to map out the web applications we are targeting in a tree structure.

**Features:**
- Every page visited while the proxy is active will be displayed on the site map
- Enables automatically generating a site map by simply browsing the web application
- **Professional:** Can use the site map to perform automated crawling of the target, exploring links between pages and mapping out as much of the site as possible
- **Community:** Can still utilize the site map to accumulate data during initial enumeration steps
- Particularly useful for mapping out APIs, as any API endpoints accessed by the web application will be captured in the site map

### Issue Definitions
Although Burp Community does not include the full vulnerability scanning functionality available in Burp Suite Professional, we still have access to a list of all the vulnerabilities that the scanner looks for.

**Content:** Provides an extensive list of web vulnerabilities, complete with descriptions and references. This resource can be valuable for referencing vulnerabilities in reports or assisting in describing a particular vulnerability that may have been identified during manual testing.

### Scope Settings
Allows us to control the target scope in Burp Suite.

**Purpose:**
- Include or exclude specific domains/IPs to define the scope of testing
- By managing the scope, we can focus on the web applications we are specifically targeting and avoid capturing unnecessary traffic

**Why Scope is Important:**
- Capturing and logging all traffic can quickly become overwhelming and inconvenient, especially when we only want to focus on specific web applications
- By setting a scope for the project, we can define what gets proxied and logged in Burp Suite
- We can restrict Burp Suite to target only the specific web application(s) we want to test

**How to Set Scope:**
1. Switch to the **Target** tab
2. Right-click on your target from the list on the left
3. Select **"Add To Scope"**
4. Burp will prompt you to choose whether you want to stop logging anything that is not in scope
5. In most cases, you want to select **"yes"**

## Settings

### Global Settings
Settings that affect the entire Burp Suite installation and are applied every time you start the application. They provide a baseline configuration for your Burp Suite environment.

### Project Settings
Settings specific to the current project and apply only during the session.

**Important:** Burp Suite Community Edition does not support saving projects, so any project-specific options will be lost when you close Burp.

### Accessing Settings
Click on the **Settings** button in the top navigation bar. This will open a separate settings window.

**Settings Window Features:**
- **Search:** Enables searching for specific settings using keywords
- **Type filter:** Filters the settings for User and Project options
- **User settings:** Shows settings that affect the entire Burp Suite installation
- **Project settings:** Displays settings specific to the current project
- **Categories:** Allows selecting settings by category

**Note:** If you have uploaded Client-Side TLS certificates, you can override these on a per-project basis (yea).

## Keyboard Shortcuts

| Shortcut | Function |
|----------|----------|
| `Ctrl + Shift + D` | Dashboard |
| `Ctrl + Shift + T` | Target tab |
| `Ctrl + Shift + P` | Proxy tab |
| `Ctrl + Shift + I` | Intruder tab |
| `Ctrl + Shift + R` | Repeater tab |

## Workflow Notes

### Using Scope for Focused Testing
- Use scope to reduce noise and focus tests
- Capture and log carefully – large captures can be overwhelming
- Add a host to scope to focus testing

### Professional Edition Benefits
- Issue/Advisory sections provide vulnerability details and suggested remediation
- Automated scanning capabilities
- Unrestricted Intruder for brute-forcing/fuzzing
- Project saving and reporting features

## Summary

Burp Suite is the industry-standard tool for web application security testing. The framework's ability to intercept, modify, and analyze HTTP/HTTPS traffic makes it essential for:
- Web application penetration testing
- Mobile application security testing
- API security testing
- Vulnerability discovery and exploitation
