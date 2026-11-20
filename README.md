# sql-customer-management
DROP DATABASE IF EXISTS Shopping_Management;
create database Shopping_Management ;
use Shopping_Management;





DROP TABLE IF EXISTS orders;

DROP TABLE IF EXISTS customers;


-- STEP 1: Create Parent Table

CREATE TABLE customers (

    Customer_Id MEDIUMINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    Customer_Name VARCHAR(100) NOT NULL,
    Email VARCHAR(254) UNIQUE,
    Phone_Number CHAR(10) NOT NULL UNIQUE,
    Age TINYINT UNSIGNED,
    Gender ENUM('Male', 'Female', 'Other'),
    Country VARCHAR(50) DEFAULT 'India',
    City VARCHAR(50) NOT NULL,
    Pincode CHAR(6) DEFAULT '201301',
    Registration_Date DATE DEFAULT (CURRENT_DATE),
    Registration_Time TIME DEFAULT (CURRENT_TIME),
    Created_At TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    Address VARCHAR(250),
    Customer_Status ENUM('Active', 'Inactive', 'Blocked')
    DEFAULT 'Active',
    Customer_Notes VARCHAR(250),

    CHECK (Age > 0 OR Age IS NULL),
    CHECK (Phone_Number REGEXP '^[0-9]{10}$'),
    CHECK (Pincode REGEXP '^[0-9]{6}$')

);


-- STEP 2: Create Child Table with Foreign Key

CREATE TABLE orders (

    Order_Id MEDIUMINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    Customer_Id MEDIUMINT UNSIGNED NOT NULL,
    Order_Date DATE DEFAULT (CURRENT_DATE),
    Order_Amount DECIMAL(10,2) NOT NULL,

    Order_Status ENUM(
        'Pending', 'Processing', 'Shipped',
        'Delivered', 'Cancelled'
    ) DEFAULT 'Pending',

    Payment_Method ENUM(
        'Cash', 'Card', 'UPI', 'Net Banking'
    ),

    Payment_Status ENUM(
        'Pending', 'Paid', 'Failed'
    ) DEFAULT 'Pending',

    Shipping_Address VARCHAR(250) NOT NULL,
    Delivery_Pincode CHAR(6),
    Quantity SMALLINT UNSIGNED NOT NULL,
    Discount_Amount DECIMAL(10,2) DEFAULT 0.00,
    Delivery_Date DATE DEFAULT NULL,
    Created_At TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    Tracking_Number VARCHAR(30) UNIQUE,

    Delivery_Status ENUM(
        'Not Dispatched', 'In Transit',
        'Delivered', 'Returned'
    ) DEFAULT 'Not Dispatched',

    Order_Notes VARCHAR(250),

    CHECK (Quantity > 0),

    FOREIGN KEY (Customer_Id)
    REFERENCES customers(Customer_Id)
    ON DELETE CASCADE
    ON UPDATE CASCADE

);


-- STEP 3: Insert values into customers

INSERT INTO customers
(Customer_Name, Email, Phone_Number, Age, Gender,
Country, City, Pincode, Address)
VALUES
('Rahul Sharma', 'rahul@gmail.com', '9876543210',
25, 'Male', 'India', 'Noida', '201301', 'Sector 15'),

('Priya Verma', 'priya@gmail.com', '9876543211',
24, 'Female', 'India', 'Delhi', '110001', 'Sector 10'),

('Aman Singh', 'aman@gmail.com', '9876543212',
26, 'Male', 'India', 'Lucknow', '226001', 'Hazratganj');


-- STEP 4: Insert values into orders

INSERT INTO orders
(Customer_Id, Order_Amount, Order_Status,
Payment_Method, Payment_Status, Shipping_Address,
Delivery_Pincode, Quantity, Discount_Amount,
Tracking_Number, Delivery_Status, Order_Notes)
VALUES

(1, 1500.50, 'Shipped', 'UPI', 'Paid',
'Sector 15, Noida', '201301', 2, 100.00,
'TRK1001', 'In Transit', 'First order'),

(1, 2500.00, 'Pending', 'Card', 'Pending',
'Sector 15, Noida', '201301', 1, 0.00,
'TRK1002', 'Not Dispatched', 'Second order'),

(2, 3500.75, 'Delivered', 'Cash', 'Paid',
'Sector 10, Delhi', '110001', 3, 200.00,
'TRK1003', 'Delivered', 'Delivered successfully'),

(3, 1200.00, 'Processing', 'UPI', 'Paid',
'Hazratganj, Lucknow', '226001', 1, 50.00,
'TRK1004', 'Not Dispatched', 'Order processing');


-- STEP 5: Display records

SELECT * FROM customers;
SELECT * FROM orders;


-- STEP 6: Test ON UPDATE CASCADE

UPDATE customers
SET Customer_Id = 101
WHERE Customer_Id = 1;

SELECT * FROM customers;
SELECT * FROM orders;


-- STEP 7: Test ON DELETE CASCADE

DELETE FROM customers
WHERE Customer_Id = 2;

SELECT * FROM customers;
SELECT * FROM orders;


-- STEP 8: Check table structure

DESCRIBE customers;
DESCRIBE orders;




