# URL Shortener Infrastructure

Infrastructure as Code (IaC) repository for provisioning and deploying the URL Shortener application on AWS using Terraform.

## Architecture

```text
Terraform
    │
    ▼
AWS EC2
    │
    ▼
User Data Bootstrap Script
    │
    ▼
Clone AWS CodeCommit Repository
    │
    ▼
Execute ec2_bootstrap.sh
    │
    ▼
Docker Compose
    ├── FastAPI API
    ├── PostgreSQL
    ├── Redis
    ├── Kafka
    ├── Analytics Consumer
    ├── Click Counter Consumer
    └── Archive Consumer
```

## Resources Provisioned

Terraform creates the following AWS resources:

- EC2 Instance
- Security Group
- IAM Role
- IAM Instance Profile
- EC2 Key Pair

The EC2 instance automatically:

1. Installs AWS CLI
2. Configures AWS CodeCommit access
3. Clones the application repository
4. Executes the bootstrap script
5. Starts the application stack using Docker Compose

---

## Prerequisites

### Local Machine

Install:

- Terraform
- AWS CLI
- SSH Client

### AWS Permissions

The user executing Terraform must have permissions to:

- Create EC2 Instances
- Create Security Groups
- Create EC2 Key Pairs
- Create IAM Roles
- Create IAM Instance Profiles
- Attach IAM Policies

### Existing AWS Resources

The following must already exist:

- AWS Account
- AWS CodeCommit Repository
- SSH Public Key

---

## Repository Structure

```text
.
├── main.tf
├── variables.tf
├── outputs.tf
├── startup.sh.tpl
├── terraform.tfvars
└── README.md
```

---

## Variables

Example:

```hcl
region         = "ap-south-2"
instance_name  = "url-shortener"
instance_type  = "t3.medium"
repo_name      = "URL-Shortener"
my_ip          = "YOUR_PUBLIC_IP"
```

---

## Deployment Workflow

Terraform provisions:

```text
EC2
Security Group
IAM Role
IAM Instance Profile
SSH Key Pair
```

During EC2 startup:

```text
Install AWS CLI
↓
Configure CodeCommit Credential Helper
↓
Clone Application Repository
↓
Execute ec2_bootstrap.sh
↓
Install Docker
↓
docker compose up
```

---

## Terraform Commands

### Initialize

```bash
terraform init
```

### Format

```bash
terraform fmt
```

### Validate

```bash
terraform validate
```

### Plan

```bash
terraform plan
```

### Apply

```bash
terraform apply
```

### Apply With Variables

```bash
terraform apply \
  -var="instance_type=t3.xlarge"
```

### Auto Approve

```bash
terraform apply -auto-approve
```

### Destroy Infrastructure

```bash
terraform destroy -auto-approve
```

---

## SSH Access

Connect to the provisioned instance:

```bash
ssh -i my-key ubuntu@<PUBLIC_IP>
```

The Security Group restricts SSH access to the IP address specified in:

```hcl
my_ip
```

---

## Security Notes

This project is intended for learning and demonstration purposes.

Current implementation:

- Uses Security Groups for SSH access control
- Uses IAM Roles for CodeCommit authentication
- Uses EC2 User Data for automated deployment

For production deployments consider:

- Remote Terraform State (S3 Backend)
- HTTPS / TLS
- Load Balancer
- Secrets Manager
- Managed Databases (RDS)
- Managed Caching (ElastiCache)
- Monitoring and Alerting

---

## References

### Terraform

https://developer.hashicorp.com/terraform

### AWS Provider

https://registry.terraform.io/providers/hashicorp/aws/latest/docs

### Ubuntu AWS AMI Documentation

https://documentation.ubuntu.com/aws/aws-how-to/instances/find-ubuntu-images/

### AWS CodeCommit

https://docs.aws.amazon.com/codecommit/

### AWS EC2 User Data

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html