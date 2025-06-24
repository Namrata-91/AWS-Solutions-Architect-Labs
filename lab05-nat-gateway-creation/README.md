# Creating NAT Gateways in AWS

## Overview

This lab guides you through the process of creating a NAT Gateway in AWS. You will set up a VPC, create public and private subnets, and configure internet connectivity for instances in both subnets using a NAT Gateway.

---

## Steps

### 1. Sign into AWS Management Console

---

### 2. Create a VPC

1. Navigate to **VPC Dashboard**.
2. Click **Create VPC**.
3. Choose "VPC only" and configure:
   - Name tag: `MyVPC`
   - IPv4 CIDR block: `10.0.0.0/16`
4. Click **Create VPC**.

---

### 3. Create Public and Private Subnets

- **Public Subnet:**
  - Go to **Subnets** → **Create Subnet**.
  - Select your VPC.
  - Set CIDR block: `10.0.1.0/24`
  - Name: `PublicSubnet`

- **Private Subnet:**
  - Create another subnet with CIDR block: `10.0.2.0/24`
  - Name: `PrivateSubnet`

---

### 4. Create an Internet Gateway

1. Go to **Internet Gateways** → **Create Internet Gateway**.
2. Name: `MyIGW`
3. Click **Create**, then **Attach to VPC** → Select your VPC.

---

### 5. Create Public Route Table and Configure

1. Go to **Route Tables** → **Create Route Table**
   - Name: `PublicRouteTable`
   - VPC: Your VPC
2. Edit **Routes**:
   - Destination: `0.0.0.0/0`
   - Target: Select your Internet Gateway
3. Associate with **Public Subnet**:
   - Click **Subnet Associations**
   - Edit and select `PublicSubnet`

---

### 6. Launch an EC2 Instance in Public Subnet

1. Go to **EC2 Dashboard** → **Launch Instance**.
2. Choose Amazon Linux AMI.
3. Network settings:
   - VPC: Your VPC
   - Subnet: `PublicSubnet`
   - Enable auto-assign public IP
4. Key pair: Select or create one.
5. Launch the instance.

---

### 7. Launch an EC2 Instance in Private Subnet

Repeat the process for EC2, but:
- Use `PrivateSubnet`
- Do **not** assign a public IP

---

### 8. SSH and Test Connectivity

- SSH into the **public instance** using its public IP.
- From the public instance, SSH into the **private instance** using its private IP.
- Test connectivity:
  ```bash
  ping 8.8.8.8
  curl https://aws.amazon.com

  ---

### 9: Create a NAT Gateway

1. Navigate to **VPC Dashboard** → **NAT Gateways**.
2. Click **Create NAT Gateway**.
3. Set the following configuration:
   - **Subnet**: Select the **PublicSubnet**.
   - **Elastic IP**: Allocate a new Elastic IP and attach it.
   - **Name tag**: `MyNATGateway`
4. Click **Create NAT Gateway**.
5. Wait for the status to become **Available**.

---

### 10: Update Route Table for Private Subnet

1. Go to **Route Tables**.
2. Select or create a route table intended for the **PrivateSubnet**.
3. Click **Edit routes** and add the following:
   - **Destination**: `0.0.0.0/0`
   - **Target**: Select the created **NAT Gateway**
4. Click **Save changes**.
5. Ensure the route table is associated with the **PrivateSubnet**:
   - Click **Subnet Associations**.
   - Edit and select `PrivateSubnet`.

---

### 11: Test Internet Access from Private Instance

1. SSH into the **public EC2 instance**.
2. From there, SSH into the **private EC2 instance** using its private IP.
3. Test internet access:

   ```bash
   curl https://www.google.com
   ping 8.8.8.8
   ```

4. You should receive successful responses, confirming that the NAT Gateway is functioning correctly.

---

## Summary

You have:
- Created a **NAT Gateway** in a public subnet
- Configured the **private subnet's route table** to route internet-bound traffic through the NAT Gateway
- Successfully verified **internet access from a private EC2 instance**

This is a crucial setup pattern in AWS for securely enabling outbound internet traffic from private network components.

