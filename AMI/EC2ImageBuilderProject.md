Project: EC2 Image Builder Project
EC2 Image Builder is used to automate creation → maintenance → validation → testing → distribution of EC2 AMIs.

Automates OS patching
Builds golden images
Installs software (Java, AWS CLI, etc.)
Can run on a schedule (weekly, cron, manual)
Free service (you only pay for underlying EC2 + EBS usage)


Architecture Flow
Image Builder → Builder EC2 Instance → Build Components Applied
              → New AMI Created → Test EC2 Instance → Validation
              → AMI Distributed → Ready for Launch


1. Open EC2 Image Builder Service

Screenshot:

![Open Image Builder](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-OpenImageBuilder.png)

2. Create Image Pipeline
Step 2.1 — Create Pipeline

Pipeline Name: demopipeline
Build Schedule Options:

Scheduled Builder
CRON exception
Manual (we choose this)

Screenshot:

![Create Pipeline](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-createpipeline.png)

3. Create Recipe
Step 3.1 — Choose Recipe Type

Choose Create a new recipe
Options: AMI & Docker image

We choose AMI

Screenshot:
![Image Type](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-ImageType.png)

Step 3.2 — General Details

Recipe Name: MyDemoRecipe
Version: 1.0.0
Source Image:

Image OS: Amazon Linux 2023
Origin: Quick Start
Image Name: Amazon Linux 2023 x86


Auto Versioning: Use latest version

Screenshot:

![Recipe General Settings](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-RecipeGeneralSettings.png)

4. Add Build Components
We add two components:
1️⃣ amazon-corretto-17-headless
→ Installs Java 17
2️⃣ aws-cli-version-2-linux
→ Installs AWS CLI v2
You can reorder components by dragging them.

Screenshot:
![Add Components](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMi-Addcomponents.png)

Post-Build Test Components
We skip test components for this demo.

5. Define Infrastructure Configuration
Step 5.1 — Create IAM Role
Create a new IAM Role for EC2 Image Builder:
Attach Policies:

EC2InstanceProfileForImageBuilder
AmazonSSMManagedInstanceCore
EC2InstanceProfileForImageBuilderECRContainerBuilds

Role Name:
EC2InstanceProfileForImageBuilder

Screenshot:

![Create IAM Role](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-CreateRole.png)

Step 5.2 — Creating a new Infra Config

Name: MyDemoInfra
IAM Role: Select the one we created
Instance Type: t2.micro (Free Tier)

Screenshot:
![Infra Configuration](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-InfraConfig.png)

6. Distribution Settings
You can:

Create new distribution settings
Or use defaults

Add regions where AMI should be distributed.

Screenshot:

![Distribution Settings](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-DistributionSettings.png)

7. Run Pipeline
Go to pipeline → Actions → Run Pipeline

Screenshot:

![Run Pipeline](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-RunPipeline.png)

9. Build Stage
EC2 Image Builder automatically creates a Builder Instance.
Check tags:
Created by EC2 Image Builder

Screenshot:

![Builder Instance](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-BuilderInstance.png)

Build stage can take 30+ minutes.

11. Testing Stage
After build completes:

Builder instance terminates
Test Instance launches
Validation runs (if tests are configured)

Screenshot:

![Test Instance](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-TestInstance.png)

10. New AMI Created
Check AMIs → you will see new AMI:

Name: MyDemoRecipe
Tags: Created by EC2 Image Builder

Screenshot:

![Created AMI](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-CreatedAMI.png)

11. Distribution Stage
If distribution is configured:

AMI is copied to other regions

12. Launch EC2 from New AMI
Launch instance → choose AMI → select MyDemoRecipe AMI

13. Validate Java & AWS CLI
SSH into instance and run:
aws --version
java --version

Expect:

AWS CLI v2 installed
Amazon Corretto 17 installed

Screenshot:

![Version Check](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/AMI/AMI-VersionCheck.png)
