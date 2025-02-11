# AWS Infrastructure Automation with Terraform

## Overview
This project demonstrates how to use Terraform to automate the deployment of AWS resources, including an EC2 instance, an S3 bucket, and a VPC with subnets and security groups.

## Prerequisites
Before proceeding, ensure you have the following installed:

- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [AWS CLI](https://aws.amazon.com/cli/)
- AWS IAM user with necessary permissions (EC2, S3, VPC)

## Project Setup

### 1. Configure AWS Credentials
Ensure your AWS credentials are configured locally using:
```sh
aws configure
```

### 2. Initialize Terraform
Run the following command to initialize Terraform in your project directory:
```sh
terraform init
```

### 3. Define Terraform Configuration Files
Create the following `.tf` files to define resources:

#### `provider.tf`
```hcl
provider "aws" {
  region = "us-east-1"  # Change as needed
}
```

#### `vpc.tf`
```hcl
resource "aws_vpc" "my_vpc" {
  cidr_block = "10.0.0.0/16"
}
```

#### `subnet.tf`
```hcl
resource "aws_subnet" "my_subnet" {
  vpc_id            = aws_vpc.my_vpc.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
}
```

#### `security_group.tf`
```hcl
resource "aws_security_group" "my_sg" {
  vpc_id = aws_vpc.my_vpc.id
  
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

#### `ec2.tf`
```hcl
resource "aws_instance" "my_ec2" {
  ami           = "ami-0c55b159cbfafe1f0"  # Update with latest AMI ID
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.my_subnet.id
  security_groups = [aws_security_group.my_sg.name]
}
```

#### `s3.tf`
```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-terraform-bucket-12345"
  acl    = "private"
}
```

### 4. Apply Terraform Configuration
Run the following commands to create the infrastructure:
```sh
terraform plan
terraform apply -auto-approve
```

### 5. Destroy Resources (If Needed)
To delete all created resources:
```sh
terraform destroy -auto-approve
```

## Conclusion
This project automates the provisioning of an AWS EC2 instance, an S3 bucket, and a VPC with subnets and security groups using Terraform. You can modify the `.tf` files to extend the infrastructure as needed.

## Next Steps
- Implement additional AWS services like RDS, Lambda, or API Gateway.
- Configure Terraform state management using AWS S3 backend.
- Automate deployments using CI/CD with Terraform Cloud or GitHub Actions.

