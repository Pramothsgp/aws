# Setting Up an EC2 Instance and MySQL Database on AWS

## 1. Create an EC2 Instance

### Step 1: Sign in to AWS Console
1. Sign in to the AWS Management Console.
2. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
3. Choose the **AWS Region** where you want to create the EC2 instance.

### Step 2: Launch an EC2 Instance
1. Go to **EC2 Dashboard** and click **Launch instance**.
---

![EC2 Launch](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/Tutorial_WebServer_11.png)

---
2. Configure the following settings:
   - **Name**: `ec2-database-connect`
   - **AMI**: Amazon Linux 2023 (Amazon Machine Image)
   - **Instance type**: `t2.micro`
---
![OS Image](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/Tutorial_WebServer_12.png)
---
   - **Key pair**: Select an existing key pair or create a new one.
   - **Network settings**: Allow SSH traffic (Choose **My IP** or a custom range).
---
![Network Settings](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/EC2_RDS_Connect_NtwkSettings.png)
---
> **Warning:** Using `0.0.0.0/0` for SSH access is insecure in a production environment.

3. Click **Launch instance**.
4. Wait until the instance state changes to **Running**.

---
![running](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/getting-started-ec2-id.png)
---

### Step 3: Retrieve Instance Details
1. Select your EC2 instance from the list.
2. Go to the **Details** tab and note:
   - **Public IPv4 DNS**
---
![DNS](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/easy-create-ec2-public-dns.png)
---
   - **Key pair name**
---
![Key Pair Name](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/easy-create-ec2-key-pair.png)
---
## 2. Create a MySQL Database in Amazon RDS

### Step 1: Sign in to RDS Console
1. Open the [Amazon RDS console](https://console.aws.amazon.com/rds/).
2. Select the same **AWS Region** used for the EC2 instance.
3. Click **Databases** > **Create database**.

### Step 2: Configure Database Settings
1. Select **Easy create**.
---
![easy create](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/easy-create-option.png)
---
2. Choose **MySQL** as the database engine.
3. Choose **Free tier** for the DB instance size.

---
---
![MySQL](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/easy-create-mysql.png)


4. Set:
   - **DB Instance Identifier**: `database-test1`
   - **Master username**: Choose a name or keep the default.
   - **Password**: Choose a password or auto-generate one.
5. Enable **Connect to an EC2 compute resource** and select the EC2 instance created earlier.
---
![EC2 Connect](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/EC2_RDS_Setup_Conn-EasyCreate.png)
---
6. Click **Create database**.

---
---
![Create Database](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/rds-create-db.png)

> The status will be **Creating** until it becomes **Available** (can take up to 20 minutes).
---
![Creating](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/MySQL-Launch06.png)
---
## 3. Connect to MySQL from EC2

### Step 1: Connect to EC2 via SSH
```bash
ssh -i /path-to-key.pem ec2-user@ec2-public-ip
```

### Step 2: Install MySQL Client
```bash
sudo dnf update -y
sudo dnf install mariadb105 -y
```

### Step 3: Connect to the MySQL Database
```bash
mysql -h your-db-endpoint -P 3306 -u admin -p
```

> Use the **endpoint** from the **Connectivity & Security** tab in RDS.

------
![Endpoint](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/MySQLConnect1.png)

### Step 4: Run SQL Commands
```sql
SELECT CURRENT_TIMESTAMP;
```

## 4. Delete Resources (Optional)

### Delete EC2 Instance
1. Open the **EC2 Console**.
2. Select the instance.
3. Click **Instance state** > **Terminate instance**.

### Delete RDS Instance
1. Open the **RDS Console**.
2. Select the database.
3. Click **Actions** > **Delete**.
4. Disable "Create final snapshot" and confirm deletion.

---
---
![Delete RDS](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/rds-delete-db.png)

## 5. (Optional) Automate with AWS CloudFormation

To provision resources using AWS CloudFormation:
1. [Download the CloudFormation template](https://github.com/aws-cloudformation/aws-cloudformation-templates).
2. Open the **AWS CloudFormation console**.
3. Click **Create Stack** > **Upload a template file**.
4. Set parameters like availability zones, key pair, and database settings.
5. Click **Submit**.

For more details, refer to [AWS CloudFormation Documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html).

---
This guide provides a step-by-step process to set up an EC2 instance, connect it to an RDS MySQL database, and manage it efficiently. 🚀

