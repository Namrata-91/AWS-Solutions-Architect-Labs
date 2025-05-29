
# Lab 03 – Implementing AWS WAF with ALB to Block SQL Injection, Geo-Location, and Query String Attacks

## 🧭 Overview

This lab demonstrates how to secure web applications using **AWS WAF** (Web Application Firewall) integrated with an **Application Load Balancer (ALB)**. The WAF will be configured to block:
- **SQL injection** attempts
- Requests from specified **geographic locations**
- Requests containing specific **query strings**

---

## 🛠️ Steps

### 1. Sign in to the AWS Management Console

---

### 2. Launch the First EC2 Instance
- Navigate to the **EC2** dashboard.
- Click **Launch Instance**.
- Choose an AMI (e.g., Amazon Linux 2).
- Select an instance type (e.g., `t2.micro`).
- Configure:
  - **Network**: Select your default or custom VPC.
  - **Subnet**: Choose one in a public AZ.
  - **Security Group**: Allow HTTP (port 80) and optionally SSH (port 22).
- In **User Data**, install a web server:
  ```bash
  #!/bin/bash
  yum update -y
  yum install -y httpd
  systemctl start httpd
  systemctl enable httpd
  echo "Hello from EC2 Instance 1" > /var/www/html/index.html
  ```

---

### 3. Launch the Second EC2 Instance
- Repeat the previous step.
- Update the User Data for distinction:
  ```bash
  echo "Hello from EC2 Instance 2" > /var/www/html/index.html
  ```

---

### 4. Create a Target Group
- Go to **EC2 > Target Groups**.
- Click **Create target group**.
- Target type: `Instances`
- Protocol: HTTP, Port: 80
- Register the two EC2 instances.
- Set **Health Checks** to use path `/`.

---

### 5. Create an Application Load Balancer (ALB)
- Go to **Load Balancers** → Click **Create Load Balancer** → Choose **Application Load Balancer**.
- Name: `MyAppLoadBalancer`
- Scheme: Internet-facing
- Listeners: HTTP (port 80)
- Availability Zones: Select at least two
- Associate the target group created earlier.

---

### 6. Test the Load Balancer
- After provisioning, find the **DNS name** of the Load Balancer.
- Open it in a browser. You should see responses from EC2 Instance 1 or 2 alternately.
- If not working, verify SG rules, instance health, and user data setup.

---

### 7. Create a WAF Web ACL
- Navigate to **AWS WAF & Shield** → **Web ACLs** → Click **Create Web ACL**.
- **Name**: `MyWebACL`
- **Scope**: Regional
- **Resource**: Select the Application Load Balancer
- Add the following rules:

  #### a. Block SQL Injection
  - Use the **Managed Rule Group**: `AWSManagedRulesSQLiRuleSet`
  - Action: Block

  #### b. Block Specific Geo Locations
  - Add a **Geo match rule**
  - Select one or more countries to block
  - Action: Block

  #### c. Block Requests Based on Query Strings
  - Add a **custom rule**
  - Match part of the request: `Query string`
  - Match type: `String match` or `Regex`
  - Define suspicious query string values (e.g., `?debug=true`, `?token=xyz`)
  - Action: Block

---

### 8. Associate the Web ACL with the Load Balancer
- During Web ACL creation or afterwards:
  - Go to the **Web ACL**
  - Under **Associated AWS resources**, choose your ALB
  - Click **Associate**

---

### 9. Test the Configuration
- Open the Load Balancer DNS again:
  - Verify that **normal requests** are allowed
  - Attempt requests with:
    - SQL keywords like `' OR 1=1`
    - IP from blocked countries (via VPN or a proxy)
    - Suspicious query strings
- Ensure the correct responses (blocked/allowed) as expected.

---

## ✅ Result

- Successfully deployed a secured web application with EC2 + ALB + AWS WAF.
- Implemented WAF rules to defend against:
  - SQL injection
  - Geo-based threats
  - Malicious or suspicious query strings
- Verified that traffic is filtered correctly by WAF.
