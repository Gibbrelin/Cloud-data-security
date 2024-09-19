# Cloud-data-security
README: AWS S3 Bucket, KMS, IAM, and Bucket Policy Setup
This guide will walk you through the process of:

Creating an S3 bucket.
Uploading an object to an S3 bucket.
Creating a Customer Master Key (CMK) in AWS KMS.
Enabling Key Rotation for KMS keys.
Encrypting and decrypting data using KMS.
Creating an IAM policy and attaching it to a user.
Creating a bucket policy with IP restrictions.
Prerequisites
Before you begin, ensure that you have the following:

AWS CLI installed and configured on your local machine.
Appropriate IAM permissions to create S3 buckets, KMS keys, IAM policies, and manage bucket policies.
1. Creating an S3 Bucket
AWS Management Console
Log in to your AWS Management Console.
Navigate to S3.
Click Create bucket.
Provide a unique bucket name.
Choose a region (preferably where you plan to use the bucket).
Set the configurations for public access, versioning, and encryption (as needed).
Click Create bucket.
AWS CLI
bash
Copy code
aws s3 mb s3://my-bucket-name --region us-east-1
Replace my-bucket-name with your desired bucket name and us-east-1 with your preferred region.

2. Uploading an Object to S3 Bucket
AWS Management Console
Go to S3 and open your bucket.
Click Upload.
Choose a file to upload.
Configure storage class, permissions, and encryption if needed.
Click Upload.
AWS CLI
bash
Copy code
aws s3 cp myfile.txt s3://my-bucket-name/
Replace myfile.txt with the file you want to upload and my-bucket-name with your bucket name.

3. Creating a Customer Master Key (CMK) in AWS KMS
AWS Management Console
Open the KMS Console.
Click Create Key.
Select Symmetric key type and click Next.
Add an alias for the key (e.g., my-cmk).
Configure key administrative permissions.
Define usage permissions for IAM users.
Click Create key.
AWS CLI
bash
Copy code
aws kms create-key --description "My KMS key" --key-usage ENCRYPT_DECRYPT
4. Enabling Key Rotation for KMS Key
AWS Management Console
Go to KMS Console.
Select your key.
Click Key Actions and select Enable key rotation.
AWS CLI
bash
Copy code
aws kms enable-key-rotation --key-id <key-id>
Replace <key-id> with the Key ID of your KMS key.

5. Encrypting and Decrypting Data Using KMS
Encrypting a File
AWS CLI
bash
Copy code
aws kms encrypt --key-id <key-id> --plaintext fileb://myfile.txt --output text --query CiphertextBlob | base64 --decode > myfile.enc
Decrypting a File
AWS CLI
bash
Copy code
aws kms decrypt --ciphertext-blob fileb://myfile.enc --output text --query Plaintext | base64 --decode > myfile-decrypted.txt
6. Creating an IAM Policy and Attaching It to a User
Creating an IAM Policy
AWS Management Console
Go to IAM and select Policies.
Click Create Policy.
Choose JSON and input your policy. Example policy granting full access to S3:
json
Copy code
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*"
    }
  ]
}
Click Review Policy.
Give the policy a name and description.
Click Create Policy.
AWS CLI
Create a JSON file (s3-policy.json) with the policy content, and then use the following command:

bash
Copy code
aws iam create-policy --policy-name MyS3FullAccess --policy-document file://s3-policy.json
Attaching IAM Policy to a User
AWS Management Console
Go to IAM and select Users.
Choose the user.
Go to the Permissions tab and click Add Permissions.
Choose Attach existing policies directly.
Select the policy you created and click Next: Review.
Click Add Permissions.
AWS CLI
bash
Copy code
aws iam attach-user-policy --policy-arn arn:aws:iam::<aws-account-id>:policy/MyS3FullAccess --user-name my-user
7. Creating a Bucket Policy for IP Restrictions
Example S3 Bucket Policy for IP Restriction
This policy denies access to the S3 bucket from any IP address other than 203.0.113.0/24.

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
Applying the Bucket Policy
AWS Management Console
Navigate to S3 and select your bucket.
Go to the Permissions tab.
Scroll to Bucket Policy and click Edit.
Paste the policy JSON and click Save.
AWS CLI
Create a file (bucket-policy.json) with the policy and run:

bash
Copy code
aws s3api put-bucket-policy --bucket my-bucket-name --policy file://bucket-policy.json
Conclusion
You have successfully set up an S3 bucket, uploaded objects, created a KMS key with encryption capabilities, created and attached IAM policies, and applied IP restrictions to your S3 bucket.

For further information, refer to the official AWS documentation:

Amazon S3 Documentation
AWS KMS Documentation
IAM Documentation
