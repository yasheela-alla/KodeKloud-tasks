# Task 1: Create an RSA Key Pair (`datacenter-kp`) in AWS (us-east-1)

This guide covers multiple approaches:

1. AWS Management Console
2. AWS CLI (using provided temporary credentials)
3. Terraform configuration
4. CloudFormation (optional alternative)

Use whichever method fits your workflow or automation style.

use the credentials given in task!

---
<img width="664" height="850" alt="Screenshot 2025-12-02 004710" src="https://github.com/user-attachments/assets/128ef663-6921-4c33-86cf-263bf1e2770a" />

---

## 1. Create Key Pair via AWS Management Console

### Steps

1. Log in to the AWS console using the provided URL and credentials.
2. Ensure your region is set to **N. Virginia (us-east-1)**.
3. Navigate to:
   EC2 Dashboard → Network & Security → **Key Pairs**
4. Select **Create key pair**.
5. Configure the key pair:

   * **Name**: `datacenter-kp`
   * **Key pair type**: **RSA**
   * **Private key file format**: choose `.pem` (used commonly for SSH)
6. Select **Create key pair**.
7. The `.pem` file downloads automatically. Store it securely.

<img width="1919" height="649" alt="Screenshot 2025-12-02 004921" src="https://github.com/user-attachments/assets/71250fe5-329a-4ff2-a98a-38d0830fb6aa" />

---
<img width="1714" height="936" alt="Screenshot 2025-12-02 005147" src="https://github.com/user-attachments/assets/3887bb66-19e0-4ad7-b5b9-db4dc2451b09" />

---

## 2. Create Key Pair via AWS CLI

You will need to configure the AWS CLI on the **aws-client** host using `aws configure` or exporting environment variables.

### Step 1: Show provided credentials (in lab)

```
showcreds
```

### Step 2: Export the credentials (recommended for temporary labs)

```bash
export AWS_ACCESS_KEY_ID="<access-key>"
export AWS_SECRET_ACCESS_KEY="<secret-key>"
export AWS_SESSION_TOKEN="<token>"
export AWS_DEFAULT_REGION="us-east-1"
```

### Step 3: Create the key pair

```bash
aws ec2 create-key-pair \
    --key-name datacenter-kp \
    --key-type rsa \
    --query "KeyMaterial" \
    --output text > datacenter-kp.pem
```

### Step 4: Secure the key

```bash
chmod 600 datacenter-kp.pem
```

<img width="1035" height="805" alt="Screenshot 2025-12-02 005653" src="https://github.com/user-attachments/assets/0a2e3c4a-cf83-4421-bd04-f0a76f67f853" />

The key is now ready for SSH access to EC2 instances.

<img width="1310" height="320" alt="Screenshot 2025-12-02 005710" src="https://github.com/user-attachments/assets/0ca31212-95ac-4309-b086-1d48cfd48a27" />

---

## 3. Create Key Pair via Terraform

Useful for IaC-driven environments.

### `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_key_pair" "datacenter" {
  key_name   = "datacenter-kp"
  key_type   = "rsa"
  public_key = file("~/.ssh/datacenter-kp.pub")
}
```

### Steps

1. Create an RSA key locally:

   ```bash
   ssh-keygen -t rsa -b 4096 -f ~/.ssh/datacenter-kp
   ```

   <img width="938" height="1032" alt="Screenshot 2025-12-02 010714" src="https://github.com/user-attachments/assets/488aaeae-6df6-4714-b361-a908ebd9d949" />

3. Initialize and apply:

   ```bash
   terraform init
   terraform apply
   ```
<img width="1057" height="902" alt="Screenshot 2025-12-02 011613" src="https://github.com/user-attachments/assets/32683b3d-1dc1-42a9-9754-a8e1dffcdd5a" />
<img width="860" height="851" alt="Screenshot 2025-12-02 011638" src="https://github.com/user-attachments/assets/5355e2a9-cb3a-43ef-beef-639925846ac9" />
<img width="798" height="876" alt="Screenshot 2025-12-02 011650" src="https://github.com/user-attachments/assets/8a1d194d-d52e-43a5-8a01-6ef17cf5bc5e" />

Terraform uploads the public key to AWS and manages it as part of your IaC lifecycle.

---
ALL THREE KEY PAIRS CREATED!!

---

<img width="1101" height="811" alt="Screenshot 2025-12-02 011850" src="https://github.com/user-attachments/assets/5994488e-95d8-4c15-9297-b1bd66375601" />

---
<img width="1307" height="409" alt="Screenshot 2025-12-02 011901" src="https://github.com/user-attachments/assets/a6d4d85d-fbc3-4d20-af20-23646422f709" />


## 4. Create Key Pair via CloudFormation (i skipped it)

A minimal template if CloudFormation is preferred.

### `keypair.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Create RSA key pair

Resources:
  DataCenterKeyPair:
    Type: AWS::EC2::KeyPair
    Properties:
      KeyName: datacenter-kp
      KeyType: rsa
```

### Deploy

```bash
aws cloudformation deploy \
  --stack-name create-datacenter-kp \
  --template-file keypair.yaml \
  --capabilities CAPABILITY_IAM
```

---
# Remove extra key pairs!
<img width="795" height="926" alt="Screenshot 2025-12-02 012013" src="https://github.com/user-attachments/assets/af97bfbd-a1ba-4b26-802d-56b68cd96b1c" />

---
<img width="779" height="433" alt="Screenshot 2025-12-02 012055" src="https://github.com/user-attachments/assets/efd097ab-4459-461d-a675-d65fd3ce6bee" />


# Validation

Regardless of the creation method, verify with:

```bash
aws ec2 describe-key-pairs \
    --key-names datacenter-kp \
    --region us-east-1
```

You should see an entry with:

* KeyName: `datacenter-kp`
* KeyType: `rsa`

---

<img width="1769" height="1074" alt="Screenshot 2025-12-02 012130" src="https://github.com/user-attachments/assets/31c3762c-8657-4d03-a3bf-59fb6ed0cdc6" />

---

<img width="978" height="605" alt="Screenshot 2025-12-02 012152" src="https://github.com/user-attachments/assets/f93d5571-058c-4b0e-8dd6-950e2e442185" />


