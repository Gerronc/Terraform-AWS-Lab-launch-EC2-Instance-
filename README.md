# 🏗️ Terraform IAC AWS Lab

> Provisioning real AWS infrastructure with code — no console clicks required.

Built as part of a hands-on DevOps learning journey, this project uses **Terraform** to define, deploy, and manage AWS cloud infrastructure entirely through code. Starting from a single EC2 instance and evolving into a full VPC architecture with public and private subnets — this is Infrastructure as Code (IaC) in practice.

---

## 🏛️ Architecture

```
AWS (us-west-2)
└── Custom VPC (10.0.0.0/16)
    ├── Public Subnet (us-west-2a)
    │    └── Internet Gateway (outbound internet access)
    ├── Private Subnet (us-west-2a)
    │    └── EC2 Instance — learn-terraform (t2.micro, Ubuntu 24.04)
    ├── Private Subnet (us-west-2b)
    ├── Route Tables (public + private)
    ├── Security Group
    └── Network ACL
```

---

## 📸 Build Walkthrough

### 1. AWS CLI Configured
AWS credentials successfully configured via shared credentials file, connecting the local environment to AWS and enabling Terraform to authenticate and provision cloud resources.

<p align="center">
<img src="https://i.imgur.com/3TP6bhR.png" height="85%" width="85%" alt="AWS CLI Configured"/>
</p>

### 2. Terraform Initialized & Validated
Terraform successfully initialized, downloading the HashiCorp AWS provider v5.100.0 and creating a lock file for consistent deployments. Configuration validated with zero errors.

<p align="center">
<img src="https://i.imgur.com/ZMH0vJn.png" height="85%" width="85%" alt="Terraform Initialized & Validated"/>
</p>

### 3. Provider Configuration (`terraform.tf`)
The `terraform.tf` file locks the AWS provider to version `~> 5.92` and requires Terraform `>= 1.2` — ensuring reproducible, version-controlled infrastructure deployments.

<p align="center">
<img src="https://i.imgur.com/04lGoto.png" height="85%" width="85%" alt="Provider Configuration (`terraform.tf`)"/>
</p>

[Terraform.tf](https://github.com/Gerronc/Terraform-AWS-Lab-launch-EC2-Instance-/blob/main/terraform.tf)

### 4. Infrastructure Definition (`main.tf`)
The core Terraform configuration file defining the AWS provider, a dynamic Ubuntu AMI data source, VPC module, and EC2 instance — with the HashiCorp VS Code extension providing intelligent autocomplete and syntax highlighting.

<p align="center">
<img src="https://i.imgur.com/g6w5KXa.png" height="85%" width="85%" alt="Infrastructure Definition (`main.tf`)"/>
</p>

[Main.tf](https://github.com/Gerronc/Terraform-AWS-Lab-launch-EC2-Instance-/blob/main/main.tf)

### 5. Terraform Output
Terraform output displaying the EC2 instance's private hostname `ip-10-0-1-243.us-west-2.compute.internal` — confirming the instance is correctly placed inside the private subnet of the custom VPC.

<p align="center">
<img src="https://i.imgur.com/BSbXowP.png" height="85%" width="85%" alt="Terraform Output"/>
</p>

### 6. Terraform State List
All 17 infrastructure resources tracked in Terraform state — including the EC2 instance, VPC, public and private subnets, route tables, route table associations, internet gateway, security group, and network ACL. Every resource was provisioned with a single `terraform apply`.

<p align="center">
<img src="https://i.imgur.com/XkyhZsk.png" height="85%" width="85%" alt="Terraform State List"/>
</p>

### 7. EC2 Instances Dashboard
AWS EC2 console showing the newly provisioned `learn-terraform` instance running in `us-west-2a`, alongside the previous terminated instance — demonstrating Terraform's destroy-and-replace workflow when moving an instance into a new VPC.

<p align="center">
<img src="https://i.imgur.com/NL0YFIA.png" height="85%" width="85%" alt="EC2 Instances Dashboard"/>
</p>

### 8. VPC Dashboard
`example-vpc` successfully created and available in `us-west-2` — a custom isolated network with CIDR block `10.0.0.0/16`, provisioned entirely through the Terraform AWS VPC module.

<p align="center">
<img src="https://i.imgur.com/kPXbOr7.png" height="85%" width="85%" alt="VPC Dashboard"/>
</p>

### 9. Subnets
Three subnets created by the VPC module — one public subnet (`example-vpc-public-us-west-2a`) and two private subnets (`example-vpc-private-us-west-2a` and `us-west-2b`) — following real-world best practice of separating public-facing and private resources across multiple availability zones.

<p align="center">
<img src="https://i.imgur.com/UP0eLoD.png" height="85%" width="85%" alt="Subnets"/>
</p>

### 10. Route Tables
Five route tables provisioned by Terraform — separate routing rules for the public subnet and each private subnet, ensuring traffic flows correctly between resources and the internet gateway.

<p align="center">
<img src="https://i.imgur.com/P0aaqVb.png" height="85%" width="85%" alt="Route Tables"/>
</p>

### 11. Internet Gateway
`example-vpc` internet gateway attached and active — providing the public subnet with outbound internet access while private subnets remain isolated, following standard AWS security architecture.

<p align="center">
<img src="https://i.imgur.com/kPXbOr7.png" height="85%" width="85%" alt=" Internet Gateway"/>
</p>

### 12. EC2 Instance Summary
EC2 instance running inside `example-vpc` with private IP `10.0.1.243`, placed in the private subnet `example-vpc-private-us-west-2a` — zero console clicks used to configure any of this networking.

<p align="center">
<img src="https://i.imgur.com/zebDYBj.png" height="85%" width="85%" alt="EC2 Instance Summary"/>
</p>

---

## 🧱 Resources Provisioned (17 Total)

| Resource | Name | Purpose |
|---|---|---|
| `aws_instance` | app_server | EC2 t2.micro running Ubuntu 24.04 |
| `aws_vpc` | example-vpc | Custom isolated network (10.0.0.0/16) |
| `aws_subnet` | public-us-west-2a | Public facing subnet |
| `aws_subnet` | private-us-west-2a | Private subnet (AZ a) |
| `aws_subnet` | private-us-west-2b | Private subnet (AZ b) |
| `aws_internet_gateway` | example-vpc | Outbound internet access |
| `aws_route_table` | public | Routing rules for public subnet |
| `aws_route_table` | private x2 | Routing rules for private subnets |
| `aws_route_table_association` | public + private x2 | Links subnets to route tables |
| `aws_default_security_group` | example-vpc | Traffic control in/out |
| `aws_default_network_acl` | example-vpc | Subnet level firewall |
| `aws_default_route_table` | example-vpc | Default routing fallback |

---

## 📁 File Structure

```
terraform-aws-lab/
├── terraform.tf       # Provider version constraints
├── main.tf            # Core infrastructure — VPC module + EC2
├── variables.tf       # Input variables (instance name, type)
├── outputs.tf         # Output values (instance hostname)
├── .gitignore         # Excludes state files and secrets
└── README.md          # This file
```

---

## ⚙️ Prerequisites

- [Terraform CLI](https://developer.hashicorp.com/terraform/install) v1.2.0+
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) v2+
- AWS account with IAM credentials configured

---

## 🚀 Usage

**1. Clone the repo**
```bash
git clone https://github.com/YOUR_USERNAME/terraform-aws-lab.git
cd terraform-aws-lab
```

**2. Configure AWS credentials**
```bash
aws configure
```

**3. Initialize Terraform**
```bash
terraform init
```

**4. Preview the plan**
```bash
terraform plan
```

**5. Deploy**
```bash
terraform apply
# Type "yes" when prompted
```

**6. View outputs**
```bash
terraform output
```

**7. View all managed resources**
```bash
terraform state list
```

**8. Test variable overrides**
```bash
terraform plan -var instance_type=t2.large
```

---

## 🧹 Cleanup

Always destroy resources when done to avoid charges:
```bash
terraform destroy
# Type "yes" when prompted
```

---

## 💡 Key Concepts Demonstrated

- **Infrastructure as Code (IaC)** — all 17 AWS resources defined in version-controlled config files
- **Terraform Modules** — used the community AWS VPC module from the Terraform Registry to provision an entire network with one block of code
- **Input Variables** — parameterized instance name and type, no hardcoded values
- **Output Values** — exposed instance hostname for use by other tools or workflows
- **Data Sources** — dynamically fetched the latest Ubuntu AMI instead of hardcoding an ID
- **State Management** — Terraform tracks all 17 resources in state, detecting drift and changes
- **Dependency Graphs** — Terraform automatically determined the correct order to create resources
- **Idempotency** — running `terraform apply` twice makes no changes if infrastructure matches config
- **The Core Workflow** — `init → validate → plan → apply → destroy`

---

## 📚 Resources

- [HashiCorp Terraform AWS Get Started](https://developer.hashicorp.com/terraform/tutorials/aws-get-started)
- [Terraform AWS VPC Module](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest)
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS Free Tier](https://aws.amazon.com/free)

---

*Built with Terraform · Deployed on AWS · Part of a DevOps learning portfolio*
