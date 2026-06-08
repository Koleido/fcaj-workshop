---

title: "Week 1 Worklog"
date: 2026-06-08
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
----------------------

<!-- {{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}} -->

### Week 1 Objectives

During the first week of the internship, my primary objective was to become familiar with the First Cloud AI Journey program, gain an overview of AWS, and prepare the necessary working environment for the upcoming weeks. As this was the foundation stage of the internship, I focused on understanding the workflow, learning fundamental cloud concepts, and performing my first hands-on activities on AWS.

### Activities Performed During Week 1

#### 1. Introduction to the Internship Program and Team Members

At the beginning of the week, I was introduced to the First Cloud AI Journey internship program, including its objectives, learning roadmap, and expected outcomes. I also had the opportunity to get acquainted with mentors and fellow participants. This helped me better understand the internship environment and establish communication channels for future collaboration and knowledge sharing.

#### 2. Learning the Fundamentals of AWS

I started studying AWS by learning the basic concepts of cloud computing and understanding the role of AWS in modern IT infrastructure. During this stage, I explored the main categories of AWS services, including:

* Compute
* Storage
* Networking
* Database
* Security
* Monitoring

Having an overview of these service groups helped me build a general understanding of AWS before diving deeper into individual services in future weeks.

#### 3. Creating an AWS Free Tier Account

After gaining basic theoretical knowledge, I created an AWS Free Tier account to prepare for practical exercises. This step was essential because most upcoming workshops and labs require direct access to AWS resources. While creating the account, I paid attention to identity verification, billing configuration, and Free Tier limitations to avoid unexpected charges.

### AWS Management Console

![AWS Console Home](/images/Week1/AwsConsoleHome.png)

During the practice session, I logged into the AWS Management Console to familiarize myself with the administration interface and observe the basic services.

*AWS Console after logging in successfully.*

#### 4. Exploring AWS Management Console and AWS CLI

Next, I learned the two primary ways of interacting with AWS:

* **AWS Management Console**: A web-based graphical interface used to manage AWS resources.
* **AWS CLI (Command Line Interface)**: A command-line tool that allows users to interact with AWS services through terminal commands.

I installed and configured AWS CLI on my personal computer, including setting up the Access Key, Secret Access Key, and Default Region. After configuration, I executed several basic commands to verify connectivity and become familiar with the command syntax.

### AWS CLI Configuration

I installed and tested the AWS CLI on my personal computer to familiarize myself with command-line operations.

![AWS CLI](/images/Week1/AwsCli.png)

*AWS CLI version and configuration check.*

#### 5. Learning Amazon EC2 and Launching an Instance

Towards the end of the week, I began learning about Amazon EC2, one of the core AWS services for cloud computing. I studied several important concepts, including:

* Instance Types
* Amazon Machine Images (AMI)
* Elastic Block Store (EBS)
* Elastic IP
* SSH Connectivity

After learning the theory, I launched my first EC2 instance and practiced connecting to it via SSH. I also explored the process of attaching an EBS volume to an EC2 instance. This activity helped me better understand how virtual servers are deployed and managed in a cloud environment.

### Achievements

By the end of the first week, I had achieved the following results:

* Gained a basic understanding of AWS and its major service categories.
* Successfully created and configured an AWS Free Tier account.
* Became familiar with the AWS Management Console.
* Installed and configured AWS CLI on my personal computer.
* Performed several basic operations using AWS CLI.
* Developed a foundational understanding of EC2, SSH, AMI, EBS, and Elastic IP.
* Successfully launched and connected to an EC2 instance for the first time.

### Challenges Encountered

During the first week, I faced several challenges:

* AWS contains a large number of services, making it difficult to understand their relationships at first.
* Concepts such as AMI, EBS, Elastic IP, and Security Groups were initially unfamiliar.
* AWS CLI configuration required careful attention, as incorrect credentials could prevent successful authentication.
* While connecting to EC2 via SSH, I needed to verify key pairs, security settings, and network configurations to troubleshoot connection issues.

Through studying documentation and repeated practice, I gradually became more comfortable with these tools and concepts.

### Lessons Learned

After completing the first week, I realized that learning cloud computing requires a strong foundation. Instead of trying to learn many services at once, it is more effective to understand how a single service works, how to manage it through the AWS Console, and how to perform equivalent operations using AWS CLI. Establishing this foundation will make it easier to learn more advanced AWS topics in the future.

### Plan for the Next Week

For the following week, I plan to:

* Review the fundamental AWS concepts learned during Week 1.
* Continue practicing with EC2 to gain more hands-on experience.
* Learn more about Amazon S3, VPC, and related networking concepts.
* Document useful commands, procedures, and common issues encountered during practice sessions for future reference and reporting.
