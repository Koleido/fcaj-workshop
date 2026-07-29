---
title: "Blogs Posted"
date: 2026-07-27
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your own report.
{{% /notice %}} -->

Throughout the **First Cloud AI Journey** internship, our team actively shared technical knowledge with the AWS learning community by publishing three blog posts in the **AWS Study Group** Facebook group.

Each team member contributed one article based on our **HCMUT Cinema** project. Rather than simply introducing AWS services, the blogs focused on practical implementation experiences, cloud-native architecture, and solutions to real-world software engineering challenges encountered during development.

The following sections summarize the three published blog posts.

---

### [Blog 1 – Understanding Race Condition and How Amazon DynamoDB Solves Concurrent Seat Booking](3.1-Blog1/)

This blog explains the concept of **Race Condition** in online ticket booking systems and demonstrates how **Amazon DynamoDB** can prevent duplicate seat reservations using **ConditionExpression** and **Time-to-Live (TTL)**. It also introduces the AWS architecture adopted in the HCMUT Cinema project and highlights practical cloud-native design principles.

---

### [Blog 2 – Building a Cloud-Native Cinema Booking System with Amazon S3, EC2, RDS, and DynamoDB](3.2-Blog2/)

This article presents the overall cloud-native architecture of the HCMUT Cinema project. It explains the role of each AWS service—including **Amazon S3**, **Amazon EC2**, **Amazon RDS PostgreSQL**, **Amazon DynamoDB**, and **Amazon SES**—and discusses how these managed services work together to build a scalable, maintainable, and modern ticket booking system.

---

### [Blog 3 – Solving Race Conditions in Online Ticket Booking with Amazon DynamoDB](3.3-Blog3/)

The third blog provides a more in-depth discussion of the real-time seat locking mechanism implemented in the project. It focuses on the use of **ConditionExpression** and **TTL** in Amazon DynamoDB to handle concurrent booking requests efficiently, while sharing practical lessons learned from applying AWS services to solve real-world engineering problems.