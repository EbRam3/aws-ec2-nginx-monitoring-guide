# **AWS EC2 NGINX Deployment with Resilient Monitoring**

This repository contains a comprehensive guide detailing the deployment of an NGINX web server on an Amazon EC2 instance, enhanced with an Elastic IP for static access and robust monitoring solutions using AWS CloudWatch and Simple Notification Service (SNS). The documentation covers the entire process from instance launch to alarm testing, ensuring a professional and organized approach to cloud infrastructure management.

## **Project Overview**

This project demonstrates how to set up a basic, yet resilient, web server environment on AWS. It covers:

* Launching and configuring an Amazon Linux 2 EC2 instance.  
* Automating NGINX installation and initial web page deployment using user data scripts.  
* Assigning a persistent public IP address (Elastic IP) to the EC2 instance.  
* Implementing proactive monitoring for instance health and CPU utilization using AWS CloudWatch.  
* Setting up email notifications via AWS SNS for critical alarms, including automatic instance recovery.  
* Testing the configured alarms to validate their functionality.

## **Features / Components**

* **Amazon EC2:** Virtual servers for hosting the NGINX web server.  
* **NGINX:** A high-performance web server configured to serve a simple "Hello" page.  
* **Elastic IP (EIP):** A static public IPv4 address ensuring persistent accessibility for the EC2 instance.  
* **AWS CloudWatch:** Monitoring service to collect metrics, set alarms, and trigger automated actions.  
* **AWS Simple Notification Service (SNS):** A messaging service used to send email notifications when CloudWatch alarms are triggered.  
* **EC2 Auto-Recovery Alarm:** A CloudWatch alarm configured to automatically recover the EC2 instance upon system check failures.  
* **CPU Utilization Alarm:** A CloudWatch alarm set to notify administrators if the EC2 instance's CPU usage exceeds a defined threshold (70%).  
* **User Data Scripting:** Automation of initial server setup on EC2 launch.

## **Prerequisites**

Before following this guide, ensure you have:

* An active AWS Account with necessary permissions to create EC2 instances, Elastic IPs, SNS topics, and CloudWatch alarms.  
* Basic understanding of AWS console navigation.  
* A text editor to prepare any scripts if needed (though the guide provides them).

## **Installation and Setup Guide**

The detailed steps for setting up this environment are provided within the main documentation file (aws-ec2-nginx-monitoring-guide.md), which includes screenshots for each step. Here's a high-level overview of the process:

1. **Launch EC2 Instance:**  
   * Select Amazon Linux 2 AMI.  
   * Choose t2.micro instance type.  
   * Create a new key pair for SSH access.  
   * Enable auto-assign public IP.  
   * Create a new security group (nginx-sg) allowing SSH (Port 22\) and HTTP (Port 80\) traffic from anywhere.  
   * Configure user data script to install NGINX and create an index.html file.  
   * Launch the instance.  
2. **Associate Elastic IP:**  
   * Allocate a new Elastic IP address in the same region as your EC2 instance.  
   * Associate the allocated Elastic IP with your running EC2 instance.  
3. **Configure SNS Topics:**  
   * Navigate to AWS SNS and create two "Standard" topics:  
     * EC2-Recovery-Topic  
     * ec2-cpu-performance  
   * Create an email subscription for each topic and confirm the subscription via the email link.  
4. **Set Up CloudWatch Alarms:**  
   * Navigate to AWS CloudWatch and create the following alarms:  
     * **Auto-recovery-ec2:** Monitors StatusCheckFailed\_System metric for your EC2 instance.  
       * Condition: Greater/Equal to 1\.  
       * Action: Send notification to EC2-Recovery-Topic and perform EC2 action: Recover this instance.  
     * **cpu-utilization:** Monitors CPUUtilization metric for your EC2 instance.  
       * Condition: Greater/Equal to 70%.  
       * Action: Send notification to ec2-cpu-performance topic.

## **Usage and Verification**

1. **Verify NGINX:** After setup, paste the Elastic IP address into your web browser. You should see the message "Hello from EC2 with NGINX".  
2. **Test CPU Alarm:**  
   * SSH into your EC2 instance.  
   * Install the stress tool: sudo yum install \-y stress.  
   * Run stress \--cpu 2 \--timeout 300 to simulate high CPU usage.  
   * Observe the cpu-utilization alarm in CloudWatch turning to "In alarm" state and check your subscribed email for notifications.  
3. **Test Instance Recovery Alarm (Optional, with caution):**  
   * Simulating a StatusCheckFailed\_System is more complex and usually requires an actual underlying hardware issue or advanced AWS Fault Injection Service (FIS). For educational purposes, understanding the setup is often sufficient without direct simulation.

## **Contributing**

Feel free to open issues or submit pull requests if you have suggestions for improvements or find any discrepancies.

## **License**

This documentation is provided "as is" for educational and informational purposes. There is no explicit license attached.