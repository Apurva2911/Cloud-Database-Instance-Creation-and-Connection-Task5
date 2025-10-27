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
import pymysql

# AWS RDS connection details
database_instance_endpoint = "database-1.c7i0qg28i5r3.ap-south-1.rds.amazonaws.com"
port = 3306
dbname = "sample"
user = "admin"
password = "Apurva1234"

try:
    print("🚀 Connecting to AWS RDS server (without DB)...")
    # Step 1: Connect without specifying a database
    connection = pymysql.connect(
        host=database_instance_endpoint,
        user=user,
        password=password,
        port=port
    )
    print("✅ Connected to RDS instance!")

    with connection.cursor() as cur:
        # Step 2: Create the database if it doesn't exist
        cur.execute(f"CREATE DATABASE IF NOT EXISTS {dbname}")
        print(f"🧱 Database '{dbname}' verified/created successfully.")

    connection.close()

    # Step 3: Reconnect to the newly created or existing database
    print(f"🔁 Reconnecting to database '{dbname}'...")
    connection = pymysql.connect(
        host=database_instance_endpoint,
        user=user,
        password=password,
        database=dbname,
        port=port
    )
    print("✅ Connected to the target database!")

    with connection.cursor() as mycur:
        # Step 4: Create the 'students' table if not exists
        create_table_query = """
        CREATE TABLE IF NOT EXISTS students (
            id INT AUTO_INCREMENT PRIMARY KEY,
            firstname VARCHAR(255) NOT NULL,
            lastname VARCHAR(255) NOT NULL,
            grade VARCHAR(10)
        ) ENGINE=InnoDB;
        """
        mycur.execute(create_table_query)
        print("📋 Table 'students' checked/created.")

        # Step 5: Insert sample data (ignore duplicates)
        insert_query = "INSERT INTO students (id, firstname, lastname) VALUES (%s, %s, %s)"
        try:
            mycur.execute(insert_query, ('12345', 'Tata', 'Tutu'))
            mycur.execute(insert_query, ('34567', 'Momo', 'Meme'))
        except pymysql.err.IntegrityError:
            print("⚠️ Duplicate IDs found — skipping inserts.")
        connection.commit()
        print("📥 Data insertion complete!")

        # Step 6: Fetch and display all rows
        mycur.execute("SELECT * FROM students")
        rows = mycur.fetchall()
        print("\n📊 Current Table Data:")
        for row in rows:
            print(row)

except Exception as e:
    print("❌ Error:", e)

finally:
    if 'connection' in locals() and connection.open:
        connection.close()
        print("\n🔒 Connection closed.")

OUTPUT : 
🚀 Connecting to AWS RDS server (without DB)...
✅ Connected to RDS instance!
🧱 Database 'sample' verified/created successfully.
🔁 Reconnecting to database 'sample'...
✅ Connected to the target database!
📋 Table 'students' checked/created.
📥 Data insertion complete!

📊 Current Table Data:
(12345, 'Tata', 'Tutu', None)
(34567, 'Momo', 'Meme', None)

🔒 Connection closed.

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
