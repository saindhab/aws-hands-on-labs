🚀 Project: AWS Tags and SSM Resource Groups (Step-by-Step Guide)

This project shows how to tag EC2 instances and use those tags to create SSM Resource Groups, 
which allow you to manage multiple instances at once for tasks like patching, automation, run commands, etc.

✅ Step 1: Add Tags to Your EC2 Instances
👉 Go to AWS Console → EC2 → Instances
Select each instance and add the following tags:

Instance 1

Name: MyDevInstance
Env: Dev
Team: Finance

📸 Screenshot: 

![01_ec2_tags_dev.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-01EC2tagDev.png)

Instance 2

Name: MyProdInstance
Env: Prod
Team: Finance

📸 Screenshot: 

![02_ec2_tags_prod.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-02EC2tagProd.png)

Instance 3

Name: MyOtherDevInstance
Env: Dev
Team: Operations

📸 Screenshot: 

![03_ec2_tags_otherdev.png](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-03EC2tagOtherDev.png)

✅ Step 2: Create an SSM Resource Group

Systems Manager → Resource Groups
Click Create Resource Group
Select Tag-based
Grouping criteria
Resource Type: AWS::EC2::Instance
Add filters:
Environment: Dev

📸 Screenshot: 

![Dev Group](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-DevGroup.png)

Click Create Resource Group
Select Tag-based
Grouping criteria
Resource Type: AWS::EC2::Instance
Add filters:
Environment: Prod

📸 Screenshot: 

![Prod Group](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-MyProdGroup.png)

Click Create Resource Group
Select Tag-based
Grouping criteria
Resource Type: AWS::EC2::Instance
Add filters:
Team: Finance

📸 Screenshot: 

![Finance Group](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/SSM/SSM-FinanceGroup.png)


