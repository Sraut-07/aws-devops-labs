 AWS VPC + Private EC2 NGINX Website with Application Load Balancer

Project Overview:
This project demonstrates how to host a website securely on a private EC2 instance using NGINX, while exposing it to the internet through an Application Load Balancer (ALB) inside a custom VPC. The architecture follows AWS best practices for security and scalability.

1️⃣ VPC Creation

Step: Create a VPC.
Details:

VPC Name: my-vpc

CIDR Block: 10.0.0.0/16

Purpose: Establishes an isolated network for deploying resources securely.
Outcome: A VPC is successfully created with the specified CIDR block.

2️⃣ Subnets Configuration

Step: Configure Public and Private Subnets.
Details:

Public Subnet 1: 10.0.1.0/24 (Availability Zone A)


Private Subnet 2: 10.0.2.0/24 (Availability Zone A)

Public Subnet: 10.0.3.0/24 (Availability Zone B)

Purpose: Segregates resources by exposure; public subnets for load balancer, private subnets for EC2 instances.

Outcome: Subnets are created and associated with appropriate availability zones.

3️⃣ Route Tables

Step: Configure Route Tables.
Details:

Public Route Table: Routes 0.0.0.0/0 → Internet Gateway (IGW)

Private Route Table: Routes 0.0.0.0/0 → NAT Gateway
Purpose: Controls traffic flow for public and private subnets.
Outcome: Network traffic is properly routed according to subnet exposure.

4️⃣ EC2 Instance Creation

Step: Launch Private EC2 Instance.
Details:

Instance Name: Private-Web-Server

Instance Type: t2.micro (example)

Subnet: Private Subnet

Public IP: Disabled
Purpose: Hosts NGINX in a private subnet for security.
Outcome: EC2 instance is launched and ready for software installation.

5️⃣ NGINX Installation

Step: Install and verify NGINX.
Commands:

sudo apt update
sudo apt install nginx -y
sudo systemctl status nginx

Purpose: Serves a static website from the private EC2 instance.
Outcome: NGINX is successfully installed and running.

6️⃣ Target Group Creation

Step: Create a Target Group for the Load Balancer.
Details:

Target Type: Instance

Protocol: HTTP

Port: 80

Instances Registered: Private EC2 instance
Purpose: Links the load balancer to backend EC2 instances.
Outcome: Target group is configured and instance is registered.

7️⃣ Target Health

Step: Verify Target Group health.
Expected Status:

Healthy: 1

Unhealthy: 0
Purpose: Ensures that the load balancer routes traffic only to healthy instances.
Outcome: EC2 instance is healthy and ready to serve traffic.

8️⃣ Load Balancer Creation

Step: Create Application Load Balancer (ALB).
Details:

Name: Example-ALB

Scheme: Internet-facing

Subnets: Public Subnet 1 & 2
Purpose: Exposes the application to the internet while keeping EC2 instances private.
Outcome: ALB is configured and ready to forward traffic.

9️⃣ Load Balancer Listener

Step: Configure Listener for HTTP traffic.
Details:

Protocol: HTTP

Port: 80

Forward To: Target Group
Purpose: Routes incoming requests to backend EC2 instances.
Outcome: Listener correctly forwards HTTP traffic.

🔟 Security Group Configuration

Step: Set Security Group Rules.
Details:

Load Balancer SG: HTTP 80 → 0.0.0.0/0 (public access)

EC2 SG: HTTP 80 → Load Balancer SG (private access)
Purpose: Allows the load balancer to access EC2 while restricting direct internet access to private instances.
Outcome: Security rules enforce proper access control.

1️⃣1️⃣ Final Website Output

Step: Test Website via ALB DNS.
Expected Result:

Browser displays: "Welcome to NGINX!"
Purpose: Confirms that the entire architecture is functioning correctly.
Outcome: Website is successfully hosted on a private EC2 instance accessed through the Application Load Balancer.
 