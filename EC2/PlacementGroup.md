🚀 Project : Create Placement Groups (Cluster, Spread, Partition)
This lab demonstrates how to create three EC2 Placement Groups and shows where to choose placement groups while launching an EC2 instance.
➡️ No need to actually launch EC2 — only show the option.
Placement Groups help you control how EC2 instances are physically deployed within AWS infrastructure.

1️⃣ Navigate to EC2 Dashboard

Open AWS Console
Search for EC2
Go to EC2 Dashboard

📸 Screenshot:
![EC2 Dashboard](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-Dashboard.png)

2️⃣ Open Placement Groups Section

In EC2 left navigation pane
Scroll down to Placement Groups (under "Network & Security")

📸 Screenshot:
![Placement Groups Menu](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-PlacementGroup.png)

3️⃣ Create First Placement Group — Cluster

Click Create placement group
Name: pg-cluster
Strategy: Cluster
Keep all other options default
Click Create group

📸 Screenshot:
![Create Cluster Group](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-pgCluster.png)

4️⃣ Create Second Placement Group — Spread

Click Create placement group
Name: pg-spread
Strategy: Spread
Instances in this group are placed on distinct hardware racks

📸 Screenshot:
![Create Spread Group](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-pgSpread.png)

5️⃣ Create Third Placement Group — Partition

Click Create placement group
Name: pg-partition
Strategy: Partition
Number of partitions: 3
Click Create group

📸 Screenshot:
![Create Partition Group](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-pgPartition.png)

6️⃣ Verify All Three Placement Groups
Your list should show:

| Name         | Strategy   | Partitions |
|:------------:|:----------:|:----------:|
| pg-cluster   | Cluster    | –          |
| pg-spread    | Spread     | –          |
| pg-partition | Partition  | 3          |

📸 Screenshot:
![Placement Group List](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-pgList.png)

7️⃣ Demonstrate Placement Group Usage in EC2 Launch
(Only show, do NOT launch instance)

Click Launch instances
Scroll down to Advanced details
Look for the field: Placement group name
Dropdown should show:

pg-cluster
pg-spread
pg-partition

📸 Screenshot:
![Placement Group Option in Launch](https://raw.githubusercontent.com/saindhab/aws-screenshots/main/ec2/EC2-PlacementGroupinLaunch.png)
This confirms your placement groups are created and selectable during EC2 provisioning.

### 🧠 Quick Interview Tip (One‑liner Summary)
| Placement Group | Purpose | Best For |
|-----------------|---------|----------|
| **Cluster** | Low latency, high bandwidth | HPC, ML, Spark |
| **Spread** | Max fault isolation | Zookeeper, etcd, critical services |
| **Partition** | Rack-aware partitions | Hadoop, Cassandra, Kafka |

Keep this table in your mind — most interview questions revolve around this.
