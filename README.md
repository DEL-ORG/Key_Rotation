DETAILED INSTRUCTION TO GUIDE YOU TOWARDS COMPLETING THIS DEMO

# AWS IAM Key Rotation with Lambda Functions

This project contains two Lambda functions to automate the key rotation process for IAM users:
1. **Key Rotation Creation**: Inactivates existing active keys and creates new keys.
2. **Key Rotation Deletion**: Deletes inactive keys.

## Project Structure

## Prerequisites

- AWS CLI configured with sufficient permissions (IAM and Secrets Manager).
- AWS Lambda with the appropriate execution roles.
- AWS Secrets Manager set up to store IAM credentials for users.

## Setup Instructions

### Step 1: Upload Lambda Functions

1. Go to the AWS Management Console.
2. Navigate to **Lambda**.
3. Create two Lambda functions:
   - One for key creation and inactivation (`key_rotation_create/lambda_function.zip`).
   - One for deleting inactive keys (`key_rotation_del/lambda_function.zip`).
4. Upload the ZIP files for both functions in their respective Lambda functions.

### Step 2: Set Up Secrets in AWS Secrets Manager

1. Go to **Secrets Manager** in AWS Management Console.
2. Create a new secret for each IAM user with the following structure in the **Secret Value**:

   ```json
   {
     "UserName": "IAM_User_Name",
     "AccessKeyId": "AKIA...",
     "SecretAccessKey": "YourSecretKey..."
   }

Replace IAM_User_Name with the actual IAM username.
Replace AccessKeyId and SecretAccessKey with the current key pair for that IAM user.
Save the secret and note the Secret ARN.

### Step 3: Set Up Lambda Environment Variables
For each Lambda function, you need to set up environment variables:

Go to the Configuration section of each Lambda function.
Add a new environment variable:
- Key: secrets
- Value: A semicolon-separated list of Secret ARNs (e.g., arn:aws:secretsmanager:us-east-1:123456789012:secret:MySecret1;arn:aws:secretsmanager:us-east-1:123456789012:secret:MySecret2).

### Step 4: Set Up IAM Roles for Lambda Functions
The Lambda functions require permissions to access IAM and Secrets Manager. Ensure the following permissions are attached to the Lambda execution role:

- secretsmanager:GetSecretValue
- secretsmanager:UpdateSecret
- iam:ListAccessKeys
- iam:UpdateAccessKey
- iam:CreateAccessKey
- iam:DeleteAccessKey

### Step 5: Testing and Invocation
You can invoke the Lambda functions directly using the AWS Management Console or AWS CLI.

## Using AWS CLI:

For the creation/inactivation Lambda function:

`aws lambda invoke --function-name <Your_Lambda_Function_Name_Create> response.json`

Sample Output: The response.json will contain logs such as:

```json
{
"For user - username, Access & Secret keys will be inactivated."
"key has been inactivated."
"A new set of keys has been created for user - username."
}

CloudWatch Logs: You can also check the logs of your Lambda functions in CloudWatch for detailed output.

### Step 6: Verify Key Rotation
Check in Secrets Manager: After invoking the create Lambda, check in AWS Secrets Manager to see if the secret has been updated with new key details.

Check in IAM: Go to the IAM Console and ensure the old keys are inactivated, new keys are created, and inactive keys are deleted.

## Clean Up
Once the project is complete or no longer needed, make sure to clean up the resources by:

1. **Deleting the Lambda functions.
2. **Deleting the secrets in AWS Secrets Manager.
3. **Removing any related IAM policies and roles.
   
## Conclusion
This project provides a demonstration for automating IAM key rotation using Lambda functions and AWS Secrets Manager. You can extend it by scheduling these Lambda functions using Amazon CloudWatch Events to periodically rotate IAM keys for your users.

### Notes for Students

1. **Objective**: This exercise aims to familiarize you with Lambda, IAM, and Secrets Manager services in AWS. You will:
   - Set up Lambda functions.
   - Automate IAM key management.
   - Use Secrets Manager for storing and retrieving IAM user credentials.

2. **Expected Outcome**: After following the steps, you should be able to invoke the Lambda functions to rotate IAM keys and delete inactive ones. You can also monitor logs in CloudWatch to verify that the key rotation was successful.

Let me know if you need further clarification or adjustments to the README file!
