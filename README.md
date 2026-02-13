# Week 4 Lab – Terraform + Cloud-Init on AWS

This repository contains the configuration for the **CIT 4640 Week 4 Lab**.  
It uses **Terraform** to provision AWS infrastructure and **cloud-init** to configure an EC2 instance at boot.

---

# Task 1 – SSH Key + Cloud-Init Configuration

## Create SSH Key Pair

<img width="500" height="168" alt="image" src="https://github.com/user-attachments/assets/15d75666-32ef-478b-b5ca-1578c148b12f" />



```bash
ssh-keygen -t ed25519 -f ~/.ssh/lab4 -C "lab4"
```


---

## Configure AWS CLI Credentials

Command used:

```bash
aws configure
```

<img width="512" height="72" alt="image" src="https://github.com/user-attachments/assets/da8763f4-feb1-47d7-a9e5-cbd40ac97d4e" />



---

# Task 2 – Terraform Initialization and Deployment

Commands used:

```bash
terraform fmt -recursive
terraform init
terraform validate
terraform plan -out plan.out
terraform apply plan.out
```

<img width="702" height="978" alt="image" src="https://github.com/user-attachments/assets/8d5e4267-864b-4d3a-bc01-35d25c116f51" />


<img width="846" height="1176" alt="image" src="https://github.com/user-attachments/assets/0e1b5fca-8044-45ce-872e-35cfb444931d" />


This confirms:

- AWS provider installed  
- Infrastructure created successfully  
- **Public IP and DNS output generated**  
- No changes required after plan re-run  

---

# Task 3 – Infrastructure Validation

Terraform validation and plan results:

```bash
terraform validate
terraform plan -out plan.out
terraform apply plan.out
```

Results show:

- Configuration is **valid**
- **No infrastructure drift**
- Apply shows **0 added, 0 changed, 0 destroyed**

This confirms the Terraform state matches the deployed AWS infrastructure.

---

# Summary

This lab demonstrates:

- Creating SSH keys for secure EC2 access  
- Configuring AWS CLI authentication  
- Using Terraform to provision AWS networking and compute resources  
- Automating instance setup with cloud-init  
- Verifying infrastructure state using Terraform plan/validate  

---
