# 🚀 EC2 Instance Launch & Connection Guide (with Screenshots)

This document contains the complete step‑by‑step process to launch an EC2 instance and connect using **EC2 Instance Connect**.  
---

## **1️. Login to AWS Console**
- Open: https://console.aws.amazon.com  
- Sign in with your IAM/Root account.

📸 **Screenshot:**
![AWS Login](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/aws-login.png)

## **2️. Open EC2 Dashboard**
- In the AWS search bar → type **EC2**  
- Click **EC2 – Virtual Servers in the Cloud**

📸 **Screenshot:**  
![EC2 Dashboard](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/EC2-Dashboard.png)

3️. Launch a New EC2 Instance**
- Click **Launch Instance**
- Enter a name (example: `ec2-demo`)
- Select:
  - **Amazon Linux 2023 (kernel-6.1) (Free tier)**
  - **t2.micro** instance type

4. Creating key pair for ssh**
![KeyPair](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSH-KeyPair.png)
- Under **Key Pair** → choose:
  - Create new key pair / select existing
  - Type: **RSA**
  - Format: **.pem**

---

## **5. Configure Security Group**
- Allow:
  - **SSH (22)** → Your IP  
  - **HTTP (80)** → Optional (for web servers)

📸 **Screenshot:**  
![Security Group](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SecurityGroup.png)

## **6️⃣ Launch the Instance**
- Scroll down  
- Click **Launch Instance**
- Wait until:
  - **Instance state:** running  
  - **Status checks:** 2/2 passed  

📸 **Screenshot:**  
![EC2 Instance Connect](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/EC2-InstanceConnect.png)
- Select your instance  
- Click **Connect**  
- Select **EC2 Instance Connect**  
- Click **Connect**

```bash
# 🟢 Basic SSH Command
ssh -i "your-key.pem" ec2-user@<PUBLIC-IP>

# 🟣 Example:
ssh -i "mykey.pem" ec2-user@13.233.122.10
