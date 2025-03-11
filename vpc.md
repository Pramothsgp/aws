## **VPC (Virtual Private Cloud)**  
A **VPC** is a logically isolated network within AWS, where you can launch AWS resources securely.  

### **Subnet**  
A **subnet** is a segment of a VPC where resources are placed. A subnet can be **public** (with internet access) or **private** (isolated from direct internet access).  

### **Route Table**  
A **route table** contains routes that direct traffic between resources in the VPC and to external networks, such as the internet or other VPCs.  

### **Internet Gateway (IGW)**  
An **Internet Gateway** allows resources in a public subnet to communicate with the internet.  

### **Elastic IP (EIP)**  
An **Elastic IP** is a static, public IPv4 address that can be assigned to AWS resources, ensuring a consistent external IP.  

### **VPC Peering**  
**VPC Peering** is a direct network connection between two VPCs, allowing private communication without using the internet.  

### **NAT Gateway**  
A **NAT (Network Address Translation) Gateway** enables instances in a private subnet to access the internet while preventing inbound connections from the internet.  

---

# **Steps to Create a VPC in AWS**  

## **1. Create a VPC**  
1. Navigate to **VPC** in the AWS Management Console.  
2. Click **"Create VPC"**.  
3. Select **"VPC only"**.  
4. Enter a **name** for your VPC.  
5. Select **IPv4 CIDR** and enter a CIDR block (e.g., `10.0.0.0/24`, `172.x.0.0/24`, or `192.168.0.0/24`).  
6. Leave all other settings as default.  
7. Click **"Create VPC"**.  

## **2. Create Subnets**  
1. Navigate to **Subnets**.  
2. Click **"Create Subnet"**.  
3. Select the **VPC** from the dropdown.  
4. Enter a **name** for the subnet.  
5. Select an **Availability Zone**.  
6. Enter the **Subnet CIDR Block** (e.g., `10.0.0.0/28`).  
7. Click **"Create Subnet"**.  

## **3. Create a Security Group**  
1. Go to **Security Groups**.  
2. Click **"Create Security Group"**.  
3. Enter a **name** and **description**.  
4. Select the **VPC**.  
5. Add **inbound rules** as required.  
6. Note: **Security Groups are stateful**—if inbound traffic is allowed, the corresponding outbound response is automatically permitted.  
7. Click **"Create Security Group"**.  

## **4. Create an EC2 Instance**  
1. Navigate to **EC2** and launch a new instance.  
2. Under **Network Settings**, select:  
   - The **VPC** you created.  
   - The **Subnet** you created.  
   - **Auto-assign Public IP** → Set to **Disabled** (if private instance).  
   - Select an **existing Security Group**.  
3. Click **"Launch Instance"**.  
4. (Optional) Associate an **Elastic IP** if public access is required.  

---
