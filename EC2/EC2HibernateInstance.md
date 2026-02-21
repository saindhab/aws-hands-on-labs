Project: Enable and Test EC2 Hibernate Feature

## 1. Launch a New EC2 Instance

Go to AWS Console → EC2 → Launch Instance
Select Amazon Linux 2 AMI (Hibernate supported)
Choose t2.micro instance type

Screenshot:

![EC2 Launch Page](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-HibernateLaunchPage.png)

## 2. Configure Storage (EBS Volume Requirements)

Set Root volume to 8 GiB (enough to store 1 GiB RAM)
Ensure volume is Encrypted

Encryption: Enabled
Key: aws/ebs

Screenshot:

![EBS Storage Config](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-EBSStorageConfig.png)

## 3. Enable Hibernate Behavior

In Advanced Details
Set Stop behavior → Hibernate
Turn ON Enable Hibernation

Screenshot:

![Enable Hibernate](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-EnableHibernate.png)

## 4. Launch the Instance

Review all settings
Click Launch Instance

Screenshot:

![Instance Launched](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-InstanceLaunched.png)

## 5. Connect to EC2 Instance
Use EC2 Instance Connect.
After connecting, check uptime

Screenshot:

![Uptime Before Hibernate](https://github.com/saindhab/aws-screenshots/blob/main/ec2/EC2-UptimeBeforeHibernate.png)

## 6. Disconnect and Hibernate the Instance
Disconnect:
ShellexitShow more lines
In AWS Console:
Instance State → Hibernate

Screenshot:

![Hibernate Action](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-HibernateAction.png)

## 7. Instance Moves to Stopped State
After hibernation, EC2 will show:
stopping → stopped

Screenshot:

![Stopped After Hibernate](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Hibernated.png)

## 8. Start the Instance Again
Click: Start Instance

Screenshot:

![Start Instance](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-StartInstance.png)

## 9. Reconnect & Check Uptime Again
Run: uptime

Expected:

Uptime should NOT reset
It should continue from earlier uptime value
This confirms Hibernate restored RAM state

Screenshot:

![Uptime After Hibernate](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-UptimeAfterHibernate.png)

## 10. Troubleshooting (Short Summary)
I faced an issue during my first hibernate test:
❌ Issue: Instance Status Check Failed After Hibernate

I hibernated the EC2 too early (only ~1–2 mins uptime).
OS services and cloud‑init had not fully completed.
The RAM snapshot saved to EBS was incomplete → caused a failed resume.

✔ Fix

Stopped the instance manually (discarded bad hibernate image).
Started the instance again → both checks passed.
Waited longer (14 mins uptime) before hibernating again.
Hibernate → Start worked successfully.

📌 Result
After resume, uptime increased from 14 mins → 25 mins, proving hibernation worked correctly.
