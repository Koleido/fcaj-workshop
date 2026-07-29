---
title: "Proposal"
date: 2026-06-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

<!-- {{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}} -->

# HCMUT Cinema
# A Cloud-Native Cinema Booking System on Amazon Web Services

## 1. Executive Summary

The **HCMUT Cinema** project is a cloud-native cinema management and ticket booking system developed on **Amazon Web Services (AWS)**. The project aims to redesign a traditional monolithic cinema booking application into a modern cloud-native architecture by separating the frontend and backend, utilizing managed AWS services, and applying best practices for scalability, reliability, and maintainability.

One of the primary objectives of this project is to solve the real-world challenges commonly found in online ticket booking systems, especially **high concurrent access** and **seat booking conflicts (Race Condition)** during peak hours. To address these issues, the project combines multiple AWS services including **Amazon EC2**, **Amazon S3**, **Amazon RDS PostgreSQL**, **Amazon DynamoDB**, and **Amazon Simple Email Service (SES)**.

The system provides two major subsystems:

- **Customer Portal**
  - Browse movies and showtimes.
  - Select seats with real-time seat locking.
  - Purchase tickets using email OTP verification.
  - Receive electronic tickets with QR Codes.

- **Administrator Portal**
  - Manage movies, cinemas, showtimes, and schedules.
  - Prevent schedule conflicts automatically.
  - Monitor ticket sales and occupancy statistics.
  - Manage customer information and cinema operations.

By deploying the system on AWS, the project demonstrates how cloud-native technologies can improve system performance, scalability, and user experience while reducing operational complexity.

---

## 2. Problem Statement

### Current Challenges

Traditional cinema management systems are commonly deployed using a monolithic architecture where all components—including the frontend, backend, and database—are tightly coupled within a single application. Although this architecture is relatively simple to develop initially, it becomes increasingly difficult to maintain and scale as the number of users grows.

One of the most significant challenges faced by online cinema booking systems is handling **high concurrent requests**. During peak periods, multiple customers may attempt to purchase the same seat simultaneously. Without an appropriate synchronization mechanism, this situation can lead to **Race Conditions**, causing duplicate bookings or inconsistent ticket information.

In addition, traditional systems often encounter several other limitations:

- Limited scalability during periods of high traffic.
- Tight coupling between frontend and backend components.
- Slow response time when processing large numbers of booking requests.
- Manual ticket confirmation and email notification processes.
- Difficulty maintaining data consistency across different services.
- Limited flexibility for future feature expansion.

These issues negatively impact customer experience and increase operational complexity for administrators.

---

### Proposed Solution

To overcome these challenges, this project proposes a **Cloud-Native Cinema Booking System** built entirely on Amazon Web Services.

Instead of deploying the entire application on a single server, the system adopts a **decoupled architecture**, separating frontend and backend components to improve maintainability and scalability.

The solution consists of several AWS managed services working together:

- **Amazon S3** hosts the static frontend website.
- **Amazon EC2** runs the backend RESTful API built with Node.js and Express.
- **Amazon RDS PostgreSQL** stores transactional data such as users, movies, showtimes, and tickets.
- **Amazon DynamoDB** manages temporary seat locking to prevent booking conflicts.
- **Amazon SES** automatically sends OTP verification emails and electronic tickets.

A key feature of the proposed solution is the implementation of **real-time seat locking** using DynamoDB's **ConditionExpression** and **Time-to-Live (TTL)** mechanism. When a customer selects a seat, the seat is temporarily locked for five minutes. If the booking process is not completed within the specified period, DynamoDB automatically releases the lock, allowing other customers to reserve the seat.

This design significantly reduces booking conflicts while maintaining high system responsiveness during concurrent access.

---

### Expected Benefits

Compared with traditional deployment models, the proposed cloud-native architecture provides several advantages.

### Technical Benefits

- Prevent seat booking conflicts during concurrent transactions.
- Improve application scalability by separating frontend and backend services.
- Increase system reliability using managed AWS services.
- Reduce operational complexity through service decoupling.
- Support automatic email notifications and electronic ticket delivery.
- Enable future expansion without major architectural changes.

### Business Benefits

- Improve customer experience during ticket booking.
- Increase booking reliability and transaction success rate.
- Reduce administrative workload through automation.
- Improve overall system maintainability.
- Provide a modern cloud-based platform suitable for future development.

---

## 3. Solution Architecture

The HCMUT Cinema system follows a **Cloud-Native Architecture** that separates presentation, business logic, data storage, and notification services into independent components deployed on AWS.

Instead of relying on a single server to perform every task, each component is responsible for a specific function within the overall system. This architecture improves scalability, simplifies maintenance, and allows each service to evolve independently.

The overall workflow of the system is illustrated below.

<!-- > *(Insert the architecture diagram here.)* -->

```text
Customer Browser
        │
        ▼
 Amazon S3 Static Website
        │
        ▼
 Amazon EC2 (Node.js REST API)
        │
 ┌──────┼───────────────┐
 ▼      ▼               ▼
Amazon RDS      DynamoDB      Amazon SES
(PostgreSQL)   Seat Locks     OTP & E-Ticket
```

### AWS Services Used

| AWS Service | Purpose |
|-------------|---------|
| Amazon S3 | Hosts the static frontend website for customers and administrators. |
| Amazon EC2 | Runs the Node.js backend application and exposes RESTful APIs. |
| Amazon RDS PostgreSQL | Stores persistent transactional data including users, movies, cinemas, schedules, and tickets. |
| Amazon DynamoDB | Handles temporary seat locking using ConditionExpression and TTL to prevent race conditions. |
| Amazon SES | Sends OTP verification emails and electronic tickets automatically after successful payment. |

### Component Design

#### Amazon S3

Amazon S3 is responsible for hosting the frontend application. Since the frontend consists primarily of HTML, CSS, and JavaScript files, S3 provides a simple, reliable, and cost-effective hosting solution with high availability.

#### Amazon EC2

Amazon EC2 hosts the backend server developed using Node.js and Express. The backend processes all business logic, communicates with databases, validates user requests, and interacts with other AWS services through the AWS SDK.

#### Amazon RDS PostgreSQL

Amazon RDS stores all critical transactional data that requires strong consistency and ACID compliance, including user accounts, movie information, cinema rooms, showtimes, bookings, and payment records.

#### Amazon DynamoDB

DynamoDB is dedicated to implementing the real-time seat locking mechanism. By combining **ConditionExpression** with **Time-to-Live (TTL)**, the system ensures that no two customers can reserve the same seat simultaneously while automatically releasing expired locks.

#### Amazon SES

Amazon Simple Email Service (SES) is responsible for sending One-Time Password (OTP) verification emails during the payment process as well as delivering electronic tickets containing QR Codes after successful booking.

## 4. Technical Implementation

### Development Phases

#### Phase 1

- Requirement analysis
- Research existing cinema booking systems
- Define project scope

#### Phase 2

- Design AWS architecture
- Database schema design
- REST API planning

#### Phase 3

- Backend development
- Frontend development
- AWS service integration

#### Phase 4

- System testing
- Concurrency testing
- Deployment on AWS
- Documentation

### Technical Requirements

Programming Languages

- JavaScript
- HTML
- CSS

Framework

- Node.js
- Express.js

Database

- PostgreSQL
- DynamoDB

Cloud Platform

- Amazon Web Services

Development Tools

- AWS CLI
- Visual Studio Code
- GitHub

---

## 5. Timeline & Milestones

| Phase | Description |
|--------|-------------|
| Week 1 | Research AWS services and system requirements |
| Week 2 | Design architecture and database |
| Week 3 | Implement backend APIs |
| Week 4 | Develop frontend interface |
| Week 5 | Integrate AWS services |
| Week 6 | Testing and debugging |
| Week 7 | Deployment and optimization |
| Week 8 | Documentation and presentation |

---

## 6. Budget Estimation

The project is developed mainly for educational purposes using AWS Free Tier whenever possible.

### Estimated Infrastructure Cost

| Service | Estimated Cost |
|----------|---------------:|
| Amazon EC2 | Free Tier |
| Amazon S3 | Free Tier |
| Amazon RDS | Free Tier |
| Amazon DynamoDB | Free Tier |
| Amazon SES | Minimal email cost |
| Data Transfer | Free Tier |

Estimated monthly cost:

**Approximately USD 0–5** depending on actual usage.

---

## 7. Risk Assessment

### Potential Risks

#### High Concurrent Booking

Multiple users may attempt to reserve the same seat simultaneously.

**Solution**

Use DynamoDB conditional writes with TTL-based locking.

---

#### Unexpected AWS Costs

Resources may continue running after testing.

**Solution**

Delete unused resources and monitor AWS Billing Dashboard.

---

#### Email Delivery Failure

OTP emails may not be delivered successfully.

**Solution**

Retry sending emails and validate email addresses before payment.

---

#### Database Failure

Unexpected database downtime.

**Solution**

Maintain regular database backups and recovery procedures.

---

## 8. Expected Outcomes

### Technical Outcomes

After completing the project, the system is expected to:

- Successfully deploy on AWS.
- Support online movie ticket booking.
- Prevent duplicate seat reservations.
- Automatically send OTP verification emails.
- Generate QR-code electronic tickets.
- Demonstrate practical usage of multiple AWS services.

### Learning Outcomes

Through this project, team members will gain experience in:

- AWS Cloud deployment
- Cloud-native application architecture
- RESTful API development
- Database design
- Microservices concepts
- Team collaboration
- Software deployment workflow