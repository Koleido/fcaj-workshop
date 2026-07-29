---
title: "Database Setup (RDS & DynamoDB)"
date: 2026-07-28
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

# Database Setup (Amazon RDS & DynamoDB)

In the HCMUT Cinema architecture, data storage is split into 2 specialized repositories to optimize performance:
1. **Amazon RDS (PostgreSQL):** Stores relational data that requires strong consistency (ACID compliance), such as Customer info, Movies, Theaters, and paid Tickets.
2. **Amazon DynamoDB:** An ultra-high-speed NoSQL database used exclusively for the "Seat Locking" feature, with an automatic TTL expiry after 5 minutes.

---

## Part 1: Deploying Amazon RDS (PostgreSQL)

![RDS Diagram](/images/5-Workshop/5.4-Database-RDS-DynamoDB/diagram_rds.png)

### Creating the Database Instance

**Step 1:** Go to the **AWS Console**, search for **RDS**, and click the **Create database** button.

![Step 1](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.1.png)

**Step 2:** Select **Standard create** as the database creation method. Under Engine options, choose **PostgreSQL**.

![Step 2](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.2.png)

**Step 3:** Scroll down to **Templates** and select **Free tier** to optimize project costs.

![Step 3](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.3.png)

**Step 4:** Under **Settings**, configure the following:
- DB instance identifier: `hcmut-cinema-db`
- Master username: `postgres`
- Master password: Set your admin password.

![Step 4](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.4.png)

**Step 5:** Under **Connectivity**, set **Public access** to **Yes**. This allows both your local machine and the EC2 server to connect directly to the database.

![Step 5](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.5.png)

**Step 6:** Scroll to the bottom and click **Create database**. Wait approximately 5–10 minutes for AWS to provision the resources. Once the status shows *Available*, the DB is ready!

![Step 6](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.6.png)

### Seeding the Database

**Step 7:** Open a Terminal on your local machine, navigate to the `backend` folder, and run the `node import_db.js` script. This script will automatically connect to RDS, create all the tables (Movies, Theaters, Tickets...) and insert the sample seed data.

```bash
cd backend
node import_db.js
```
![Step 7](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.7.png)

---

## Part 2: Deploying Amazon DynamoDB (NoSQL)

The ticket booking system requires a seat holding mechanism (Lock) for 5 minutes while a user is completing payment. DynamoDB with its **Time-To-Live (TTL)** feature is the perfect solution — it allows the system to automatically clean up "expired" seat locks without needing to write any polling loops that would burden the server.

![DynamoDB Diagram](/images/5-Workshop/5.4-Database-RDS-DynamoDB/diagram_dynamo.png)

### Creating the Seat Status Table

**Step 1:** Navigate to the **DynamoDB** service on the AWS Console and click **Create table**.

![Step 1](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.8.png)

**Step 2:** In the **Table name** field, enter exactly `HCMUTCinema_SeatLocks` — the name defined in the Backend configuration.

![Step 2](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.9.png)

**Step 3:** In the **Partition key** field, enter `LockID` and select the data type **String**. Scroll to the bottom and click **Create table**.

![Step 3](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.10.png)

### Enabling the Automatic Seat Release (TTL)

**Step 4:** After the table is created, click on the table name. Go to the **Additional settings** tab, scroll down to **Time to Live (TTL)**, and click Enable.
Set the TTL attribute to `ExpirationTime`. From this point on, any seat lock pushed into DynamoDB will be automatically deleted by AWS after exactly 5 minutes, releasing the seat for other customers!

![Step 4](/images/5-Workshop/5.4-Database-RDS-DynamoDB/5.4.11.png)

Our data storage infrastructure is now fully deployed!
