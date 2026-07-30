---
title: "Clean up"
date: 2026-07-28
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

After completing the workshop and taking all necessary screenshots for the report, it is very important to **delete all the resources** you have created. Leaving unused resources running will continue to generate costs (even in Free Tier after the free period ends).

Follow the steps below **in order** to clean up everything safely.

---

### 1. Terminate the EC2 Instance

**Step 1:** Go to the **EC2** Console → click **Instances**.

**Step 2:** Select the instance `HCMUT-Cinema-Backend-Server` → click **Instance state** → **Terminate instance**.

**Step 3:** Confirm the termination.

![Terminate EC2](/images/5-Workshop/5.6-Cleanup/5.6.1.jpg)

![Terminate EC2](/images/5-Workshop/5.6-Cleanup/5.6.2.jpg)

---

### 2. Delete the S3 Bucket

**Step 1:** Go to the **S3** Console.

**Step 2:** Select your frontend bucket → click **Empty**. Confirm by typing `permanently delete`.

**Step 3:** After the bucket is empty, select it again → click **Delete**. Type the bucket name to confirm.

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.3.jpg)

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.4.jpg)

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.5.jpg)

![Terminate S3](/images/5-Workshop/5.6-Cleanup/5.6.6.jpg)

---

### 3. Delete the RDS Database

**Step 1:** Go to the **RDS** Console → **Databases**.

**Step 2:** Select `hcmut-cinema-db` → click **Actions** → **Delete**.

**Step 3:** 
- Uncheck “Create final snapshot” (unless you want to keep data).
- Uncheck "Retain automated backups".
- Check "I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available."
- Type `delete me` to confirm.
- Click **Delete**.

> **Note:** It may take a few minutes for the database to be fully deleted.  

![Terminate RDS Database](/images/5-Workshop/5.6-Cleanup/5.6.7.jpg)

![Terminate RDS Database](/images/5-Workshop/5.6-Cleanup/5.6.8.jpg)

---

### 4. Delete the DynamoDB Table

**Step 1:** Go to the **DynamoDB** Console → **Tables**.

**Step 2:** Select the table `HCMUTCinema_SeatLocks` → click **Delete**.

**Step 3:** Confirm the deletion by typing `delete`.

![Terminate DynamoDB Table](/images/5-Workshop/5.6-Cleanup/5.6.9.jpg)

![Terminate DynamoDB Table](/images/5-Workshop/5.6-Cleanup/5.6.10.jpg)

---

### 5. Delete the SES Email Identity

**Step 1:** Go to the **Amazon SES** Console → **Identities**.

**Step 2:** Select the verified email address → click **Delete**.

**Step 3:** Confirm the deletion.

![Terminate SES](/images/5-Workshop/5.6-Cleanup/5.6.11.jpg)

![Terminate SES](/images/5-Workshop/5.6-Cleanup/5.6.12.jpg)

---

### 6. Delete the IAM User & Access Key

**Step 1:** Go to the **IAM** Console → **Users**.

**Step 2:** Click on the user `hcmut-cinema-backend`.

**Step 3:** Go to the **Security credentials** tab → deactivate all **Access keys**.

**Step 4:** Go back and click **Delete** user. Confirm by typing the username.

![Terminate IAM](/images/5-Workshop/5.6-Cleanup/5.6.13.jpg)

![Terminate IAM](/images/5-Workshop/5.6-Cleanup/5.6.14.jpg)


---