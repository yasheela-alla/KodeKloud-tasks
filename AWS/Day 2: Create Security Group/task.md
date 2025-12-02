# Task 2: Create `datacenter-sg` Security Group in Default VPC (us-east-1)

This task requires creating a security group in the **default VPC** with two inbound rules allowing HTTP (80) and SSH (22) from `0.0.0.0/0`.
<img width="1685" height="923" alt="Screenshot 2025-12-02 225312" src="https://github.com/user-attachments/assets/61431812-5369-4312-8bb3-d3083b6623e1" />

This guide covers:

1. AWS Management Console
2. AWS CLI
3. Terraform

---

## 1. Create the Security Group using AWS Management Console

### Steps

1. Log in to the AWS console using the provided URL and credentials.
2. Confirm the region is **N. Virginia (us-east-1)**.
3. Navigate to:
   EC2 Dashboard → Network & Security → **Security Groups**
4. Select **Create security group**.
5. Fill in security group details:
<img width="1599" height="760" alt="Screenshot 2025-12-02 225541" src="https://github.com/user-attachments/assets/777ed76d-1d5a-4b36-af32-7943c2ad1c03" />

   * **Security group name**: `datacenter-sg`
   * **Description**: `Security group for Nautilus App Servers`
   * **VPC**: Select the **default VPC**
6. Add inbound rules:

   | Type | Protocol | Port | Source    |
   | ---- | -------- | ---- | --------- |
   | HTTP | TCP      | 80   | 0.0.0.0/0 |
   | SSH  | TCP      | 22   | 0.0.0.0/0 |
7. Create the security group.
<img width="1620" height="794" alt="Screenshot 2025-12-02 225618" src="https://github.com/user-attachments/assets/e86c2ec2-89db-4a73-b5b2-3c92e1a8dfaf" />
<img width="1695" height="904" alt="Screenshot 2025-12-02 225824" src="https://github.com/user-attachments/assets/ee48c7bd-3c61-4ed9-9ad6-2167d4e62899" />

---

## 2. Create the Security Group using AWS CLI

### Step 1: Export credentials on the aws-client host

```bash
export AWS_ACCESS_KEY_ID="<access-key>"
export AWS_SECRET_ACCESS_KEY="<secret-key>"
export AWS_SESSION_TOKEN="<session-token>"
export AWS_DEFAULT_REGION="us-east-1"
```

### Step 2: Retrieve the default VPC ID

```bash
aws ec2 describe-vpcs --filters Name=isDefault,Values=true --query "Vpcs[0].VpcId" --output text
```

Store the VPC ID in a variable:

```bash
VPC_ID=$(aws ec2 describe-vpcs \
  --filters Name=isDefault,Values=true \
  --query "Vpcs[0].VpcId" \
  --output text)
```

### Step 3: Create the security group

```bash
SG_ID=$(aws ec2 create-security-group \
  --group-name datacenter-sg \
  --description "Security group for Nautilus App Servers" \
  --vpc-id $VPC_ID \
  --query "GroupId" \
  --output text)
```

<img width="1337" height="631" alt="Screenshot 2025-12-02 231751" src="https://github.com/user-attachments/assets/505b012b-1bfc-4701-a2c9-075ad5ac7526" />


### Step 4: Add inbound rules

HTTP rule:

```bash
aws ec2 authorize-security-group-ingress \
  --group-id $SG_ID \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0
```

SSH rule:

```bash
aws ec2 authorize-security-group-ingress \
  --group-id $SG_ID \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0
```

### Step 5: Validate

```bash
aws ec2 describe-security-groups --group-ids $SG_ID
```
<img width="1223" height="782" alt="Screenshot 2025-12-02 231826" src="https://github.com/user-attachments/assets/2ecb1b99-0499-4377-9c3b-4989ca6af707" />
<img width="825" height="377" alt="Screenshot 2025-12-02 231913" src="https://github.com/user-attachments/assets/3695133f-4ddb-46de-9e6a-2dbbe2d5ff5d" />

---

## 3. Create the Security Group using Terraform

### `main.tf`

<img width="600" height="999" alt="Screenshot 2025-12-02 232220" src="https://github.com/user-attachments/assets/bd82bfed-c309-4b0c-be4d-996e5598a00e" />


```hcl
provider "aws" {
  region = "us-east-1"
}

data "aws_vpc" "default" {
  default = true
}

resource "aws_security_group" "datacenter" {
  name        = "datacenter-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "Allow HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

### Deploy

```bash
terraform init
terraform apply
```
<img width="988" height="913" alt="Screenshot 2025-12-02 232734" src="https://github.com/user-attachments/assets/da4f6262-b876-4bea-a294-3ede9a6f367a" />

<img width="1742" height="1036" alt="Screenshot 2025-12-02 233414" src="https://github.com/user-attachments/assets/13d665fb-9271-4c52-af4f-68ca19ada83c" />


## Create the Security Group using CloudFormation

### `security-group.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Security group for Nautilus App Servers

Resources:
  DataCenterSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupName: datacenter-sg
      GroupDescription: Security group for Nautilus App Servers
      VpcId:
        Fn::ImportValue: DefaultVPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
```

If you don't have the default VPC exported, replace the `VpcId` property manually with the VPC ID retrieved earlier.

### Deploy

```bash
aws cloudformation deploy \
  --stack-name datacenter-sg-stack \
  --template-file security-group.yaml
```

---

# Validation

Verify the security group exists in us-east-1:

```bash
aws ec2 describe-security-groups \
  --filters Name=group-name,Values=datacenter-sg \
  --query "SecurityGroups[*].[GroupName,Description,VpcId]"
```
<img width="915" height="313" alt="Screenshot 2025-12-02 233200" src="https://github.com/user-attachments/assets/d5aa633d-3c5e-4709-a849-df62cbd0c6fd" />

You should see:


<img width="1707" height="922" alt="Screenshot 2025-12-02 233137" src="https://github.com/user-attachments/assets/0b0b1491-4faf-4194-bd8a-3628e84d0c77" />


* Group name: `datacenter-sg`
* Description: `Security group for Nautilus App Servers`
* Inbound rules on ports 80 and 22 from `0.0.0.0/0`

---

<img width="988" height="629" alt="Screenshot 2025-12-02 233452" src="https://github.com/user-attachments/assets/dc65462a-f999-4e1e-a737-bf8aa6e98886" />

