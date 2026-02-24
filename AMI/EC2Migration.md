Project — EC2 Instance Migration using AMIs
This documentation includes screenshot references pointing to your GitHub repo folder:
aws-screenshot/

1. EC2 Migration Across Availability Zones
Steps
Step 1: Launch EC2 in us-east-1d
Screenshot:
![Launch EC2](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-EC2Launch.png)

Step 2: Create an AMI from this instance
Screenshot:
![Create AMI](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-MyAMI.png)

Step 3: Wait until AMI status becomes "available"
Screenshot:
![AMI Available](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-AMIavailable.png)

Step 4: Launch new EC2 in us-east-1a using this AMI
Screenshot:
![Launch from AMI](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-LaunchInstancefromAMI.png)

2. Cross‑Account AMI Sharing (Encrypted with CMK‑A)
Source Account A

AMI with Encrypted EBS Volume
Encrypted with CMK‑A
Share AMI with Account B
Also share KMS CMK-A with permissions:

kms:DescribeKey
kms:Decrypt
kms:ReEncrypt

Architecture Screenshots:
![AMI Share](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-AMIshare.png)

3. Cross‑Account AMI Copy (Encrypted AMI)
Source Account A

AMI encrypted using CMK‑A
Must share:

AMI, Snapshot, KMS CMK-A permissions

Target Account B

Can now Copy AMI
While copying, target account can choose to:

Keep CMK-A
OR re-encrypt using CMK‑B

Architecture Screenshots:
![AMI Copy](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMi-AMIcopyMigration.png)

4. AMI Permission Tab Instructions

Select AMI → Permissions
Set AMI availability:

Screenshots:
![AMI Permissions](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-AMIPermissions.png)

Public (only unencrypted AMIs)
Private (encrypted AMIs)


Add AWS Account IDs to share
Check: Add create volume permission to associated snapshots

Screenshots:
![Create Volume Permission](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-Volume&Account.png)
