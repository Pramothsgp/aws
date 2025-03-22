
## **AWS Auto Scaling Setup with Load Balancer**

### **1. Create a Load Balancer (LB)**
1. **Go to EC2 Dashboard → Load Balancers** → Create a new **Application Load Balancer (ALB)**.
2. **Security Group:** 
   - Create a **new security group** allowing **HTTP (port 80)**.
   - Attach the security group to the Load Balancer.
3. **Target Group:**
   - Create an **empty target group** (since Auto Scaling will register instances dynamically).
   - Select **HTTP** as the protocol.

---

### **2. Create a Launch Template**
1. **Go to EC2 Dashboard → Launch Templates** → Create a new **Launch Template**.
2. **Configure Basic Settings:**
   - **AMI (Amazon Machine Image):** Choose OS (e.g., Amazon Linux 2, Ubuntu).
   - **Instance Type:** Choose based on workload (e.g., `t2.micro` for testing).
   - **Security Group:** Create or use an existing group that allows **SSH (22) & HTTP (80)**.
   - **Key Pair:** Select or create a key pair for SSH access.
3. **User Data Script (Bootstrap Configuration)**  
   - Add a **User Data script** to install and start a web server on launch:
   ``` sh
   #!/bin/bash
    sudo yum update -y
    sudo yum install -y httpd
    sudo systemctl start httpd
    sudo systemctl enable httpd

    # Fetch private IP address of the instance
    PRIVATE_IP=$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)

    # Create an HTML file displaying the private IP
    echo "<h1>Instance Running</h1>" > /var/www/html/index.html
    echo "<p>Private IP: $PRIVATE_IP</p>" >> /var/www/html/index.html

   ```
4. **Create the Launch Template**.

---

### **3. Create an Auto Scaling Group (ASG)**
1. **Go to EC2 Dashboard → Auto Scaling Groups** → Create a new **Auto Scaling Group**.
2. **Attach the Launch Template** created earlier.
3. **Select VPC and Subnets**:
   - Ensure subnets are spread across **multiple availability zones** (AZs).
4. **Attach Load Balancer**:
   - Select the previously created **Load Balancer**.
   - Enable **Elastic Load Balancing (ELB) Health Checks**.
5. **Set Scaling Parameters:**
   - **Minimum Capacity:** The least number of instances running at any time.
   - **Desired Capacity:** The initial number of instances launched.
   - **Maximum Capacity:** The maximum limit of instances that can be scaled.
6. **Review & Create the Auto Scaling Group**.

---

### **4. Deleting Resources (Cleanup)**
To delete the setup after testing:
1. **Delete Auto Scaling Group (ASG)**.
   - Go to **Auto Scaling Groups** and delete the group.
2. **Terminate EC2 Instances** (Auto Scaling instances).
3. **Delete Load Balancer (LB) & Target Group**.
4. **Delete the Launch Template**.

---

### **Additional Notes**
- **Ping Uses ICMP Protocol:**  
  `ping` works on the **ICMP (Internet Control Message Protocol)**, not TCP/UDP.
- **Auto Scaling Works with CloudWatch:**  
  - You can set CloudWatch alarms to scale based on **CPU Usage, Memory, or Request Count**.
