---
title: "Frontend Deployment (Amazon S3)"
date: 2026-07-28
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

# Deploying the Web Interface to Amazon S3

In this hands-on section, we will push the entire static frontend source code (HTML, CSS, JS) of **HCMUT Cinema** to the cloud using **Amazon S3**.

Amazon S3 has a powerful feature called **Static Website Hosting**, which transforms a regular storage bucket into a static web server at an extremely low cost and with near-unlimited scalability — all without any server configuration required.

Below is the flow diagram for the static Frontend hosted on S3:

![S3 Frontend Diagram](/images/5-Workshop/5.3-Deploy-S3-Frontend/diagram_s3.png)

---

### Step-by-Step Instructions

**Step 1:** Navigate to the Amazon S3 Console. On the main screen, click the **Create bucket** button.

![Step 1](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.1.png)

**Step 2:** Enter a name for your bucket. Note that the bucket name must be **globally unique** across all of AWS. Select the Region `ap-southeast-1` (Singapore) for the fastest access speeds from Vietnam.

![Step 2](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.2.png)

**Step 3:** Under **Block Public Access settings for this bucket**, **uncheck** the "Block all public access" option. This allows anyone on the Internet to access and view your website.

![Step 3](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.3.png)

**Step 4:** AWS will display a confirmation warning about enabling public access. Tick the **I acknowledge...** checkbox to confirm you understand the implications.

![Step 4](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.4.png)

**Step 5:** Scroll to the bottom of the page and click **Create bucket** to finalize the setup.

![Step 5](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.5.png)

**Step 6:** After the bucket is created, click on its name. Navigate to the **Properties** tab and scroll to the very bottom to find **Static website hosting**. Click **Edit**.

![Step 6](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.6.png)

**Step 7:** Enable **Static website hosting**. In the **Index document** field, enter `index.html` (this is the default homepage that loads when users visit the website URL). Click Save changes.

![Step 7](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.7.png)

**Step 8:** Go to the **Permissions** tab. Scroll down to **Bucket policy** and click Edit. Paste the JSON policy granting `s3:GetObject` permission to everyone (`"Principal": "*"`). This makes your code files publicly readable as web pages.

![Step 8](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.8.png)

**Step 9:** Finally, go back to the **Objects** tab, click **Upload**, and upload all frontend files (HTML, CSS, JS, Images) from the `s3_frontend` folder. Once uploaded, return to the Properties tab, copy the website endpoint URL from the Static website hosting section, and paste it into your browser to see the result!

![Step 9](/images/5-Workshop/5.3-Deploy-S3-Frontend/5.3.9.png)

---
> **💡 Tip:** Whenever you update the source code (e.g., changing the API IP in `app.js`), simply upload the updated file to the S3 Bucket Objects tab and the website will be updated instantly!