# banking_system_DB

A complete database project designed to manage core banking operations, including customers, accounts, loans, and transactions. This project demonstrates the use of SQL, normalization, database relationships, joins, triggers, and ACID properties using MySQL Workbench.

📘 Step 1: Objective / Aim

The aim of this project is to design and implement a database system that handles essential banking functions such as:

Customer management

Account management

Loan records

Transactions


The system ensures data consistency, accuracy, and secure handling of financial operations.


🛠️ Step 2: Tools Used

Component	Details

Software	MySQL Workbench 8.x
Language	SQL (Structured Query Language)
Platform	Windows 10 / 11
Hardware	Dell Latitude E6440 (Intel i5, 8GB RAM)



🗄️ Step 3: Creating the Database

A new database named banking_system was created to store all tables related to the banking project.

CREATE DATABASE banking_system;
USE banking_system;



🧱 Step 4: Creating Tables

Four main tables were created:

1. Customer


2. Account


3. Loan


4. Transaction



Each table includes a Primary Key, and tables are linked using Foreign Keys.

✅ Customer Table

Stores customer details such as name, phone, and email.

✅ Account Table

Stores account type, balance, and customer reference.

✅ Loan Table

Stores loan amount, type, status, and customer reference.

✅ Transaction Table

Stores deposits/withdrawals linked with account ID.


📚 Normalization

Normalization organizes data to reduce duplication and improve consistency.

1NF:

Each table has a primary key

No repeating groups

Each field has atomic values


2NF:

Table is already in 1NF

Non-key fields depend fully on the primary key


3NF:

Table is in 2NF

No transitive dependencies


How Normalization Was Applied

Customer, Account, Loan, and Transaction tables were separated

Each table stores only one category of information

Foreign keys connect related data

No repeated or unnecessary values



🗃️ Step 5: Viewing Tables

To check if tables are created:

SHOW TABLES;

To view structure:

DESCRIBE customer;
DESCRIBE account;
DESCRIBE loan;
DESCRIBE transaction;


🔗 Defining Relationships

Primary Key:

Unique identifier for each record.

Foreign Key:

Links one table to another using the primary key.

Relationships:

One Customer → Many Accounts (1:N)

One Customer → Many Loans (1:N)

One Account → Many Transactions (1:N)



🖼️ ER Diagram

The ERD shows how all four tables are connected through primary and foreign keys.

To generate ERD in MySQL Workbench:

Database → Reverse Engineer → Select database → Finish



📝 Step 6: Insert Sample Data

Sample data was inserted into:

Customer

Account

Loan

Transaction


This allows queries to be tested properly.



🔍 Step 7: Checking Data

Use:

SELECT * FROM customer;
SELECT * FROM account;
SELECT * FROM loan;
SELECT * FROM transaction;



▶️ Step 8: Executing Queries (System Demonstration)

1. Show all customers

SELECT * FROM customer;

2. Show accounts with customer names

SELECT a.*, c.customer_name 
FROM account a 
JOIN customer c ON a.customer_id = c.customer_id;

3. Show loans with customer names

SELECT l.*, c.customer_name 
FROM loan l 
JOIN customer c ON l.customer_id = c.customer_id;

4. Show transactions with account & customer details

SELECT t.*, a.account_type, c.customer_name  
FROM transaction t
JOIN account a ON t.account_id = a.account_id
JOIN customer c ON a.customer_id = c.customer_id;

5. Customers with approved loans

SELECT c.customer_name, l.status 
FROM customer c 
JOIN loan l ON c.customer_id = l.customer_id
WHERE l.status = 'Approved';

6. Total balance in all accounts

SELECT SUM(balance) AS total_balance FROM account;

7. Customers with savings accounts

SELECT c.customer_name 
FROM customer c
JOIN account a ON c.customer_id = a.customer_id
WHERE a.account_type = 'Savings';

8. Total approved loan amount

SELECT SUM(amount) FROM loan WHERE status='Approved';

9. Number of transactions per account

SELECT account_id, COUNT(*) AS total_transactions 
FROM transaction 
GROUP BY account_id;

10. Total amount deposited by each customer

SELECT c.customer_name, SUM(t.amount)
FROM transaction t
JOIN account a ON t.account_id = a.account_id
JOIN customer c ON a.customer_id = c.customer_id
WHERE t.type='Deposit'
GROUP BY c.customer_name;


🔗 Step 9: SQL Joins

INNER JOIN

Returns only matching records.

LEFT JOIN

Returns all records from left table even without matches.

RIGHT JOIN

Returns all records from the right table.

FULL JOIN

(MySQL simulates this with UNION.)

Examples:

1. Customers and their accounts

SELECT c.customer_name, a.account_type
FROM customer c
LEFT JOIN account a ON c.customer_id = a.customer_id;

2. Accounts with transactions

SELECT a.account_id, t.amount, t.type
FROM account a
LEFT JOIN transaction t ON a.account_id = t.account_id;

3. Customers with no loans

SELECT c.customer_name
FROM customer c
LEFT JOIN loan l ON c.customer_id = l.customer_id
WHERE l.loan_id IS NULL;


⚙️ Step 10: Database Trigger

A trigger was created to automatically update account balance when a new transaction occurs.

Trigger Logic:

If transaction is a Deposit → balance increases

If Withdrawal → balance decreases


This ensures accuracy without manual updates.



🧪 Step 11: ACID Properties

A – Atomicity

All steps of a transaction complete or none do.

C – Consistency

Data remains accurate before & after a transaction.

I – Isolation

Transactions don’t interfere with each other.

D – Durability

Once saved, data remains safe even after crashes.

A money transfer example demonstrates all ACID steps.


🔐 Step 12: Security & Recovery

🔒 Security Measures

User accounts with passwords

Limited privileges

Only admins can modify/delete data

Regular backups


Example:
Create read-only user:

CREATE USER 'bank_user'@'localhost' IDENTIFIED BY '12345';
GRANT SELECT ON banking_system.* TO 'bank_user';

🛠️ Recovery

Using COMMIT & ROLLBACK:

START TRANSACTION;

UPDATE account SET balance = balance - 500 WHERE account_id=1;
UPDATE account SET balance = balance + 500 WHERE account_id=2;

COMMIT;

If failure occurs → ROLLBACK.


🏁 Conclusion

This project successfully demonstrates a complete banking database system with:

Well-structured normalized tables

Clear relationships

Use of joins

Automated triggers

ACID-compliant transactions

Security and recovery mechanisms


It provides a strong understanding of how real banking systems store and manage data reliably.
