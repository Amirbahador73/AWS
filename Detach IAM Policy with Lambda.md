# 🛠️ AWS Practice: Automatically Detach IAM Policy with Lambda

This simple step-by-step guide shows how to create an AWS Lambda function that automatically **detaches a managed IAM policy** from a user. This is useful for scenarios like granting temporary admin access and removing it later via automation.

---

## ✅ Step 1: Write the Lambda Function

Create a new Lambda function in Python, and paste the following code:

```python
import boto3

def lambda_handler(event, context):
    boto3.client('iam').detach_user_policy(
        UserName=event.get('user_name'),
        PolicyArn=event.get('policy_arn')
    )
    return "Policy detached"
```

## ✅ Step 2: Grant IAM Permission to the Lambda Role

Your Lambda function needs permission to detach policies. Here's how to give it access:

1. Go to the Configuration tab of your Lambda function.

2. Under Permissions, click on the execution role name.

3. In the IAM console, click “Add permissions” → “Create inline policy” (or attach a new one).

4. Paste the following JSON policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:DetachUserPolicy",
            "Resource": "*"
        }
    ]
}
```

---

## ✅ Step 3: Test the Lambda

1. In the Lambda Console, click Test.

2. Create a new test event and name it (e.g., DetachTest).

3. Paste the following JSON into the test event body:

```json
{
  "user_name": "shaghayegh",
  "policy_arn": "arn:aws:iam::922557514172:policy/Test"
}
```
4. Click Save and then Test.
