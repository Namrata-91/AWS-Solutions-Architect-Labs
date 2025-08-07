# Setting up the VPC environment, launching EC2 instances, and testing reachability using the AWS Reachability Analyzer

## Step 1: Sign in to AWS Management Console

## Step 2: Create a VPC and Subnets
1. In the AWS Management Console, navigate to the **VPC Dashboard**.
2. Click on **Your VPCs** in the left-hand menu.
3. Click on the **Create VPC** button.
   - **Name tag**: Enter a name for your VPC (e.g., `MyVPC`).
   - **IPv4 CIDR block**: Choose a CIDR block (e.g., `10.0.0.0/16`).
   - **IPv6 CIDR block**: (Optional) You can leave this blank or select an option.
   - **Tenancy**: Default.
4. Click **Create VPC**.

### Create Subnets
1. In the left-hand menu, click on **Subnets**.
2. Click on the **Create subnet** button.
   - **Name tag**: Enter a name for your first subnet (e.g., `MySubnetA`).
   - **VPC**: Select the VPC you just created.
   - **Availability Zone**: Choose an availability zone.
   - **IPv4 CIDR block**: (e.g., `10.0.1.0/24`).
3. Click **Create subnet**.
4. Repeat the above steps to create a second subnet (e.g., `MySubnetB` with CIDR `10.0.2.0/24`).

### Configure the route tables and internet gateway for the public subnet 
1. For the public subnet to have internet access, you'll need to configure the route table and attach an internet gateway. 
2. Go to the Route Tables section in the VPC service and create a new route table for the public subnet. 
   - Route Table Name: (MY-RT)
3. Next, go to the __Internet Gateways__ section and create a new __internet gateway.__
   - Internet Gateway Name: (My-Internet-Gateway)
   - Once Internet gateway is created attach it the VPC that you have created.
4. In the route table, click on Edit routes, then add your internet gateway. The destination should be (0.0.0.0/0), and the target should be the internet gateway.
   
### Step 3: Launch EC2 Instances
1. Navigate to the **EC2 Dashboard**.
2. Click on **Instances** in the left-hand menu.
3. Click on the **Launch Instances** button.
4. Configure your instance:
   - **Name tag**: Enter a name for your first EC2 instance (e.g., `MyInstance1`).
   - **AMI**: Choose an Amazon Machine Image (e.g., Amazon Linux 2).
   - **Instance type**: Select an instance type (e.g., `t2.micro`).
   - **VPC**: Select the VPC you created.
   - **Subnet**: Select `MySubnetA`.
5. Click **Next: Configure Instance Details** and configure any additional settings.
6. Click **Next: Add Storage** and configure storage settings if necessary.
7. Click **Next: Add Tags** and add any tags if needed.
8. Click **Next: Configure Security Group** (MyEC2Server_SG) and configure your security group settings.
   - We will now add the security group rules. SSH will already be present there.
   - For HTTP, Select Add security group rule Button
     - Choose Type: Select HTTP Source: Select Anywhere 
9. Click **Review and Launch** and then **Launch**.
10. Repeat steps 3-9 to launch your second EC2 instance (e.g., `MyInstance2` in `MySubnetB`). 
    - Add Security Group for the second EC2 instance (MyInstance2): 
    - Name: PrivateEC2Server_SG (or any descriptive name) 
    - Description: Security Group for Private EC2 Instance 
    - Inbound Rules: Allow All Traffic (or specific ports/protocols) from the MyEC2Server_SG security group 

## Step 4: Now We are going to analyze the path to both the source and destination (Check Connectivity Using VPC Reachability Analyzer)
1. In the AWS Management Console, navigate to the **VPC Dashboard**.
2. In the left-hand menu, click on **Reachability Analyzer**.
3. Click on the **Create and analyze path** button.
4. Configure the path:
   - **Source**: Select the first EC2 instance (`MyInstance1`).
   - **Destination**: Select the second EC2 instance (`MyInstance2`).
5. Click **Analyze Path**.
6. Review the results to check the reachability between the two instances.
7. If the test result shows Reachable, it indicates that the two EC2 instances can communicate with each other within the VPC you created.
8. If the test result shows unreachable, the Reachability Analyzer will provide detailed information about the issue and potential reasons for the connectivity problem between the two instances. 

## Conclusion
You have successfully set up a VPC, launched two EC2 instances, and tested their connectivity using the AWS Reachability Analyzer. For further configurations and optimizations, consider exploring AWS documentation or additional features.
Introducing VPC Reachability Analyzer, a game-changer in AWS network troubleshooting. Unlike traditional methods, it dynamically maps network traffic, providing real-time insights across VPCs and subnets. This end-to-end visibility extends beyond boundaries,
automating root cause analysis and integrating seamlessly with AWS services like CloudWatch. Not just troubleshooting, it ensures compliance and security, scaling effortlessly from small deployments to enterprise networks. With its speed, accuracy, and automation,
VPC Reachability Analyzer revolutionizes network management in AWS environments. 
