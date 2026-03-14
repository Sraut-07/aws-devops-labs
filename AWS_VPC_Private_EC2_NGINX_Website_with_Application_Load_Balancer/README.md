AWS VPC + Private EC2 NGINX Website with Application Load Balancer
Project Overview

This project demonstrates how to host a website securely on a private EC2 instance using NGINX, while exposing it to the internet through an Application Load Balancer (ALB) inside a custom VPC.

The architecture follows a production-style AWS network design where the web server is not directly exposed to the internet.


Architecture Diagram

Internet
   │
Application Load Balancer
(Public Subnets)
   │
Target Group
   │
Private EC2 Instance (NGINX)
   │
NAT Gateway

Technologies Used

AWS VPC

EC2

NGINX

Application Load Balancer

Target Groups

NAT Gateway

Internet Gateway

Security Groups

Linux (Ubuntu)


Step 1 – Create VPC

Go to VPC Dashboard

Click Create VPC

Select VPC and more

Configure:

VPC CIDR: 10.0.0.0/16
Availability Zones: 2
Public Subnets: 2
Private Subnets: 1
NAT Gateway: 1
Internet Gateway: Enabled

This creates:

Public subnets

Private subnet

Internet Gateway

NAT Gateway

Route tables

Step 2 – Launch Private EC2 Instance

Go to EC2 → Launch Instance

Configure:

AMI: Ubuntu
Instance type: t2.micro
Subnet: Private Subnet
Auto assign public IP: Disabled

Security group:

Allow HTTP (80)
Source: Load Balancer Security Group

Launch instance.

Step 3 – Connect to Private EC2

Connect using EC2 Instance Connect or bastion method.

Update system:

sudo apt update

Install NGINX:

sudo apt install nginx -y

Start NGINX:

sudo systemctl start nginx

Check service:

sudo systemctl status nginx
Step 4 – Create Target Group

Go to:

EC2 → Target Groups → Create Target Group

Configuration:

Target type: Instance
Protocol: HTTP
Port: 80
Health check path: /

Register the private EC2 instance.

Wait until target status becomes:

Healthy
Step 5 – Create Application Load Balancer

Go to:

EC2 → Load Balancers → Create Load Balancer

Select:

Application Load Balancer

Configuration:

Scheme: Internet-facing
Subnets: Two Public Subnets
Listener: HTTP 80

Attach the target group created earlier.

Step 6 – Configure Security Groups
Load Balancer Security Group

Inbound rules:

HTTP 80 → 0.0.0.0/0
Private EC2 Security Group

Inbound rules:

HTTP 80 → Load Balancer Security Group

This allows only the load balancer to access the server.

Step 7 – Access the Website

Copy the Load Balancer DNS name.

Example:

http://web-test-1786502202.ap-south-1.elb.amazonaws.com

Output:

Welcome to nginx!
Key Learning Outcomes

This project demonstrates:

Secure AWS network architecture

Hosting a website in a private subnet

Using a Load Balancer for public access

Health checks and target groups

AWS security group configuration

Future Improvements

Add Auto Scaling Group

Configure HTTPS with SSL

Deploy using Terraform

Use Route53 domain

Author

Saurav Chintamni Raut

IT Support Engineer | Learning DevOps | AWS | Linux | Networking