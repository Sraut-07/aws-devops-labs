# AWS VPC Public & Private Subnet Lab Documentation

## Objective

To design, configure, and validate an AWS VPC with one public subnet and one private subnet, ensuring secure inbound access via a bastion host and outbound internet access for private instances using a NAT Gateway.

---

## Architecture Overview

* **VPC CIDR:** 10.0.0.0/16
* **Public Subnet:** 10.0.1.0/24
* **Private Subnet:** 10.0.2.0/24
* **Internet Gateway (IGW):** For public internet access
* **NAT Gateway:** For private subnet outbound internet access
* **EC2 Instances:**

  * Public EC2 (Bastion Host)
  * Private EC2 (Application Server)

---

## Step 1: Create VPC

1. Open AWS VPC Console
2. Create VPC

   * IPv4 CIDR: `10.0.0.0/16`
3. Enable DNS hostnames and DNS resolution

---

## Step 2: Create Subnets

### Public Subnet

* CIDR: `10.0.1.0/24`
* Availability Zone: us-east-1a
* Enable **Auto-assign public IPv4 address**

### Private Subnet

* CIDR: `10.0.2.0/24`
* Availability Zone: us-east-1a
* Auto-assign public IPv4 address: **Disabled**

---

## Step 3: Create and Attach Internet Gateway

1. Create Internet Gateway
2. Attach it to the VPC

---

## Step 4: Create NAT Gateway

1. Allocate Elastic IP
2. Create NAT Gateway in **Public Subnet**
3. Attach Elastic IP

---

## Step 5: Configure Route Tables

### Public Route Table

* Routes:

  * `10.0.0.0/16` → local
  * `0.0.0.0/0` → Internet Gateway
* Associate with **Public Subnet**

### Private Route Table

* Routes:

  * `10.0.0.0/16` → local
  * `0.0.0.0/0` → NAT Gateway
* Associate with **Private Subnet**

---

## Step 6: Launch EC2 Instances

### Public EC2 (Bastion Host)

* Subnet: Public Subnet
* Auto-assign Public IP: Enabled
* Security Group:

  * SSH (22) → My IP

### Private EC2

* Subnet: Private Subnet
* Auto-assign Public IP: Disabled
* Security Group:

  * SSH (22) → Source: Public EC2 Security Group

---

## Step 7: Connect to Instances

### Connect to Public EC2

```bash
ssh -i sauravusec2.pem ubuntu@<PUBLIC_EC2_PUBLIC_IP>
```

### Copy Key to Public EC2

```bash
scp -i sauravusec2.pem sauravusec2.pem ubuntu@<PUBLIC_EC2_PUBLIC_IP>:/home/ubuntu/
chmod 400 sauravusec2.pem
```

### Connect to Private EC2 via Public EC2

```bash
ssh -i sauravusec2.pem ubuntu@<PRIVATE_EC2_PRIVATE_IP>
```

---

## Step 8: Validation Tests

### Test Internet from Public EC2

```bash
ping 8.8.8.8
curl ifconfig.me
```

### Test Internet from Private EC2 (via NAT)

```bash
ping 8.8.8.8
curl ifconfig.me
```

Expected: Public IP displayed should be NAT Gateway Elastic IP

---

## Security Best Practices

* Do not expose private EC2 directly to the internet
* Use Bastion Host or AWS SSM Session Manager
* Restrict SSH access using Security Groups

---

## Conclusion

This lab successfully demonstrates a secure AWS VPC architecture using public and private subnets, validating real-world best practices for networking, routing, and access control.

---

## Interview Summary (One-Liner)

> I designed a VPC with public and private subnets, validated secure access via a bastion host, and enabled outbound internet access for private instances using a NAT Gateway.
