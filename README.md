# AWS-RDS-POSTGREDB


## Getting started

Here's a step-by-step guide on setting up RDS PostgreSQL on AWS using the provided Terraform script:

1. **Install Terraform:** Download and install Terraform from the official website (https://www.terraform.io/downloads.html) based on your operating system.

2. **Create a directory:** Create a new directory on your machine and navigate to it using a command line interface.

3. **Initialize the working directory:** Run the following command to initialize the Terraform working directory:

_Copy code_
terraform init

4. **Configure the AWS credentials:** Ensure that you have configured your AWS credentials properly. You can either set environment variables or use the AWS CLI to configure the credentials.

5. **Create an RDS PostgreSQL instance**: Run the following command to create the RDS PostgreSQL instance:

_Copy code_
terraform apply

This will provision the RDS PostgreSQL instance using the specified configurations in the Terraform script. It will also create supporting resources such as VPC, security group, subnets, etc.

6. **Monitor the progress**: Terraform will display the execution plan, and you will be prompted to confirm the creation of resources. Type yes and press Enter to proceed.

7. **Wait for completion:** Terraform will create the RDS PostgreSQL instance and other associated resources. It may take a few minutes to complete. You can monitor the progress in the command line output.

8. **Access the outputs:** Once the provisioning is complete, Terraform will display the outputs defined in the OUTPUTS.tf file. These outputs contain useful information about the RDS instance, such as its address, endpoint, credentials, etc.

Example outputs:

- db_instance_address: The address of the RDS instance
- db_instance_endpoint: The connection endpoint of the RDS instance
- db_instance_username: The master username for the database
- db_instance_password: The database password


9. **Access the RDS instance:** You can now access the RDS PostgreSQL instance using the provided credentials and endpoint. Connect to the database using your preferred PostgreSQL client or tool.

## Script details

1. **Provider Configuration:**

_Copy code_
provider "aws" {
  region = local.region
}

This block configures the AWS provider and specifies the AWS region to use. The local.region variable holds the region value, which in this case is "eu-north-1".

2. **Data Blocks:**

- data "aws_caller_identity" "current" {}: This block retrieves the caller identity information for the current AWS account. It is used to authenticate and authorize the Terraform operations.
- data "aws_availability_zones" "available" {}: This block retrieves the available availability zones (AZs) in the specified region. It is used to determine the AZs to distribute the resources.


3. **Locals Block:**

_Copy code_
locals {
  name    = "complete-postgresql"
  region  = "eu-north-1"
  region2 = "eu-central-1"

  vpc_cidr = "10.0.0.0/16"
  azs      = slice(data.aws_availability_zones.available.names, 0, 3)

  tags = {
    Name       = local.name
    Example    = local.name
    Repository = "https://github.com/terraform-aws-modules/terraform-aws-rds"
  }
}

The locals block defines local variables that can be used throughout the script. It defines the following variables:

- name: A descriptive name for the resources (e.g., "complete-postgresql").
- region: The AWS region to deploy the resources.
- region2: An additional region (eu-central-1) that is not used in this script.
- vpc_cidr: The CIDR block for the Virtual Private Cloud (VPC).
- azs: A subset of available availability zones (AZs) in the specified region.
- tags: A map of tags to be applied to the resources for identification and organization purposes.
- RDS Module:

4. **RDS Module**

The module "db" block specifies the configuration for the RDS module, which provisions an RDS PostgreSQL instance. It includes the following parameters:

- **source**: Specifies the source of the module (terraform-aws-modules/rds/aws).
- **identifier**: A unique identifier for the RDS instance

It also specifies various RDS instance settings, such as the database engine, engine version, instance class, storage allocation, multi-AZ deployment, subnet group, security group, backup configuration, performance insights, monitoring, and other parameters.

The tags block assigns tags to the RDS resources for identification and organization. The db_option_group_tags and db_parameter_group_tags specify tags for DB option groups and DB parameter groups, respectively.

**Supporting Resources**:

- **module "vpc"**: This module provisions a Virtual Private Cloud (VPC) using the terraform-aws-modules/vpc/aws module. It sets up public and private subnets, along with a database subnet group.
- **module "security_group"**: This module creates a security group using the terraform-aws-modules/security-group/aws module. It allows inbound access on port 5432 (PostgreSQL) from within the VPC.
- **Output Definitions**:The output block defines various outputs to retrieve information about the RDS instance and related resources. These outputs include the RDS instance address, ARN, availability zone, endpoint, engine, username, password, port, subnet group details, parameter group details, and more.


That's a detailed breakdown of the provided Terraform script for setting up RDS PostgreSQL on AWS. It includes the configuration of the RDS module, supporting resources like VPC and security group, and outputs to retrieve information about the provisioned resources.
