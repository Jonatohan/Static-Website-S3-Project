# Static-Website-S3-Project
# AWS S3 Bucket Creation with Terraform

This repository contains Terraform configurations for setting up an AWS S3 bucket. The purpose of this project is to demonstrate my skills in Infrastructure as Code (IaC) using Terraform and AWS. This project is part of my portfolio to showcase my abilities to potential employers.

## Project Structure

- **main.tf**: Contains the primary Terraform configuration for provisioning the S3 bucket.
- **variables.tf**: Defines the variables used in the configuration for flexibility and reusability.
- **outputs.tf**: Specifies the outputs of the Terraform configuration, providing useful information after deployment.
- **.gitignore**: Specifies files and directories to be ignored by Git, ensuring sensitive and unnecessary files are not included in the repository.

## Project Details

### S3 Bucket Configuration

- **Bucket Name**: The name of the S3 bucket is defined as a variable, allowing it to be easily customized.
- **Versioning**: S3 bucket versioning is enabled to maintain different versions of the objects stored.
- **Server-Side Encryption**: Server-side encryption is enabled to ensure that all objects stored in the bucket are encrypted.
- **Public Access Block**: Public access to the S3 bucket is blocked to enhance security.

### Terraform Files

# `main.tf`

hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

provider "aws" {
  region = var.region
}

resource "aws_s3_bucket" "bucket" {
  bucket = var.bucket_name

  versioning {
    enabled = true
  }

  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }

  block_public_acls   = true
  block_public_policy = true
}

resource "aws_s3_bucket_public_access_block" "block" {
  bucket = aws_s3_bucket.bucket.id

  block_public_acls   = true
  block_public_policy = true
  ignore_public_acls  = true
  restrict_public_buckets = true
}
