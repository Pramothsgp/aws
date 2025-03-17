## **Complete Guide: Installing Docker & Running MySQL in a Container**

This guide will help you **install Docker**, set up **MySQL in a Docker container**, and **restore a database from a backup file**.

---

## **1. Install Docker**
### **1.1 Install Docker on Linux**
If you’re using **Amazon Linux, CentOS, Ubuntu, or Debian**, follow these steps:

1️⃣ **Update the package list**  
```bash
sudo apt-get update -y  # For Ubuntu/Debian
sudo yum update -y      # For Amazon Linux/CentOS
```

2️⃣ **Install Docker**  
```bash
sudo apt-get install -y docker.io  # For Ubuntu/Debian
sudo yum install -y docker         # For Amazon Linux/CentOS
```

3️⃣ **Start the Docker service and enable it at boot**  
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

4️⃣ **Verify that Docker is installed**  
```bash
docker --version
```
You should see something like:
```
Docker version 24.0.5, build ced0996
```

5️⃣ **Allow your user to run Docker without `sudo`** (optional)  
```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## **2. Install MySQL Using Docker**
### **2.1 Pull the MySQL Docker Image**
```bash
docker pull mysql:8.0
```
- This downloads MySQL 8.0 from Docker Hub.
- Run `docker images` to verify the image is downloaded.

### **2.2 Create a MySQL Container**
```bash
docker run --name db -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=password \
  -v mysql-data:/var/lib/mysql \
  -d mysql:8.0
```
- `--name db` → Names the container `db`
- `-p 3306:3306` → Maps MySQL’s default port (3306) to the host
- `-e MYSQL_ROOT_PASSWORD=password` → Sets MySQL root password
- `-v mysql-data:/var/lib/mysql` → Creates a persistent volume for MySQL data
- `-d` → Runs the container in **detached mode** (background)

### **2.3 Verify That MySQL is Running**
```bash
docker ps
```
You should see:
```
CONTAINER ID  IMAGE       COMMAND                  STATUS         PORTS                    NAMES
c3a5b9c7b8f2  mysql:8.0   "docker-entrypoint.s…"   Up 10 seconds  0.0.0.0:3306->3306/tcp   db
```

### **2.4 Connect to MySQL**
To open MySQL inside the running container:
```bash
docker exec -it db mysql -u root -p
```
- Enter the password (`password` in this example).

---

## **3. Restore a MySQL Database from a Backup File**
### **3.1 Place the Backup File in Your Server**
Make sure your MySQL dump file (`backup.sql`) is in a known directory.

Verify the file exists:
```bash
ls -lh ./Ecart/backup.sql
```

### **3.2 Import the Database**
**Wrong command (causes errors):**
```bash
docker exec -it db mysql -u root ecart < ./Ecart/backup.sql
```
**Correct way (using `cat` and `docker exec`):**
```bash
cat ./Ecart/backup.sql | docker exec -i db mysql -u root ecart
```
- `cat ./Ecart/backup.sql` → Reads the backup file
- `|` (pipe) → Passes the data into the MySQL command
- `docker exec -i db mysql -u root ecart` → Runs MySQL inside the container

### **3.3 Verify the Imported Database**
Inside MySQL, check the database:
```sql
SHOW DATABASES;
```
Switch to the imported database:
```sql
USE ecart;
SHOW TABLES;
```

---

## **4. Managing the MySQL Container**
### **4.1 Stop the MySQL Container**
```bash
docker stop db
```

### **4.2 Restart the MySQL Container**
```bash
docker start db
```

### **4.3 Check Logs if MySQL Fails to Start**
```bash
docker logs db
```

### **4.4 Remove MySQL Container (if needed)**
```bash
docker rm -f db
docker volume rm mysql-data
```

---

## **5. Automate MySQL Startup with Docker Compose**
Instead of running long commands, you can use **Docker Compose**.

### **5.1 Install Docker Compose (if not installed)**
```bash
sudo apt-get install -y docker-compose  # For Ubuntu/Debian
sudo yum install -y docker-compose      # For Amazon Linux/CentOS
```

### **5.2 Create a `docker-compose.yml` file**
```yaml
version: '3.8'
services:
  db:
    image: mysql:8.0
    container_name: db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql
volumes:
  mysql-data:
```

### **5.3 Start MySQL Using Docker Compose**
```bash
docker-compose up -d
```
- Runs MySQL with **persistent storage**.
- `docker-compose down` stops and removes containers.

---

## **6. Summary**
| **Step** | **Command** |
|----------|------------|
| **Install Docker** | `sudo apt-get install -y docker.io` |
| **Start Docker** | `sudo systemctl start docker && sudo systemctl enable docker` |
| **Pull MySQL Image** | `docker pull mysql:8.0` |
| **Run MySQL Container** | `docker run --name db -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password -v mysql-data:/var/lib/mysql -d mysql:8.0` |
| **Check Running Containers** | `docker ps` |
| **Connect to MySQL** | `docker exec -it db mysql -u root -p` |
| **Restore Database** | `cat ./Ecart/backup.sql | docker exec -i db mysql -u root ecart` |
| **Stop MySQL** | `docker stop db` |
| **Restart MySQL** | `docker start db` |
| **View Logs** | `docker logs db` |

---

This guide **covers everything** from **installing Docker** to **running MySQL** and **restoring backups**. Let me know if you have any issues! 🚀