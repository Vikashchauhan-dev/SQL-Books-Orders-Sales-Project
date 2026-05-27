# Books Orders Sales Analysis

## Project Overview

This SQL project analyzes a bookstore sales database using MySQL queries. The project includes database creation, table creation, CSV data import, data cleaning, and SQL analysis queries for generating business insights.

The project helps understand:

* Book sales performance
* Customer purchasing behavior
* Revenue generation
* Stock management
* Genre and author analysis

---

# Project Objectives

* Create and manage a bookstore database using MySQL
* Import CSV datasets into SQL tables
* Perform data cleaning operations
* Execute basic and advanced SQL queries
* Generate meaningful business insights from sales data

---

# Tools and Technologies Used

| Tool / Technology | Purpose               |
| ----------------- | --------------------- |
| MySQL             | Database Management   |
| SQL               | Data Analysis Queries |
| CSV Files         | Dataset Storage       |
| MySQL Workbench   | Query Execution       |

---

# Dataset Description (Data Dictionary)

## 1. Books Table

| Column Name    | Data Type     | Description              |
| -------------- | ------------- | ------------------------ |
| Book_ID        | INT           | Unique ID for each book  |
| Title          | VARCHAR(150)  | Name of the book         |
| Author         | VARCHAR(50)   | Author name              |
| Genre          | VARCHAR(50)   | Book category or genre   |
| Published_Year | INT           | Year of publication      |
| Price          | DECIMAL(10,2) | Book price               |
| Stock          | INT           | Available stock quantity |

---

## 2. Customers Table

| Column Name | Data Type    | Description            |
| ----------- | ------------ | ---------------------- |
| Customer_ID | INT          | Unique customer ID     |
| Name        | VARCHAR(50)  | Customer name          |
| Email       | VARCHAR(100) | Customer email address |
| Phone       | BIGINT       | Customer phone number  |
| City        | VARCHAR(50)  | Customer city          |
| Country     | VARCHAR(150) | Customer country       |

---

## 3. Orders Table

| Column Name  | Data Type     | Description                |
| ------------ | ------------- | -------------------------- |
| Order_ID     | INT           | Unique order ID            |
| Customer_ID  | INT           | Customer placing the order |
| Book_ID      | INT           | Ordered book ID            |
| Order_Date   | DATE          | Date of order              |
| Quantity     | INT           | Quantity ordered           |
| Total_Amount | DECIMAL(10,2) | Total order amount         |

---
# Key Questions Answered

1. Which books belong to the Fiction genre?
2. Which books were published after 1950?
3. Which customers are from Canada?
4. Which orders were placed in November 2023?
5. What is the total stock available in the bookstore?
6. Which is the most expensive book?
7. Which orders contain quantity greater than 1?
8. Which orders have total amount greater than $20?
9. What genres are available in the bookstore?
10. Which book has the lowest stock?
11. What is the total revenue generated from all orders?
12. How many books were sold in each genre?
13. What is the average price of Fantasy books?
14. Which customers placed at least 2 orders?
15. Which book is ordered most frequently?
16. Which are the top 3 expensive Fantasy books?
17. How many books were sold by each author?
18. Which cities contain customers spending over $30?
19. Which customer spent the highest amount on orders?
20. What stock remains after completing all orders?

---

# Data Exploratory Analysis (SQL Analysis & Queries)

## Basic SQL Queries

### 1. Retrieve all books in the "Fiction" genre

```sql
SELECT * FROM Books
WHERE Genre = 'Fiction';
```

### 2. Find books published after the year 1950

```sql
SELECT Title, Published_Year FROM Books
WHERE Published_Year > 1950
ORDER BY Published_Year;
```

### 3. List all customers from the Canada

```sql
SELECT * FROM Customers
WHERE TRIM(Country) = 'Canada';
```

### 4. Show orders placed in November 2023

```sql
SELECT * FROM Orders
WHERE Order_Date BETWEEN '2023-11-01' AND '2023-11-30'
ORDER BY Order_Date;
```

### 5. Retrieve the total stock of books available

```sql
SELECT SUM(Stock) AS Total_Stock_Of_Books
FROM Books;
```

### 6. Find the details of the most expensive book

```sql
SELECT * FROM Books
WHERE Price = (SELECT MAX(Price) FROM Books);
```

### 7. Show all customers who ordered more than 1 quantity of a book

```sql
SELECT * FROM Orders
WHERE Quantity > 1;
```

### 8. Retrieve all orders where the total amount exceeds $20

```sql
SELECT * FROM Orders
WHERE Total_Amount > 20;
```

### 9. List all genres available in the Books table

```sql
SELECT DISTINCT Genre FROM Books;
```

### 10. Find the book with the lowest stock

```sql
SELECT * FROM Books
WHERE Stock = (SELECT MIN(Stock) FROM Books);
```

### 11. Calculate the total revenue generated from all orders

```sql
SELECT SUM(Total_Amount)
FROM Orders;
```

---

### 12. Retrieve the total number of books sold for each genre

```sql
SELECT b.Genre, SUM(o.Quantity) AS Total_Books
FROM Books b
JOIN Orders o
ON o.Book_ID = b.Book_ID
GROUP BY b.Genre;
```

### 13. Find the average price of books in the "Fantasy" genre

```sql
SELECT ROUND(AVG(Price),2) AS Avg_Price
FROM Books
WHERE Genre = 'Fantasy';
```

### 14. List customers who have placed at least 2 orders

```sql
SELECT c.Customer_ID, c.Name,
COUNT(o.Order_ID) AS No_Of_Orders
FROM Customers c
JOIN Orders o
ON o.Customer_ID = c.Customer_ID
GROUP BY c.Customer_ID, c.Name
HAVING No_Of_Orders >= 2;
```

### 15. Find the most frequently ordered book

```sql
SELECT b.Book_ID, b.Title,
COUNT(o.Order_ID) AS NO_Orders
FROM Orders o
JOIN Books b
ON b.Book_ID = o.Book_ID
GROUP BY b.Book_ID, b.Title
ORDER BY NO_Orders DESC;
```

### 16. Show the top 3 most expensive books of 'Fantasy' Genre

```sql
SELECT * FROM Books
WHERE Genre = 'Fantasy'
ORDER BY Price DESC
LIMIT 3;
```

### 17. Retrieve the total quantity of books sold by each author

```sql
SELECT b.Author,
SUM(o.Quantity) AS Total_Quantity
FROM Books b
JOIN Orders o
ON o.Book_ID = b.Book_ID
GROUP BY b.Author;
```

### 18. List the cities where customers who spent over $30 are located

```sql
SELECT c.City, o.Total_Amount
FROM Customers c
JOIN Orders o
ON o.Customer_ID = c.Customer_ID
WHERE o.Total_Amount > 30;
```

### 19. Find the customer who spent the most on orders

```sql
SELECT c.Customer_ID, c.Name,
COUNT(o.Order_ID) AS No_Orders,
SUM(o.Total_Amount) AS Total_Amt_Spent
FROM Orders o
JOIN Customers c
ON o.Customer_ID = c.Customer_ID
GROUP BY c.Customer_ID, c.Name
ORDER BY Total_Amt_Spent DESC;
```

### 20. Calculate the stock remaining after fulfilling all orders

```sql
SELECT b.Book_ID, b.Title, b.Stock,
COALESCE(SUM(o.Quantity),0) AS Order_Qty,
b.Stock - COALESCE(SUM(o.Quantity),0) AS Remaining_Stock
FROM Books b
LEFT JOIN Orders o
ON o.Book_ID = b.Book_ID
GROUP BY b.Book_ID;
```
---
# Key Insights

* Fiction and Fantasy books are among the most analyzed genres.
* Some customers placed multiple orders, showing repeat purchasing behavior.
* Revenue analysis helps identify high-value orders.
* Stock tracking helps monitor inventory availability.
* Author-wise and genre-wise analysis improves business decision-making.

---

# Project Structure

```text
books-orders-sales-sql-project/
│
├── Dataset/
│   ├── Books.csv
│   ├── Customers.csv
│   └── Orders.csv
│
├── SQL Queries/
│   └── Books_Orders_Sales_Projects.sql
│
├── README.md
│
└── Screenshots/
    └── output_images.png
```
---
# Result & Conclusion

This SQL project successfully demonstrates:

* Database creation and management
* Data import using CSV files
* Data cleaning techniques
* Basic and advanced SQL analysis
* Business insight generation from bookstore sales data

The project improves SQL query writing skills and provides practical experience in data analysis using MySQL.

---
## Author & Contact

---
### Author
Vikash Chauhan

### Contact
- LinkedIn: www.linkedin.com/in/vikashchauhan01
- GitHub: https://github.com/Vikashchauhan-dev
- Email: Vikashchauhan10211@gmail.com
---
