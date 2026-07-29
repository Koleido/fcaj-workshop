---
title: "Blog 2"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Building a Cloud-Native Movie Ticket Booking System with Amazon S3, EC2, RDS, and DynamoDB

This blog introduces the overall architecture of the **HCMUT Cinema** project, a cloud-native movie ticket booking system developed on **Amazon Web Services (AWS)**.

Using this project as a practical example, the article explains how multiple AWS services can be combined to build a scalable, maintainable, and reliable application capable of supporting real-world business requirements.

## Main Contents

### Project Overview

Online movie ticket booking systems involve various business operations, including:

- Movie management
- Ticket booking
- Seat selection
- Payment processing
- Email notifications
- Administrative management

Deploying all these functionalities on a single server can make the system difficult to scale and maintain as user traffic increases. Therefore, the project adopts a **Cloud-Native Architecture**, where different system components are separated and deployed using specialized AWS services.

### Amazon S3 – Static Website Hosting

The frontend application is hosted on **Amazon S3**, which provides a simple and cost-effective solution for serving static web content.

Amazon S3 is responsible for:

- Hosting HTML, CSS, and JavaScript files.
- Delivering the customer and administrator interfaces.
- Providing reliable and highly available static website hosting.

### Amazon EC2 – Backend Application

The backend application is deployed on **Amazon EC2** using **Node.js** and **Express**.

The backend handles all business logic, including:

- User authentication.
- Ticket booking.
- Payment processing.
- Database communication.
- Email service integration.

EC2 serves as the core processing layer of the entire system.

### Amazon RDS PostgreSQL – Transactional Database

Persistent business data is stored in **Amazon RDS PostgreSQL**, including:

- Users
- Movies
- Cinemas
- Showtimes
- Tickets
- Payment records

PostgreSQL provides strong consistency and ACID-compliant transactions, making it suitable for critical business data.

### Amazon DynamoDB – Temporary Seat Locking

**Amazon DynamoDB** is dedicated to implementing temporary seat locking.

When a customer selects a seat:

- A temporary lock is created.
- The seat remains reserved for approximately five minutes.
- The lock expires automatically if payment is not completed.

This mechanism effectively prevents duplicate seat reservations during concurrent booking requests.

### Amazon SES – Email Notification Service

After a successful booking, **Amazon Simple Email Service (SES)** automatically sends:

- OTP verification emails.
- Electronic tickets.
- QR Code confirmations.

The entire notification process is automated without requiring manual intervention.

### Benefits of Cloud-Native Architecture

The cloud-native design provides several important advantages:

- Complete separation between frontend and backend.
- Independent scalability for each system component.
- Appropriate database selection for different types of data.
- Efficient utilization of AWS managed services.
- Easier maintenance and future feature expansion.

## Blog Illustration

![Blog 2](/images/Blogs/Blog2-1.png)

![Blog 2](/images/Blogs/Blog2-2.png)

## Blog Link

The article was published in the **AWS Study Group** Facebook community as part of the internship knowledge-sharing activities.

> **Facebook Post:** *[(Building a Cloud-Native Movie Ticket Booking System with Amazon S3, EC2, RDS, and DynamoDB)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2224973238267636&hoisted_section_header_type=recently_seen&__cft__[0]=AZYk_Ku1LBPhTfWojUpXBqOTOKNMBOkd-thieWVEqABkcQfS_mCvm3BmbYJWPt8FT6511oD6O7gXr-ROprbXQLW_cQCpwz-Muem-xLM1DQ3YPbq4hFUHG-Y1ppwQHDPYkx254rZtCHSqvnuMjvKAk4y5&__tn__=%2CO%2CP-R)*

## Reflection

Writing and sharing this blog provided an opportunity to summarize the overall cloud-native architecture implemented in the **HCMUT Cinema** project.

The article demonstrates how AWS services such as **Amazon S3**, **Amazon EC2**, **Amazon RDS PostgreSQL**, **Amazon DynamoDB**, and **Amazon SES** can be integrated into a complete application, where each service is responsible for a specific function while collectively delivering a scalable, reliable, and maintainable solution.

Publishing this technical article also helped reinforce architectural design concepts and promoted knowledge sharing within the AWS learning community, especially for students who are beginning to explore Cloud Computing and cloud-native application development.