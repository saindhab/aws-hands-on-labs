Install Apache HTTP Server Using SSM Run Command & Custom SSM Document

📌 Project Overview
This project demonstrates how to install and configure the Apache HTTP server (httpd) 
on multiple EC2 instances without SSH access, using:

A custom SSM Command Document (YAML)
SSM Run Command
Parameter injection
Concurrency control
CloudWatch log streaming

1️⃣ Prerequisites

EC2 Instances (Amazon Linux 2/2023).
SSM Agent installed (preinstalled on Amazon Linux).
IAM Role attached: AmazonSSMManagedInstanceCore and CloudWatchAgentServerPolicy (for writing logs to cloudwatch)
Security Group allows port 80 inbound.

2️⃣ Allow HTTP (Port 80) in Security Group
Go to:
EC2 → Instances → Select Instance → Security → Security Groups → Edit inbound rules
Add:
Type: HTTP
Port: 80
Source: 0.0.0.0/0

📸 Screenshot Placeholder

![SG Port 80](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-SGPort80.png)

Now test access:
Open:http://<your-ec2-public-dns>

📸 Screenshot Placeholder

![apache not installed](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-ApachenotInstalled.png)

This is expected — port 80 is open but Apache is not installed.

3️⃣ Create a Custom SSM Document
Navigate to:
Systems Manager → Documents → Create Document
Document Details

Name: InstallApache
Target Type: AWS::EC2::Instance
Document Type: Command

📸 Screenshot Placeholder

![Create Document Screen](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-CreateDocScreen.png)

4️⃣ Paste YAML Content
In the editor, choose YAML mode and paste:

📸 Screenshot Placeholder

![YAML Document Content](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-YAMLdocContent.png)

Click Create Document.
Your document is now owned by you, not AWS.

5️⃣ Run the Document Using SSM Run Command
Go to:
Systems Manager → Run Command → Run Command
Search for your document:
Filter:
Owner: Owned by me
Select:InstallApache

📸 Screenshot Placeholder
![Run Command Select Document](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-RunCommandSelectdoc.png)

6️⃣ Fill Command Parameters
Version Default version: 1
Message Parameter: Custom Hello World

📸 Screenshot Placeholder

![Document Parameters](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-DocParameters.png)

7️⃣ Select Targets
Choose:
✔ Manually select instances
Select all EC2 instances you want to configure.

📸 Screenshot Placeholder

![Target Instances](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-TargetEC2's.png)

8️⃣ Configure Concurrency & Error Threshold
Set :Concurrency: 1 target at a time
Error Threshold: 5%

📸 Screenshot Placeholder

![Concurrency Settings](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-Concurrencysetting.png)

9️⃣ Enable CloudWatch Logs Output
Under Output Options, enable: 

Enable CloudWatch Logs
Log Group Name: /aws/ssm/RunCommandOutput

📸 Screenshot Placeholder

![CloudWatch Logs Setting](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-CloudwatchSetting.png)

🔟 Run the Command
Click: Run

You will now see:

1 instance → In progress
The other 2 → Pending
(because concurrency is set to 1 at a time)

📸 Screenshot Placeholder

![Run Progress](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-RunProgress.png)

1️⃣1️⃣ Verify Output Per Instance
Click each instance in the command result:

You should see stdout:
Installed: httpd
Started httpd service
Enabled httpd service

📸 Screenshot Placeholder

![SSM Output](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-SSMOutput.png)

1️⃣2️⃣ View CloudWatch Logs
Go to:
CloudWatch → Logs → /aws/ssm/RunCommandOutput
You will see:

stdout
stderr
SSM execution logs

📸 Screenshot Placeholder

![CloudWatch Logs](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-CloudwatchLogs.png)

1️⃣3️⃣ Test Apache Installation
Open your EC2 Public DNS: http://ec2-100-30-241-252.compute-1.amazonaws.com/

You should see: Hello World from <hostname>

This confirms:
✔ Apache installed
✔ Service started
✔ index.html written
✔ The parameter value is injected
✔ No SSH required

📸 Screenshot Placeholder

![Apache Working](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-ApacheWorking.png)

🎯 Final Summary
We have successfully implemented:

Custom SSM Command Document
YAML scripting
Parameterized automation
SSM Run Command
Concurrency control
Zero‑SSH installation
CloudWatch Logs integration
Multi-instance Apache deployment
Browser validation



