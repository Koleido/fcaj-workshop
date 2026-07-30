---
title: "Blog 3"
date: 2026-07-27
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Solving Race Conditions in Online Ticket Booking with Amazon DynamoDB

One of the most challenging problems our team encountered while developing the **HCMUT Cinema** project on Amazon Web Services (AWS) was preventing multiple users from booking the same seat simultaneously.

This blog explains the concept of **Race Condition**, discusses why it frequently occurs in online booking systems, and introduces how **Amazon DynamoDB** can be used to implement an efficient real-time seat locking mechanism.

## Main Contents

### Understanding Race Conditions

A Race Condition occurs when multiple users or processes attempt to access and modify the same resource at nearly the same time.

For example, suppose seat **A10** is the last available seat for a movie. If two customers attempt to reserve it simultaneously, both requests may succeed if the application only checks seat availability before updating the database. This results in duplicate reservations and inconsistent booking information.

### Seat Locking with Amazon DynamoDB

To prevent this issue, our project uses **Amazon DynamoDB** instead of relying solely on a relational database.

Whenever a customer selects a seat, the backend creates a temporary seat lock in DynamoDB. If another user has already locked the same seat, the new request is rejected immediately, ensuring that only one customer can reserve the seat at a time.

### Using ConditionExpression

The key feature used in our implementation is **ConditionExpression**.

The system creates a seat lock only when the corresponding record does not already exist:

```text
ConditionExpression:
attribute_not_exists(SeatKey)
```

Since DynamoDB performs this operation atomically, multiple concurrent requests cannot create duplicate seat locks.

### Automatic Lock Release with TTL

Another important consideration is handling customers who reserve seats but never complete the payment process.

To solve this problem, our project uses **Time-to-Live (TTL)** in DynamoDB.

Each temporary seat lock automatically expires after approximately five minutes. Once the expiration time is reached, DynamoDB removes the record automatically without requiring scheduled cleanup jobs or additional background services.

This approach helps keep the booking system responsive while allowing other customers to reserve seats that were previously abandoned.

### Overall System Architecture

The HCMUT Cinema project combines several AWS services, each responsible for a specific component of the system:

- **Amazon S3** – Hosts the frontend website.
- **Amazon EC2** – Runs the Node.js backend application.
- **Amazon RDS PostgreSQL** – Stores transactional business data.
- **Amazon DynamoDB** – Manages temporary seat locks.
- **Amazon SES** – Sends OTP verification emails and electronic tickets.

This cloud-native architecture improves scalability, maintainability, and overall system reliability.

## Blog Illustration

![Blog 3](/images/Blogs/Blog3-1.png)

![Blog 3](/images/Blogs/Blog3-2.png)

## Blog Link

The original article was published in the **AWS Study Group** Facebook community as part of our internship knowledge-sharing activities.

> **Facebook Post:** *[(Solving Race Conditions in Online Ticket Booking with Amazon DynamoDB)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2224958578269102&hoisted_section_header_type=recently_seen&__cft__[0]=AZZg1au5zUs8YYSUu_gkvVNkZj0Xfp8LEeKGInZG4R2Vx-iIr8FN9kQJHx7zzPUD_rt9N7nKm47pZd3YkPIjp23fnm65eldG2Lz0MaRRAcJAQd7Yxmf-aYbdm9guWlodWjtBk3uwGfM5FcIuZtZhuuuL&__tn__=%2CO%2CP-R)*

## Reflection

Writing this blog allowed our team to better understand how AWS managed services can be applied to solve real-world software engineering problems.

Through the implementation of **Amazon DynamoDB**, **ConditionExpression**, and **Time-to-Live (TTL)**, we learned that handling concurrent access is not only a database problem but also an architectural design consideration.

By sharing this experience with the AWS learning community, we hope to provide a practical example of how cloud-native technologies can be used to build scalable, reliable, and modern online booking systems.