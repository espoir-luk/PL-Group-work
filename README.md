## GROUP MEMBERS
Rukundo Espoir [27678]

Gatera K Jessica [27630]

Shyaka Chriss [27889]


# Bookstore Database Project
## Overview
This project is a short report detailing the design and implementation of a relational database for a bookstore. It was created as a group assignment to demonstrate fundamental database concepts using SQL. The database consists of three related tables: Authors, Books, and Sales.

# Key Concepts Demonstrated
**Database Design**: Creating tables with appropriate columns and applying constraints (PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL).

**Joins**: Using INNER, LEFT, RIGHT, and FULL joins to retrieve data from multiple related tables.

**Indexing**: Creating an index on a foreign key to optimize query performance.

**Views**: Creating a view to simplify a frequently used query.

# Database Schema
The database is composed of the following tables:

**Authors**: Stores author information.

author_id (PRIMARY KEY)

first_name

last_name

author_name (UNIQUE)

**Books**: Stores book information.

book_id (PRIMARY KEY)

title

publication_year

author_id (FOREIGN KEY)

**Sales**: Records sales transactions.

sale_id (PRIMARY KEY)

book_id (FOREIGN KEY)

sale_date

price

# SQL Queries
All queries used for this project are included in the repository for reference.

# Table Creation and Data Insertion

-- Create the Authors table

CREATE TABLE Authors (
    author_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    author_name VARCHAR(100) UNIQUE NOT NULL
);

-- Create the Books table

CREATE TABLE Books (
    book_id INT PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    publication_year INT,
    author_id INT,
    FOREIGN KEY (author_id) REFERENCES Authors(author_id)
);

-- Create the Sales table

CREATE TABLE Sales (
    sale_id INT PRIMARY KEY,
    book_id INT,
    sale_date DATE,
    price DECIMAL(5, 2) NOT NULL,
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);

-- Insert sample data into all tables

INSERT INTO Authors (author_id, first_name, last_name, author_name) 
VALUES
(1, 'Jane', 'Austen', 'Jane Austen'),
(2, 'George', 'Orwell', 'George Orwell'),
(3, 'J.K.', 'Rowling', 'J.K. Rowling');

INSERT INTO Books (book_id, title, publication_year, author_id) 
VALUES
(101, 'Pride and Prejudice', 1813, 1),
(102, '1984', 1949, 2),
(103, 'Harry Potter and the Sorcerer''s Stone', 1997, 3),
(104, 'Animal Farm', 1945, 2),
(105, 'Emma', 1815, 1);

INSERT INTO Sales (sale_id, book_id, sale_date, price) 
VALUES
(501, 101, '2025-09-01', 12.99),
(502, 102, '2025-09-02', 15.50),
(503, 104, '2025-09-02', 10.00),
(504, 103, '2025-09-03', 25.75),
(505, 101, '2025-09-04', 12.99);

# Joins
The project report demonstrates the use of joins to combine data from multiple tables.

-- INNER JOIN: Find all books that have sales records.

SELECT
    b.title,
    b.publication_year,
    s.sale_date,
    s.price
FROM
    Books b
INNER JOIN
    Sales s ON b.book_id = s.book_id;

-- LEFT JOIN: Find all authors and the books they have written.

SELECT
    a.first_name,
    a.last_name,
    b.title
FROM
    Authors a
LEFT JOIN
    Books b ON a.author_id = b.author_id;

-- RIGHT JOIN: Find all books and the authors who wrote them.
SELECT
    b.title,
    a.first_name,
    a.last_name
FROM
    Authors a
RIGHT JOIN
    Books b ON a.author_id = b.author_id;

-- FULL JOIN (for MySQL): Combines a LEFT and RIGHT join to get all records from both tables.

SELECT
    a.first_name,
    a.last_name,
    b.title
FROM
    Authors a
LEFT JOIN
    Books b ON a.author_id = b.author_id
UNION
SELECT
    a.first_name,
    a.last_name,
    b.title
FROM
    Authors a
RIGHT JOIN
    Books b ON a.author_id = b.author_id
WHERE a.author_id IS NULL;

# Index and View

-- Create an index to improve query performance on the Books table

CREATE INDEX idx_author_id ON Books(author_id);

-- Create a view to simplify a frequently used query

CREATE VIEW vw_BooksAndAuthors AS
SELECT
    b.title,
    a.author_name
FROM
    Books b
INNER JOIN
    Authors a ON b.author_id = a.author_id;
