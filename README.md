This module provides a reusable Terraform configuration to create an AWS VPC with multiple subnets (public, private, and database), a NAT Gateway, Internet Gateway, route tables, 
and optional VPC peering. It helps to set up a secure and well-organized network infrastructure in AWS for your projects.

Features
VPC Creation: This module creates a VPC with customizable CIDR blocks.
Subnets: You can define public, private, and database subnets across multiple availability zones.
Internet Gateway: It automatically creates an Internet Gateway to provide outbound internet access for public subnets.
NAT Gateway: A NAT Gateway is created to allow private and database subnets to access the internet.
Route Tables: It configures the necessary route tables and route table associations for each type of subnet.
VPC Peering (Optional): If required, the module can establish a VPC peering connection with the default VPC.
Tagging: Tags for all resources are set based on user-defined inputs, ensuring consistent resource management.

Requirements
Terraform version 1.0 or higher.
AWS account with appropriate permissions to create VPCs, subnets, route tables, and other related resources.
AWS provider credentials configured (via AWS CLI, environment variables, or IAM roles).

Module Inputs
Required Inputs
project_name: The name of the project, used in resource names and tags.
environment: The environment type (e.g., dev, prod).
vpc_cidr: CIDR block for the VPC.
common_tags: Common tags to apply to all resources within the module.
public_subnet_cidrs: List of CIDR blocks for public subnets (two subnets are required).
private_subnet_cidrs: List of CIDR blocks for private subnets (two subnets are required).
database_subnet_cidrs: List of CIDR blocks for database subnets (two subnets are required).

Optional Inputs
enable_dns_hostnames: Whether to enable DNS hostnames in the VPC (defaults to true).
vpc_tags: Tags to apply to the VPC (default is an empty map).
igw_tags: Tags for the Internet Gateway (default is an empty map).
public_subnet_tags: Tags for public subnets (default is an empty map).
private_subnet_tags: Tags for private subnets (default is an empty map).
database_subnet_tags: Tags for database subnets (default is an empty map).
nat_gateway_tags: Tags for the NAT Gateway (default is an empty map).
public_route_table_tags: Tags for the public route table (default is an empty map).
private_route_table_tags: Tags for the private route table (default is an empty map).
database_route_table_tags: Tags for the database route table (default is an empty map).
is_peering_required: Whether to create a VPC peering connection with the default VPC (defaults to false).
vpc_peering_tags: Tags for the VPC peering connection (default is an empty map).

Module Outputs
azs_info: Information about the available availability zones in your region.
subnets_info: List of all public subnets created.
vpc_id: The ID of the created VPC.
public_subnet_ids: A list of IDs of the created public subnets.
private_subnet_ids: A list of IDs of the created private subnets.
database_subnet_ids: A list of IDs of the created database subnets.

Example Usage

module "vpc" {
  source            = "path/to/vpc-module"
  project_name      = "Project_name"
  environment       = "prod"
  vpc_cidr          = "10.0.0.0/16"
  common_tags       = {
    "Owner" = "YourName"
    "Project" = "ProjectName"
  }
  public_subnet_cidrs = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnet_cidrs = ["10.0.3.0/24", "10.0.4.0/24"]
  database_subnet_cidrs = ["10.0.5.0/24", "10.0.6.0/24"]
}

Module Resources
VPC
The module creates a VPC with a customizable CIDR block, and tags it based on the user-provided values.

Subnets
Public Subnets: Public subnets are created across multiple availability zones. These subnets are automatically configured to map public IPs to instances launched within them.
Private Subnets: Private subnets are created, which do not have direct internet access but can access the internet via the NAT Gateway.
Database Subnets: Special subnets dedicated to database instances, with similar routing to private subnets.

Internet Gateway & NAT Gateway
Internet Gateway: An Internet Gateway is created for providing internet access to public subnets.
NAT Gateway: A NAT Gateway is created and associated with the public subnet to allow private subnets and database subnets to access the internet.

Route Tables & Associations
The module creates separate route tables for public, private, and database subnets and associates them with the corresponding subnets.
Public subnets use the Internet Gateway for outbound internet access, while private and database subnets route traffic through the NAT Gateway.
VPC Peering (Optional)
If the is_peering_required input is set to true, the module establishes a VPC peering connection between the main VPC and the default VPC. Additionally, it configures routes for public, private, and database subnets to use the peering connection.

Dependencies
The following AWS resources are created by the module:
VPC
Subnets (public, private, and database)
Internet Gateway
NAT Gateway
Route Tables and Route Table Associations
Optionally, VPC Peering Connection

Author
This module is maintained by [Sindhuri Kolluri].

