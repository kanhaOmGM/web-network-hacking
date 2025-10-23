# SQL & Relational Databases – Complete Guide

## Overview

SQL (Structured Query Language) is the standard language for interacting with relational databases. Understanding SQL is crucial for web penetration testing, as SQL injection (SQLi) remains one of the most critical web vulnerabilities. This guide covers database fundamentals, SQL syntax, and security considerations.

## Database Types

### Relational Databases

**Definition:** Store structured data in tabular format with defined relationships between tables.

**Characteristics:**
- Data follows a predefined structure/schema
- Organized in rows and columns within tables
- Relationships established between tables
- ACID compliant (Atomicity, Consistency, Isolation, Durability)
- Examples: MySQL, PostgreSQL, Microsoft SQL Server, Oracle

**Example Structure:**
```
Users Table:
- user_id (Primary Key)
- first_name
- last_name
- email_address
- username
- password

Order_History Table:
- order_id (Primary Key)
- user_id (Foreign Key)
- product_name
- order_date
- total_price
```

**When to Use:**
- Structured data with clear relationships
- Complex queries and joins needed
- Data integrity is critical
- ACID properties required

### Non-Relational Databases

**Definition:** Store data in non-tabular formats without strict schemas.

**Characteristics:**
- Flexible, schema-less structure
- Suitable for varying data types
- Document-based, key-value, graph, or column-family storage
- Examples: MongoDB, Cassandra, Redis, DynamoDB

**Example Use Case:**
- Scanning and storing documents with varying fields
- Unstructured data (JSON documents, logs)
- Rapidly changing data structures

## Key Database Concepts

### Primary Keys

**Definition:** A column (or combination of columns) that uniquely identifies each record in a table.

**Characteristics:**
- Must contain unique values
- Cannot contain NULL values
- Only one primary key per table
- Often auto-incrementing integers

**Example:**
```sql
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

**Security Note:** Predictable primary keys can lead to IDOR (Insecure Direct Object Reference) vulnerabilities.

### Foreign Keys

**Definition:** A column that references the primary key of another table, establishing relationships.

**Purpose:**
- Link tables together
- Maintain referential integrity
- Enable JOIN operations
- Enforce data consistency

**Example:**
```sql
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    order_date DATE,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

**Relationship Types:**
- **One-to-One:** One record in Table A relates to one record in Table B
- **One-to-Many:** One record in Table A relates to multiple records in Table B
- **Many-to-Many:** Multiple records in Table A relate to multiple records in Table B (requires junction table)

## CRUD Operations

CRUD represents the four basic operations for data management:
- **Create:** Insert new records
- **Read:** Retrieve existing records
- **Update:** Modify existing records
- **Delete:** Remove records

### CREATE

#### Creating Databases
```sql
CREATE DATABASE thm_bookmarket_db;
```

#### Creating Tables
```sql
CREATE TABLE book_inventory (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    publication_date DATE
);
```

#### Altering Tables (Adding Columns)
```sql
ALTER TABLE book_inventory
ADD page_count INT;
```

**Common Data Types:**
- `INT` - Integer numbers
- `VARCHAR(n)` - Variable-length strings (max n characters)
- `TEXT` - Long text strings
- `DATE` - Date values (YYYY-MM-DD)
- `DATETIME` - Date and time values
- `DECIMAL(p,s)` - Decimal numbers (precision, scale)
- `BOOLEAN` - True/false values

### CREATE (INSERT)

**Definition:** Add new records to a table.

**Syntax:**
```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

**Example:**
```sql
INSERT INTO books (id, name, published_date, description)
VALUES (1, "Android Security Internals", "2014-10-14", "An In-Depth Guide to Android's Security Architecture");
```

**Multiple Inserts:**
```sql
INSERT INTO users (username, email, role)
VALUES 
    ("alice", "alice@example.com", "admin"),
    ("bob", "bob@example.com", "user"),
    ("charlie", "charlie@example.com", "user");
```

**Security Note:** Always use parameterized queries to prevent SQL injection when inserting user data.

### READ (SELECT)

**Definition:** Retrieve data from tables.

**Basic Syntax:**
```sql
SELECT column1, column2 FROM table_name;
```

**Select All Columns:**
```sql
SELECT * FROM books;
```

**With Conditions (WHERE):**
```sql
SELECT * FROM hacking_tools WHERE category = 'Multi-tool';
```

**Sorting Results (ORDER BY):**
```sql
SELECT * FROM hacking_tools ORDER BY name ASC;
```

**Sorting Options:**
- `ASC` - Ascending order (default)
- `DESC` - Descending order

**Limiting Results:**
```sql
SELECT * FROM users LIMIT 10;
```

### UPDATE

**Definition:** Modify existing records in a table.

**Syntax:**
```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

**Example:**
```sql
UPDATE books
SET description = "An In-Depth Guide to Android's Security Architecture."
WHERE id = 1;
```

** Critical Warning:** Always include a WHERE clause! Without it, ALL records will be updated:
```sql
-- DANGEROUS! Updates ALL records
UPDATE users
SET role = 'admin';

-- SAFE: Updates specific record
UPDATE users
SET role = 'admin'
WHERE user_id = 5;
```

### DELETE

**Definition:** Remove records from a table.

**Syntax:**
```sql
DELETE FROM table_name WHERE condition;
```

**Example:**
```sql
DELETE FROM books WHERE id = 1;
```

** Critical Warning:** Always include a WHERE clause! Without it, ALL records will be deleted:
```sql
-- DANGEROUS! Deletes ALL records
DELETE FROM users;

-- SAFE: Deletes specific record
DELETE FROM users WHERE user_id = 10;
```

**Safer Alternative (Soft Delete):**
```sql
-- Instead of deleting, mark as deleted
UPDATE users
SET is_deleted = 1, deleted_at = NOW()
WHERE user_id = 10;
```

## Advanced SQL Clauses

### DISTINCT

**Purpose:** Remove duplicate values from results.

**Syntax:**
```sql
SELECT DISTINCT column_name FROM table_name;
```

**Example:**
```sql
SELECT DISTINCT category FROM hacking_tools;
```

### GROUP BY

**Purpose:** Aggregate data and group results by column values.

**Syntax:**
```sql
SELECT column1, COUNT(*) 
FROM table_name
GROUP BY column1;
```

**Example:**
```sql
SELECT category, COUNT(*) as tool_count
FROM hacking_tools
GROUP BY category;
```

**Common with Aggregate Functions:**
- `COUNT()` - Count rows
- `SUM()` - Sum values
- `AVG()` - Average values
- `MIN()` - Minimum value
- `MAX()` - Maximum value

### HAVING

**Purpose:** Filter grouped results (WHERE filters before grouping, HAVING filters after).

**Syntax:**
```sql
SELECT column1, COUNT(*)
FROM table_name
GROUP BY column1
HAVING COUNT(*) > value;
```

**Example:**
```sql
SELECT category, COUNT(*) as tool_count
FROM hacking_tools
GROUP BY category
HAVING COUNT(*) > 5;
```

**Difference: WHERE vs HAVING:**
- `WHERE` - Filters individual rows before grouping
- `HAVING` - Filters groups after aggregation

### ORDER BY

**Purpose:** Sort results by one or more columns.

**Syntax:**
```sql
SELECT * FROM table_name
ORDER BY column1 ASC, column2 DESC;
```

**Example:**
```sql
SELECT name, LENGTH(name) AS name_length 
FROM hacking_tools 
ORDER BY name_length ASC;
```

## Logical Operators

### LIKE Operator

**Purpose:** Pattern matching in WHERE clauses.

**Wildcards:**
- `%` - Matches any sequence of characters (zero or more)
- `_` - Matches exactly one character

**Examples:**
```sql
-- Names starting with 'A'
SELECT * FROM users WHERE name LIKE 'A%';

-- Names ending with 'son'
SELECT * FROM users WHERE name LIKE '%son';

-- Names containing 'admin'
SELECT * FROM users WHERE name LIKE '%admin%';

-- Names with exactly 5 characters
SELECT * FROM users WHERE name LIKE '_____';

-- Names starting with 'J' and ending with 'n'
SELECT * FROM users WHERE name LIKE 'J%n';
```

### AND Operator

**Purpose:** Combine multiple conditions (all must be TRUE).

**Example:**
```sql
SELECT name FROM hacking_tools 
WHERE amount < 100 AND category = 'Network Intelligence';
```

### OR Operator

**Purpose:** Combine conditions (at least one must be TRUE).

**Example:**
```sql
SELECT * FROM users 
WHERE role = 'admin' OR role = 'moderator';
```

### NOT Operator

**Purpose:** Negate a condition.

**Example:**
```sql
SELECT * FROM products 
WHERE NOT category = 'Electronics';
```

### BETWEEN Operator

**Purpose:** Test if value exists within a range (inclusive).

**Example:**
```sql
SELECT * FROM products 
WHERE price BETWEEN 10 AND 50;

SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
```

### Comparison Operators

**Equal To (=):**
```sql
SELECT * FROM hacking_tools WHERE category = 'Multi-tool';
```

**Not Equal To (!=):**
```sql
SELECT * FROM users WHERE role != 'guest';
```

**Less Than (<):**
```sql
SELECT * FROM products WHERE price < 100;
```

**Greater Than (>):**
```sql
SELECT * FROM products WHERE stock > 50;
```

**Less Than or Equal To (<=):**
```sql
SELECT category FROM hacking_tools WHERE amount <= 300;
```

**Greater Than or Equal To (>=):**
```sql
SELECT category FROM hacking_tools WHERE amount >= 300;
```

**Complex Condition Example:**
```sql
SELECT name FROM hacking_tools 
WHERE amount < 100 
  AND category = 'Network Intelligence'
  AND price BETWEEN 20 AND 100;
```

## SQL Functions

### String Functions

#### CONCAT()

**Purpose:** Combine multiple strings into one.

**Syntax:**
```sql
CONCAT(string1, string2, ...)
```

**Example:**
```sql
SELECT CONCAT(name, " & ", category, " book.") AS book_info 
FROM books;
```

**Output:** `"Android Security Internals & Security book."`

#### GROUP_CONCAT()

**Purpose:** Concatenate values from multiple rows into a single string.

**Syntax:**
```sql
GROUP_CONCAT(column SEPARATOR 'delimiter')
```

**Example:**
```sql
SELECT GROUP_CONCAT(name, '&') AS conc 
FROM hacking_tools 
WHERE amount % 10 != 0;
```

**Output:** `"Nmap&Wireshark&Metasploit"`

#### SUBSTRING()

**Purpose:** Extract a portion of a string.

**Syntax:**
```sql
SUBSTRING(string, start_position, length)
```

**Example:**
```sql
SELECT SUBSTRING(name, 1, 5) AS short_name 
FROM products;

-- Extract from position 3 to end
SELECT SUBSTRING(email, 3) FROM users;
```

#### LENGTH()

**Purpose:** Return the number of characters in a string (includes spaces and punctuation).

**Syntax:**
```sql
LENGTH(string)
```

**Example:**
```sql
SELECT name, LENGTH(name) AS name_length 
FROM hacking_tools 
ORDER BY name_length ASC;
```

### Aggregate Functions

#### COUNT()

**Purpose:** Count the number of records.

**Example:**
```sql
SELECT COUNT(*) AS total_users FROM users;

SELECT COUNT(DISTINCT category) AS unique_categories 
FROM products;
```

#### SUM()

**Purpose:** Calculate the sum of numeric values (ignores NULL).

**Example:**
```sql
SELECT SUM(amount) AS total FROM hacking_tools;

SELECT category, SUM(price) AS category_total
FROM products
GROUP BY category;
```

#### MIN()

**Purpose:** Find the minimum value in a column.

**Example:**
```sql
SELECT MIN(price) AS cheapest FROM products;

SELECT category, MIN(price) AS min_price
FROM products
GROUP BY category;
```

#### MAX()

**Purpose:** Find the maximum value in a column.

**Example:**
```sql
SELECT MAX(price) AS most_expensive FROM products;

SELECT category, MAX(amount) AS max_stock
FROM inventory
GROUP BY category;
```

#### AVG()

**Purpose:** Calculate the average of numeric values.

**Example:**
```sql
SELECT AVG(price) AS average_price FROM products;

SELECT category, AVG(rating) AS avg_rating
FROM products
GROUP BY category;
```

## JOIN Operations

### INNER JOIN

**Purpose:** Return records with matching values in both tables.

**Example:**
```sql
SELECT users.username, orders.order_date, orders.total
FROM users
INNER JOIN orders ON users.user_id = orders.user_id;
```

### LEFT JOIN

**Purpose:** Return all records from left table, and matching records from right table.

**Example:**
```sql
SELECT users.username, orders.order_id
FROM users
LEFT JOIN orders ON users.user_id = orders.user_id;
```

### RIGHT JOIN

**Purpose:** Return all records from right table, and matching records from left table.

### FULL OUTER JOIN

**Purpose:** Return all records when there's a match in either table.

## Practical SQL Examples

### Example 1: E-commerce Query
```sql
SELECT 
    u.username,
    COUNT(o.order_id) AS total_orders,
    SUM(o.total_price) AS total_spent
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
GROUP BY u.user_id, u.username
HAVING total_orders > 5
ORDER BY total_spent DESC;
```

### Example 2: Inventory Management
```sql
SELECT 
    category,
    COUNT(*) AS product_count,
    SUM(stock) AS total_stock,
    AVG(price) AS avg_price
FROM products
WHERE stock > 0
GROUP BY category
HAVING product_count > 3;
```

### Example 3: User Search
```sql
SELECT * FROM users
WHERE 
    (username LIKE '%admin%' OR email LIKE '%admin%')
    AND created_date >= '2024-01-01'
    AND is_active = 1
ORDER BY created_date DESC
LIMIT 20;
```

## SQL Injection (SQLi) - Security Perspective

### What is SQL Injection?

SQL Injection occurs when user input is improperly validated and directly concatenated into SQL queries, allowing attackers to manipulate database operations.

### Vulnerable Code Example

**Backend (Vulnerable):**
```php
$username = $_POST['username'];
$password = $_POST['password'];

$query = "SELECT * FROM users 
          WHERE username = '$username' 
          AND password = '$password'";
```

**Attack Vector:**
```
Username: admin' OR '1'='1' --
Password: anything
```

**Resulting Query:**
```sql
SELECT * FROM users 
WHERE username = 'admin' OR '1'='1' --' AND password = 'anything'
```

The `--` comments out the rest, and `'1'='1'` is always true, bypassing authentication.

### Common SQLi Techniques

#### 1. Authentication Bypass
```
' OR '1'='1' --
' OR 1=1 --
admin' --
' OR 'a'='a
```

#### 2. Union-Based SQLi
```sql
' UNION SELECT null, username, password FROM users --
```

#### 3. Error-Based SQLi
```
' AND 1=CONVERT(int, (SELECT @@version)) --
```

#### 4. Blind SQLi (Boolean)
```
' AND 1=1 --  (true - normal response)
' AND 1=2 --  (false - different response)
```

#### 5. Time-Based Blind SQLi
```sql
' AND SLEEP(5) --
' OR IF(1=1, SLEEP(5), 0) --
```

### Prevention Methods

#### 1. Parameterized Queries (Prepared Statements)

**PHP (PDO):**
```php
$stmt = $pdo->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->execute([$username, $password]);
```

**Python:**
```python
cursor.execute("SELECT * FROM users WHERE username = %s AND password = %s", 
               (username, password))
```

#### 2. Stored Procedures

```sql
CREATE PROCEDURE GetUser(IN user_name VARCHAR(50))
BEGIN
    SELECT * FROM users WHERE username = user_name;
END;
```

#### 3. Input Validation

- Whitelist acceptable characters
- Limit input length
- Use type checking
- Escape special characters

#### 4. Least Privilege Principle

- Database users should have minimal permissions
- Read-only access for SELECT operations
- Separate users for different operations

#### 5. Web Application Firewall (WAF)

- Filter malicious patterns
- Block common SQLi attempts
- Log suspicious activity

### Testing for SQL Injection

**Manual Testing Payloads:**
```
'
"
`
')
")
`)
'))
"))
`))
' OR '1'='1
' OR 1=1 --
' OR 'a'='a
1' ORDER BY 1--
1' ORDER BY 2--
1' ORDER BY 3--
1' UNION SELECT NULL--
1' UNION SELECT NULL, NULL--
```

**Tools:**
- **SQLMap:** Automated SQL injection tool
- **Burp Suite:** Intercept and test requests
- **Manual testing:** Always most thorough

## Database Security Best Practices

### 1. Access Control
- Strong authentication
- Role-based access control (RBAC)
- Principle of least privilege
- Regular access audits

### 2. Encryption
- Encrypt sensitive data at rest
- Use TLS/SSL for data in transit
- Hash passwords (bcrypt, Argon2)
- Never store plaintext passwords

### 3. Input Validation
- Validate all user input
- Use parameterized queries
- Sanitize special characters
- Implement length restrictions

### 4. Error Handling
- Don't expose database errors to users
- Log errors securely
- Generic error messages for users
- Detailed logging for administrators

### 5. Monitoring & Auditing
- Log all database access
- Monitor for suspicious queries
- Regular security audits
- Implement intrusion detection

### 6. Regular Updates
- Keep database software updated
- Apply security patches promptly
- Update libraries and drivers
- Follow vendor security advisories

## Common SQL Security Mistakes

###  Hardcoded Credentials
```sql
-- NEVER DO THIS
$conn = mysqli_connect("localhost", "root", "password123", "database");
```

###  Dynamic Query Building
```php
// VULNERABLE
$query = "SELECT * FROM users WHERE id = " . $_GET['id'];
```

###  Exposing Database Errors
```php
// DANGEROUS
die("Database error: " . mysqli_error($conn));
```

###  Excessive Permissions
```sql
-- TOO PERMISSIVE
GRANT ALL PRIVILEGES ON *.* TO 'webapp'@'%';
```

###  Correct Approaches
```php
// Use prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$id]);

// Generic error messages
if (!$result) {
    error_log("Database error: " . mysqli_error($conn));
    die("An error occurred. Please try again later.");
}

// Minimal permissions
GRANT SELECT, INSERT, UPDATE ON app_db.* TO 'webapp'@'localhost';
```

## Conclusion

SQL is a powerful language for data management, but requires careful security considerations. Key takeaways:

1. **Always use parameterized queries** - Primary defense against SQLi
2. **Never trust user input** - Validate and sanitize everything
3. **Implement least privilege** - Minimize database user permissions
4. **Use prepared statements** - Not string concatenation
5. **Test thoroughly** - Both functional and security testing
6. **Monitor continuously** - Log and audit database access
7. **Keep updated** - Apply security patches regularly

Understanding SQL from both development and security perspectives is essential for building secure applications and conducting effective penetration tests. SQL injection remains a critical vulnerability, making proper database security practices more important than ever.
