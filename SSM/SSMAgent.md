🚀 AWS SSM Hands‑On Project – Start EC2 Instances With SSM Agent

A practical hands‑on guide demonstrating how to launch EC2 instances without SSH access and manage
them securely using AWS Systems Manager (SSM). This project shows how to use SSM Agent, IAM roles, 
Fleet Manager, and Session Manager to control instances without any inbound ports.

A complete step‑by‑step guide with screenshot references.

✅ Step 1: Open AWS Systems Manager → Fleet Manager

Go to AWS Console
Navigate to Systems Manager
Click Fleet Manager in the left menu
You will see 0 Managed Nodes

📸 Screenshot Reference:

![fleet_manager_empty.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-FleetManagerEmpty.png)

✅ Step 2: Create IAM Role for EC2 (Required for SSM)

Open IAM Console
Click Roles → Create Role
Select: AWS Service → EC2
Click Next: Permissions
Search for and select the policy:

AmazonSSMManagedInstanceCore

Click Next
Name your role:
EC2-SSM-Managed-Role
Click Create Role

📸 Screenshot Reference:

![create_iam_role.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-CreateIAMRole.png)

✅ Step 3: Launch EC2 Instances With No Inbound Rules
3.1 — Start Instance Creation

Go to EC2 Console → Launch Instance
Name your instance:
ssm-node-1
(launch 3 instances later)

3.2 — Select AMI
Choose: ✔ Amazon Linux 2023 AMI (SSM Agent preinstalled)

3.3 — Choose Instance Type

Pick t2.micro (free tier eligible)

3.4 — Key Pair
Select “Proceed without a key pair”
(SSM does NOT require SSH keys)

3.5 — Configure Security Group

Click Create Security Group
Delete ALL inbound rules
Leave outbound rules default (Allow All)

📸 Screenshot:

![07_security_group_no_inbound.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-SGNobound.png)

3.6 — Attach IAM Role
Scroll to Advanced Details → IAM Instance Profile
Select the IAM role created earlier:
✔ EC2-SSM-Managed-Role

📸 Screenshot:

![select_iam_role.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-SelectedIAMRole.png)

3.7 — Launch 3 Instances
Repeat or set Number of Instances = 3

✅ Step 4: Verify Instances in Fleet Manager (Managed Nodes)

Go to Systems Manager → Fleet Manager
You will now see your EC2 instances under:
Managed Nodes
Check:

Instance ID
OS
SSM Agent version
Status: Online

📸 Screenshot:

![managed_nodes_list.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-ManagedNodesList.png)

✅ Step 5: Test SSM Session Manager (No SSH Needed)

Select an instance from the list
Click Node actions → Start session
A terminal window opens inside your browser
Run commands to verify access:

📸 Screenshot:

![session_manager_terminal.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-SessionManagerTerminal.png)

