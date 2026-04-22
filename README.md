# AWS 2-Tier Architecture Deployment

## Overview

This project demonstrates the deployment of a **secure and scalable 2-tier architecture on AWS**.

The architecture is divided into:

* **Web Tier** → Hosted on EC2 (WordPress application)
* **Database Tier** → Hosted on Amazon RDS (MySQL)

It follows real-world cloud practices such as **network isolation, private database access, and controlled communication using security groups**.

---

## Architecture

### Components:

* Custom VPC 
* Public Subnet → EC2 (Web Server)
* Private Subnet → RDS (Database)
* Internet Gateway
* Route Tables
* Security Groups

### Architecture Flow:

* Users access the application via EC2 public IP
* EC2 communicates securely with RDS inside private subnet

---

## Project Walkthrough

Below are step-by-step screenshots of the deployment process.

---

## Deployment Steps

### 1. VPC Setup

Created a custom VPC with CIDR block `10.0.0.0/16`.

![VPC Setup](./screenshots/vpc.png)

---

### 2. Subnet Configuration

Created public and private subnets within the VPC:

* Public Subnet: `10.0.0.0/24`
* Private Subnet: `10.0.1.0/24`

![Public Subnet](./screenshots/public-subnet.png)
![Public Subnet](./screenshots/private-subnet.png)

---

### 3. Internet Gateway

Created and attached an Internet Gateway to enable internet access for the public subnet.

![Internet Gateway](./screenshots/igw.png)

---

### 4. Route Tables

Configured route tables:

* Public route table → routes internet traffic via IGW
* Private route table → internal traffic only

![Public Route Table](./screenshots/public-routetable.png)
![Public Route Table Subnet Association](./screenshots/public-RT-Subnet.png)
![Private Route Table](./screenshots/private-routetable.png)
![Private Route Table Subnet Association](./screenshots/private-RT-Subnet.png)

---

### 5. Security Groups

Configured security groups for controlled access:

* Web Server SG:

    * Allow HTTP (80)
    * Allow SSH (22)
* Database SG:

    * Allow MySQL (3306) only from Web Server SG

![Web Server Security Group](./screenshots/Web-SG.png)
![Database Security Group](./screenshots/DB-SG.png)
---

### 6. EC2 Instance (Web Server)

Launched an EC2 instance in the public subnet:

* AMI: Amazon Linux 2
* Instance type: t2.micro
* Enabled public IP

Installed:

* Apache (httpd)
* PHP
* WordPress

![EC2 Instance](./screenshots/ec2.png)
![EC2 Instance Security Group](./screenshots/ec2-security-group.png)

Install Apache web server
![Apache Web Server](./screenshots/install-httpd.png)
![Apache Web Server](./screenshots/install-httpd-2.png)
![Apache Web Server](./screenshots/install-httpd-3.png)
![Apache Web Server](./screenshots/result-httpd.png)

Install php
![PHP](./screenshots/install-php.png)
![PHP](./screenshots/install-6-php.png)

Install SQL
![SQL](./screenshots/install-7-sql.png)

Install wordpress tar
![wordpress](./screenshots/install-8-wordpress-tar.png)
![wordpress](./screenshots/install-9-config.png)


---

### 7. RDS Database (Private)

Created MySQL RDS instance:

* Deployed in private subnet
* No public access
* Connected via security group

![RDS Instance](./screenshots/RDS.png)
![RDS Instance](./screenshots/RDS-SG.png)

RDS instance configured with a DB subnet group across private subnets and public access disabled, ensuring secure database deployment.
The RDS instance is deployed using a DB subnet group that includes multiple private subnets across different Availability Zones.

This ensures:
- High availability (multi-AZ readiness)
- Isolation from public internet
- Secure communication within VPC

![RDS Subnets](./screenshots/DB-subnets.png)
---

### 8. WordPress Configuration

Configured WordPress by editing `wp-config.php`:

* Database name
* Username & password
* RDS endpoint

![WordPress Setup](./screenshots/database-connection.png)
![WordPress Setup](./screenshots/databse-connection2.png)

Updated WordPress security keys (salts) in `wp-config.php` to enhance authentication security.

Salts are cryptographic keys used to secure user sessions and cookies, preventing session hijacking and unauthorized access.

This step ensures the application follows basic security best practices.

![WordPress Setup](./screenshots/database-connection3.png)
![WordPress Setup](./screenshots/databse4.png)
![WordPress Setup](./screenshots/databse-5.png)
![WordPress Setup](./screenshots/databse-6.png)
---

### 9. Final Output

Successfully deployed WordPress application.

Access:
http://44.193.210.31/wp-admin

![Final Output](./screenshots/result.png)
![Final Output](./screenshots/result2.png)
![Final Output](./screenshots/result3.png)
![Final Output](./screenshots/result4.png)

---

### 10. Configure Route 53

Configure Route 53 for domain name

![Route 53](./screenshots/route-53.png)
![Route 53 Output](./screenshots/route-53-output.png)

##  AWS Services Used

* Amazon VPC
* Amazon EC2
* Amazon RDS (MySQL)
* Internet Gateway
* Route Tables
* Security Groups


---

## Security Best Practices Implemented

* Database deployed in private subnet
* No public access to RDS
* Security group chaining (EC2 → RDS)
* Restricted inbound access
* Custom VPC instead of default VPC

---

## Key Learnings

* Designing VPC from scratch
* Understanding public vs private subnets
* Configuring secure communication between services
* Deploying real-world applications on AWS
* Managing EC2, RDS, and networking 

---
## Challenges Faced

- Initial DB subnet group creation failed due to single AZ
- Resolved by adding a second private subnet in a different Availability Zone
---

## Design Decisions

- Used private subnets for RDS to ensure database is not publicly accessible
- Implemented security group chaining to restrict database access only from EC2
- Used separate public and private subnets to isolate web and database layers
- Configured DB subnet group across multiple AZs for high availability readiness