# eks-infra

Terraform configuration to provision an AWS EKS cluster with supporting VPC infrastructure.

## Overview

This repository contains Terraform code to deploy:

- AWS VPC with public and private subnets
- EKS cluster
- EKS managed node groups
- S3 backend state storage with DynamoDB locking

## Repo structure

- main.tf — root Terraform configuration, provider, backend, and module wiring
- `variables.tf` — root input variables
- `output.tf` — root outputs
- `modules/vpc/` — VPC module resources
- `modules/eks/` — EKS cluster and node group module
- `backend/main.tf` — backend configuration details (if used separately)

## Prerequisites

- Terraform >= 1.0
- AWS CLI configured with credentials
- AWS account with permissions to create:
  - VPCs, subnets, security groups
  - IAM roles and policies
  - EKS clusters and node groups
  - S3 bucket and DynamoDB table for backend state

## Usage

1. Initialize Terraform

    ```bash
    terraform init
    ```

2. Review the execution plan

    ```bash
    terraform plan
    ```

3. Apply the configuration

    ```bash
    terraform apply
    ```

4. Destroy when no longer needed

    ```bash
    terraform destroy
    ```

## Main variables

Typical root variables include:

- `region`
- `cluster_name`
- `cluster_version`
- `vpc_cidr`
- `public_subnet_cidrs`
- `private_subnet_cidrs`
- `availability_zones`
- `node_groups`

Define values via `terraform.tfvars`, environment variables, or CLI flags.

## Backend

The root `terraform` block uses an S3 backend:

- bucket: `terraform-eks-state-bucket`
- key: `terraform.tfstate`
- region: `canada-central-1`
- dynamodb_table: `terraform-eks-state-lock`

Update these values to match your AWS backend configuration.

## Notes

- `module.vpc` outputs subnet IDs and VPC ID for the EKS module
- `module.eks` creates IAM roles, the EKS cluster, and managed node groups
- Ensure the backend S3 bucket and DynamoDB table exist before initialization if not managed by Terraform

