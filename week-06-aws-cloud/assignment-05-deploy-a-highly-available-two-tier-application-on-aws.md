# Assignment 5 — Deploy a Highly Available Two-Tier Application on AWS (VPC + ALB + ASG + Multi-AZ RDS)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will design and deploy a highly available two-tier web application on AWS: highly available networking across two Availability Zones, an Application Load Balancer, an Auto Scaling Group for the web tier, and a private Multi-AZ RDS database. You must prove high availability with real failure tests.

---

# Task 1 — Create HA Networking (VPC + 4 Subnets + IGW + NAT + Route Tables)

## Goal

Build a VPC (10.0.0.0/16) with two public and two private subnets across two Availability Zones, an Internet Gateway, a NAT Gateway, and the matching public/private route tables.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

- ![Task 1 — VPC details showing CIDR 10.0.0.0/16](screenshots/Week06_Assignment5_Task1_Screenshot1.jpg)

---

#### Screenshot 2 — Subnets list showing four subnets and their Availability Zones

- ![Task 1 — Subnets list showing four subnets and their Availability Zones](screenshots/Week06_Assignment5_Task1_Screenshot2.jpg)

---

#### Screenshot 3 — Public route table showing the Internet Gateway route and both public-subnet associations

- ![Task 1 — Public route table showing the Internet Gateway route and both public-subnet associations](screenshots/Week06_Assignment5_Task1_Screenshot3.jpg)

---

#### Screenshot 4 — Private route table showing the NAT Gateway route and both private-subnet associations

- ![Task 1 — Private route table showing the NAT Gateway route and both private-subnet associations](screenshots/Week06_Assignment5_Task1_Screenshot4.jpg)

---

#### Screenshot 5 — NAT Gateway status showing Available and the Elastic IP

- ![Task 1 — NAT Gateway status showing Available and the Elastic IP](screenshots/Week06_Assignment5_Task1_Screenshot5.jpg)

---

# Task 2 — Create Security Groups (ALB, EC2, RDS) with Least Privilege

## Goal

Create `ha-alb-sg` (HTTP public), `ha-web-sg` (HTTP only from `ha-alb-sg`, SSH from your IP), and `ha-db-sg` (database port only from `ha-web-sg`).

### Evidence

#### Screenshot 6 — ALB Security Group inbound rules

- ![Task 2 — ALB Security Group inbound rules](screenshots/Week06_Assignment5_Task2_Screenshot6.jpg)

---

#### Screenshot 7 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP

- ![Task 2 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP](screenshots/Week06_Assignment5_Task2_Screenshot7.jpg)

---

#### Screenshot 8 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group

- ![Task 2 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group](screenshots/Week06_Assignment5_Task2_Screenshot8.jpg)

---

# Task 3 — Deploy Database Tier (RDS Multi-AZ in Private Subnets)

## Goal

Launch a private, Multi-AZ RDS database (MySQL or PostgreSQL) using the private DB Subnet Group and `ha-db-sg`.

### Evidence

#### Screenshot 9 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No

- ![Task 3 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No](screenshots/Week06_Assignment5_Task3_Screenshot9.jpg)

---

#### Screenshot 10 — RDS connectivity section showing the DB Subnet Group and Security Group

- ![Task 3 — RDS connectivity section showing the DB Subnet Group and Security Group](screenshots/Week06_Assignment5_Task3_Screenshot10.jpg)

---

# Task 4 — Build a Launch Template (User Data Installs App + Connects to DB)

## Goal

Create a Launch Template whose user data installs the web-server runtime, deploys the application, configures the database connection, and starts the required services.

### Evidence

#### Screenshot 11 — Launch Template details showing that user data exists, including a visible snippet

- ![Task 4 — Launch Template details showing that user data exists, including a visible snippet](screenshots/Week06_Assignment5_Task4_Screenshot11.jpg)

---

#### Screenshot 12 — A running instance created from the template showing that the application responds on port 80 through a local test or browser using its public IP

- ![Task 4 — A running instance created from the template showing that the application responds on port 80 through a local test or browser using its public IP](screenshots/Week06_Assignment5_Task4_Screenshot12.jpg)

---

# Task 5 — Create an Application Load Balancer (ALB) Across 2 Public Subnets

## Goal

Create an internet-facing ALB across both public subnets with an HTTP listener and a healthy instance target group.

### Evidence

#### Screenshot 13 — ALB details showing two public subnets in two Availability Zones

- ![Task 5 — ALB details showing two public subnets in two Availability Zones](screenshots/Week06_Assignment5_Task5_Screenshot13.jpg)

---

#### Screenshot 14 — Target group showing at least one healthy target

- ![Task 5 — Target group showing at least one healthy target](screenshots/Week06_Assignment5_Task5_Screenshot14.jpg)

---

# Task 6 — Create Auto Scaling Group (ASG) in 2 Public Subnets

## Goal

Create an Auto Scaling Group from the Launch Template across both public subnets, with desired capacity 2, minimum 2, and maximum 4, registered to the ALB target group.

### Evidence

#### Screenshot 15 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones

- ![Task 6 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones](screenshots/Week06_Assignment5_Task6_Screenshot15.jpg)

---

#### Screenshot 16 — EC2 instances list showing two running instances in different Availability Zones

- ![Task 6 — EC2 instances list showing two running instances in different Availability Zones](screenshots/Week06_Assignment5_Task6_Screenshot16.jpg)

---

# Task 7 — Configure App to Use RDS + Validate Read/Write

## Goal

Confirm the application communicates with the RDS database through the ALB DNS name with at least one read and one write operation.

### Evidence

#### Screenshot 17 — Browser showing the application loaded through the ALB DNS name with the URL visible

- ![Task 7 — Browser showing the application loaded through the ALB DNS name with the URL visible](screenshots/Week06_Assignment5_Task7_Screenshot17.jpg)

---

#### Screenshot 18 — Proof of a database write through a UI message or database query output

- ![Task 7 — Proof of a database write through a UI message or database query output](screenshots/Week06_Assignment5_Task7_Screenshot18.jpg)

---

# Task 8 — High Availability Tests (Must Do Both)

## Goal

Test A: terminate one web instance and confirm the Auto Scaling Group replaces it automatically without interrupting the ALB.

Test B: simulate an Availability Zone impact (stop, detach, or reduce desired capacity in one AZ) and confirm the application stays available.

### Evidence

#### Screenshot 19 — EC2 showing the terminated instance and the newly launched instance; timestamps are helpful

- ![Task 8 — EC2 showing the terminated instance and the newly launched instance; timestamps are helpful](screenshots/Week06_Assignment5_Task8_Screenshot19.jpg)

---

#### Screenshot 20 — Target group showing healthy targets after replacement

- ![Task 8 — Target group showing healthy targets after replacement](screenshots/Week06_Assignment5_Task8_Screenshot20.jpg)

---

#### Screenshot 21 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone

- ![Task 8 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone](screenshots/Week06_Assignment5_Task8_Screenshot21.jpg)

---

#### Screenshot 22 — Browser showing that the ALB DNS endpoint still works during the change

- ![Task 8 — Browser showing that the ALB DNS endpoint still works during the change](screenshots/Week06_Assignment5_Task8_Screenshot22.jpg)

---

# Task 9 — Architecture and Test-Results Summary

## Goal

Summarize the VPC/subnet layout, the ALB and Auto Scaling Group setup, the private Multi-AZ RDS setup, and the results of both high-availability tests.

### Evidence

#### Screenshot 23 — A simple architecture diagram, which may be hand-drawn, or an AWS console overview showing the components

- ![Task 9 — A simple architecture diagram, which may be hand-drawn, or an AWS console overview showing the components](screenshots/Week06_Assignment5_Task9_Screenshot23.jpg)

---

### Notes

Summarize the VPC and subnets across the two Availability Zones.

A custom VPC with the CIDR block 10.0.0.0/16 was created across two Availability Zones (eu-west-2a and eu-west-2b) to provide high availability. The architecture consists of two public subnets (10.0.1.0/24 and 10.0.2.0/24) hosting the Application Load Balancer and EC2 web servers, and two private subnets (10.0.11.0/24 and 10.0.12.0/24) hosting the Amazon RDS database. Public subnets route internet traffic through an Internet Gateway, while private subnets use a NAT Gateway for outbound internet access without allowing inbound internet connections.


Summarize the ALB and Auto Scaling Group setup.

An internet-facing Application Load Balancer (ALB) is deployed across both public subnets and distributes incoming HTTP requests to healthy EC2 instances using a target group. The web servers are managed by an Auto Scaling Group configured with a desired capacity of 2 instances, a minimum of 2, and a maximum of 4. Instances are launched from a predefined launch template and configured automatically using a user data script. ALB health checks ensure that unhealthy instances are replaced automatically to maintain application availability.

Summarize the private Multi-AZ RDS setup.

The database tier uses an Amazon RDS MySQL db.t3.micro instance configured with Multi-AZ deployment for high availability. The database is placed in a subnet group containing only the two private subnets and is not publicly accessible. Access is restricted through security groups, allowing MySQL traffic (port 3306) only from the EC2 web servers, ensuring the database remains protected from direct internet access.

Summarize the results of both high-availability tests.

The first test verified automatic recovery by terminating a web server instance. The Auto Scaling Group detected the failure, launched a replacement instance within a few minutes, and the Application Load Balancer continued serving user requests without interruption. The second test reduced the Auto Scaling Group to a single instance, leaving one Availability Zone serving all traffic. Continuous testing returned successful HTTP 200 responses throughout the process, demonstrating that the application remained available. After the test, the Auto Scaling Group restored the second instance and rebalanced the workload across both Availability Zones.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the high-availability build, including the ALB URL (or a redacted screenshot), three to five lines on what you built and how you tested high availability, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

https://lnkd.in/p/eWMi2kiS

---

#### Screenshot of LinkedIn post

- ![LinkedIn Post Screenshot](screenshots/Week06_Assignment5_Task9_Linkedinpost.jpg)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose passwords, connection strings, private keys, or account IDs

---

# Completion Checklist

- [ ] Task 1: VPC, four subnets, IGW, NAT Gateway, and route tables created (Screenshots 1–5)
- [ ] Task 2: Least-privilege ALB, EC2, and RDS security groups created (Screenshots 6–8)
- [ ] Task 3: Private Multi-AZ RDS created (Screenshots 9–10)
- [ ] Task 4: Self-configuring Launch Template created and tested (Screenshots 11–12)
- [ ] Task 5: ALB created across both public subnets (Screenshots 13–14)
- [ ] Task 6: Auto Scaling Group running two instances across two AZs (Screenshots 15–16)
- [ ] Task 7: Application verified through the ALB with a database read and write (Screenshots 17–18)
- [ ] Task 8: Both high-availability tests completed (Screenshots 19–22)
- [ ] Task 9: Architecture and test-results summary completed (Screenshot 23 & Notes)
- [ ] LinkedIn post published and URL submitted
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*