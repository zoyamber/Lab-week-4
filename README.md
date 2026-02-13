# Week 4 Lab – Terraform + Cloud-Init on AWS

**CIT 4640 Week 4 Lab**.  
It uses **Terraform** to provision AWS infrastructure and **cloud-init** to configure an EC2 instance at boot.

---

# Before Starting

## Install Terraform
Follow the official installation guide:  
https://developer.hashicorp.com/terraform/downloads

Verify installation:

```bash
terraform -version
```

## Clone This Repository

```bash
git clone https://github.com/yayeseydi/Lab-week4.git
cd Lab-week4
```

## Configure AWS Credentials

```bash
aws configure
```

Provide:

- AWS Access Key ID  
- AWS Secret Access Key  
- Default region: **us-west-2**  
- Default output format: **json**

---

# Task 1 – SSH Key + Cloud-Init Configuration

## Create a New SSH Key Pair

```bash
ssh-keygen -t ed25519 -f ~/.ssh/lab4 -C "lab4"
```

This creates:

- `~/.ssh/lab4` → private key  
- `~/.ssh/lab4.pub` → public key  

Import the public key into AWS:

```bash
aws ec2 import-key-pair \
  --key-name lab4 \
  --public-key-material fileb://~/.ssh/lab4.pub \
  --region us-west-2
```

## Cloud-Init Configuration

The **cloud-config.yaml** file:

- Updates packages  
- Installs **nginx** and **nmap**  
- Creates a **web user** with sudo access  
- Adds the SSH public key  
- Enables nginx at boot  

---

# Task 2 – Terraform Infrastructure

Run Terraform from the project root:

```bash
terraform init        # initialize providers
terraform fmt         # format configuration
terraform validate    # validate syntax
terraform plan        # preview infrastructure changes
terraform apply       # create AWS resources
```

View the instance public IP and DNS:

```bash
terraform output instance_ip_addr
```

The Terraform configuration creates:

- VPC  
- Public subnet  
- Internet gateway  
- Route table with default route  
- Security group allowing **SSH (22)** and **HTTP (80)**  
- Debian **t2.micro EC2 instance**  
- Cloud-init user-data configuration  
- Output showing **public IP and DNS**

---

# Task 3 – Connect to the EC2 Instance

Set correct permissions on the private key:

```bash
chmod 600 ~/.ssh/lab4
```

SSH into the **web user** created by cloud-init:

```bash
ssh -i ~/.ssh/lab4 web@<PUBLIC_DNS_OR_IP>
```

Example:

```bash
ssh -i ~/.ssh/lab4 web@ec2-35-95-142-96.us-west-2.compute.amazonaws.com
```

---

# Optional Cleanup

To remove all created infrastructure:

```bash
terraform destroy
```

---

# Repository Contents

- **README.md** → setup instructions and commands  
- **main.tf** → Terraform infrastructure configuration  
- **cloud-config.yaml** → cloud-init system configuration  
