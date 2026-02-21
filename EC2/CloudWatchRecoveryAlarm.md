🚀 Project: EC2 Instance Status Checks + CloudWatch Recovery Alarm
This hands‑on lab demonstrates:

System vs Instance Status Checks
Reporting issues to AWS
Creating a CloudWatch alarm
Triggering EC2 recovery action
Viewing alarm history
Testing alarm manually via CloudShell

AWS documentation confirms EC2 performs two main status checks: System Status Check and Instance Status Check to 
detect underlying hardware issues or OS boot issues respectively. 
Troubleshooting resources confirm instance checks fail on boot errors, OS issues, or EBS mount failures.

1️⃣ Understanding EC2 Status Checks
🔵 System Status Check
Checks AWS infrastructure hosting your instance:

Physical host hardware failures
AWS networking issues
Power or hypervisor issues
If this fails → AWS’s responsibility.
Fix: Stop → Start to migrate host.

🟢 Instance Status Check
Checks OS health inside your EC2:

Boot failures
Kernel panic
Wrong /etc/fstab
Network misconfig
Resource exhaustion
If this fails → your responsibility.

📸 Screenshot:

![EC2_Status_Checks_2of2](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-statuscheck.png)

2️⃣ Launch an EC2 Instance

EC2 → Launch Instance
Name: status-check-lab
AMI: Amazon Linux 2 / AL2023
t2.micro
SG → SSH only (22)

📸 Screenshot:
!EC2_Launch_Screen
Wait until:
2/2 checks passed

3️⃣ Report Instance Status (AWS Support)
If there is an issue:

Select EC2 instance
Actions → Instance settings → Report instance status

This sends a health issue ticket to AWS about infrastructure problems.

4️⃣ Alarm Notification — SNS Topic

Click Create new topic
Topic name: ec2-status-check-alerts
Enter your email
Confirm subscription email

📸 Screenshot:

![SNS_Topic_Creation](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-SNS_Topic_Creation.png)

5️⃣ Create a CloudWatch Alarm for Status Check Failure
🔧 Steps:

EC2 → Instances → Select your instance
Monitoring tab
Click Create Alarm

📸 Screenshot:
![Monitoring_Tab](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Monitoring_Tab.png)

6️⃣ Alarm Metric Setup
Type of data to sample
Choose:
StatusCheckFailed_System

This alarms if system status check fails.
📸 Screenshot:

![Metric_Selection](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Metric_Selection.png)

7️⃣ Alarm Threshold
Set:

Threshold type: Static
Whenever metric >= 1
For:  1 consecutive period
Period:  5 minutes

This means:
If system check fails even once within a 5‑minute period → ALARM.

📸 Screenshot:

![Alarm_Threshold](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Alarm_Threshold.png)

8️⃣ Alarm Action — Recover Instance
Choose:
EC2 action → Recover this instance

Recovery migrates your EC2 to new healthy hardware if AWS host has issues.
📸 Screenshot:

![Recover_Instance_Action](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Recover_Instance_Action.png)

9️⃣ Create the Alarm
Click Create Alarm.
Go to:
CloudWatch → Alarms
You should see:
State: OK
Reason: "The system status check is passing"

📸 Screenshot:

![Alarm_OK_State](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Alarm_OK_State.png)

🔟 View Alarm History
Go inside the alarm → History tab.
You should see:

“Alarm created”
“State updated from INSUFFICIENT_DATA → OK”

📸 Screenshot:

![Alarm_History](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Alarm_History.png)

1️⃣1️⃣ Trigger the Alarm Manually via CloudShell
We can simulate a failure using AWS CLI (for testing only).
Open CloudShell:

📸 Screenshot:

![CloudShell_Open](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-CloudShell_Open.png)

Run this AWS command from documentation:

--------------------------------------------------------------------------------------------------
aws cloudwatch set-alarm-state \
  --alarm-name "awsec2-i-01d9d938dd20fabb9-GreaterThanOrEqualToThreshold-StatusCheckFailed_System" \
  --state-value ALARM \
  --state-reason "Testing recovery action"
---------------------------------------------------------------------------------------------------

This forces the alarm into ALARM state.

📸 Screenshot:

![Set_Alarm_State](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Set_Alarm_State.png)

1️⃣2️⃣ Check Alarm → Actions Tab
After running the command:

Alarm state = ALARM
Actions tab will show:EC2 recovery action executed

📸 Screenshot:

![Alarm_Action_Triggered](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Alarm_Action_Triggered.png)

1️⃣3️⃣ Why You Don't “See” the Recovery Physically
EC2 recovery action works in the background, silently:

AWS migrates your instance to a healthy host
Public IP remains the same
Private IP remains the same
The instance does NOT reboot from OS point of view
No downtime (usually under few seconds)

This is why:
➡️ You won’t see any reboot message
➡️ No “Instance restarted” log
➡️ Only CloudWatch alarm history shows “Recover action invoked”.
AWS documentation confirms system status checks can trigger automatic recovery for impaired instances.
