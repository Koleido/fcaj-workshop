---
title: "Project Overview"
date: 2026-07-28
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

# Introduction

**HCMUT Cinema** is a Real-time Cinema Ticket Booking System. The system is designed following a Cloud-Native architecture, fully leveraging AWS cloud services to ensure High Availability, flexible Scalability, and robust data security.

In this workshop, you will be guided step by step through the configuration and deployment of 5 core AWS services: **Amazon S3, Amazon EC2, Amazon RDS, Amazon DynamoDB, and Amazon SES**.

# Architecture Overview

The diagram below illustrates the overall system architecture, showing how each component and AWS service interacts within the cloud ecosystem:

![System Architecture Diagram](/images/5-Workshop/5.1-Architecture-Overview/architecture.png)

The system adheres to the **Decoupled** principle to maximize performance, specifically:

* **Amazon S3:** Hosts the static web interface (Frontend). It operates completely independently from the server, helping reduce Backend load and improving page load speed.
* **Amazon EC2:** A virtual machine running the Node.js Backend logic, acting as the API Server and managing real-time WebSocket connections (Socket.io).
* **Amazon RDS (PostgreSQL):** A relational database storing persistent data (movies, theaters, showtimes, users, and invoices).
* **Amazon DynamoDB:** A high-speed NoSQL database used exclusively for the "Real-time Seat Locking" feature. It leverages **TTL (Time-To-Live)** to automatically release seats if a customer does not complete payment within 5 minutes, eliminating the need for complex Backend Cron Jobs.
* **Amazon SES (Simple Email Service):** An automated email service that sends e-tickets and OTP verification codes, ensuring high inbox deliverability and full security through the AWS SDK.
