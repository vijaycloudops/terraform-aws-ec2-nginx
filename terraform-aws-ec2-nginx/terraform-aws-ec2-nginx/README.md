# AWS EC2 Provisioning with Terraform

![Terraform](https://img.shields.io/badge/Terraform-IaC-7B42BC?logo=terraform)
![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?logo=amazonaws)
![Nginx](https://img.shields.io/badge/Web_Server-Nginx-009639?logo=nginx)

## Project Overview

This project demonstrates automated AWS infrastructure provisioning using Terraform. It provisions an Amazon Linux EC2 instance within a custom VPC, configures networking and security groups, and automatically installs and starts the Nginx web server using an EC2 user data script.

The project covers key Infrastructure as Code (IaC) concepts, including Terraform configuration, AWS networking, IAM roles, EC2 provisioning, and automated server configuration.

## Architecture

```text
Your browser
    |
    | HTTP :80
    v
Internet Gateway
    |
Public Route Table
    |
Public Subnet (10.0.1.0/24)
    |
Security Group (HTTP :80)
    |
EC2 instance (Amazon Linux 2023)
    |
Nginx web server
```

The EC2 instance has no inbound SSH rule. An IAM instance role is attached for AWS Systems Manager (Session Manager) access, avoiding the need to expose port 22.

## Resources Created

- VPC: `10.0.0.0/16`
- Public subnet: `10.0.1.0/24`
- Internet Gateway and public route table
- Security group allowing inbound HTTP (port 80)
- EC2 instance using Amazon Linux 2023
- IAM role and instance profile for Systems Manager
- Nginx installed using EC2 user data

## Prerequisites

- An AWS account
- Terraform installed
- AWS CLI installed and configured with credentials that can create the resources in this project

Check your AWS identity:

```bash
aws sts get-caller-identity
```

## Deploy

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_GITHUB_USERNAME/terraform-aws-ec2-nginx.git
cd terraform-aws-ec2-nginx
```

### 2. Initialize Terraform

```bash
terraform init
```

### 3. Format and validate

```bash
terraform fmt -recursive
terraform validate
```

### 4. Review the planned changes

```bash
terraform plan
```

### 5. Create the infrastructure

```bash
terraform apply
```

Type `yes` when prompted.

### 6. Open the website

After deployment, Terraform prints the public IP and website URL. Open the `website_url` output in your browser. Nginx may take a minute or two to finish installing.

```bash
terraform output
```

## Connect to the EC2 Instance (Optional)

The instance is configured for AWS Systems Manager Session Manager. Ensure the instance has registered with Systems Manager, then run:

```bash
aws ssm start-session --target <INSTANCE_ID>
```

Get the instance ID with:

```bash
terraform output instance_id
```

Your AWS identity needs permission to start SSM sessions, and the instance needs network access to Systems Manager endpoints.

## Clean Up

To avoid ongoing AWS charges, delete the resources when you finish:

```bash
terraform destroy
```

Type `yes` when prompted.

## Important Notes

- Review the plan before applying it.
- AWS resources can incur charges. Destroy the environment after practice.
- Do not commit AWS access keys, secret keys, Terraform state files, or personal credentials.
- This is a learning/portfolio environment, not a production-hardened architecture.
- The project uses an AWS-managed SSM policy. Confirm the instance appears as managed in Systems Manager before attempting a Session Manager connection.

## Author

**Vijay Kumar Kunja**

GitHub: [vijaycloudops](https://github.com/vijaycloudops)
