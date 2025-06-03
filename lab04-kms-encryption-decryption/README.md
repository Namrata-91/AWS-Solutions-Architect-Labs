# Lab 4: Encryption and Decryption Using AWS KMS

## Overview
In this lab, we will explore how to use AWS Key Management Service (KMS) for encrypting and decrypting data. We will create IAM users, set up permissions, create a KMS key, and perform encryption and decryption operations.

## Steps

### 1. Sign in to AWS Management Console
- Access the AWS Management Console and log in with your credentials.

### 2. Create a Group for KMS Users and Attach a Policy
- Navigate to the **IAM** dashboard.
- Click on **User Groups** and then **Create group**.
- Name the group (e.g., `KMSUsers`).
- Attach the following policy to the group:
  - **AWSKeyManagementServicePowerUser** (or a custom policy that grants necessary permissions for KMS).

### 3. Create Two Users for Managing KMS
- In the IAM dashboard, click on **Users** and then **Add user**.
- Create two users (e.g., `KMSUser1` and `KMSUser2`).
- Assign both users to the `KMSUsers` group created earlier.
- Make sure to enable programmatic access to generate access keys.

### 4. Create a KMS Key
- Navigate to the **KMS** dashboard.
- Click on **Customer managed keys** and then **Create key**.
- Select the key type and configure key settings:
  - Define a key alias (e.g., `MyKMSKey`).
  - Set up key administrative and usage permissions (you can allow the `KMSUsers` group).
- Complete the key creation process.

### 5. Launch an EC2 Instance
- Navigate to the **EC2** dashboard.
- Click on **Launch Instance**.
- Choose an Amazon Machine Image (AMI), select an instance type, and configure settings.
- Ensure that the instance has a security group allowing SSH access.

### 6. SSH into EC2 Instance
- Once the EC2 instance is running
  
  1) use SSH to connect to the instance.
  2) First we need to create a file with the name secret.txt , Execute the command
  
      echo “My Secret data” > secret.txt
  
  3) Now that we have created a file secret.txt we need to execute the following configuration command.

  aws configure

  4) Enter the AWS Access key ID and AWS Secret Access Key of the user KeyEncryption that you downloaded
  5) Enter default region as us-east-1
  6) Leave default output as blank and press Enter
  7) Once AWS configure is complete, we need to execute the command for encryption.
  8) Run the following command to encrypt text file but first replace <replace-key-id> with the Key ID copied earlier.

   aws kms encrypt --key-id <replace-key-id> --plaintext fileb://secret.txt --output text --query CiphertextBlob | base64 --decode > encryptedsecret.txt
  
    Note: The following command encrypts data from the "secret.txt" file using AWS Key Management Service (KMS). It specifies the encryption key (--key-id), reads plaintext from the file in binary mode (--plaintext), and retrieves the base64-encoded ciphertext (--output text --query CiphertextBlob). The subsequent Unix shell command decodes the ciphertext and saves the binary data in "encryptedsecret.txt". This process ensures secure encryption using AWS KMS.

  9) We have successfully encrypted our text file. To view the statement execute
    cat encryptedsecret.txt
  10) We are going to decrypt the encrypted file to view the data
   aws kms decrypt --ciphertext-blob fileb://encryptedsecret.txt --output text --query Plaintext | base64 --decode > decryptedsecret.txt

      Note: The following command decrypts data from the "encryptedsecret.txt" file using AWS Key Management Service (KMS). It takes the base64-encoded ciphertext from the file (--ciphertext-blob fileb://encryptedsecret.txt), and the --output text --query Plaintext flags specify that the output should be in plain text, querying and retrieving the plaintext. The Unix shell command then decodes the base64-encoded plaintext and saves the resulting binary data in "decryptedsecret.txt". This process ensures secure decryption of previously encrypted data using AWS KMS.

  11) We have successfully decrypted our text file . To view the statement execute
      
    cat decryptedsecret.txt
  
  12) Run the following command to re-encrypt text file but first replace <replace-key-id> with the Key ID copied earlier

    aws kms encrypt --key-id <replace-key-id> --plaintext fileb://decryptedsecret.txt --output text --query CiphertextBlob > newencryptedsecret.txt

  13) You can check the created files by using command
    
    ls -lrt

  14) You can check the created files by using command
  
   cat newencryptedsecret.txt

  15) We have successfully executed the re-encrypt statement
  16) We need to enable the key rotation of KMS so that they can be periodically changed or in response to a potential leak or compromise. Do replace <replace-key-id> with the Key ID copied earlier.

    aws kms enable-key-rotation --key-id <replace-key-id>

    Note: The following command is an AWS CLI command used to enable key rotation for a specified AWS Key Management Service (KMS) key. The --key-id <replace-key-id> flag identifies the key to which rotation is applied. Enabling key rotation is a security best practice, ensuring that cryptographic keys used for encryption are regularly rotated to enhance overall security. This command, when executed, activates the automatic rotation of the specified KMS key, helping to mitigate potential risks associated with long-term key usage.
    


***KMS*** enforces access control policies to ensure that only authorized individuals or systems can use or manage cryptographic keys. This helps prevent unauthorized access to sensitive information.
