# Terraform AWS VPC Module

This module provides a reusable Terraform configuration to create an AWS VPC with multiple subnets (public, private, and database), a NAT Gateway, Internet Gateway, route tables, and optional VPC peering. It helps to set up a secure and well-organized network infrastructure in AWS for your projects.

---

## 🚀 Features

### ✅ VPC Creation  
- This module creates a **VPC** with customizable **CIDR blocks**.  

### ✅ Subnets  
- Define **public, private, and database subnets** across multiple **Availability Zones**.  

### ✅ Internet Gateway  
- Automatically creates an **Internet Gateway** for public subnets.  

### ✅ NAT Gateway  
- A **NAT Gateway** is created to allow **private and database subnets** to access the internet.  

### ✅ Route Tables  
- Configures **route tables** and **associates them** with each subnet type.  

### ✅ VPC Peering (Optional)  
- Supports **VPC Peering** with the **default VPC** if enabled.  

### ✅ Tagging  
- Applies **user-defined tags** to all resources.  

---

## 📌 Requirements  

| Requirement              | Details |
|--------------------------|---------|
| **Terraform Version**    | `1.0` or higher |
| **AWS Account**          | Required |
| **Permissions**          | Must have access to create VPCs, subnets, route tables, and other related resources |
| **AWS Provider Config**  | AWS credentials must be configured via CLI, environment variables, or IAM roles |

---

## ⚙️ Module Inputs  

### 🔹 Required Inputs  

| Variable                 | Description | Example |
|--------------------------|-------------|---------|
| `project_name`           | The project name used in resource names & tags | `"my-project"` |
| `environment`            | Environment type (`dev`, `prod`, etc.) | `"prod"` |
| `vpc_cidr`              | CIDR block for the VPC | `"10.0.0.0/16"` |
| `common_tags`           | Tags applied to all resources | `{ "Owner": "Sindhuri", "Project": "VPC-Module" }` |
| `public_subnet_cidrs`   | CIDR blocks for public subnets (2 required) | `["10.0.1.0/24", "10.0.2.0/24"]` |
| `private_subnet_cidrs`  | CIDR blocks for private subnets (2 required) | `["10.0.3.0/24", "10.0.4.0/24"]` |
| `database_subnet_cidrs` | CIDR blocks for database subnets (2 required) | `["10.0.5.0/24", "10.0.6.0/24"]` |

### 🔹 Optional Inputs  

| Variable                      | Description | Default |
|--------------------------------|-------------|---------|
| `enable_dns_hostnames`        | Enable DNS hostnames in the VPC | `true` |
| `vpc_tags`                    | Tags for VPC | `{}` |
| `igw_tags`                    | Tags for Internet Gateway | `{}` |
| `public_subnet_tags`          | Tags for public subnets | `{}` |
| `private_subnet_tags`         | Tags for private subnets | `{}` |
| `database_subnet_tags`        | Tags for database subnets | `{}` |
| `nat_gateway_tags`            | Tags for NAT Gateway | `{}` |
| `public_route_table_tags`     | Tags for public route table | `{}` |
| `private_route_table_tags`    | Tags for private route table | `{}` |
| `database_route_table_tags`   | Tags for database route table | `{}` |
| `is_peering_required`         | Enable VPC peering connection | `false` |
| `vpc_peering_tags`            | Tags for VPC peering | `{}` |

---

## 📤 Module Outputs  

| Output                | Description |
|-----------------------|-------------|
| `azs_info`           | Availability Zones info |
| `subnets_info`       | List of all created public subnets |
| `vpc_id`             | ID of the created VPC |
| `public_subnet_ids`  | IDs of public subnets |
| `private_subnet_ids` | IDs of private subnets |
| `database_subnet_ids` | IDs of database subnets |

---

## 🚀 Example Usage  

```hcl
module "vpc" {
  source = "path/to/vpc-module"

  project_name         = "Project_name"
  environment          = "prod"
  vpc_cidr            = "10.0.0.0/16"
  
  common_tags = {
    "Owner"  = "YourName"
    "Project" = "ProjectName"
  }

  public_subnet_cidrs  = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnet_cidrs = ["10.0.3.0/24", "10.0.4.0/24"]
  database_subnet_cidrs = ["10.0.5.0/24", "10.0.6.0/24"]
}
