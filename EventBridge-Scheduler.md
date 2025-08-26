## 🕓 Schedule Lambda Execution with EventBridge

To automatically run your Lambda function at a specific time (e.g., 1 hour later), follow these steps to create an **EventBridge Schedule**.

---

### ✅ Step 1: Create a Schedule in EventBridge

1. Go to the **AWS Console** → **Amazon EventBridge Scheduler**
2. Click **“Create schedule”**
3. Choose:
   - **Schedule type**: One-time schedule
   - **Name**: e.g., `detach-policy-schedule`
   - **Date and time**: Set the specific time when the Lambda should run (UTC)

---

### ✅ Step 2: Configure Target (Lambda Function)

1. For **Target type**, choose: `Lambda function`
2. For **Function**, select the Lambda you created earlier (e.g., `detach-policy`)
3. Under **Input**, paste the following JSON (your test values):
```json
{
   "user_name": "shaghayegh",
  "policy_arn": "arn:aws:iam::922557514172:policy/Test"
}
```
---

### ✅ Step 3: Grant Permission to EventBridge Scheduler Role

After creating the schedule, you need to give EventBridge permission to detach the IAM policy on your behalf (since it's the one invoking the Lambda function).

1. In the **schedule summary**, find the **IAM role name** associated with your scheduler  
   (It will look something like `Amazon_EventBridge_Invoke_Lambda_<random>`)

2. Go to **IAM > Roles** in the AWS Console

3. Search for and click on the scheduler role

4. Click **Add permissions** → **Create inline policy** (or attach a new one)

5. In the **JSON tab**, paste the following policy:

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

