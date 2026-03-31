# 🏗️ Terraform AWS Lab

> Provisioning real AWS infrastructure with code — no console clicks required.

Built as part of a hands-on DevOps learning journey, this project uses **Terraform** to define, deploy, and manage AWS cloud infrastructure entirely through code. This is Infrastructure as Code (IaC) in practice.

---

## 📸 Project Walkthrough

### 1. AWS CLI Configured
AWS credentials successfully configured via shared credentials file, connecting the local environment to AWS and enabling Terraform to authenticate and provision cloud resources.

<p align="center">
<img src="https://i.imgur.com/a0EqEjB.png" height="85%" width="85%" alt="AWS CLI Configured"/>
</p>

### 2. Terraform Initialized & Validated
Terraform successfully initialized, downloading the HashiCorp AWS provider v5.100.0 and creating a lock file for consistent deployments. Configuration validated with zero errors.

<p align="center">
<img src="https://i.imgur.com/lcP9Psf.png" height="85%" width="85%" alt="Terraform Initialized & Validated"/>
</p>

### 3. Provider Configuration (`terraform.tf`)
The `terraform.tf` file locks the AWS provider to version `~> 5.92` and requires Terraform `>= 1.2` — ensuring reproducible, version-controlled infrastructure deployments.

<p align="center">
<img src="https://i.imgur.com/yT6fMuI.png" height="85%" width="85%" alt="Provider Configuration"/>
</p>

### 4. Infrastructure Definition (`main.tf`)
The core Terraform configuration file defining the AWS provider, a dynamic Ubuntu AMI data source, and an EC2 t2.micro instance — complete with the HashiCorp VS Code extension providing intelligent autocomplete and syntax highlighting.

<p align="center">
<img src="https://i.imgur.com/d2w0a7C.png" height="85%" width="85%" alt="Infrastructure Definition"/>
</p>

### 5. EC2 Instance Confirmed via CLI
Using the AWS CLI to verify the deployed instance — `i-0cbb2ddebc3404c94` confirmed as running in `us-west-2a`, proving infrastructure was provisioned successfully through code.

<p align="center">
<img src="https://i.imgur.com/UYqE6sA.png" height="85%" width="85%" alt="EC2 Instance Confirmed via CLI"/>
</p>

### 6. EC2 Instance Live in AWS Console
The provisioned EC2 instance visible in the AWS Console — running in `us-west-2a`, passing all status checks, with a public IP of `54.184.232.194`. Zero console clicks were used to create this.

<p align="center">
<img src="https://i.imgur.com/Exl7et5.png" height="85%" width="85%" alt="EC2 Instance Live in AWS Console"/>
</p>



---

## 🧱 What This Builds

| Resource | Details |
|---|---|
| **EC2 Instance** | `t2.micro` running Ubuntu 24.04 (free tier eligible) |
| **AMI** | Dynamically fetched — always uses the latest Ubuntu Noble release |
| **Region** | `us-west-2` (Oregon) |
| **Availability Zone** | `us-west-2a` |

---


---

## ⚙️ Prerequisites

- [Terraform CLI](https://developer.hashicorp.com/terraform/install) v1.2.0+
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) v2+
- AWS account with IAM credentials configured

---

## 🚀 Usage


**1. Configure AWS credentials**
```bash
aws configure
```

**2. Initialize Terraform**
```bash
terraform init
```

**3. Preview the plan**
```bash
terraform plan
```

**4. Deploy**
```bash
terraform apply
# Type "yes" when prompted
```

**5. Verify**
```bash
aws ec2 describe-instances \
  --query "Reservations[*].Instances[*].[InstanceId,State.Name,Placement.AvailabilityZone]" \
  --output table
```

---

## 🧹 Cleanup

Always destroy resources when done to avoid charges:
```bash
terraform destroy
# Type "yes" when prompted
```

---

## 💡 Key Concepts Learned

- **Infrastructure as Code (IaC)** — defining cloud resources in version-controlled config files
- **Terraform Providers** — plugins that let Terraform talk to AWS APIs
- **Data Sources** — dynamically querying AWS for the latest AMI instead of hardcoding IDs
- **State Management** — how Terraform tracks deployed resources in `terraform.tfstate`
- **The Core Workflow** — `init → validate → plan → apply → destroy`

---

## 📚 Resources

- [HashiCorp Terraform AWS Tutorial](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create)
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS Free Tier](https://aws.amazon.com/free)

---

*Built with Terraform · Deployed on AWS · 



