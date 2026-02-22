Project: View the "No Reboot" Option Before Creating an AMI
This hands-on shows how to locate the No Reboot option during AMI creation.
We are not creating the AMI — only demonstrating where the option appears.

## 1. Launch a New EC2 Instance

Go to EC2 → Launch Instance
Select Amazon Linux 2
Instance type: t2.micro
Launch the instance

Screenshot:

![Launch Instance](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-InstanceRunning.png)

## 2. Open Create Image (AMI) Menu
Once the instance is running:
Go to:
Instance → Actions → Image and templates → Create image
Screenshot:

![Create Image Menu](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-CreateImage.png)

## 3. Locate the “No Reboot” Option
On the Create Image screen, AWS shows this checkbox:
✔ No reboot

Screenshot:

![No Reboot Option](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-NoReboot.png)

## ⚠ Why “No Reboot” Is Not Recommended (One‑Liner)
No Reboot is not recommended because the OS may not flush in‑memory data to disk, 
causing the AMI to capture an inconsistent filesystem state.
