Project: Create & Use AMI (Amazon Machine Image)
This project demonstrates how to launch an EC2 instance with user data, install Apache (httpd), 
create a custom AMI from that instance, and then launch a new instance using that AMI with faster boot time.

## 1. Launch a New EC2 Instance

Go to EC2 → Launch Instance
Select Amazon Linux 2 AMI
Choose t2.micro
Keep default network settings

## 2. Add EC2 User Data (Install httpd)
In Advanced Details → User data

✔ This script installs and starts Apache HTTP Server.
Screenshot:

![User Data Script](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-UserDataScript.png)

## 3. Launch the Instance
Click Launch Instance.
Important Note:
If you open the public DNS too quickly, you may get:

Connection refused
Timeout

Because Apache service takes some time to start.
Wait a few seconds until the server is ready.

Screenshot:

![Instance Running](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-InstanceRunning.png)

## 4. Access the EC2 Public DNS
Paste Public DNS into browser:
You should see:
👉 Apache Test Page
This means httpd installed successfully.

Screenshot:

![Apache Test Page](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-ApacheTestpage.png)

## 5. Create an AMI from This Instance
Go to:
Instance → Actions → Image and templates → Create Image (AMI)
Provide:

AMI Name
Optional description

Then click Create Image.

Screenshot:

![Create AMI](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-CreateImage.png)

## 6. Verify AMI Creation
Go to:
EC2 → AMIs → My AMIs
Check AMI status:
✔ Available

Screenshot:

![AMI Available](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-AvailableImage.png)

## 7. Launch New Instance from the AMI
Go to:
AMIs → Select your AMI → Launch Instance
This time, in Advanced Details → User Data, 

💡 No need to install Apache again —
The AMI already has httpd installed.
This speeds up boot time.
Screenshot:

![Launch from AMI](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-LaunchfromAMI.png)

## 8. Access New Instance Public DNS
Open the Public DNS in your browser.
You should immediately see: Hello World from <hostname>

✔ Much faster because Apache was pre-installed in AMI
✔ Only index.html was created via user data

Screenshot:

![Hello World Page](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-HelloWorldPage.png)

10. Troubleshooting (Short Summary)
Common Issue 1 — Connection Refused
Cause: httpd service still starting
Fix: Wait 20–40 seconds before testing
Common Issue 2 — Apache Not Installed
Cause: wrong user data or missing #!/bin/bash
Fix: ensure script starts at line 1
