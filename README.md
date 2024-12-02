# GitHub Actions Terraform AWS VPC Project

## Overview
This project demonstrates how to create a VPC in AWS using Terraform and automate the deployment using GitHub Actions.

## Prerequisites
- AWS account
- GitHub account
- Terraform CLI
- AWS CLI
- GitHub CLI

## Steps
1. Set up your environment
2. Create and apply Terraform configuration
3. Automate with GitHub Actions

### Steps

### 1. Set Up Your Environment

**Install Terraform:**

- Visit the [Terraform download page](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli).
- Download and install Terraform for your operating system.

**Install AWS CLI:**

- Follow the [AWS CLI installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

**Configure AWS CLI:**

```bash
aws configure
AWS Access Key ID [None]: <Your_AWS_Access_Key_ID>
AWS Secret Access Key [None]: <Your_AWS_Secret_Key>
Default region name [None]: us-east-2
Default output format [None]: json
```

**Install GitHub CLI:**

- Follow the [GitHub CLI installation guide](https://cli.github.com/manual/installation).

**Authenticate GitHub CLI:**

```bash
gh auth login
```

### 2. Create a Terraform Configuration

**Initialize a New Terraform Project:**

```bash
mkdir terraform-aws-vpc
cd terraform-aws-vpc
```

**Create `main.tf`:**

```hcl
provider "aws" {
  region = "us-east-2"
  access_key = var.aws_access_key
  secret_key = var.aws_secret_key
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "subnet" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-2a"
}

output "vpc_id" {
  value = aws_vpc.main.id
}

output "subnet_id" {
  value = aws_subnet.subnet.id
}
```

**Create `variables.tf`:**

```hcl
variable "aws_access_key" {
  description = "The AWS access key"
}

variable "aws_secret_key" {
  description = "The AWS secret key"
}
```

**Create `terraform.tfvars`:**

```hcl
aws_access_key = "Your_AWS_Access_Key_ID"
aws_secret_key = "Your_AWS_Secret_Key"
```

### 3. Initialize and Apply the Configuration

**Initialize Terraform:**

```bash
terraform init
```

**Plan the Infrastructure Changes:**

```bash
terraform plan
```

**Apply the Configuration:**

```bash
terraform apply
```
- Review the plan and confirm by typing `yes` when prompted.

### 4. Automate with GitHub Actions

**Create the GitHub Actions directories:**

```bash
mkdir -p .github/workflows
```

**Create `terraform.yml`:**

```yaml
name: 'Terraform AWS VPC'
on:
  push:
    branches:
      - main

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Set up Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Init
        run: terraform init
        working-directory: terraform-aws-vpc

      - name: Terraform Plan
        run: terraform plan
        working-directory: terraform-aws-vpc

      - name: Terraform Apply
        run: terraform apply -auto-approve
        working-directory: terraform-aws-vpc
```

**Commit and Push Your Workflow:**

```bash
git add .github/workflows/terraform.yml
git commit -m "Add GitHub Actions workflow for Terraform AWS VPC"
git push origin main
```

### 5. Monitor the Workflow

**Go to Your Repository on GitHub:**

- Navigate to the Actions tab to monitor the workflow's progress.
- Ensure that the workflow runs and completes successfully.

### 6. Verify Terraform Deployment

**Check AWS Management Console:**

- Log in to your AWS Management Console and navigate to the VPC section to verify that the VPC and subnet are created as expected.

---
   
![image](https://github.com/user-attachments/assets/2a7e78e7-cbce-4449-a86b-8a4c4bbf824e)

## Resources
- [Terraform](https://developer.hashicorp.com/terraform)
- [AWS CLI](https://aws.amazon.com/cli/)
- [GitHub CLI](https://cli.github.com/)

