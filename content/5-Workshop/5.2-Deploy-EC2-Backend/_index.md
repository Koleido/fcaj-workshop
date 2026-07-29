---
title: "Backend Deployment (EC2 & SES)"
date: 2026-07-28
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

# Backend API & Email Service Deployment (EC2 & SES)

In this section, we will carry out 2 major tasks:
1. Launch, configure the firewall, and deploy the Node.js source code to an Amazon EC2 virtual machine.
2. Configure Amazon SES (Simple Email Service) so the system can automatically send OTP verification codes and e-tickets via email.

---

## Part 1: Deploying the Amazon EC2 Server

Amazon EC2 serves as the heart of the Backend system, providing the computing power to handle complex business logic such as seat lookups, fare calculation, and maintaining real-time WebSocket connections (Socket.IO).

![EC2 Diagram](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/diagram_ec2.png)

### Launching the Virtual Machine (Instance)

**Step 1:** From the **AWS Console** homepage, search for the **EC2** service. On the EC2 dashboard, click the orange **Launch instance** button.

![Step 1](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.1.png)

**Step 2:** Enter a name for your server (e.g., `HCMUT-Cinema-Backend-Server`). Under **Application and OS Images**, select **Ubuntu Server 22.04 LTS** — a stable, widely-used OS for Node.js applications that falls within the Free Tier.

![Step 2](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.2.png)

**Step 3:** Scroll down to **Instance type** and keep the default `t2.micro`. Under **Key pair (login)**, create a new key pair or select an existing `.pem` key. This `.pem` file is the only "key" that allows you to SSH into your server later.

![Step 3](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.3.png)

**Step 4:** Under **Network settings**, click **Edit**. You need to open additional ports for the Backend to function:
* Keep the default rule: **SSH (Port 22)** to allow server access.
* Click *Add security group rule*, set Type to **Custom TCP**, Port to **3000** (the port Node.js listens on), and Source type to **Anywhere (0.0.0.0/0)** to allow the Frontend to reach the API.

![Step 4](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.4.png)

**Step 5:** Review all settings in the Summary panel on the right. Once confirmed, click **Launch instance**. Wait about 1 minute for the instance status to change to *Running*.

![Step 5](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.5.png)

### Deploying Source Code and Starting Node.js

**Step 6:** Open a Terminal (PowerShell or Git Bash) on your local machine. Use the `scp` (Secure Copy Protocol) command to copy the Backend source code to the EC2 server (replace the IP with your actual EC2 IP address):

```bash
scp -i hcmutkey.pem -o StrictHostKeyChecking=no -r backend/routes backend/config backend/server.js backend/package.json backend/.env ubuntu@<EC2_IP>:/home/ubuntu/backend/
```

![Terminal screenshot of SCP file upload](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.6.png)

**Step 7:** Use the `ssh` command to log in directly to the EC2 server:

```bash
ssh -i hcmutkey.pem -o StrictHostKeyChecking=no ubuntu@<EC2_IP>
```

![Terminal screenshot of SSH connection](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.7.png)

**Step 8:** From inside the EC2 server, install the dependencies (`node_modules`) and use **PM2** to start the server. PM2 keeps the Backend running 24/7 in the background and automatically restarts it if the server crashes.

```bash
cd /home/ubuntu/backend
npm install
pm2 start server.js --name "hcmut-cinema"
```

![Terminal screenshot of PM2 startup](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.2.8.png)

The Backend API system is now officially online on the Cloud!

---

## Part 2: Configuring Amazon SES (Simple Email Service)

Amazon SES was chosen for its ability to send large volumes of emails reliably, at extremely low cost, and its seamless integration with Node.js via the `aws-sdk` library.

Since our student AWS account is in Sandbox mode, we must first verify any email address we intend to use for sending and receiving mail. Follow these steps:

**Step 1:** Go to the **AWS Console**, search for **Amazon SES**. In the left menu, select **Verified identities**, then click the orange **Create identity** button.

![Step 1](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.5.1.png)

**Step 2:** Set the **Identity type** to **Email address**. Enter your personal email address in the **Email address** field. Scroll to the bottom and click **Create identity**.

![Step 2](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.5.2.png)

**Step 3:** The email status will initially show *Unverified*. AWS has automatically sent a verification email to your inbox. Open the email titled "Amazon Web Services – Email Address Verification Request...", open it and click the confirmation link. Return to the SES page and refresh (F5) — the status will now show a beautiful green **Verified**.

From this point on, the Node.js Backend has full permission to use this email identity to send tickets to customers!

![Step 3](/images/5-Workshop/5.2-Deploy-EC2%20&%20SES/5.5.3.png)