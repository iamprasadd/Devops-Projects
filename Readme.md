# Terraform AWS 3‑Tier Web Application Infrastructure

This repository contains a production‑grade **AWS 3‑Tier Architecture** fully automated using **Terraform**. The project implements networking, security, compute, load balancing, RDS, Route53, ACM, and CI/CD with GitHub Actions.

## 🏗️ Architecture Overview

The deployed 3‑tier application follows this layered structure:

- **Presentation Tier (Public Layer)**  
  Application Load Balancer (ALB) in public subnets.

- **Application Tier (Private Layer)**  
  Auto Scaling Group (EC2) running the web application.

- **Database Tier (Private Data Layer)**  
  Amazon RDS (Multi‑AZ) inside isolated DB subnets.

- **Networking & Security**
  - VPC (10.10.0.0/16)
  - Public, App, DB subnets (across 2 AZs)
  - NAT Gateways
  - Route Tables
  - Security Groups per tier

- **Additional Services**
  - ACM SSL Certificate
  - DNS Routing via Route53
  - Remote backend using S3 + DynamoDB

---

## 📸 Architecture Diagram

![Architecture](./.images/3tier-web-application-architecture.png)

---

## 🚀 Why This Project Is Useful

This repository is built for:

- Demonstrating real‑world AWS infra using Infrastructure‑as‑Code.
- Learning how Terraform modules are designed and consumed.
- Deploying scalable, secure, multi‑AZ production systems.
- Practicing DevOps skills with CI/CD pipelines.
- Providing a reference template for enterprise‑grade infra deployments.

---

## 📂 Project Structure

```
terraform-aws-3tier-webapp-infra/
│
├── modules/
│   ├── vpc/
│   ├── subnets/
│   ├── routing/
│   ├── security/
│   ├── alb_asg/
│   ├── rds/
│   └── route53/
│
├── envs/
│   ├── dev/
│   ├── stage/
│   └── prod/
│
├── images/
│   └── 3tier.png
│
└── .github/workflows/
    └── terraform-ci.yml
```

---

## 🧠 Challenges Faced

### 1. ALB Health Check Failing  
Instances kept terminating due to failed health checks.  
**Fix:** Corrected user‑data and ensured nginx starts reliably.

### 2. SSM Agent Not Working  
EC2 instances didn't appear in SSM.  
**Fix:** Added proper IAM role + AmazonSSMManagedInstanceCore policy.

### 3. NAT Gateway Placement Confusion  
Learned that NAT must be in **public** subnet to access IGW.

### 4. ACM Certificate Validation Errors  
Invalid domain names caused issuance failures.  
**Fix:** Added correct Route53 Hosted Zone + domain validation records.

### 5. GitHub Actions Failing Due to AWS Profile  
CI pipeline failed because provider had `profile = default`.  
**Fix:** Removed profile & used credentials from GitHub Secrets.

### 6. Terraform State Lock Errors in CI/CD  
Pipeline failed due to stale DynamoDB lock.  
**Fix:** Enabled automated lock deletion & improved workflow retry behavior.

---

## ▶️ Getting Started

### **1. Clone the repository**
```bash
git clone https://github.com/iamprasadd/terraform-aws-3tier-webapp-infra.git
cd terraform-aws-3tier-webapp-infra/envs/dev
```

### **2. Configure backend (optional for local testing)**
Update `backend.tf` based on your S3 bucket & DynamoDB table.

### **3. Initialize Terraform**
```bash
terraform init
```

### **4. Validate & plan**
```bash
terraform validate
terraform plan
```

### **5. Apply**
```bash
terraform apply
```

---

## ❓ Need Help?

Issues can be raised in the repository’s **Issues** tab.

Documentation:  
- Terraform AWS Provider  
- AWS Architecture Center  

---

## 👤 Maintainer

**Prasad**  
AWS Cloud Engineer

---

## 🤝 Contributions

Contributions, issues, and feature requests are welcome!  
Please create a PR or open an issue.

---

## 📄 License

MIT License. See the `LICENSE` file.
