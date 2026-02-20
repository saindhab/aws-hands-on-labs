🚀 Project: CloudWatch – Unified CloudWatch Agent (Metrics + Apache Logs)

## This version exactly follows your flow:

IAM Role
EC2 launch
Apache install
View access + error logs
Install CloudWatch Agent
Run Config Wizard
Pull config from SSM Parameter Store
See logs + metrics in CloudWatch
Testing & validation

## This hands‑on lab covers how to install and configure the Unified CloudWatch Agent to collect:

Apache access_log
Apache error_log
System metrics (CPU, memory, disk, etc.)

1️⃣ Create IAM Role for CloudWatch Agent
We need an IAM Role that allows EC2 to send metrics + logs + fetch parameter store values.
Steps

Go to IAM → Roles → Create Role
Trusted entity: AWS Service
Use case: EC2
Attach these policies:

CloudWatchAgentAdminPolicy
AmazonSSMManagedInstanceCore (needed for SSM parameter retrieval)

📸 Screenshot placeholder:

![IAM_Role_Creation](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2_IAM_Role_Creation.png)

Name the role: EC2CloudWatchAgentAdminRole

2️⃣ Launch EC2 Instance

Go to EC2 → Launch Instance
Name: cloudwatch-agent-lab
AMI: Amazon Linux 2 / Amazon Linux 2023
Instance Type: t2.micro
IAM Role: Select EC2CloudWatchAgentAdminRole
Security Group:

Inbound: SSH (22)
Inbound: HTTP (80)

📸 Screenshot placeholder:

![EC2_Launch_Page](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Launch_InstanceChangeType.png)

3️⃣ Connect via EC2 Instance Connect

Select instance
Click Connect → EC2 Instance Connect
Connect

📸 Screenshot placeholder:

![EC2_Instance_Connect](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-InstanceConnect2.png)

4️⃣ Install & Start Apache HTTP Server
Run the following commands:

--------------------------------------------
sudo su
yum install httpd -y

echo "Hello World" > /var/www/html/index.html

systemctl start httpd
systemctl enable httpd
--------------------------------------------

📸 Screenshot placeholder:

![Apache_Installed](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Apache_Installed.png)

Open in browser:

You should see Hello World.

📸 Screenshot placeholder:

![Apache_Hello_World](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Apache_Hello_World.png)

5️⃣ View Apache Logs

Access log: cat /var/log/httpd/access_log

Error log: cat /var/log/httpd/error_log

📸 Screenshot placeholder:

![Apache_Logs](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Apache_Logs.png)

Now we will send both logs to CloudWatch.

6️⃣ Install Unified CloudWatch Agent
For Amazon Linux:

-------------------------------------------
sudo yum install amazon-cloudwatch-agent -y
-------------------------------------------

📸 Screenshot placeholder:

![CWAgent_Installed](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-CWAgent_Installed.png)

7️⃣ Run CloudWatch Agent Config Wizard (SSM Parameter Store)
This wizard helps us build the JSON config automatically.

Run:

-------------------------------------------------------------------------------
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
-------------------------------------------------------------------------------

Wizard Notes:

Accept defaults for:

Logs
Metrics
Enable both logs + metrics

Select Apache log paths:

/var/log/httpd/access_log
/var/log/httpd/error_log

CollectD → Select: No

At the end, the wizard will save the configuration to:
✔ SSM Parameter Store under key: AmazonCloudWatch-linux

📸 Screenshot placeholder:

![CWAgent_Wizard_Completed](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-CWAgent_Wizard_Completed.png)

8️⃣ View Parameter Store Config
Go to:
Systems Manager → Parameter Store → AmazonCloudWatch-linux
You will see a full JSON configuration containing all your answers.

📸 Screenshot placeholder:

![SSM_Parameter_Store_JSON](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-SSM_Parameter_Store_JSON.png)

9️⃣ Start CloudWatch Agent Using SSM Parameter
Now fetch config + start agent:

-----------------------------------------------------------------------------------------------------------------------------
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:AmazonCloudWatch-linux -s
-----------------------------------------------------------------------------------------------------------------------------

📸 Screenshot placeholder:

![CWAgent_Started](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-CWAgent_Started.png)

----------------------------------------------------------------
Verify agent is running: systemctl status amazon-cloudwatch-agent
-----------------------------------------------------------------

🔟 Verify Logs in CloudWatch
Go to:
CloudWatch → Logs → Log Groups
You should see new groups created for:

/var/log/httpd/access_log
/var/log/httpd/error_log

📸 Screenshot placeholder:

![CW_Log_Groups](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-CW_Log_Group.png)

1️⃣1️⃣ Verify Metrics in CloudWatch
Go to:
CloudWatch → Metrics → CWAgent namespace
You will see metrics like:

mem_used_percent (memory)
cpu_usage_idle
disk_used_percent
swap_used
processes_idle

📸 Screenshot placeholder:

![CWAgent_Metrics](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-CWAgent_Metrics.png)

Note: Memory metrics are ONLY available if CloudWatch Agent is installed (default system metrics do NOT include memory).

1️⃣2️⃣ Test: Generate Apache Logs
Refresh your webpage:
http://ec2-3-92-56-44.compute-1.amazonaws.com/

Each refresh will create new entries in:

/var/log/httpd/access_log
/var/log/httpd/error_log

Agent will automatically ship these to CloudWatch.
Refresh CloudWatch log stream → entries should appear.
📸 Screenshot placeholder:

![CWAgent_Log_Test](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-CWAgent_Log_Test.png)

1️⃣3️⃣ Additional Info: Metrics Collected by CloudWatch Agent
From AWS documentation (types of metrics CloudWatch Agent collects):
Examples include:

CPU: cpu_usage_idle, cpu_usage_user, cpu_usage_system
Disk Space: disk_used_percent
Disk I/O: io_reads, io_writes
Memory: mem_used_percent, mem_available
Processes: processes_idle
Swap: swap_used, swap_free

