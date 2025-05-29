# Lab 01: Discover Sensitive Data in S3 Using Amazon Macie

## 🧠 What is Amazon Macie?

Amazon Macie is a data security service that uses pattern matching and machine learning to:
- Automatically discover and protect sensitive data in Amazon S3
- Detect personally identifiable information (PII), including:
  - Names, addresses, credit card numbers, email IDs, etc.
- Provide visibility into S3 bucket security configurations:
  - Public access
  - Unencrypted buckets
  - Buckets shared across accounts

> **Note**: Macie offers a 30-day free trial for **S3 bucket evaluation and monitoring**, but **sensitive data discovery** may incur charges beyond the free tier.

---

## 🧪 Lab Objective

Set up and run an Amazon Macie job to detect sensitive data stored in S3 buckets and review findings.

---

## 📝 Steps

1. **Sign in** to the AWS Management Console.
2. Navigate to **Amazon Macie**.
3. Enable Macie for your AWS account:
   - Accept service-linked roles if prompted.
4. Create a **Macie Job**:
   - Select one or more S3 buckets to scan.
   - Choose types of sensitive data to discover (e.g., PII).
   - Configure job settings (e.g., one-time or scheduled).
5. **Run the Job**:
   - Wait for Macie to analyze the data.
6. **Review Findings**:
   - Go to the **Findings dashboard** to view:
     - Sensitive data detected
     - Bucket-level security insights

---

## ✅ Result

Successfully:
- Enabled Macie and ran a discovery job
- Detected sensitive data types (e.g., PII) within S3 objects
- Identified security risks like public or unencrypted buckets

---

## 📘 Learning Outcome

- Gained practical experience using Amazon Macie to protect S3 data
- Understood its relevance for **compliance, auditing, and data protection**
