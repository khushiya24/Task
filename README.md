# 📘 AWS ReadOnly Access Automation using CloudFormation & Python

This project automates the provisioning of AWS ReadOnly IAM policies via CloudFormation and retrieves AWS inventory using a Python script running on an IAM-authenticated EC2 instance.

---

## 📁 Project Structure

```
.
├── read_only_access_template.yaml   # CloudFormation template (hosted on S3)
├── fetch_aws_inventory.py           # Python script to fetch AWS resources
└── README.md                        # Documentation file
```

---

## 🚀 1. CloudFormation Template

### ✅ Description
Creates:
- An IAM Group (`ReadOnlyAccessGroup`)
- A Custom Managed Policy (`CustomReadOnlyAccessPolicy`) with full read-only permissions

### 📦 Hosted on S3
URL:  
```
https://my-cloudformation-templates-kb.s3.ap-south-1.amazonaws.com/read_only_access_template.yaml
```

### 🔗 Launch Stack:
Click the link below to launch the CloudFormation stack in your AWS account:

[![Launch Stack](https://docs.aws.amazon.com/cloudformation/assets/cfn-launch-stack.png)](https://ap-south-1.console.aws.amazon.com/cloudformation/home?region=ap-south-1#/stacks/create/template?templateURL=https://my-cloudformation-templates-kb.s3.ap-south-1.amazonaws.com/read_only_access_template.yaml)

---

## 🐍 2. Python Script: `fetch_aws_inventory.py`

### ✅ Purpose
Fetches all EC2 instances and S3 buckets in the account using credentials from the instance profile.

### 🔐 No Access Keys Required
The script uses **instance metadata-based authentication** via the attached IAM role.

### 🧠 Script Logic:
- Connect to AWS CloudFormation
- Fetch resources from the specified stack
- List all EC2 instances and S3 buckets

### ▶️ How to Run:

```bash
python3 fetch_aws_inventory.py
```

### 💡 Sample Output:

```
📦 Fetching resources from stack: ReadOnlyAccessStack
 - AWS::IAM::Group: ReadOnlyAccessGroup
 - AWS::IAM::ManagedPolicy: arn:aws:iam::<account-id>:policy/CustomReadOnlyAccessPolicy

🖥️  Listing EC2 Instances:
 - Instance ID: i-xxxxxxxxxxxxxx, State: running, Type: t2.micro

🪣 Listing S3 Buckets:
 - my-cloudformation-templates-kb
```

---

## ⚙️ 3. EC2 Instance Setup

1. Launch an EC2 instance.
2. Attach the IAM Role created by the CloudFormation stack.
3. Install dependencies:

```bash
sudo apt update
sudo apt install python3-pip unzip -y
pip3 install boto3
```

4. Set region (optional):

```bash
export AWS_DEFAULT_REGION=ap-south-1
```

---

## 📌 Notes

- The IAM role must have at least `ReadOnlyAccess` or the custom policy attached.
- No hardcoded credentials are used anywhere.
