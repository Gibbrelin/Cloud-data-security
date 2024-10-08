# Cloud-data-security
# AWS S3 Bucket, KMS, IAM, and Bucket Policy Setup

This guide will walk you through the process of:

1. Creating an S3 bucket.

2. Uploading an object to an S3 bucket.

3. Creating a Customer Master Key (CMK) in AWS KMS.

4. Enabling Key Rotation for KMS keys.

5. Encrypting and decrypting data using KMS.

6. Creating an IAM policy and attaching it to a user.

7. Creating a bucket policy with IP restrictions.

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
![Screenshot 2024-09-19 081345](https://github.com/user-attachments/assets/94278049-2826-47f4-a9f8-de05cca20eb4)

. Define usage permissions for IAM users.
![Screenshot 2024-09-19 081430](https://github.com/user-attachments/assets/5a364cd5-56c3-47c6-b2fe-b7f9a8e12f43)

. Review credentials and Click Create key.
![Screenshot 2024-09-19 081532](https://github.com/user-attachments/assets/feb7edd9-3173-484d-a7fb-4337fb892e88)

# 4. Enabling Key Rotation for KMS Key

AWS Management Console
![Screenshot 2024-09-19 173652](https://github.com/user-attachments/assets/051feb14-f0e7-4db3-9062-2a2776944706)

. Go to KMS Console.
![Screenshot 2024-09-19 081704](https://github.com/user-attachments/assets/c9fadd90-a5ba-4370-9c8d-88079629e032)

. Select your key.
![Screenshot 2024-09-19 081740](https://github.com/user-attachments/assets/57558cc5-1977-4de5-af73-b15121878118)

. Click on Key Rotation and select Enable key rotation.
![Screenshot 2024-09-19 081836](https://github.com/user-attachments/assets/b0d43ec4-9457-49c3-a4ae-cf9e9fe8bd08)

# 5. Encrypting and Decrypting Data Using KMS
Aws kms key can be used on AWS CLI(command line interface) and in ec2 instance (for this illustration i'll be using an ec2 instance (amazon linux)) to demonstrate the encryption and decryption using AWS CMK (customer master key)

1. Encrypting a File

. Install the AWS CLI or SDK

. Ensure that the AWS CLI is installed on your EC2 instance.

. Configure the CLI with the required permissions using an IAM role that has access to KMS.

           aws configure

![Screenshot 2024-09-19 100952](https://github.com/user-attachments/assets/990b3e97-b48d-4f1c-8573-4c51b58c85a1)

. **Encrypt Data Using AWS KMS with CLI**

You can use the AWS CLI to encrypt a file with a KMS key. Here’s how you can encrypt a file:

    (aws kms encrypt --key-id <key-id> --plaintext fileb://myfile.txt --output text --query CiphertextBlob | base64 --decode > myfile.enc)

![Screenshot 2024-09-19 102826](https://github.com/user-attachments/assets/01f6fccd-70ee-4ab1-b4ee-cfed34bd137a)

In this command:

Replace <KMS-Key-ID-or-ARN> with the ID or ARN of the KMS key.
Replace path_to_your_file with the path to the file you want to encrypt.

. **Decrypt Data Using AWS KMS with CLI**

To decrypt the file, use the following command:

        aws kms decrypt --ciphertext-blob fileb://myfile.enc --output text --query Plaintext | base64 --decode > myfile-decrypted.txt

![Screenshot 2024-09-19 103552](https://github.com/user-attachments/assets/b4f44685-1b9f-4bec-93a5-5a7f2c7d908f)

In this command:

Replace <KMS-Key-ID-or-ARN> with the ID or ARN of the KMS key.
Replace path_to_your_file with the path to the file you want to decrypt.

This will decrypt the file and store the plain text in decrypted_file.


# 6. Creating an IAM Policy and Attaching It to a User

**Creating an IAM Policy**

AWS Management Console

![Screenshot 2024-09-19 173652](https://github.com/user-attachments/assets/051feb14-f0e7-4db3-9062-2a2776944706)

. Go to IAM and select Policies in the left hand side bar.

![Screenshot 2024-09-20 112130](https://github.com/user-attachments/assets/fddc418b-f43e-4143-83fa-286c86661d13)

. Click Create Policy.

![Screenshot 2024-09-19 105544](https://github.com/user-attachments/assets/c1983c93-5b1d-46ba-ae6d-9ce60a8d74b8)

. Choose your preffered policy editor (Visuals or json)

![Screenshot 2024-09-19 105634](https://github.com/user-attachments/assets/fd2a6912-3206-4603-b1d8-7af80665da86)

. Add preffered policy

![Screenshot 2024-09-19 105651](https://github.com/user-attachments/assets/5aa590c3-9f07-48cf-9869-392fb75e7d97)

. Click Review Policy.

. Give the policy a name and description.

![Screenshot 2024-09-19 105900](https://github.com/user-attachments/assets/86c574f3-8148-4717-a4d3-ed1739a1241b)

. Click Create Policy.


**Attaching IAM Policy to a User**

AWS Management Console

. Go to IAM and select Users.

![Screenshot 2024-09-20 112130](https://github.com/user-attachments/assets/7730e3d6-ec51-47e9-ae8e-58dbead664e0)

. Choose the user.

![Screenshot 2024-09-19 110042](https://github.com/user-attachments/assets/6f1e2f9f-3246-4729-9a06-882d53209aef)

. Go to the Permissions tab and click Add Permissions.

![Screenshot 2024-09-19 110108](https://github.com/user-attachments/assets/e45f4c1a-24e6-4a8c-b69e-630b9e8557ac)

. Choose Attach existing policies directly.

![Screenshot 2024-09-19 110127](https://github.com/user-attachments/assets/7a8cbe1d-e33b-4064-af2c-dc3a49cb340d)

. Select the policy you created and click Next: Review.

![Screenshot 2024-09-19 110150](https://github.com/user-attachments/assets/b1510cc1-520c-4fdb-8956-f6d0cb806de4)

. Click Add Permissions.

![Screenshot 2024-09-19 110202](https://github.com/user-attachments/assets/d1421652-26fd-4b45-aa1d-1b52daf09349)

.  Testing the policy
 I logged in into the user account that i have given the policy i created to (The policy has only bucket and object listing permissions) and i tried to create a bucket in the users account but the access was denied 

![Screenshot 2024-09-19 110813](https://github.com/user-attachments/assets/280641fb-8fe2-4e8c-8db3-6631e12fecc9) 

# 7. Creating a Bucket Policy for IP Restrictions

AWS Management Console

. Navigate to S3 and select your bucket.

![Screenshot 2024-09-19 111103](https://github.com/user-attachments/assets/42c2ab89-ed1d-46b8-b9c7-5db8fb6c8667)

. Go to the Permissions tab.

![Screenshot 2024-09-19 113954](https://github.com/user-attachments/assets/57044ad5-3e19-499d-a03a-8ef65bddd99c)

. Scroll to Bucket Policy and click Edit.

![Screenshot 2024-09-20 122207](https://github.com/user-attachments/assets/67321a42-6c35-4c17-89b5-de97ac72f949)

. Paste the policy JSON and click Save.

![Screenshot 2024-09-19 113825](https://github.com/user-attachments/assets/a35ba272-68e6-4a91-8e53-c5a59f41059a)

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
