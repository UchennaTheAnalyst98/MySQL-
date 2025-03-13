#           BUILDING A DATABASE, WE CODE;



DROP DATABASE IF EXISTS jtech;

CREATE DATABASE IF NOT EXISTS jtech;

# TO MAKE USE OF THE DATABASE, WE EITHER USE THE FUNCTION "USE" OR WE SET AS DEFAULT

USE jtech;

# LET'S CREATE A TABLE EMPLOYEES, WE CODE;
CREATE TABLE employees (
	  employee_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(30) NOT NULL,
    last_name VARCHAR(30),
    email VARCHAR(100) UNIQUE,
    phone_number CHAR(15) UNIQUE,
    hire_date DATE);

SELECT *
FROM employees;


# WE WANT TO RESET THE AUTO_INCREMENT TO 1000
ALTER TABLE employees
AUTO_INCREMENT = 1000;

# ADDING COLUMNS "SALARY", "GENDER" INTO THE EMPLOYEES TABLE, WE CODE;
ALTER TABLE employees
ADD COLUMN salary int
AFTER phone_number;

ALTER TABLE employees
ADD COLUMN gender CHAR(8)
AFTER last_name;

SELECT *
FROM employees;

# LETS POPULATE OUR TABLE WITH FEW RECORDS, WE CODE;
INSERT INTO employees (first_name, last_name, gender, email, phone_number, salary, hire_date)
VALUES	("Uchenna", "Otuonye", "Male", "uchenna@jtech.com", "08099332100", 23000, "2022-01-24");

SELECT * FROM employees;


# STILL ON ADDING MORE RECORDS;
INSERT INTO employees (first_name, last_name, gender, email, phone_number, salary, hire_date)
VALUES	("Festus", "Daniel", "Male", "festus@jtech.com", "08099893100", 19000, "2023-04-14"),
		    ("Sandra", "Nathaniel", "Female", "sandra@jtech.com","07099239085", 21500, "2023-01-22"),
        ("Gideon", "Ebuka", NULL, "gideon@jtech.com", "08123908465", 18000, "2024-01-29"),
        ("Hannah", "Solomon", "Female", "hannah@jtech.com", "07043674859", 20000, "2023-06-17"),
        ("Ibrahim", "Suleiman", NULL, NULL, "07021900083", 21000, "2022-12-28");
        
SELECT *
FROM employees;

# ADDING A NEW EMPLOYEE BUT THE SALARY SHOULD NOT BE LESS THAN 10000 IN ACCORDANCE TO OUR FIRMS REMUNERATION POLICIES.
ALTER TABLE employees
ADD CONSTRAINT chk_sal
CHECK (salary >= 10000);

INSERT INTO employees (first_name, last_name, gender, email, phone_number, salary, hire_date)
VALUES	("Jessica", "Chibuike", NULL, "jessica@jtech.com", "09092348576", 10500, "2025-02-03");

SELECT *
FROM employees;


# UPDATING THE INFORMATIONS OF OUR EMPLOYEES WITH GENDER SET TO NULL.
UPDATE employees
SET gender = "Male"
WHERE employee_id IN (1003, 1005);

UPDATE employees
SET gender = "Female"
WHERE employee_id = 1006;

# UPDATING THE INFORMATION OF THE EMAIL COLUMN FOR THOSE WHO DON'T HAVE AN EMAIL ADDRESS;
UPDATE employees
SET email = "ibrahimm@jtech.com"
WHERE employee_id = 1005;

SELECT *
FROM employees;

# CREATING A NEW TABLE FOR CUSTOMERS
CREATE TABLE customers (
	  customer_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(30) NOT NULL,
    last_name VARCHAR(30),
    phone_number CHAR(15) UNIQUE NOT NULL,
    email_address VARCHAR(100) UNIQUE,
    house_address TEXT);
    
SELECT *
FROM customers;

# SETTING THE AUTO_INCREMENT FROM 8000
ALTER TABLE customers
AUTO_INCREMENT = 8000;

# ADDING RECORDS TO THE CUSTOMERS TABLE.
INSERT INTO customers (first_name, last_name, phone_number, email_address, house_address)
VALUES	("Faith", "Dineme", "07012536475", "faithy63@gmail.com", "19, badure, Abuja"),
		    ("Simba", "Faruq", "08123990768", "faruq90@gmail.com", "rayfield resort, Plateau"),
        ("Habiba", "Suleiman", "07035465190", "habibs88@yahoo.com", "dinret str, Taraba");


SELECT *
FROM customers;

# LETS ADD ANOTHER COLUMN OF GENDER AS WELL;
ALTER TABLE customers
ADD COLUMN gender CHAR(9)
AFTER last_name;

SELECT *
FROM customers;

# UPDATE INDIVIDUAL RECORDS WITH THEIR RESPECTIVE GENDERS
UPDATE customers
SET gender = "Male"
WHERE customer_id IN (8000, 8002);

UPDATE customers
SET gender = "Female"
WHERE customer_id IN (8001, 8003);

SELECT * FROM customers;

# CREATING THE ORDERS TABLE
CREATE TABLE orders (
	  order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    employee_id INT NOT NULL,
    order_date DATE NOT NULL,
    order_status ENUM("Pending", "Shipped", "Delivered", "Canceled"),
    FOREIGN KEY (customer_id) REFERENCES customers (customer_id),
    FOREIGN KEY (employee_id) REFERENCES employees (employee_id));

ALTER TABLE orders
AUTO_INCREMENT = 100200;


SELECT * FROM employees;

INSERT INTO orders (customer_id, employee_id, order_date, order_status)
VALUES	(8000, 1004, '2025-03-01', 'Pending'),
    		(8003, 1006, '2025-03-02', 'Shipped'),
    		(8001, 1000, '2025-03-03', 'Delivered'),
    		(8003, 1002, '2025-03-04', 'Canceled'),
    		(8000, 1005, '2025-03-05', 'Pending'),
    		(8002, 1003, '2025-03-06', 'Shipped'),
    		(8002, 1003, '2025-03-07', 'Delivered'),
    		(8001, 1004, '2025-03-08', 'Pending'),
    		(8002, 1005, '2025-03-09', 'Shipped'),
    		(8003, 1003, '2025-03-10', 'Delivered'),
    		(8001, 1006, '2025-03-11', 'Pending'),
    		(8002, 1002, '2025-03-12', 'Shipped'),
    		(8001, 1006, '2025-03-13', 'Delivered'),
    		(8000, 1003, '2025-03-14', 'Canceled'),
    		(8003, 1004, '2025-03-15', 'Pending'),
    		(8001, 1002, '2025-03-16', 'Shipped'),
    		(8000, 1006, '2025-03-17', 'Delivered'),
    		(8002, 1003, '2025-03-18', 'Pending');
        
ALTER TABLE orders
ADD COLUMN product_id INT
AFTER employee_id;

# UPDATING THE RECORDS OF OUR ORDERS TABLE FOR THE COLUMN PRODUCT_ID WITH DIFFERENT PRODUCT_ID
UPDATE orders
SET product_id = CASE 
	  WHEN order_id = 100215 THEN 10
    WHEN order_id = 100213 THEN 19
    WHEN order_id = 100204 THEN 12
END 
WHERE order_id IN (100215, 100213, 100204);

SELECT * FROM orders;

# STILL ON THE UPDATE
UPDATE orders
SET product_id = CASE
	  WHEN  order_id = 100200 THEN 08
    WHEN order_id = 100201 THEN 14
    WHEN order_id = 100212 THEN 13
    WHEN order_id = 100205 THEN 13
    WHEN order_id = 100203 THEN 21
    WHEN order_id = 100207 THEN 12
END
WHERE order_id IN (100200, 100201, 100212, 100205, 100203, 100207);


# LETS TRY ANOTHER EASIER FORMAT WITH SAME PRODUCT_ID FOR DIFFERENT ORDER_ID
UPDATE orders
SET product_id = 17
WHERE order_id IN (100209, 100211);

UPDATE orders
SET product_id = 13
WHERE order_id IN (100216, 100208);

UPDATE orders
SET product_id = 05
WHERE order_id IN (100216);

UPDATE orders
SET product_id = 07
WHERE order_id IN (100206);

# CREATING THE PRODUCTS TABLE
CREATE TABLE products (
	  product_id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(50),
    category CHAR(20),
    price FLOAT,
    stock_quantity INT NOT NULL,
    supplier_id INT,
    added_date TIMESTAMP DEFAULT NOW());

ALTER TABLE products
AUTO_INCREMENT = 1000;
			
INSERT INTO products (product_name, category, price, stock_quantity, supplier_id) 
VALUES	('Apple iPhone 13', 'Electronics', 899.99, 50, 2001),
    		('Samsung Galaxy S22', 'Electronics', 799.99, 60, 2001),
    		('Dell XPS 13 Laptop', 'Electronics', 1299.99, 40, 2002),
    		('Sony WH-1000XM4 Headphones', 'Electronics', 349.99, 70, 2003),
    		('Logitech MX Master 3 Mouse', 'Electronics', 99.99, 100, 2004),
    		('Nike Air Force 1 Sneakers', 'Fashion', 120.00, 80, 2005),
    		('Adidas Ultraboost Running Shoes', 'Fashion', 180.00, 50, 2005),
    		('Levi’s 501 Jeans', 'Fashion', 60.00, 90, 2006),
    		('Ray-Ban Aviator Sunglasses', 'Fashion', 150.00, 40, 2007),
    		('Michael Kors Leather Handbag', 'Fashion', 250.00, 30, 2008),
    		('Wooden Dining Table', 'Home & Furniture', 499.99, 20, 2009),
    		('Memory Foam Mattress', 'Home & Furniture', 699.99, 15, 2010),
    		('Dyson V11 Vacuum Cleaner', 'Home & Furniture', 599.99, 25, 2011),
    		('IKEA Bookshelf', 'Home & Furniture', 199.99, 35, 2012),
    		('Instant Pot Duo 7-in-1', 'Home & Furniture', 89.99, 55, 2013),
    		('Organic Almonds - 1kg', 'Groceries', 15.99, 100, 2014),
    		('Italian Olive Oil - 500ml', 'Groceries', 12.99, 80, 2015),
    		('Nespresso Coffee Capsules - 10pk', 'Groceries', 7.99, 120, 2016),
    		('Kellogg’s Corn Flakes - 750g', 'Groceries', 5.49, 150, 2017),
    		('Coca-Cola - 12 Pack', 'Groceries', 8.99, 90, 2018),
    		('Dumbbell Set - 20kg', 'Sports & Fitness', 75.00, 50, 2019),
    		('Yoga Mat - Non-Slip', 'Sports & Fitness', 25.00, 70, 2020);
  
SELECT * FROM products;

SELECT * FROM orders;

SELECT * FROM customers;

SELECT * FROM employees;
