https://github.com/mir-owahed/infrastructure-repository
GitOps ArgoCD Implementation: Automate Infrastructure Creation with Terraform & GitHub Actions, and Application Deployment on EKS Cluster
This guide provides a comprehensive walkthrough for setting up a GitOps workflow. It uses Terraform and GitHub Actions to automate infrastructure provisioning on AWS EKS and leverages ArgoCD for automating application deployments.

Table of Contents
Overview
Pre-requisites
Step 1: Create GitHub Repositories
1.1. Create Infrastructure Repository
1.2. Create Application Repository
Step 2: Configure GitHub Secrets
Step 3: Configure Terraform for EKS Setup
3.1. Create Terraform Files
3.2. Initialize and Validate Infrastructure
Step 4: Create GitHub Actions Workflow
Step 5: Install ArgoCD on EKS Cluster
Step 6: Access the ArgoCD UI
Step 7: Configure ArgoCD for Automated Application Deployment

Overview
This guide will help you set up an automated GitOps pipeline:

Automate Infrastructure: Use Terraform and GitHub Actions to provision an EKS cluster.
Automate Application Deployment: Use ArgoCD to monitor the application repository and deploy updates to the EKS cluster automatically.
https://www.youtube.com/watch?v=dy1CkxQv0SM
Pre-requisites
GitHub account to create repositories.
AWS account with permissions to create EKS resources.
AWS CLI installed and configured on your local machine.
kubectl installed for Kubernetes cluster management.

Step 1: Create GitHub Repositories
1.1. Create Infrastructure Repository
Create a GitHub repository called infrastructure to store Terraform configurations.
Initialize the repository with a README.md file.
1.2. Create Application Repository
Create a separate GitHub repository called application to store Kubernetes manifest files.
Initialize the repository with a README.md file.

Step 2: Configure GitHub Secrets
2.1. GitHub Secrets Setup
To authenticate GitHub Actions with AWS for infrastructure deployment:

Go to your Infrastructure Repository in GitHub.
Navigate to Settings > Secrets and variables > Actions.
Add the following secrets:
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
These secrets are necessary for AWS authentication when GitHub Actions runs the Terraform configuration.

Step 3: Configure Terraform for EKS Setup
3.1. Create Terraform Files
In the infrastructure repository, create the following Terraform files:

main.tf (Terraform configuration for EKS)
{content: 
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
}

resource "aws_route_table" "main" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }

  route {
    cidr_block = "10.0.0.0/16"
    gateway_id = "local"
  }
}

resource "aws_route_table_association" "subnet_1_association" {
  subnet_id      = aws_subnet.subnet_1.id
  route_table_id = aws_route_table.main.id
}

resource "aws_route_table_association" "subnet_2_association" {
  subnet_id      = aws_subnet.subnet_2.id
  route_table_id = aws_route_table.main.id
}

resource "aws_route_table_association" "subnet_3_association" {
  subnet_id      = aws_subnet.subnet_3.id
  route_table_id = aws_route_table.main.id
}

resource "aws_subnet" "subnet_1" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "ap-south-1a"
  map_public_ip_on_launch = true
}

resource "aws_subnet" "subnet_2" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.2.0/24"
  availability_zone       = "ap-south-1b"
  map_public_ip_on_launch = true
}

resource "aws_subnet" "subnet_3" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.3.0/24"
  availability_zone       = "ap-south-1c"
  map_public_ip_on_launch = true
}

module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "~> 20.0"

  cluster_name    = "my-cluster"
  cluster_version = "1.31"

  cluster_endpoint_public_access = true

  cluster_addons = {
    coredns                = {}
    eks-pod-identity-agent = {}
    kube-proxy             = {}
    vpc-cni                = {}
  }

  vpc_id                   = aws_vpc.main.id
  subnet_ids               = [aws_subnet.subnet_1.id, aws_subnet.subnet_2.id, aws_subnet.subnet_3.id]
  control_plane_subnet_ids = [aws_subnet.subnet_1.id, aws_subnet.subnet_2.id, aws_subnet.subnet_3.id]

  eks_managed_node_groups = {
    green = {
      ami_type       = "AL2023_x86_64_STANDARD"
      instance_types = ["m5.xlarge"]

      min_size     = 1
      max_size     = 1
      desired_size = 1
    }
  }
}
}
