🔐 Secure Private Infrastructure with Bastion Host
📌 Project Overview

This project demonstrates a secure AWS architecture where application servers run in private subnets and are accessed through a Bastion Host.
EC2 instances use IAM Roles to securely access AWS services without storing access keys.

🏗 Architecture
Developer / User
      │
  HTTP / HTTPS
      ▼
Internet Gateway
      │
VPC (10.0.0.0/16)
      │
Public Subnet
 ├─ Bastion Host (EC2 + Nginx Proxy)
 └─ NAT Gateway
      │
Private Subnet
 └─ Private EC2 (Application Server)
      │
    IAM Role
      │
    Amazon S3
⚙️ AWS Services Used

Amazon EC2

Amazon VPC

Internet Gateway

NAT Gateway

IAM Role

Amazon S3

Nginx

🚀 Implementation Steps
Step 1 – Create VPC

Create a custom VPC

CIDR block:

10.0.0.0/16
Step 2 – Create Subnets

Create the following subnets:

Public Subnet

10.0.1.0/24

Private Subnet

10.0.3.0/24

Enable auto-assign public IP only for the public subnet.

Step 3 – Configure Internet Gateway

Create an Internet Gateway

Attach it to the VPC

Add route:

0.0.0.0/0 → Internet Gateway
Step 4 – Create NAT Gateway

Create NAT Gateway in Public Subnet

Allocate Elastic IP

Update private route table:

0.0.0.0/0 → NAT Gateway
Step 5 – Launch Bastion Host

Launch EC2 instance in Public Subnet.

Configuration:

Instance Type: t2.micro

Enable Public IP

Security Group:

SSH (22) → Your IP only
HTTP (80) → 0.0.0.0/0

Install Nginx for reverse proxy.

Step 6 – Launch Private EC2 Server

Launch EC2 instance in Private Subnet.

Configuration:

No public IP

Security group allows:

SSH → Bastion Security Group
HTTP → Bastion Security Group

Install web server:

sudo yum install nginx -y
sudo systemctl start nginx
Step 7 – Create IAM Role

Create IAM Role for EC2 with policy:

AmazonS3ReadOnlyAccess

Attach the role to the Private EC2 instance.

Test access:

aws s3 ls
🔑 Security Features

Private EC2 has no public IP

Access allowed only via Bastion Host

IAM Role authentication instead of access keys

Internet access through NAT Gateway

🔄 Access Flow
User → Bastion Host → Private EC2 → IAM Role → S3
📚 Learning Outcomes

Designing secure AWS VPC architecture

Implementing Bastion Host access

Using IAM Roles for secure authentication

Working with private subnets and NAT Gateway
