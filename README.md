🎯 Objective

To understand how Cloud Databases work by creating a managed SQL database instance, connecting to it securely, and executing SQL commands for table creation, data insertion, and retrieval.

🧰 Tools Used

Cloud Provider: AWS RDS (Free Tier)
Database Engine: MySQL
Connection Tools: MySQL Workbench / DBeaver / AWS CloudShell
Optional IDEs: VS Code, Terminal

⚙️ Steps to Perform
1️⃣ Create a Cloud Database Instance

Log in to AWS Management Console.
Go to RDS → Databases → Create Database.
Select Standard Create → MySQL.
Choose Free Tier template.
Configure:
Region: Nearest to you (e.g., ap-south-1)
DB Instance Identifier: database-1
Master Username: admin
Master Password: yourpassword
Enable Public Access → Yes.
Select a VPC with DNS Hostnames and DNS Resolution enabled.
Wait until the instance status shows Available.

2️⃣ Configure Access

In Connectivity & Security, copy the Endpoint.

Make sure:

Port 3306 is open in your RDS security group.

Your system’s public IP is added to the inbound rules.

3️⃣ Connect to the Database

Use any of the following methods:

🔹 MySQL Workbench / DBeaver

Hostname: database-1.xxxxx.ap-south-1.rds.amazonaws.com
Username: admin
Password: yourpassword
Port: 3306


🔹 CloudShell (Alternative)

mysql -h database-1.xxxxx.ap-south-1.rds.amazonaws.com -u admin -p

4️⃣ Create Database and Table
CREATE DATABASE intern_demo;
USE intern_demo;

CREATE TABLE students (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50),
  domain VARCHAR(30),
  score INT
);

5️⃣ Insert and View Data
INSERT INTO students (name, domain, score)
VALUES ('Aarav', 'Cloud', 95), ('Diya', 'DevOps', 89);

SELECT * FROM students;

6️⃣ Optional – Remote Access via Python
import mysql.connector

conn = mysql.connector.connect(
  host="database-1.xxxxx.ap-south-1.rds.amazonaws.com",
  user="admin",
  password="yourpassword",
  database="intern_demo"
)

cursor = conn.cursor()
cursor.execute("SELECT * FROM students")
for row in cursor.fetchall():
    print(row)

7️⃣ Clean Up

Stop or delete your RDS instance after testing to avoid unnecessary billing.

📦 Deliverables

✅ Screenshot of running RDS instance

✅ Screenshot of successful connection (Workbench or CloudShell)

✅ SQL commands and results

✅ Short summary of what you learned

🧠 Outcome

By completing this project, you will:

Understand DBaaS concepts in AWS RDS.

Learn to create, configure, and manage cloud SQL databases.

Practice SQL CRUD operations on a live cloud-hosted database.

Gain skills useful for data engineering, analytics, and full-stack applications.

✍️ Author
Apurva Kadam
📧 Email: apurvak2911@gmail.com
📍 Passionate about Cloud Computing,DevOps,AWS
