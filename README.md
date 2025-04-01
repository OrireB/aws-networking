# AWS Networking

This repository contains the configuration and setup for the AWS Networking project. The project covers the creation of a VPC with subnets in different availability zones, implementing security controls (Security Group and NACL), and setting up a NAT Gateway for instances in private subnets.

## Steps:

### 1. Create a Virtual Private Cloud (VPC):
   - A VPC was created with CIDR block `10.0.0.0/16`.
   - Log in to **AWS Management Console**.
   - Navigate to VPC under the "Networking & Content Delivery" section.
   - Click on Create VPC.
     - **Name**: MyVPC-test
     - **IPv4 CIDR block**: 10.0.0.0/16 (or another CIDR block based on your need)
     - **IPv6 CIDR block**: Leave it as No IPv6 CIDR Block (unless you want to enable IPv6)
     - **Tenancy**: Default
   - Click Create VPC.

### 2. Create Subnets in Different Availability Zones:
   - Navigate to Subnets under your VPC dashboard.
   - Click on Create Subnet.
   - Create at least two subnets in different availability zones:
     - **Subnet 1**
     - **Name**: MyPublicSubnet-test
     - **VPC**: MyVPC-test
     - **Availability Zone**: Choose one availability zone (e.g., us-east-1a)
     - **IPv4 CIDR Block**: 10.0.1.0/24

     - **Subnet 2**
     - **Name**: MyPrivateSubnet-test
     - **VPC**: MyVPC-test
     - **Availability Zone**: Choose one availability zone (e.g., us-east-1b)
     - **IPv4 CIDR Block**: 10.0.2.0/24
   - Click Create.

### 3. Create an Internet Gateway (IGW):
   - Navigate to Internet Gateways in the VPC dashboard.
   - Click on Create Internet Gateway.
     - **Name**: MyInternetGateway-test
   - Click Create.

### 4. Create a NAT Gateway:
   - Navigate to NAT Gateways in the VPC dashboard.
   - Click on Create NAT Gateway.
     - **Name**: MyNATGateway-test
     - **Subnet**: Select the public subnet (MyPublicSubnet-test).
   - Click Create.

### 5. Update Route Tables:
   - **Public Route Table**:
   - Navigate to Route Tables in the VPC dashboard.
   - Select the default route table for your VPC and edit routes.
     - **Add a route**: Destination 0.0.0.0/0 (or ::/0 for IPv6)
     - **Target**: the Internet Gateway.
   
   - **Private Route Table**:
   - Navigate to Route Tables in the VPC dashboard.
   - Create a new route table and associate it with the private subnet (MyPrivateSubnet-test).
     - **Add a route**: Destination 0.0.0.0/0 
     - **Target**: the newly created NAT Gateway.

### 6. Implement a Security Group for Traffic Control:
   - **Step 1**: Create a Security Group.
   - Go to the Security Groups section in the EC2 dashboard.
   - Click on Create Security Group.
     - **Name**: MySecurityGroup-test
     - **Description**: Security group for my test EC2 instances
     - **VPC**: MyVPC-test

   - **Step 2: Add Inbound Rules**
   - Click on Inbound Rules and then click Edit inbound rules.
   - Add rules to allow traffic from the NAT Gateway
     - **Name**: MySecurityGroup-test
     - **Type**: SSH
     - **Protocol**: TCP
     - **Port Range**: 22
     - **Source**: My IP (Enter the NAT Gateway's IP address )
     - **Description**: Allow SSH from NAT Gateway.
   - Click Save rules.

   - **Step 3: Add Outbound Rules**
     - By default, security groups allow all outbound traffic, so you usually don’t need to modify outbound rules unless you have specific needs.

   - **Step 4: Apply the Security Group**
     - Attach this security group to your EC2 instances.

### 7. Implement a Network Access Control List (NACL) for Traffic Control:
   - Create and Configure a Network Access Control List (NACL)
   - Navigate to Network ACLs under the VPC dashboard.
   - Click on Create Network ACL.
     - **Name**:MyNetworkACL-test
     - **VPC**: MyVPC-test
     - **Description**: Network ACL for my test Private Subnet
   - Create inbound and outbound rules to allow or deny traffic as per your requirements.

   -  **Inbound traffic**:
     - **Rule #1**: 100
     - **Type**: All traffic
     - **Protocol**: All 
     - **Port Range**: All ports
     - **Source**: 0.0.1.0/24 (public subnet).102.88.108.59/32
     - **Allow/Deny**: ALLOW

   -  **Outbound traffic**:
     - **Rule #1**: 100
     - **Type**: All traffic
     - **Protocol**: All 
     - **Port Range**: All ports
     - **Destination**: 0.0.0.0/0 
     - **Allow/Deny**: ALLOW

### Associate NACL with Subnet:
   - Once the NACL is configured, you’ll need to associate it with the subnets you want it to control.
   - Go to the Subnet Associations tab in the NACL page and click Edit subnet associations.
   - Select the subnets (private subnets)to this NACL

---

## Screenshots:
- Here are the key screenshots showing the steps:

- **Screenshot 1**: VPC Setup Dashboard
  ![VPC Setup Screenshot]()
  
- **Screenshot 2**: VPC Setup Dashboard
  ![NAT Gateway Setup Screenshot]()

- **Screenshot 1**: VPC Setup Dashboard
  ![Security Group Setup Screenshot]()

- **Screenshot 1**: VPC Setup Dashboard
  ![NACL Setup Screenshot]()

---

## Conclusion

Through this project, I successfully created the fundamentals of launching an EC2 Instance using **AWS Management Console**. This process helped me understand how to create an instances in private subnets.

---
