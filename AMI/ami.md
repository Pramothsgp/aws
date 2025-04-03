
### **Step 1: Deploy Application in Region A**
1. **Launch an EC2 Instance** in **Region A**.
   - Choose an appropriate AMI (e.g., Amazon Linux, Ubuntu).
   - Select instance type (e.g., t2.micro for small workloads).
   - Configure security group to allow HTTP (port 80) or HTTPS (port 443).
   - Attach an **EBS Volume** for storing the application files.

2. **Install and Configure Web Server**
   - Update the system:  
     ```sh
     sudo yum update -y  # For Amazon Linux
     sudo apt update && sudo apt upgrade -y  # For Ubuntu
     ```
   - Install necessary packages:  
     ```sh
     sudo yum install -y httpd  # For Amazon Linux
     sudo apt install -y apache2  # For Ubuntu
     ```
   - Start and enable the web server:  
     ```sh
     sudo systemctl start httpd
     sudo systemctl enable httpd
     ```
   - Deploy the **"Hello World"** application by placing the HTML file in `/var/www/html/index.html`.

3. **Attach EBS Volume and Store Application Files**
   - Create and attach an **EBS Volume** to the EC2 instance.
   - Mount the volume:  
     ```sh
     sudo mkdir /mnt/data
     sudo mount /dev/xvdf /mnt/data
     ```
   - Copy application files to the volume:
     ```sh
     sudo cp /var/www/html/index.html /mnt/data/
     ```

---

### **Step 2: Create an AMI and Snapshot**
1. **Create an Amazon Machine Image (AMI)**
   - Go to **EC2 Dashboard > Instances**.
   - Select the running instance and click **"Create Image"**.
   - Provide a name and description, and confirm.

2. **Create an EBS Snapshot**
   - Go to **EC2 Dashboard > Elastic Block Store > Snapshots**.
   - Select the attached **EBS Volume** and click **"Create Snapshot"**.

---

### **Step 3: Replicate Application in Region B**
1. **Copy AMI to Region B**
   - Go to **EC2 Dashboard > AMIs**.
   - Select the AMI, click **"Actions" > "Copy AMI"**.
   - Choose **Region B** and initiate the copy process.

2. **Copy EBS Snapshot to Region B**
   - Go to **EC2 Dashboard > Snapshots**.
   - Select the snapshot, click **"Actions" > "Copy"**.
   - Choose **Region B** and initiate the copy.

3. **Launch EC2 Instance in Region B**
   - Use the copied AMI to launch a new EC2 instance.
   - Ensure security group and configurations match those in **Region A**.

4. **Create and Attach EBS Volume from Snapshot**
   - In **Region B**, go to **EC2 Dashboard > Elastic Block Store > Snapshots**.
   - Select the copied snapshot and create a new EBS Volume.
   - Attach it to the new EC2 instance and mount it similarly.

---

### **Step 4: Verify the Deployment**
- Check if the web application is accessible in **Region B**.
- Ensure the EBS volume contains the application files.

---

### **Conclusion**
By following these steps, the startup successfully deploys and replicates its **"Hello World"** application across AWS regions, ensuring high availability and scalability. 🚀