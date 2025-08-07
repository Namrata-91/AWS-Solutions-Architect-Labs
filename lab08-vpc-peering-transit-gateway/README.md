# AWS Peer VPC with Transit Gateway and its components

This document provides step-by-step instructions for creating two VPCs, setting up a Transit Gateway, and enabling connectivity between them.

# AWS Transit Gateway

## Overview
The AWS Transit Gateway helps you connect multiple VPCs and on-premises networks through a central hub. It simplifies your network architecture by managing VPCs and on-premises connections, solving the problem of complex peering relationships.

With VPC peering using the Transit Gateway, your data is always encrypted and no longer uses the public internet for communication.

## Benefits of Using Transit Gateway
- **Easy to Connect**: Simplifies the network topology.
- **Full Control**: Offers greater control over routing and traffic management.
- **Greater Security**: Ensures secure data transmission without using public internet.
- **Multicast Feature**: Supports multicast traffic for applications that require it.

## Reasons to Use Transit Gateway Over VPC Peering
- **Transitive Peering**: VPC peering does not support transitive peering, meaning you can only peer two VPCs at a time.
- **On-Premises Connections**: VPC peering cannot connect your VPC with an on-premises network, whereas Transit Gateway supports this.

## Transit Gateway Limits
- Per Transit Gateway, you can have:
  - 20 Transit Gateway route tables.
  - 10,000 routes.
  - 50 Transit Gateway attachments.
- Per VPC, you can have 5 unique Transit Gateways.

## Use Cases
- **Global Application Delivery**: Applications can be delivered around the world seamlessly.
- **Network Design Flexibility**: Move your network design from Multi-AZ to Multi-region.
- **Scalability**: Scale quickly and respond to spikes in traffic smoothly.
- **Unified Network Connection**: Connect to all types of networks in one place.

## Detailed Steps

### 1. Sign into the AWS Management Console

### 2. Create the First VPC
- Go to the VPC Dashboard.
- Click on "Create VPC."
- Specify the CIDR block (e.g., `10.0.0.0/16`).

### 3. Create a Public Subnet in the First VPC
- Click on "Subnets."
- Click on "Create subnet."
- Choose your VPC and specify the CIDR block (e.g., `10.0.1.0/24`).

### 4. Create and Attach an Internet Gateway
- Click on "Internet Gateways."
- Click on "Create Internet Gateway."
- Attach the Internet Gateway to your first VPC.

### 5. Create a Public Route Table and Associate It with the Subnet
- Click on "Route Tables."
- Create a new route table for your first VPC.
- Associate it with the public subnet.

### 6. Add a Public Route in the Route Table
- Edit the route table and add a route:
  - Destination: `0.0.0.0/0`
  - Target: Your Internet Gateway.

### 7. Launch an EC2 Instance in the First VPC
- Go to the EC2 Dashboard.
- Click on "Launch Instance."
- Choose an AMI and instance type.
- Ensure it’s in the public subnet you created.

### 8. Create a Second VPC
- Go to the VPC Dashboard.
- Click on "Create VPC."
- Specify a different CIDR block (e.g., `10.1.0.0/16`).

### 9. Create a Private Subnet in the Second VPC
- Click on "Subnets."
- Click on "Create subnet."
- Choose your second VPC and specify the CIDR block (e.g., `10.1.1.0/24`).

### 10. Launch an EC2 Instance in the Second VPC
- Go to the EC2 Dashboard.
- Click on "Launch Instance."
- Choose an AMI and instance type.
- Ensure it’s in the private subnet you created.

### 11. Create a Transit Gateway
- Go to the VPC Dashboard.
- Click on "Transit Gateways."
- Click on "Create Transit Gateway" and configure the settings.

### 12. Create Two Transit Gateway Attachments for the VPCs
- Click on "Transit Gateway Attachments."
- Click on "Create Transit Gateway Attachment."
- Select the Transit Gateway and attach it to both VPCs.

### 13. Add Routes in the First VPC’s Route Table
- Edit the route table for the first VPC.
- Add a route to the CIDR block of the second VPC via the Transit Gateway.

### 14. Add Routes in the Second VPC’s Route Table
- Edit the route table for the second VPC.
- Add a route to the CIDR block of the first VPC via the Transit Gateway.

### 15. Test Connectivity Between Two VPCs
- SSH into the EC2 instance in the first VPC.
- Attempt to ping the private IP address of the EC2 instance in the second VPC.

## Conclusion
You have successfully set up two VPCs with a Transit Gateway, allowing for secure communication between them. This setup provides a scalable solution for connecting multiple VPCs and on-premises networks.
With Transit Gateway, you can easily add or remove VPC connections as your network grows or changes, without impacting existing connections. It provides a flexible and scalable solution for interconnecting VPCs and simplifies network administration, 
routing, and security.



## Conclusion
AWS Transit Gateway is a powerful service that simplifies network management, enhances security, and provides a scalable solution for connecting multiple networks. Its benefits and use cases make it a valuable tool for organizations looking to optimize their cloud architecture.
