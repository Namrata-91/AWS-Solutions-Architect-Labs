# Access S3 from Private EC2 Instance Using VPC Endpoint

This guide provides step-by-step instructions to access Amazon S3 from a private EC2 instance using a VPC Endpoint.

## Steps

### 1. Sign into AWS Management Console

### 2. Create a VPC
1. Navigate to the VPC Dashboard.
2. Click on "Create VPC."
3. Choose an appropriate CIDR block (e.g., `10.0.0.0/16`).

### 3. Create and Attach an Internet Gateway
1. In the VPC Dashboard, click on "Internet Gateways."
2. Click on "Create Internet Gateway."
3. Attach the Internet Gateway to your VPC.

### 4. Create a Public and Private Subnet
- **Public Subnet:**
  1. Click on "Subnets" in the VPC Dashboard.
  2. Create a subnet in your VPC with a CIDR block (e.g., `10.0.1.0/24`).

- **Private Subnet:**
  1. Create another subnet with a CIDR block (e.g., `10.0.2.0/24`).

### 5. Enable Auto-Assign Public IPv4 Address for Public Subnet
1. Select your Public Subnet.
2. Click on "Actions" and choose "Edit subnet settings."
3. Enable the option for "Auto-assign public IPv4 address."

### 6. Create a Route Table for the Public Subnet
1. Click on "Route Tables" in the VPC Dashboard.
2. Create a route table and associate it with your Public Subnet.
3. Add a route to allow internet access (Destination: `0.0.0.0/0`, Target: your Internet Gateway).
4. Repeat the same steps to create a route table for the Private subnet.
5. Now we will associate the subnets to the route tables.
6. PublicRouteTable: Add a route to allow Internet traffic to the VPC.
7. Go to Routes tab, click on Edit routes and on the next page, click on Add route button.
   - Specify the following values: 
     Destination: Enter 0.0.0.0/0
     Target: Select Internet Gateway from the dropdown menu 
     Click on Save changes button.   

### 7. Create Security Groups
1. Create a security group for the Bastion Host allowing SSH access from your IP.
   - We will add 3 rules for the Bastion host security group i.e. SSH, HTTP, and HTTPS.
3. Create a endpoint security group for the Private EC2 instance allowing access from the Bastion Host's security group.
   - We will add 1 rule for the S3 endpoint security group i.e. SSH only. But here our source will be Bastion host security group, 
   - you will select the ID of Bastion-SG security group.

### 8. Create a Bastion Host (Publicly Accessible EC2 Instance)
1. Launch an EC2 instance in the Public Subnet using an appropriate AMI (e.g., Amazon Linux).
2. Assign the Bastion Host the security group created in the previous step.

### 9. Create an Endpoint Instance (Privately Accessible EC2 Instance)
1. Launch an EC2 instance in the Private Subnet using an appropriate AMI.
2. Assign it the endpoint security group that allows access from the Bastion Host.

### 10. SSH into Endpoint Instance through Bastion Host
1. SSH into the Bastion Host.
2. From the Bastion Host, SSH into the Private EC2 instance.
3. Navigate to the Bastion Instance and create a file named (Filename).pem using the below command
   vi (Filename).pem
4. Paste the content and save it by pressing shift+colon  followed by :wq! and then enter to save your private key.
5. Make sure you have changed the permission of the key file to 400. You can change the permission using the below command:
   chmod 400 (Filename).pem
6. Now you can log into the web servers using the private key copied to the bastion server with the help of the below commands
   ssh -i (Filename).pem  ec2-user@<Endpoint instance's Private IP>
7. Let's add the permission to access the S3 endpoints using the VPC Endpoint for S3.

### 11. Create a VPC Endpoint for S3
1. In the VPC Dashboard, click on "Endpoints."
2. Click on "Create Endpoint."
3. Choose the S3 service and attach it to the Route Table of the Private Subnet.

### 12. List All S3 Buckets and Their Objects
1. Navigate back to your Bastion Instance and enter aws configure.
2. Access Key: Paste the access key provided to you.
 - Secret Key: Paste the secret key provided to you.
 - Default region name: us-east-1
 - Default output-format: Enter [ENTER]
3. List all the bucket's using the following AWS CLI command:
   aws s3 ls

## Conclusion
VPC Endpoint Service enables private connectivity, you can avoid data transfer charges associated with public internet traffic. By utilizing VPC endpoints, you can significantly reduce data transfer costs,
especially if you have large volumes of data flowing between your VPC and AWS services.
