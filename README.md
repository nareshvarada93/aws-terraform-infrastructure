

# AWS Infrastructure Automation with Terraform

## Project Overview

This project demonstrates automated AWS infrastructure provisioning using **Terraform**.

The infrastructure is created using Infrastructure as Code (IaC), allowing AWS resources to be deployed consistently through Terraform configuration files instead of creating them manually from the AWS Console.

The project provisions an AWS VPC, public subnet, Internet Gateway, route table, security group, and EC2 web server. The EC2 instance automatically installs and configures Apache using Terraform `user_data`.

## Architecture

```text
                         Internet
                            |
                            v
                  +-------------------+
                  | Internet Gateway  |
                  +-------------------+
                            |
                            v
                  +-------------------+
                  |     AWS VPC       |
                  |   10.0.0.0/16     |
                  |                   |
                  |  +-------------+  |
                  |  |Public Subnet|  |
                  |  |10.0.1.0/24  |  |
                  |  |             |  |
                  |  |    EC2      |  |
                  |  |   Apache    |  |
                  |  +-------------+  |
                  +-------------------+
                            |
                            v
                       Web Browser
                    HTTP Port 80
```

## Technologies Used

* Terraform
* Amazon Web Services (AWS)
* Amazon VPC
* Amazon EC2
* Internet Gateway
* Route Tables
* Security Groups
* Ubuntu 24.04
* Apache Web Server
* AWS CLI
* Git
* GitHub

## AWS Resources Created

### 1. VPC

Creates a VPC with the CIDR block:

```text
10.0.0.0/16
```

### 2. Public Subnet

Creates a public subnet:

```text
10.0.1.0/24
```

The subnet is configured to automatically assign public IP addresses to launched instances.

### 3. Internet Gateway

An Internet Gateway provides internet connectivity between the VPC and the internet.

### 4. Route Table

The public route table contains:

```text
0.0.0.0/0 → Internet Gateway
```

This allows internet-bound traffic from the public subnet.

### 5. Security Group

The web server security group allows:

| Protocol | Port | Purpose |
| -------- | ---: | ------- |
| TCP      |   22 | SSH     |
| TCP      |   80 | HTTP    |

Outbound traffic is allowed.

### 6. EC2 Instance

An Ubuntu 24.04 EC2 instance is deployed inside the public subnet.

Terraform automatically configures the instance using `user_data`.

The script:

```bash
apt-get update -y
apt-get install -y apache2
systemctl start apache2
systemctl enable apache2
```

creates an Apache web server.

## Terraform Project Structure

```text
aws-terraform-infrastructure/
│
├── main.tf
├── provider.tf
├── variables.tf
├── outputs.tf
├── .gitignore
└── README.md
```

### `provider.tf`

Defines the AWS provider and Terraform version requirements.

### `variables.tf`

Contains configurable Terraform variables such as:

```text
aws_region
instance_type
```

### `main.tf`

Contains the AWS infrastructure resources:

* VPC
* Subnet
* Internet Gateway
* Route Table
* Route Table Association
* Security Group
* EC2 Instance

### `outputs.tf`

Displays important infrastructure information such as:

* VPC ID
* Subnet ID
* Internet Gateway ID
* Security Group ID
* EC2 Instance ID
* EC2 Public IP

## Terraform Workflow

The project follows this Terraform workflow:

```text
Write Terraform Configuration
          |
          v
   terraform init
          |
          v
   terraform validate
          |
          v
      terraform fmt
          |
          v
     terraform plan
          |
          v
     terraform apply
          |
          v
     AWS Resources
```

## How to Deploy

### 1. Clone the repository

```bash
git clone https://github.com/nareshvarada93/aws-terraform-infrastructure.git
```

Navigate to the project:

```bash
cd aws-terraform-infrastructure
```

### 2. Initialize Terraform

```bash
terraform init
```

### 3. Format the configuration

```bash
terraform fmt
```

### 4. Validate the configuration

```bash
terraform validate
```

Expected result:

```text
Success! The configuration is valid.
```

### 5. Review the execution plan

```bash
terraform plan
```

### 6. Deploy the infrastructure

```bash
terraform apply
```

Enter:

```text
yes
```

when Terraform asks for confirmation.

## View Infrastructure Outputs

After deployment:

```bash
terraform output
```

To display the EC2 public IP:

```bash
terraform output ec2_public_ip
```

Open the returned IP address in a web browser:

```text
http://YOUR_PUBLIC_IP
```

## Website Result

The EC2 instance automatically installs Apache and creates the following web page:

```text
AWS Infrastructure created using Terraform
```

This confirms that the EC2 instance was successfully provisioned and configured using Terraform.

## Useful Terraform Commands

Initialize Terraform:

```bash
terraform init
```

Format files:

```bash
terraform fmt
```

Validate configuration:

```bash
terraform validate
```

Preview changes:

```bash
terraform plan
```

Create infrastructure:

```bash
terraform apply
```

View outputs:

```bash
terraform output
```

View current state:

```bash
terraform show
```

Destroy infrastructure:

```bash
terraform destroy
```

## Cleanup

When the infrastructure is no longer required, destroy the AWS resources with:

```bash
terraform destroy
```

Enter:

```text
yes
```

Terraform will remove the infrastructure that it manages.

## Learning Outcomes

Through this project, I practiced:

* Infrastructure as Code using Terraform
* AWS VPC networking
* Public subnet configuration
* Internet Gateway configuration
* Route table configuration
* AWS Security Groups
* EC2 provisioning
* Automated server configuration using `user_data`
* Terraform variables and outputs
* AWS CLI usage
* Git and GitHub version control

## Future Improvements

Possible improvements for this project include:

* Terraform modules
* Private subnet architecture
* NAT Gateway
* Remote Terraform state using Amazon S3
* State locking
* GitHub Actions CI/CD
* Automated Terraform validation and planning
* Improved security group rules
* HTTPS configuration
* Load Balancer integration

## Author

**Naresh Varada**

GitHub:
https://github.com/nareshvarada93

Portfolio:
https://nareshvarada-portfolio.netlify.app/
