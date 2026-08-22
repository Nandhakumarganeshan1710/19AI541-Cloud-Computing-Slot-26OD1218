# Lab 3: Introduction to Amazon EC2

## Lab Overview and Objectives

![EC2 Overview](media/image.png)

This lab provides you with a basic overview of launching, resizing, managing, and monitoring an Amazon EC2 instance.

**Amazon Elastic Compute Cloud (Amazon EC2)** is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.

Amazon EC2's simple web service interface allows you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and lets you run on Amazon's proven computing environment. Amazon EC2 reduces the time required to obtain and boot new server instances to minutes, allowing you to quickly scale capacity, both up and down, as your computing requirements change.

Amazon EC2 changes the economics of computing by allowing you to pay only for capacity that you actually use. Amazon EC2 provides developers the tools to build failure resilient applications and isolate themselves from common failure scenarios.

### Learning Objectives

After completing this lab, you should be able to:

- Launch a web server with termination protection enabled
- Monitor your EC2 instance
- Modify the security group that your web server is using to allow HTTP access
- Resize your Amazon EC2 instance to scale and enable stop protection
- Explore EC2 limits
- Test stop protection
- Stop your EC2 instance

## Duration

This lab takes approximately **35 minutes** to complete.

## AWS Service Restrictions

In this lab environment, access to AWS services and service actions might be restricted to the ones that are needed to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that are described in this lab.

## Accessing the AWS Management Console

1. At the top of these instructions, choose **Start Lab**.
   - The lab session starts.
   - A timer displays at the top of the page and shows the time remaining in the session.

   > **Tip:** To refresh the session length at any time, choose **Start Lab** again before the timer reaches 0:00.

   - Before you continue, wait until the circle icon to the right of the AWS link in the upper-left corner turns green.

2. To connect to the AWS Management Console, choose the **AWS** link in the upper-left corner.
   - A new browser tab opens and connects you to the console.

   > **Tip:** If a new browser tab does not open, a banner or icon is usually at the top of your browser with the message that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and then choose **Allow pop-ups**.

3. Arrange the AWS Management Console tab so that it displays alongside these instructions. Ideally, you will be able to see both browser tabs at the same time, to make it easier to follow the lab steps.

## Getting Credit for Your Work

At the end of this lab you will be instructed to submit the lab to receive a score based on your progress.

> **Tip:** The script that checks your work may only award points if you name resources and set configurations as specified. In particular, values in these instructions that appear in `This Format` should be entered exactly as documented (case-sensitive).

## Task 1: Launch Your Amazon EC2 Instance

In this task, you will launch an Amazon EC2 instance with *termination protection* and *stop protection*. Termination protection prevents you from accidentally terminating the EC2 instance and stop protection prevents you from accidentally stopping the EC2 instance. You will also specify a User Data script when you launch the instance that will deploy a simple web server.

### Launch Instance

4. In the **AWS Management Console** choose **Services**, choose **Compute** and then choose **EC2**.

   > **Note:** Verify that your EC2 console is currently managing resources in the **N. Virginia** (us-east-1) region. You can verify this by looking at the drop down menu at the top of the screen, to the left of your username. If it does not already indicate N. Virginia, choose the N. Virginia region from the region menu before proceeding to the next step.

5. Choose the **Launch instance** menu and select **Launch instance**.

### Step 1: Name and Tags

6. Give the instance the name `Web Server`.

The Name you give this instance will be stored as a tag. Tags enable you to categorize your AWS resources in different ways, for example, by purpose, owner, or environment. This is useful when you have many resources of the same type — you can quickly identify a specific resource based on the tags you have assigned to it. Each tag consists of a Key and a Value, both of which you define. You can define multiple tags to associate with the instance if you want to.

In this case, the tag that will be created will consist of a *key* called Name with a *value* of Web Server.

### Step 2: Application and OS Images (Amazon Machine Image)

7. In the list of available *Quick Start* AMIs, keep the default **Amazon Linux** AMI selected.

8. Also keep the default **Amazon Linux 2023 AMI** selected.

An **Amazon Machine Image (AMI)** provides the information required to launch an instance, which is a virtual server in the cloud. An AMI includes:

- A template for the root volume for the instance (for example, an operating system or an application server with applications)
- Launch permissions that control which AWS accounts can use the AMI to launch instances
- A block device mapping that specifies the volumes to attach to the instance when it is launched

The **Quick Start** list contains the most commonly-used AMIs. You can also create your own AMI or select an AMI from the AWS Marketplace, an online store where you can sell or buy software that runs on AWS.

### Step 3: Instance Type

9. In the *Instance type* panel, keep the default **t2.micro** selected.

Amazon EC2 provides a wide selection of *instance types* optimized to fit different use cases. Instance types comprise varying combinations of CPU, memory, storage, and networking capacity and give you the flexibility to choose the appropriate mix of resources for your applications. Each instance type includes one or more *instance sizes*, allowing you to scale your resources to the requirements of your target workload.

The t2.micro instance type has 1 virtual CPU and 1 GiB of memory.

> **Note:** You may be restricted from using other instance types in this lab.

### Step 4: Key Pair (Login)

10. For **Key pair name - *required***, choose **vockey**.

Amazon EC2 uses public–key cryptography to encrypt and decrypt login information. To ensure you will be able to log in to the guest OS of the instance you create, you identify an existing key pair or create a new key pair when launching the instance. Amazon EC2 then installs the key on the guest OS when the instance is launched. That way, when you attempt to login to the instance and you provide the private key, you will be authorized to connect to the instance.

> **Note:** In this lab you will not actually use the key pair you have specified to log into your instance.

### Step 5: Network Settings

11. Next to Network settings, choose **Edit**.

12. For **VPC**, select **Lab VPC**.

The Lab VPC was created using an AWS CloudFormation template during the setup process of your lab. This VPC includes two public subnets in two different Availability Zones.

> **Note:** Keep the default subnet **PublicSubnet1**. This is the subnet in which the instance will run. Notice also that by default, the instance will be assigned a public IP address.

13. Under **Firewall (security groups)**, choose **Create security group** and configure:
    - **Security group name:** `Web Server security group`
    - **Description:** `Security group for my web server`

A *security group* acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you associate one or more security groups with the instance. You add *rules* to each security group that allow traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group.

- Under **Inbound security group rules**, notice that one rule exists. **Remove** this rule.

### Step 6: Configure Storage

14. In the *Configure storage* section, keep the default settings.

Amazon EC2 stores data on a network-attached virtual disk called *Elastic Block Store*.

You will launch the Amazon EC2 instance using a default 8 GiB disk volume. This will be your root volume (also known as a 'boot' volume).

### Step 7: Advanced Details

15. Expand **Advanced details**.

16. For **Termination protection**, select **Enable**.

When an Amazon EC2 instance is no longer required, it can be *terminated*, which means that the instance is deleted and its resources are released. A terminated instance cannot be accessed again and the data that was on it cannot be recovered. If you want to prevent the instance from being accidentally terminated, you can enable *termination protection* for the instance, which prevents it from being terminated as long as this setting remains enabled.

17. Scroll to the bottom of the page and then copy and paste the code shown below into the **User data** box:

```bash
#!/bin/bash
dnf install -y httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html