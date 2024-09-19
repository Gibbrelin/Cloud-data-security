# Cloud-data-security
# AWS S3 Bucket, KMS, IAM, and Bucket Policy Setup

This guide will walk you through the process of:

. Creating an S3 bucket.

. Uploading an object to an S3 bucket.

. Creating a Customer Master Key (CMK) in AWS KMS.

. Enabling Key Rotation for KMS keys.

. Encrypting and decrypting data using KMS.

. Creating an IAM policy and attaching it to a user.

. Creating a bucket policy with IP restrictions.

# Prerequisites

Before you begin, ensure that you have the following:

AWS CLI installed and configured on your local machine.

Appropriate IAM permissions to create S3 buckets, KMS keys, IAM policies, and manage bucket policies.

# 1. Creating an S3 Bucket

AWS Management Console

. Log in to your AWS Management Console.
![Screenshot 2024-09-19 173652](https://github.com/user-attachments/assets/051feb14-f0e7-4db3-9062-2a2776944706)

. Navigate to S3.
![Screenshot 2024-09-18 144958](https://github.com/user-attachments/assets/33aa478c-e79d-4b90-89d5-33053406dcd8)

. Click Create bucket.

. Provide a unique bucket name.
![Screenshot 2024-09-18 145103](https://github.com/user-attachments/assets/1cabf6aa-d742-46b7-be0d-26806479ee59)

. Choose a region (preferably where you plan to use the bucket).

. Set the configurations for public access, versioning, and encryption (as needed).
![Screenshot 2024-09-18 145214](https://github.com/user-attachments/assets/9ade9570-5406-40b1-bad7-d9442006fb9e)
![Screenshot 2024-09-18 145228](https://github.com/user-attachments/assets/81021cff-cf57-463a-92aa-9b64627c3d8b)
![Screenshot 2024-09-18 145307](https://github.com/user-attachments/assets/63b00937-ac93-432a-a4b7-9a7099991825)


. Click Create bucket.

# 2. Uploading an Object to S3 Bucket

AWS Management Console
![Screenshot 2024-09-19 173652](https://github.com/user-attachments/assets/051feb14-f0e7-4db3-9062-2a2776944706)

. Go to S3 and open your bucket.
![Screenshot 2024-09-18 150414](https://github.com/user-attachments/assets/6a796dee-f649-4ed7-baef-9d6b1c1b2772)
![Screenshot 2024-09-18 150436](https://github.com/user-attachments/assets/8597c619-e275-4351-ab87-f14ced287f28)

. Click Upload.

. Choose a file to upload.
![Screenshot 2024-09-18 150503](https://github.com/user-attachments/assets/ebc71f0e-b5ab-462c-9bb6-e907d02dd753)

. Configure storage class, permissions, and encryption if needed.
![Screenshot 2024-09-18 150648](https://github.com/user-attachments/assets/2c3b1c1e-5934-4807-8b7d-a1355b2c7ba1)

. Click Upload.

# 3. Creating a Customer Master Key (CMK) in AWS KMS

AWS Management Console
![Screenshot 2024-09-19 173652](https://github.com/user-attachments/assets/051feb14-f0e7-4db3-9062-2a2776944706)

. Open the KMS Console.
![Screenshot 2024-09-19 081134](https://github.com/user-attachments/assets/1e09ba16-edc9-4ae5-9264-493a480a56a9)

. Click Create Key.

. Select Symmetric key type and click Next.
![Screenshot 2024-09-19 081214](https://github.com/user-attachments/assets/232fc64c-b958-4311-b46b-db7b55c8a908)

. Add an alias for the key (e.g., my-cmk).
![Screenshot 2024-09-19 081308](https://github.com/user-attachments/assets/e0fe8880-becf-41ec-9d1b-373dcb640a6e)

. Configure key administrative permissions.

. Define usage permissions for IAM users.

. Click Create key.

# 4. Enabling Key Rotation for KMS Key

AWS Management Console

. Go to KMS Console.

. Select your key.

. Click Key Actions and select Enable key rotation.

# 5. Encrypting and Decrypting Data Using KMS

Encrypting a File

AWS CLI

aws kms encrypt --key-id <key-id> --plaintext fileb://myfile.txt --output text --query CiphertextBlob | base64 --decode > myfile.enc

Decrypting a File

AWS CLI

aws kms decrypt --ciphertext-blob fileb://myfile.enc --output text --query Plaintext | base64 --decode > myfile-decrypted.txt

# 6. Creating an IAM Policy and Attaching It to a User

**Creating an IAM Policy**

AWS Management Console

. Go to IAM and select Policies.

. Click Create Policy.

. Choose your preffered policy editor (Visuals or json)

. Add preffered policy

. Click Review Policy.

. Give the policy a name and description.

. Click Create Policy.


**Attaching IAM Policy to a User**

AWS Management Console

. Go to IAM and select Users.

. Choose the user.

. Go to the Permissions tab and click Add Permissions.

. Choose Attach existing policies directly.

. Select the policy you created and click Next: Review.

. Click Add Permissions.

# 7. Creating a Bucket Policy for IP Restrictions

. Example S3 Bucket Policy for IP Restriction

. This policy denies access to the S3 bucket from any IP address other than 203.0.113.0/24.

json
Copy code
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::my-bucket-name",
        "arn:aws:s3:::my-bucket-name/*"
      ],
      "Condition": {
        "NotIpAddress": {
          "aws:SourceIp": "203.0.113.0/24"
        }
      }
    }
  ]
}

**Applying the Bucket Policy**

AWS Management Console

. Navigate to S3 and select your bucket.

. Go to the Permissions tab.

. Scroll to Bucket Policy and click Edit.

. Paste the policy JSON and click Save.

# Conclusion
You have successfully set up an S3 bucket, uploaded objects, created a KMS key with encryption capabilities, created and attached IAM policies, and applied IP restrictions to your S3 bucket.

For further information, refer to the official AWS documentation:

Amazon S3 Documentation
AWS KMS Documentation
IAM Documentation

# Best practices for cloud data security

. Data Encryption: Encrypt data both at rest and in transit to prevent unauthorized access.

. Access Control: Implement strong identity and access management (IAM) policies to ensure only authorized users can access sensitive data.

. Multi-Factor Authentication (MFA): Require MFA for user access to critical systems to add an extra layer of security.

. Regular Auditing & Monitoring: Continuously audit and monitor cloud activity to detect and respond to suspicious behavior or security breaches.

. Data Backups: Regularly back up data to prevent data loss due to accidental deletion or ransomware attacks.

. Data Loss Prevention (DLP): Implement DLP tools to identify and prevent the transfer of sensitive information outside of authorized channels.
