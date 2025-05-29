# Lab 02: Blocking Web Traffic with AWS WAF

## 🔐 What is AWS WAF?

AWS WAF (Web Application Firewall) is a security service that protects your web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources.

### Key Benefits:
- Create security rules to block attack patterns like SQL injection and cross-site scripting (XSS)
- Control how traffic reaches your applications
- Custom rule creation based on request patterns
- Pay-as-you-go pricing based on number of rules and web requests

You can deploy AWS WAF with services such as:
- Amazon CloudFront (global CDN)
- Application Load Balancer (ALB)
- Amazon API Gateway

---

## 🌟 Features of AWS WAF

### ✅ Web Traffic Filtering Using Custom Rules
You can create custom rules to allow or block traffic based on:
- IP addresses
- Request patterns
- Header values
- URI strings

### ✅ Blocking Malicious Requests
Configure rules to detect and block common threats such as:
- SQL injections
- Cross-site scripting (XSS)

### ✅ Monitor and Tune Rules
Continuously monitor traffic, review WAF logs, and adjust rules to prevent evolving threats.

---

## 🎯 Lab Objective

To block specific web traffic based on IP addresses using AWS WAF, and verify its functionality using a Load Balancer and EC2-based web servers.

---

## 🛠️ Steps

### 1. Sign in to the AWS Management Console


### 2. Create a Security Group for the Load Balancer
- Navigate to **EC2 > Security Groups**
- Click **Create Security Group**
  - Name: `LB-Security-Group`
  - Description: Security group for Load Balancer
  - Inbound Rules: Allow HTTP (port 80) and HTTPS (port 443)


### 3. Launch Web Servers
- Launch **two EC2 instances** in the same VPC.
- Assign them the `LB-Security-Group`.
- Install a web server (e.g., Apache or Nginx) on each instance.

  Example (for Apache):
  ```bash
  sudo yum install -y httpd
  sudo systemctl start httpd
  sudo systemctl enable httpd
  echo "Welcome from $(hostname)" > /var/www/html/index.html


### 4. Create an Application Load Balancer

- Navigate to **EC2 > Load Balancers** in AWS Console.
- Click **Create Load Balancer** → Choose **Application Load Balancer**
- Configure:
  - **Name:** `MyAppLoadBalancer`
  - **Scheme:** Internet-facing
  - **Listeners:** HTTP (port 80), optionally HTTPS (port 443)
  - **Availability Zones:** Select at least 2 subnets
- Create or select a **Target Group**:
  - Add the EC2 instances launched earlier.
- Register your web servers and finish creating the ALB.


### 5. Test the Load Balancer

- Access the **DNS name** of the Load Balancer in a browser.
- Ensure the web pages from your EC2 servers load correctly.
- If load balancing is configured properly, traffic should alternate between both servers.


### 6. Create an IP Set in AWS WAF

- Go to **AWS WAF & Shield > IP Sets**.
- Click **Create IP set**:
  - **Name:** `BlockedIPs`
  - **Scope:** Regional
  - **Region:** Same as your ALB
  - Add IP addresses you want to block (e.g., `192.0.2.1/32`)
- Save the IP set.


### 7. Create a Web ACL

- Go to **WAF & Shield > Web ACLs**.
- Click **Create Web ACL**:
  - **Name:** `MyWebACL`
  - **Scope:** Regional
  - **Associated Resource:** Your Application Load Balancer
- Add a rule to block traffic:
  - **Rule Type:** IP Set match
  - **IP Set:** Select `BlockedIPs`
  - **Action:** Block
- Complete and save the Web ACL.


### 8. Test WAF Blocking

- Try accessing the Load Balancer from a **blocked IP address**.
  - Result: Access should be **denied**.
- Try accessing from a **non-blocked IP address**.
  - Result: Access should be **allowed**.


### 9. Unblock IP (Optional)

- Go to **WAF > IP Sets**.
- Edit `BlockedIPs`:
  - Remove the IP(s) you wish to unblock.
- Save changes.
- The Web ACL will reflect the updated IP list automatically.


### ✅ Summary

Results Successfully implemented AWS WAF to block specific web traffic based on IP addresses. Verified functionality through testing access from both blocked and unblocked IPs.

Web Application Firewall (WAF): A Web Application Firewall is a security service that sits between a web application and its users, inspecting and filtering incoming web traffic. In the context of AWS, the AWS WAF service provides this functionality. It helps protect web applications by examining HTTP/HTTPS requests and applying a set of predefined rules or custom rule configurations to block, allow, or filter traffic based on specified criteria.
