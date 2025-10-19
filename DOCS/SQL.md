
---

## `docs/SQL.md`
```markdown
# SQL & Relational Databases — Summary & Examples

**Summary:** Relational DBs store structured tabular data with relationships; non-relational DBs store non-tabular documents. CRUD = Create, Read, Update, Delete. :contentReference[oaicite:16]{index=16}

## Concepts
- **Primary Key:** unique identifier for rows.
- **Foreign Key:** linking column to another table.
- **CRUD:** basic operations (INSERT, SELECT, UPDATE, DELETE).

## Examples
```sql
CREATE DATABASE thm_bookmarket_db;

CREATE TABLE book_inventory (
  book_id INT AUTO_INCREMENT PRIMARY KEY,
  book_name VARCHAR(255) NOT NULL,
  publication_date DATE
);

ALTER TABLE book_inventory
ADD page_count INT;

INSERT INTO books (id, name, published_date, description)
VALUES (1, "Android Security Internals", "2014-10-14", "An In-Depth Guide to Android's Security Architecture");

UPDATE books
SET description = "An In-Depth Guide to Android's Security Architecture."
WHERE id = 1;

DELETE FROM books WHERE id = 1;
