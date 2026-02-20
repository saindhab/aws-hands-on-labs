# 🔄 Change EC2 Instance Type (Using EC2 Instance Connect + EBS-backed Instance)

> **RULE:** Changing EC2 instance type only works when the instance is **EBS-backed**.  
> Supported flow: **Stop → Change Instance Type → Start**  
> This lab verifies file persistence + RAM check before/after.

 1️⃣ Launch an EC2 Instance (Free Tier)
- AWS Console → Search **EC2**
- Go to **Instances → Launch instances**
- **Name**: `change-instance-type-lab`
- **AMI**: Amazon Linux 2023 / Amazon Linux 2  
- **Instance Type**: `t2.micro` (Free Tier)
- **Key Pair**: Not required (EC2 Instance Connect)
- **Security Group**: SSH (22) from My IP
- **Storage**: 8 GiB gp3 (EBS-backed)

📸 Screenshot: 
![Launch Instance Page](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Launch_InstanceChangeType.png)

2️⃣ Wait for Running + Status Checks
- Instance → **Running**
- Status → **2/2 checks passed**

📸 Screenshot: 
![Instance Running](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2_Syscheck.png)

3️⃣ Connect Using EC2 Instance Connect
- Select instance → **Connect**
- Choose **EC2 Instance Connect**
- User: `ec2-user`
- Click **Connect**

📸 Screenshot: 
![EC2 Instance Connect Window](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-InstanceConnect1.png)

4️⃣ Create File + Check Memory (Before Change)

Run inside terminal:

```bash
# Create and verify a file
echo "Hello World" > hello.txt
ls -l hello.txt
cat hello.txt

📸 Screenshot: 
![File Created](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-FileCreated.png)

# Memory checks
free -h
free -m

📸 Screenshot: 
![Memory Before Change](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-PreMemory.png)

Expected memory for t2.micro:

~996 MiB (approx 1 GB)

5️⃣ STOP the Instance (Required Step)

Go to EC2 → Instances
Select your instance
Click Instance state → Stop
Confirm stop
Wait until State = Stopped

📸 Screenshot: 
![Stop Instance](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Stopped.png)

⚠️ Because root volume = EBS, your hello.txt persists after stop/start.

6️⃣ CHANGE the Instance Type

Select instance
Click Actions → Instance settings → Change instance type

📸 Screenshot: 
![Change Instance Setting](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-ChangeInstanceSetting.png)

📸 Screenshot: 
![Change Instance Menu](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-ChangeInstanceMenu.png)

👉 Select t3.micro → Apply
** Although memory remains same but VCPU is 2 for t3.micro
📸 Screenshot: 
![Select t3.micro](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-t3micro.png)

7️⃣ Verify EBS-Optimized Setting
Scroll to Instance Details and confirm:

EBS-optimized: true
EBS bandwidth value

📸 Screenshot: !EBS Optimized Section
Why EBS-Optimized matters:
✔ Dedicated storage bandwidth
✔ Lower latency
✔ No network I/O contention
✔ Consistent performance
(t3/t4g micro = Always EBS‑optimized)

8️⃣ START the Instance

Instance state → Start
Wait for Running + 3/3 checks
3/3 = System + Instance + Attached EBS checks passed (newer/expanded health model)

📸 Screenshot: 
![Start Instance](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-statuscheck.png)

9️⃣ Reconnect via EC2 Instance Connect

Select instance → Connect → EC2 Instance Connect → Connect

🔟 Verify File Persistence + Memory After Change
Run:
# File should still exist (EBS volume persisted)
ls -l hello.txt
cat hello.txt

# Memory after instance type change
free -h
free -m

📸 Screenshot: 
![EC2 Connect After Change](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-statuscheck.png)
