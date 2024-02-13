# Seamless Data Replication For Data Sharing: Integrating Data with DynamoDB via S3 Bucket Across Regions

## Abstract
The following architecture describes a seamless data replication process that integrates data with DynamoDB via S3 bucket across different AWS regions. This process enables efficient storage of data in NoSQL format using DynamoDB. When a file is uploaded to the S3 bucket in the "nv-us-east-1" region, it triggers a Lambda function that stores the data in both DynamoDB and another S3 bucket in the "Mumbai-ap-south-1" region. Another Lambda function in the Mumbai region is triggered to store the data in DynamoDB as well.

Additionally, when data is uploaded, information is sent to the user's email through SNS via Email. The uploaded data is also stored in a GitHub repository using an EC2 instance within the VPC of "nv-us-east-1" region with IAM role permissions for S3 full access and DynamoDB full access. The EC2 instance creates a file and loads it into the S3 bucket.

Moreover, the existing data in the S3 bucket is used to load into the database, avoiding the need to create and insert data separately. The data in the GitHub repository can also be pulled into the S3 bucket. The entire process is monitored by CloudWatch, which is in another IAM user. CloudWatch sends an email to the subscriber when certain constraints are reached.

## Steps to Replicate Data Seamlessly

1. **Create S3 Buckets**: Create two S3 buckets, one in the "nv-us-east-1" region and another in the "Mumbai-ap-south-1" region. These buckets will store the uploaded data and facilitate seamless data replication.

2. **Set Up IAM Roles**: Create IAM roles with permissions for both Lambda functions, EC2 instance, and CloudWatch. Ensure that the roles have S3 full access and DynamoDB full access permissions as needed.

3. **Configure Lambda Functions**:
   - Create two Lambda functions, one for each region.
   - Configure the "nv-us-east-1" Lambda function to trigger on S3 bucket uploads.
   - Use the Boto3 library in the Lambda function to store data in both DynamoDB and the "Mumbai-ap-south-1" S3 bucket.
   - Similarly, configure the Lambda function in the "Mumbai-ap-south-1" region to store data in DynamoDB.

4. **Set Up SNS Topic**:
   - Create an SNS topic to send notifications to users when data is uploaded or constraints are reached.
   - Configure the SNS topic to send notifications via email.

5. **Enable S3 Event Notifications**:
   - Configure event notifications on the "nv-us-east-1" S3 bucket to trigger the Lambda function when new objects are uploaded.
   - Similarly, configure event notifications on the "Mumbai-ap-south-1" S3 bucket to trigger the Lambda function in that region.

6. **Create EC2 Instance**:
   - Launch an EC2 instance within the VPC of the "nv-us-east-1" region.
   - Use the IAM role with S3 full access and DynamoDB full access permissions.
   - Install necessary dependencies like Git and Boto3 on the EC2 instance.

7. **Pull Data from GitHub**:
   - Use the EC2 instance to pull data from the GitHub repository.
   - Create a file containing the data to be uploaded.

8. **Load Data into S3 Bucket**:
   - Use the Boto3 library in the EC2 instance to upload the file into the "nv-us-east-1" S3 bucket.

9. **Utilize Existing Data for Database Loading**:
   - Configure the Lambda functions to use existing data from the S3 bucket to load into DynamoDB.
   - Avoid creating and inserting data separately by utilizing the data in the S3 bucket.

10. **Monitor with CloudWatch**:
   - Set up CloudWatch to monitor the entire process.
   - Define specific constraints for CloudWatch to trigger email notifications to the subscriber.

## Conclusion
The seamless data replication architecture ensures that data uploaded to the "nv-us-east-1" S3 bucket is efficiently stored in DynamoDB and another S3 bucket in the "Mumbai-ap-south-1" region. Email notifications are sent to users, and data is also pulled from the GitHub repository and loaded into the S3 bucket. The process is closely monitored using CloudWatch to ensure smooth and reliable data replication.

For further details on implementation and specific code snippets, refer to the associated code repository and documentation.
