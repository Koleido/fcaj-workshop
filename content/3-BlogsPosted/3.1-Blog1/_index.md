---
title: "Blog 1"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Understanding Race Condition and How Amazon DynamoDB Solves Concurrent Seat Booking

This blog discusses one of the most common challenges in online ticket booking systems: **preventing multiple users from reserving the same seat simultaneously**.

Using the **HCMUT Cinema** project as a practical example, the article explains the concept of **Race Condition**, analyzes why it occurs in high-concurrency environments, and introduces how **Amazon DynamoDB** can be used to provide an efficient and scalable solution.

## Main Contents

### What is a Race Condition?

A Race Condition occurs when multiple users or processes attempt to access and modify the same resource at the same time.

For example, if two customers select seat **A10** simultaneously, both requests may succeed if the system only checks seat availability before updating the database. This can result in duplicate bookings and inconsistent ticket information.

### Why Not Use Only a Traditional Database?

Traditional relational databases such as PostgreSQL provide transactions and locking mechanisms to maintain data consistency.

However, under heavy workloads, excessive database locking may reduce system performance and limit scalability. For this reason, many modern cloud-native applications combine relational databases with NoSQL services to handle high-concurrency operations more efficiently.

### Using Amazon DynamoDB for Seat Locking

The proposed solution uses **Amazon DynamoDB** to implement temporary seat locking.

Each seat is identified by a unique key composed of:

- Movie ID
- Showtime ID
- Seat Number

When a customer selects a seat:

- The backend sends a request to DynamoDB.
- DynamoDB uses **ConditionExpression** to determine whether the seat is already locked.
- If no lock exists, a new lock is created.
- Otherwise, the request is rejected immediately.

Because the write operation is atomic, multiple users cannot reserve the same seat simultaneously.

### Automatic Lock Expiration with TTL

Another challenge is handling customers who reserve seats but do not complete payment.

To address this issue, the solution uses **Time-to-Live (TTL)** in DynamoDB.

Each seat lock automatically expires after approximately five minutes. Once expired, DynamoDB removes the record automatically without requiring additional background services.

This mechanism ensures that:

- Seats are released automatically.
- Other customers can reserve available seats.
- The booking system remains responsive during peak traffic.

### AWS Architecture

Besides DynamoDB, the project integrates several AWS services:

- **Amazon S3** – Hosts the static frontend website.
- **Amazon EC2** – Runs the Node.js and Express backend.
- **Amazon RDS PostgreSQL** – Stores transactional data.
- **Amazon DynamoDB** – Implements real-time seat locking.
- **Amazon SES** – Sends OTP verification emails and electronic tickets.

Together, these services form a cloud-native architecture that improves scalability, maintainability, and system reliability.

## Blog Illustration

![Blog 1](/images/Blogs/Blog1-1.png)

![Blog 1](/images/Blogs/Blog1-2.png)

## Blog Link

The article was published in the **AWS Study Group** Facebook community as part of the internship knowledge-sharing activities.

> **Facebook Post:** *[(Understanding Race Condition and How Amazon DynamoDB Solves Concurrent Seat Booking)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2224962828268677&hoisted_section_header_type=recently_seen&__cft__[0]=AZaFKQ8ESzH6KnoRHd6s91kKW-a8aVyJEUB__zBvPzUOmnZZprSgF7t8FVPiLj2uU1tVgJTYWsySdAKKDDhUPJwaNvVUGcq39FUNHzdiV56ZLkTLz48J5qtFwX4_Uf3kugJRqWUWPy4LOiWregnazW7k&__tn__=%2CO%2CP-R)*

## Reflection

Writing and sharing this blog provided an opportunity to summarize a practical solution for handling **high-concurrency** scenarios in cloud-native applications.

The article demonstrates how **Amazon DynamoDB**, together with **ConditionExpression** and **Time-to-Live (TTL)**, can effectively prevent race conditions while maintaining a responsive and scalable online ticket booking system.

Publishing technical content also helped strengthen knowledge-sharing skills and encouraged discussion with other members of the AWS learning community.